# linux-notes
Tips and tricks for a deep learning Linux setup

---
### Basics
#### Adding an alias and exporting to path in ~/.bash_profile ("exec bash" to implement changes)
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

#### Install python package from source
```bash
pip install -e /path/to/package
```
(The package should contain a setup.py file)

---
### Remoting
#### Generate public key
`ssh-keygen` (push `Enter` a few times) generates a random public key at `~/.ssh/id_rsa.pub`

#### Mount remote drive
- Install FUSE and SSHFS on Mac (https://osxfuse.github.io/)
- Create alias in `~/.bash_profile`: `alias <mount_alias>="sshfs -o rw,allow_other <username>@<ip_address>:/path/to/remote/drive /path/to/local/mount"`

#### Configuration file
Create or modify an ssh configuration file at `~/.ssh/config`. Add a host using the following template:
```bash
Host <alias>
HostName <ip_address>
User <username>
Port <number>
IdentityFile /path/to/private/key
```
(`Port` and `IdentityFile` are optional)

#### Run background process on remote computer
After sshing in, create a personal screen only accessible by the user
```bash
screen -dR
```
Run the desired process. Detach the process with `ctrt-a` then `d`. Now you can do whatever you want, including ending the ssh session. After sshing back in, the following command brings the personal screen back up
```bash
screen -dR
```
See the previous output you missing while away with `ctrl-a` then `esc`. Kill the screen with `exit` or `ctrl-d`.

---
### VS Code
#### Basic Editing
- Keyboard Shortcuts: https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference
- Customize Keyboard Shortcuts: https://code.visualstudio.com/docs/getstarted/keybindings#_customizing-shortcuts
  - Code -> Preferences -> Keyboard Shortcuts
  - Search for command
  - Change/add keybinding by clicking the pencil symbol
- Manual multiple selections: Create multiple cursors to edit different sections simultaneously with `Alt-Click`
- Automatic multiple selections: Highlight word of interest, and use `⌘D` to select subsequent occurances
- Column (box) selection: `Shift+Option(⌥)`

#### Running Code
- Standard run: `Play Button (▷)` or `Ctrl+Option+N` - Uses `Code Runner` to execute script in Output Window. No code interactivity after run completes. Can be canceled during the run with `Ctrl+Option+N`
- Interactive run all: `Ctrl+Enter` - Runs file in Python Interactive Window (must bind `Ctrl+Enter` to `Run Current File in Python Interactive Window` in Code -> Preferences -> Keyboard Shortcuts)
- Interactive run selected: `Shift+Enter` - Runs highlighted selection or line in  Python Interactive Window

#### Extensions
(Reference: https://medium.com/issuehunt/10-visual-studio-code-extensions-for-python-development-de0be51bbeed)
- `Python` by Microsoft: Linting, debugging, IntelliSense, code navigation, code formatting, refactoring, unit tests, snippets, and more
  - Enable `Data Science: Send Selection To Interactive Window` setting
- `autoDocstring` by Nils Werner: Quickly generate docstring snippets that can be tabbed through
- `Code Runner` by Jun Han: Execute statements from a variety of languages and output the results to the built-in Output Window

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
