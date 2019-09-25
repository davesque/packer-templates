# windows: a Vagrant box for Windows guests

# FEATURES

* Supports `vagrant ssh [--no-tty -c <command>]`
* Supports `vagrant rsync`
* Supports file and shell Vagrant provisioners
* Includes [Chocolatey](https://chocolatey.org/) package manager

Note that Vagrant support for Windows guests exhibits quirks of varying disquiet. Suffice to say:

* `vagrant ssh` launches an interactive Command Prompt cmd session (Use `dir` here).
* `vagrant ssh --no-tty -c "<command>"` executes a bash command (Use `ls` here).
* `vagrant ssh --no-tty -c "powershell -Command \"<command>\""` executes a PowerShell command.
* `vagrant ssh --no-tty -c "sh <cygwin-path>.sh` executes a bash script.
* `vagrant ssh --no-tty -c "powershell -NoLogo -ExecutionPolicy RemoteSigned -File <cygwin-path>.ps1` executes a PowerShell script.
* `vagrant ssh --no-tty -c "powershell -NoLogo -ExecutionPolicy RemoteSigned -File <cygwin-path>-shim.ps1"` can invoke an MS-DOS `.bat`/`.cmd` script using a PowerShell wrapper.
* `vagrant rsync` uses `/cygdrive/c/...` syntax for cygwin paths
* `vagrant ssh --no-tty -c` and bash.exe use `/c/...` syntax for cygwin paths
* `vagrant ssh --no-tty -c "echo"`... often fails to recognize either cygwin or Windows paths; Use `vagrant ssh --no-tty -c "powershell -Command \"echo`...`\""` instead.
* Ampersands aggregating multiple commands must be escaped according to MSDOS batch syntax: `vagrant ssh --no-tty -c '... ^&^& ...'`
* Powershell's echo inserts Unicode BOMs into text, which corrupts grep and other common POSIX tools. Such files should be searched with [ripgrep](https://github.com/BurntSushi/ripgrep) instead.
