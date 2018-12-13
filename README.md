# bash-notes
Tips and tricks for using Bash (Unix shell command language)

### Adding a folder to your path in ~/.bashrc
```bash
export PATH="<path>:$PATH"
```

### Adding an alias to ~/.bashrc for commonly typed expressions
```bash
alias <variable>="<command>"
```

### Creating a temporary environment to run a command that requires libraries you don't want to install on your system
```bash
env LD_LIBRARY_PATH="~/.local/lib/" <command>
```

### Wrong ELF class usually means you're using the wrong bit version (32 vs 64)
```bash
<library>.so: wrong ELF class: ELFCLASS32
```

### Set up NVIDIA GTX 1080 Ti GPU on Ubuntu
- Remove old NVIDIA installation, and find and remove dependencies of deleted files
```bash
sudo apt purge nvidia*
sudo apt autoremove
```
- Download Nvidia driver installation runfile (https://www.nvidia.com/Download/index.aspx) and run script with the option --no-opengl-files:
```bash
chmod a+x NVIDIA-Linux-x86_64-410.78.run
sudo ./NVIDIA-Linux-x86_64-410.78.run --no-opengl-files
```
