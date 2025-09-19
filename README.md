# ROCm Installation with AMD RX 6000 Series GPUs on Arch Linux and Pytorch (Definitive Guide)

Installing ROCM on unsupported RX 6000 series GPUs on Arch Linux and testing using pytorch using the official pip package. This is the easiest and fastest guide on the process, and doesn't involve docker containers, instead, it integrates into a typical python developer workflow with virtual environments and pip. It is not necessary to develop inside a docker container regardless of what AMD recommends.

The tutorial assumes you already have python3 installed.

## Install ROCM

```
yay -S rocm-hip-sdk rocm-opencl-sdk
```

## Configure ROCM

Add user to user groups

```
sudo gpasswd -a username render
sudo gpasswd -a username video
```

Configure ROCM shared objects.

```
sudo tee --append /etc/ld.so.conf.d/rocm.conf <<EOF
/opt/rocm/lib
/opt/rocm/lib64
EOF
sudo ldconfig
```

Edit environmental variables in your shell (bashrc or zshrc depending on which one you're using). 

```
sudo nano ~/.bashrc
```

Add the following two lines at the end of bashrc. This is specific to all RX 6000 series GPUs.

```
export PATH=$PATH:/opt/bin
export HSA_OVERRIDE_GFX_VERSION=10.3.0
```

## Validate ROCM Installation

```
rocm-smi
```
You can monitor the status of your GPU with the following command. This is especially useful when ascertaining whether your GPU is utilised during ML training or AI applications.

```
watch rocm-smi
```

## Use ROCM with Pytorch

Open your IDE of choice and create a new virtual environment (venv) based on the latest global python version.

Visit https://pytorch.org/. In the installation selector select stable, linux, pip, and rocm. Copy the installation command it outputs.

Next, open the bash terminal of the venv, and run the output of the website. It should look something like this, but the version will be different as ROCM is developed:

```
pip3 install torch torchvision --index-url https://download.pytorch.org/whl/rocm6.4
```

Congratulations! You now have ROCM up and running and working with pytorch. All torch commands will now work just as if it were an Nvidia card.


