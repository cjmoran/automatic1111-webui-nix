# [AUTOMATIC1111/stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) for CUDA and ROCm on NixOS

This is literally just a `shell.nix`/`flake.nix` for stable-diffusion-webui that also enables CUDA/ROCm on NixOS.
This supports NVIDIA GPUs (using CUDA), AMD GPUs (using ROCm), and CPU compute (including Apple silicon).

## Usage

### Setup

First, run:
```bash
git clone https://github.com/virchau13/automatic1111-webui-nix
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui
cp automatic1111-webui-nix/*.nix stable-diffusion-webui/
cp automatic1111-webui-nix/.envrc stable-diffusion-webui/
cd stable-diffusion-webui
git add *.nix .envrc
```

After that, choose the command according to your platform:

#### Flakes

```bash
nix develop .#cpu # CPU or Apple silicon
nix develop .#cuda # CUDA
nix develop .#rocm # ROCm
```

#### Non-flakes

```bash
nix-shell --argstr variant CPU # CPU
nix-shell --argstr variant CUDA # CUDA
nix-shell --argstr variant ROCM # ROCm
```

### Launching webui

```bash
cd automatic1111-webui-nix
./webui.sh --xformers
# follow the tutorials at the original project for setting up Stable Diffusion / GFPGAN / whatever
```

You might want to switch to high performance mode on battery-powered devices.

#### Troubleshooting

Run ./webui.sh --xformers to set up xformers. If this command fails, try git pulling from the source repository, then deleting the venv (rm -r venv) and reinstalling everything from scratch again (sometimes the venv will keep old packages around forever even if they should be updated.)

Maybe need to reinstall xformers with its own CUDA kernel via:

```
source venv/bin/activate
pip3 install -U xformers --index-url https://download.pytorch.org/whl/cu121
```

Another method (didn't work but maybe did it wrong) from https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Xformers#building-xformers-on-linux-from-anonymous-user

    1. go to the webui directory
    2. source ./venv/bin/activate
    3. cd repositories
    4. git clone https://github.com/facebookresearch/xformers.git
    5. cd xformers
    6. git submodule update --init --recursive
    7. pip install -r requirements.txt
    8. pip install -e .


## Is this completely pure?

This is just a Nix shell for bootstrapping the web UI, not an actual pure flake; the `./webui.sh` will still install
a bunch of Python packages (into a venv, so not polluting your system) when you run it.

## Troubleshooting

### xformers
Run `./webui.sh --xformers` to set up xformers. If this command fails, try git pulling from the source repository, then deleting the venv (`rm -r venv`) and reinstalling everything from scratch again (sometimes the venv will keep old packages around forever even if they should be updated.)

## Credits
- AUTOMATIC1111 for obvious reasons.
- rprospero for [ROCm support](https://github.com/virchau13/automatic1111-webui-nix/pull/3).
- Cloudef for [CPU compute / Apple Silicon support](https://github.com/virchau13/automatic1111-webui-nix/pull/10).
- polypoyo for [the original draft of this](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/4736).
