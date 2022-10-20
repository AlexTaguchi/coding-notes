# coding-notes
Tips and tricks for setting up and interacting with my coding environment

---
### Shell scripting
#### Permanently creating an alias or adding to path
- Bash (`~/.bashrc` on Ubuntu, `~/.bash_profile` on Mac)
  ```bash
  alias <variable>="<expression>"
  export PATH="<path>:$PATH"
  ```
- Zsh (`~/.zshrc` on Ubuntu, `~/.zprofile` on Mac)
  ```zsh
  alias <variable>="<expression>"
  export PATH="<path>:$PATH"
  ```
- Fish (`~/.config/fish/config.fish`)
  ```fish
  alias <variable>="<expression>"
  set -x PATH $PATH <path>
  ```

#### Setting a variable
- Bash/Zsh: `<variable>=<number/string>; echo "The value is $<variable>"`
- Fish: `set <variable> <number/string>; echo "The value is $<variable>"`

#### Array operations
- Bash/Zsh:
  ```bash
  <array>=(<element1> <element2> <element3>)
  # First: ${<array>[0]} (0-indexed)
  # Last: ${<array>[-1]}
  # Slice: ${<array>[@]:<s>:<n>} (<s> = starting index, <n> = number of elements)
  # Length: ${#<array>[@]}
  # Append: <array>+=(<element4>)
  # Overwrite: <array>[0]=<element4>
  ```
- Fish:
  ```fish
  set <array> <element1> <element2> <element3>
  # First: $<array>[1] (1-indexed)
  # Last: $<array>[-1]
  # Slice: $<array>[<s>..<e>] (<s> = starting index, <e> = ending index)
  # Length: count $<array>
  # Append: set -a <array> <element4> (append), set -p <array> <element4> (prepend)
  # Overwrite: set <array>[1] <element4>
  ```

#### For loops
- Bash/Zsh:
  ```bash
  for <variable> in <iterable>
  do
    <expression>
  done
  # Elements: <iterable> = <element1> <element2> <element3>
  # Array: <iterable> = "${<array>[@]}"
  # Range: <iterable> = {<start>..<end>..<increment>}
  # Command: <iterable> = $(<command>)
  # Glob: <iterable> = glob/path/*.ext
  ```
- Fish:
  ```fish
  for <variable> in <iterable>
    <expression>
  end
  # Elements: <iterable> = <element1> <element2> <element3>
  # Array: <iterable> = $<array>
  # Range: <iterable> = (seq <start> <end> <increment>)
  # Command: <iterable> = (<command>)
  # Glob: <iterable> = glob/path/*.ext
  ```

#### Conditionals
- Bash/Zsh:
  ```bash
  if [[<condition>]]
  then
    <expression>
  elif [[<condition>]]
    <expression>
  then
  else
    <expression>
  fi
  # Syntax for <condition>:
  # - Variable: -n/-z <variable> (length > 0, empty)
  # - Numbers: <number1> -eq/-ne/-gt/-lt/-ge/-le <number2> (==, !=, >, <, ≥, ≤)
  # - Strings: <string1> =/!= <string2> (equal, not equal)
  # - File: -r/-w/-x/-d/-f/-e <file> (readable, writeable, executable, directory, file, exists)
  # NOTE: "[[<condition>]]" construct can be replaced with a command for testing exit status
  ```
- Fish:
  ```fish
  if test <condition>
    <expression>
  else if test <condition>
    <expression>
  else
    <expression>
  end
  # Syntax for <condition>:
  # - Variable: -n/-z <variable> (length > 0, empty)
  # - Numbers: <number1> -eq/-ne/-gt/-lt/-ge/-le <number2> (==, !=, >, <, ≥, ≤)
  # - Strings: <string1> =/!= <string2> (equal, not equal)
  # - File: -r/-w/-x/-d/-f/-e <file> (readable, writeable, executable, directory, file, exists)
  # NOTE: "test <condition>" construct can be replaced with a command for testing exit status
  ```

#### Pause and resume terminal job
- Pause job: `ctrl-z`
- Resume job: `fg` (or `fg %<job>` using the output of `jobs`)

