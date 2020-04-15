
Booste runs through the WSL terminal as it would on Linux. 

The filesync directory, created at ~/Booste is not visible from Windows-based IDEs and code editors.

This can be worked around with [symlinks](https://en.wikipedia.org/wiki/Symbolic_link).

Open WSL and navigate to the files visible to Windows:
```bash
cd mnt/c/Users/{your-user-name}/
```

Place your own filesync directory wherever you please within the Windows system:
```bash
cd {wherever}
mkdir Booste
```

Install Booste, if you haven't already:
```bash
pip3 install booste-cli
```

Log in for the first time. This creates the filesync directory in the Linux system.
```bash
booste login
```

Navigate into it to confirm.
```bash
cd ~/Booste
```

To make files in the filesync directory editable by Windows-based GUI code editors, we'll replace the default filesync directory with a symlink to the directory we created before.
```bash
rm -rf ~/Booste
ln -s /mnt/c/Users/{your-user-name}/{wherever}/Booste/ ~/Booste
```

You've now pointed the address ~/Booste toward your custom filesync location.