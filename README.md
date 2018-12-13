# linux-notes
Tips and tricks for a deep learning Linux setup

---
### Basics
#### Adding an alias and exporting to path in ~/.bashrc ("exec bash" to implement changes)
```bash
alias <variable>="<command>"
export PATH="<path>:$PATH"
```

#### Creating a temporary environment to run a command that requires libraries you don't want to install on your system
```bash
env LD_LIBRARY_PATH="~/.local/lib/" <command>
```

#### Wrong ELF class usually means you're using the wrong bit version (32 vs 64)
```bash
<library>.so: wrong ELF class: ELFCLASS32
```

---
### Setting up NVIDIA GTX 1080 Ti GPU on Ubuntu
(Reference: https://gist.github.com/alexlee-gk/76a409f62a53883971a18a11af93241b)
- Remove old NVIDIA installation and its dependencies
```bash
sudo apt purge nvidia*
sudo apt autoremove
```
- Download NVIDIA driver installation runfile (https://www.nvidia.com/Download/index.aspx) and run script with the option --no-opengl-files (ignore preinstallation error and answer "no" to other options):
```bash
chmod +x NVIDIA-Linux-x86_64-410.78.run
sudo ./NVIDIA-Linux-x86_64-410.78.run --no-opengl-files
```
- If the installation fails due to an X server error, stop the display manager and rerun the script:
```bash
sudo service lightdm stop
<Ctrl+Alt+F1>
sudo ./NVIDIA-Linux-x86_64-410.78.run --no-opengl-files
sudo service lightdm start
```
- Reboot
- Download NVIDIA CUDA "runfile (local)" (https://developer.nvidia.com/cuda-downloads) and run script (respond "no" when asked "Install NVIDIA accelerated Graphics Driver...?")
```bash
chmod +x cuda_10.0.130_410.48_linux.run
sudo ./cuda_10.0.130_410.48_linux.run 
```
- Configure xorg.conf (Modify or create the file /etc/X11/xorg.conf to specify that the NVIDIA GPU should NOT be used for the primary screen)
```bash
Section "ServerLayout"
    Identifier "layout"
    Screen 0 "intel"
```
- Reboot
- Download NVIDIA cuDNN tar file (https://developer.nvidia.com/cudnn), extract, and copy to CUDA directory
```bash
tar -xzvf cudnn-10.0-linux-x64-v7.4.1.5.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
- Done!