#### Zip directory recursively and unzip into new directory
```bash
zip -r <file.zip> path/to/directory/to/zip
unzip <file.zip> -d path/to/new/directory
```

#### Recursively grant full permissions for directory
```bash
chmod -R 777 path/to/directory/
```

#### Recursively overwrite contents of destination folder with source folder
```bash
rsync -rltDhv --delete --progress path/to/source/folder/ path/to/destination/folder/
# -r: recurse into directories
# -l: copy symlinks as symlinks
# -t: preserve modification times
# -D: preserve device files (super-user only) and special files
# -h: output numbers in a human-readable format
# -v: increase verbosity
# --delete: delete extraneous files from destination directories
# --progress: show progress during transfer
```

#### Recursively rename files by regex pattern
```bash
find . -name "*<string1>*" | rename "s/<string1>/<string2>/"
```

#### Increase number of files that can be opened at once
```bash
ulimit -n <number>
```

---
### Fish
#### Mac Install
(https://medium.com/tuannguyendotme/set-up-the-fish-shell-on-mac-step-by-step-6a77bcb2687c)
1) Homebrew
    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2) Fish
    ```bash
    brew install fish
    echo "/usr/local/bin/fish" | sudo tee -a /etc/shells
    ```
3) Change to Fish shell
    ```bash
    chsh -s `which fish`
    ```
4) Launch new terminal
5) Bob the fish
    ```fish
    curl -L https://get.oh-my.fish | fish
    omf install bobthefish
    ```
6) Nerd fonts
    ```fish
    brew tap homebrew/cask-fonts
    brew cask install font-firacode-nerd-font
    set -U theme_nerd_fonts yes
    ```
7) Config file
    ```fish
    echo > ~/.config/fish/config.fish "\
    # Anaconda
    source path/to/anaconda3/etc/fish/conf.d/conda.fish
    conda activate

    # Fish
    set -g theme_date_format '+%Y/%m/%d %H:%M:%S'
    set -g theme_newline_cursor yes
    set -g theme_newline_prompt '\$ '

    # Git
    set -g theme_display_git_untracked no
    set -g theme_display_git_master_branch yes"
    ```
8) Finally, change the default Terminal font to see the powerline symbols: `Terminal -> Preferences -> Font -> Change -> {Collection: All Fonts, Family: FuraCode Nerd Font}`

#### Mac Uninstall
1) Change to Bash shell
    ```fish
    chsh -s `which bash`
    ```
2) Uninstall with Homebrew
    ```bash
    brew uninstall fish
    ```


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
Run the desired process. Detach the process with `ctrl-a` then `d`. Now you can do whatever you want, including ending the ssh session. After sshing back in, the following command brings the personal screen back up
```bash
screen -dR
```
See the previous output you missing while away with `ctrl-a` then `esc`. Kill the screen with `exit` or `ctrl-d`.


---
### VI
#### Find and replace one at a time
```
:%s/<string-to-find>/<replacement-string>/gc
```

#### Find and replace all at once
```
:%s/<string-to-find>/<replacement-string>/g
```

---
### VS Code
#### Extensions
Click the Extensions icon (looks like a box) on the left sidebar and install the following:
- `Python` by Microsoft: Linting, debugging, IntelliSense, code navigation, code formatting, refactoring, unit tests, snippets, and more
- `Code Runner` by Jun Han: Execute statements from a variety of languages and output the results to the built-in Output Window
- `autoDocstring` by Nils Werner: Quickly generate docstring snippets that can be tabbed through
- `Remote Development` by Microsoft: Edit and interact with files and folders on a remote machine

*References:*
- https://medium.com/issuehunt/10-visual-studio-code-extensions-for-python-development-de0be51bbeed
- https://code.visualstudio.com/docs/remote/ssh

