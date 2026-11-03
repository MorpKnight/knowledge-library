---
title: Reproducing Qwen-Image-2.1 GGUF in ComfyUI on a GCP GPU
description: A reproducible record of a five-hour Flex-start GPU experiment, including setup, prompts, observed results, and limitations.
date: 2026-11-03
tags:
  - generative-ai
  - comfyui
  - gcp
  - gpu
publish: true
draft: false
---

# Reproducing Qwen-Image-2.1 GGUF in ComfyUI on a GCP GPU

## Question and outcome

Can a separate GPU VM run the `0xSojalSec/Qwen-Image-2.1-Uncensored-GGUF` model through ComfyUI, and can its generated images be inspected interactively? On 3 November 2026, a standalone Google Compute Engine Flex-start VM with one NVIDIA RTX PRO 6000 Blackwell Server Edition successfully generated a 512 × 512 test image using the Q8_0 GGUF, the Qwen3-VL text encoder, and the Qwen-Image-2.1 VAE. Several subsequent text-to-image runs also completed. The VM and its 150 GB boot disk were later deleted at the user's request; the images, workflow, environment, and logs on that disk no longer exist.

The experiment record is this conversation and the commands and outputs observed during it. The original output image was not retained in the knowledge library; visual quality, exact reproduction, and claims about the model's censorship behavior were not systematically assessed. Reference-image editing was not independently validated. Do not use private photos or generated images without the subject's consent.

## Architecture and recorded setup

- Machine: GCP `g4-standard-48`, one RTX PRO 6000 Blackwell Server Edition; `nvidia-smi` reported 97,887 MiB GPU memory. Zone used: `us-central1-b`.
- Provisioning: `FLEX_START`, maximum run duration **5 hours**, termination action `STOP`. At the time of the run, standard capacity was unavailable in several zones and a different region had zero GPU quota; availability and quota can change. Flex-start may wait for capacity. A stopped VM's disk continues to incur storage charges. [GCP Flex-start documentation](https://docs.cloud.google.com/compute/docs/instances/about-flex-start-vms)
- OS and driver: Ubuntu 24.04 image `ubuntu-2404-noble-amd64-v20261024`, Python 3.12.3, NVIDIA open driver 580.178.04. `nvidia-smi` showed CUDA driver capability 13.0; PyTorch used its own CUDA 12.8 wheels (`2.11.0+cu128`). A CUDA tensor multiplication returned the expected value.
- ComfyUI 0.43.0 and frontend 1.57.6; `leejet/ComfyUI-GGUF` plus `rgthree-comfy`. Repository commit IDs and a dependency lockfile were not captured, so installing current versions will not guarantee identical behavior.
- Models: `qwen-image-2.1-Q8_0.gguf`, `qwen3vl_8b_int8_convrot.safetensors`, and `qwen_image_2.1_vae_bf16.safetensors`. The GGUF SHA-256 matched the publisher's manifest: `763ee46b76069d9c54ab26719a0b9222575751560c8bf3a48275736c44351c0e`. The text encoder and VAE were successfully loaded in the smoke test, but separate checksums were not recorded.
- Access: ComfyUI listened on VM loopback; an SSH local port forward exposed it only on the operator's machine. No public ComfyUI firewall rule was created.