#### Customize User Settings
Click the Gear (lower-left corner) -> Settings -> User tab (not Workspace) -> Open Settings (upper-right corner), and add the following code:
```
{
    // Code Runner
    "code-runner.clearPreviousOutput": true,
    "code-runner.executorMap": {"python": "/opt/anaconda3/bin/python"},
    "code-runner.fileDirectoryAsCwd": true,
    "code-runner.saveFileBeforeRun": true,

    // Fish
    "terminal.integrated.fontFamily": "FuraCode Nerd Font",
    "terminal.integrated.fontSize": 14,
    "terminal.integrated.shell.osx": "/usr/local/bin/fish",

    // Git
    "git.autofetch": true,

    // Python
    "python.pythonPath": "/opt/anaconda3/bin/python",
    "python.terminal.executeInFileDir": true,
    "python.terminal.launchArgs": ["-i"],
    "terminal.integrated.inheritEnv": false,

    // VS Code
    "files.autoSave": "off",
    "diffEditor.ignoreTrimWhitespace": false,
}
```

#### Running Code
- Standard run: `Play Button (▷)` or `Ctrl+Option+N` - Uses `Code Runner` to execute script in Output Window. No code interactivity after run completes. Can be canceled during the run with `Ctrl+Option+M`
- Interactive run: `Ctrl+Enter` - Runs file in Terminal Window (must bind `Ctrl+Enter` to `Run Python File in Terminal` in Code -> Preferences -> Keyboard Shortcuts)

#### Editing Shortcuts
- Column (box) selection: `Shift+Option(⌥)`
- Automatic multiple selections: Highlight word of interest, and use `⌘D` to select subsequent occurances
- Keyboard Shortcuts: https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference
- Customize Keyboard Shortcuts: https://code.visualstudio.com/docs/getstarted/keybindings#_customizing-shortcuts
  - Code -> Preferences -> Keyboard Shortcuts
  - Search for command
  - Change/add keybinding by clicking the pencil symbol

---
### Git
#### Markdown symbols in README.md
- &deg; `&deg;`
- &plusmn; `&plusmn;`

#### Undo unadded changes
```bash
git checkout .
```

#### Undo last add
```bash
git reset
```

#### Undo last commit
```bash
git reset --soft HEAD~1
```

#### Overwrite local repository with remote master
```bash
git fetch --all
git reset --hard origin/master
```


---
### NVIDIA Titan RTX GPU on Ubuntu 18
(Reference: https://gist.github.com/alexlee-gk/76a409f62a53883971a18a11af93241b)
- Uninstall old NVIDIA drivers
  ```bash
  sudo nvidia-uninstall
  sudo apt purge nvidia*
  sudo apt autoremove
  ```
- Download NVIDIA driver installation runfile (https://www.nvidia.com/Download/index.aspx) and run with --no-opengl-files:
  ```bash
  chmod +x NVIDIA-Linux-x86_64-430.09.run
  sudo ./NVIDIA-Linux-x86_64-430.09.run --no-opengl-files
  ```
- Download NVIDIA CUDA "runfile (local)" (https://developer.nvidia.com/cuda-downloads) and run script (respond "no" when asked "Install NVIDIA accelerated Graphics Driver...?"):
  ```bash
  chmod +x cuda_10.1.105_418.39_linux.run
  sudo ./cuda_10.1.105_418.39_linux.run
  ```
- Modify or create `/etc/X11/xorg.conf` to avoid using NVIDIA GPU for primary screen (BusID outputs of `lspci | egrep 'VGA|3D'` must match):
  ```bash
  Section "ServerLayout"
      Identifier "layout"
      Screen 0 "intel"
      Screen 1 "nvidia"
  EndSection
  
  Section "Device"
      Identifier "intel"
      Driver "intel"
      BusID "PCI:0@0:2:0"
      Option "AccelMethod" "SNA"
  EndSection
  
  Section "Screen"
      Identifier "intel"
      Device "intel"
  EndSection
  
  Section "Device"
      Identifier "nvidia"
      Driver "nvidia"
      BusID "PCI:1@0:0:0"
      Option "ConstrainCursor" "off"
  EndSection
  
  Section "Screen"
      Identifier "nvidia"
      Device "nvidia"
      Option "AllowEmptyInitialConfiguration" "on"
      Option "IgnoreDisplayDevices" "CRT"
  EndSection
  ```
- Reboot