The [GGUF model card](https://huggingface.co/0xSojalSec/Qwen-Image-2.1-Uncensored-GGUF) describes the file as a quantization of the original image-transformer weights, without fine-tuning or other weight modification. Its “uncensored” label reflects the publisher's tests, not a measured property established in this experiment. The GGUF alone is insufficient: the text encoder and VAE are separate downloads.

## Reproduce

These commands are a **reconstruction from the session**, not a newly tested installation script. Review current [G4 requirements](https://cloud.google.com/compute/docs/accelerator-optimized-machines), regional quotas, available capacity, licenses, and pricing before creating resources. Pick your own project and zone. The full-GPU G4 boot disk must use Hyperdisk Balanced. A 150 GB disk accommodates these model files, Python packages, and working space, but continues to cost money while retained. GCP may not be able to allocate the machine immediately.

### 1. Create a five-hour Flex-start VM

Set your own values locally; this record intentionally omits the original project identifier.

```bash
PROJECT_ID="your-project-id"
ZONE="us-central1-b"
VM_NAME="gpu-workstation"

gcloud compute instances create "$VM_NAME" \
  --project="$PROJECT_ID" --zone="$ZONE" \
  --machine-type=g4-standard-48 \
  --provisioning-model=FLEX_START \
  --max-run-duration=5h --request-valid-for-duration=600s \
  --instance-termination-action=STOP --maintenance-policy=TERMINATE \
  --image-family=ubuntu-2404-lts-amd64 --image-project=ubuntu-os-cloud \
  --boot-disk-size=150GB --boot-disk-type=hyperdisk-balanced \
  --no-service-account --no-scopes
```

Wait for `RUNNING`, then verify the provisioning model, max duration, and stop action:

```bash
gcloud compute instances describe "$VM_NAME" --project="$PROJECT_ID" --zone="$ZONE" \
  --format='json(status,scheduling.provisioningModel,scheduling.maxRunDuration,scheduling.instanceTerminationAction)'
gcloud compute ssh "$VM_NAME" --project="$PROJECT_ID" --zone="$ZONE"
```

### 2. Install the driver and Python environment on the VM

The observed run used the Ubuntu package below. Kernel and package availability can change; verify the NVIDIA module builds for the VM's running kernel.

```bash
sudo apt-get update
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y \
  "linux-headers-$(uname -r)" nvidia-driver-580-open \
  python3-venv python3-pip git tmux curl zstd
sudo modprobe nvidia
sudo modprobe nvidia_uvm
nvidia-smi

python3 -m venv ~/comfyui-venv
~/comfyui-venv/bin/pip install --upgrade pip
~/comfyui-venv/bin/pip install torch torchvision torchaudio \
  --index-url https://download.pytorch.org/whl/cu128
```

The unpinned installation commands above can install newer releases than those tested. For strict reproduction, first identify compatible revisions and capture a complete lockfile; exact revisions for this run are unavailable.

### 3. Install ComfyUI, the GGUF loader, and workflow dependency

```bash
git clone https://github.com/Comfy-Org/ComfyUI.git ~/ComfyUI
git clone https://github.com/leejet/ComfyUI-GGUF.git ~/ComfyUI/custom_nodes/ComfyUI-GGUF
git clone https://github.com/rgthree/rgthree-comfy.git ~/ComfyUI/custom_nodes/rgthree-comfy
~/comfyui-venv/bin/pip install -r ~/ComfyUI/requirements.txt \
  -r ~/ComfyUI/custom_nodes/ComfyUI-GGUF/requirements.txt
~/comfyui-venv/bin/pip check
```

The selected workflow uses the rgthree LoRA node even with its LoRA slots disabled. Record `git rev-parse HEAD` in each cloned repository and `pip freeze` for future runs.

### 4. Download only the necessary model files

```bash
cd ~/ComfyUI
mkdir -p models/diffusion_models models/text_encoders models/vae user/default/workflows
curl -fL --retry 3 -o models/diffusion_models/qwen-image-2.1-Q8_0.gguf \
  https://huggingface.co/0xSojalSec/Qwen-Image-2.1-Uncensored-GGUF/resolve/main/qwen-image-2.1-Q8_0.gguf
curl -fL --retry 3 -o models/text_encoders/qwen3vl_8b_int8_convrot.safetensors \
  https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3vl_8b_int8_convrot.safetensors
curl -fL --retry 3 -o models/vae/qwen_image_2.1_vae_bf16.safetensors \
  https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/vae/qwen_image_2.1_vae_bf16.safetensors
sha256sum models/diffusion_models/qwen-image-2.1-Q8_0.gguf
```

Compare the digest with the [publisher's SHA256SUMS](https://huggingface.co/0xSojalSec/Qwen-Image-2.1-Uncensored-GGUF/blob/main/SHA256SUMS). The source manifest lists an absolute build path, so do not blindly run it with `sha256sum -c`. Verify the other downloads against their current publisher metadata as well. Pin model revisions and hashes for future experiments.

### 5. Install the example workflow and start ComfyUI

```bash
curl -fL -o ~/ComfyUI/user/default/workflows/Qwen-Image-2.1.json \
  https://raw.githubusercontent.com/foprc/qwen-image-2.1-comfyui-workflow/master/workflows/qwen_image_2_1_Q8_0_t2i.json
cd ~/ComfyUI
~/comfyui-venv/bin/python main.py --listen 127.0.0.1 --port 8188
```

Leave this process running. The original deployment used a systemd service to start it automatically; the foreground command above is simpler and has not been retested after cleanup. The [workflow source](https://github.com/foprc/qwen-image-2.1-comfyui-workflow) includes an optional reference-image branch and LoRA slots. Its supplied `LoadImage` node may refer to an example image that is not bundled: select an existing image or supply a harmless placeholder if validation requires one. Treat any reference images as private.

On the local computer, in a second terminal, establish a loopback-only tunnel:

```bash
gcloud compute ssh "$VM_NAME" --project="$PROJECT_ID" --zone="$ZONE" \
  -- -N -L 127.0.0.1:8188:127.0.0.1:8188
```

Open `http://127.0.0.1:8188/`, open the saved `Qwen-Image-2.1` workflow, confirm its GGUF/text encoder/VAE selections, and select **Run**. SSH connectivity must be configured for the project; the tunnel is not a public sharing URL.

### 6. Smoke test and prompts

The independently submitted API smoke test used this prompt:

> A red apple on a wooden table, daylight photograph

Parameters: 512 × 512, seed `42`, 12 steps, CFG `1.0`, Euler, simple scheduler, denoise `1.0`. ComfyUI returned `execution_success` and a 512 × 512 PNG after approximately 25.7 seconds including initial model loading. The image was not retained; this is an execution check, not an assessment of visual fidelity.

The prepared interactive workflow started at 25 steps, CFG 1.0, Euler/simple, 1 MP, with the reference switch OFF. The following prompts were suggested during the session; they were not all independently validated as successful outputs:

**General English prompt:**

> A cozy Japanese street at night, warm lanterns, light rain, cinematic photography

**Indonesian language comparison:**

> Foto artistik seorang perempuan dewasa berusia sekitar 30 tahun, duduk di kursi kayu di studio, pencahayaan lembut dari jendela, latar belakang netral, komposisi seluruh tubuh, gaya fotografi klasik.

**Adult artistic-nudity probe:**

> Fine art studio photograph of an adult woman, approximately 30 years old, posing nude on a simple chair, full-body composition, soft window light, neutral background, natural anatomy, tasteful and non-explicit, classical portrait photography.

**Fantasy variation:**

> A fierce adult warrior queen, around 30 years old, standing nude on a volcanic cliff beneath a blood-red moon, her long hair whipping in the wind, surrounded by fire, smoke, and ancient ruins. Powerful, confident pose, dramatic cinematic lighting, intricate body paint, photorealistic, full-body composition, dark fantasy atmosphere.

**Reference-image edit request suggested in conversation:**

> Edit the provided image of the adult subject, who is over 18. Remove the clothing and depict the subject nude while preserving their facial features, hairstyle, body proportions, pose, lighting, camera angle, and background. Maintain a natural, photorealistic appearance.

This is a prompt record, not evidence of a successful edit. Use only images for which the subject has granted permission. A reference image was selected in a later run, but the switch was OFF, so that run did not use it. Turning it ON was suggested, but a successful reference edit was not verified.

## Troubleshooting and quality experiments

- **KSampler validation failed:** both `positive` and `negative` inputs were missing. Reconnect them to the corresponding outputs of `TextEncodeQwenImage21`; the negative connection is still required even if the negative prompt has no effect at CFG 1.
- **Reference ignored:** the last examined run had `LoadImage` configured but its Boolean switch was `false`. Switch it to `true` to route the image reference. Reference editing and identity preservation require their own validation.
- **Stuck on splash screen:** the server and GPU responded while the browser showed the splash. A reload and additional time eventually exposed the workflow. No root cause was established; avoid claiming the reload alone fixed it. Preserve unsaved edits before reloading.
- **Resolution:** the examined edit workflow used `resolution=1024`, which resizes references to approximately a one-megapixel budget. Try `1536`, then `2048`; `0` uses source dimensions rounded to multiples of 32, which can be costly. Higher resolution does not guarantee fidelity. The official [Qwen Image 2.1 workflow documentation](https://docs.comfy.org/tutorials/image/qwen/qwen-image-2-1) explains the resolution and sampler trade-offs. These changes were suggested but not tested in this session.
- **Sampling:** keep CFG 1.0, Euler/simple, and 25 steps as a baseline; compare 30 or 40 steps at a fixed seed if fine details are poor. The official guide notes that negative prompts do not affect the CFG 1 path. This comparison was not run.
- A text-to-image model produces still images. Image-to-video requires a separate model, weights, and workflow; none was installed or tested here.

## Stop and clean up

Close the SSH tunnel with Ctrl+C. Stop the VM when finished rather than waiting for the five-hour limit:

```bash
gcloud compute instances stop "$VM_NAME" --project="$PROJECT_ID" --zone="$ZONE"
gcloud compute instances describe "$VM_NAME" --project="$PROJECT_ID" --zone="$ZONE" --format='value(status)'
```

`TERMINATED` means the VM is stopped, but its retained disk still incurs storage charges. Deleting the VM and boot disk permanently removes the installed models, workflow, and outputs: back up anything needed and explicitly choose disk retention or deletion. The original session's VM and disk were deleted only after the user requested total cleanup; other research resources were left untouched.
