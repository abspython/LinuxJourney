# A Debian Journey 

> "Only wimps use tape backup: _real_ men just upload their important stuff on ftp, and let the rest of the world mirror it."  --- Linus Torvalds



## How to install non-free firmware

1. Open file at ```etc/apt/sources.list```

2. Paste the repository source, which in my case is,

     ``deb http://ftp.de.debian.org/debian stretch main non-free``

     Change 'ftp.de.debain.org/debian' to required mirror

3. Do ```sudo apt-get install firmware-iwlwifi``` and ``sudo apt-get install firmware-realtek``

4. Restart the computer.



## I broke my SUDO!

1. Gain superuser access
2. Run ```visudo```
3. Append the file with
    ``username	ALL=(ALL) ALL``
4. Close the terminal and you are get to go.



## Installing a .deb file

You can install it using  ```dpkg```

```shell
sudo dpkg -i /path/to/deb/file
```

 followed by 

```shell
sudo apt-get install -f
```



You can install it using ```apt```

```shell
sudo apt install ./name.deb
```

or

```shell
sudo apt install /path/to/package/name.deb
```

##### Install ```gdebi``` and open your .deb file using it (Right-click -> Open with). It will install your .deb package with all its dependencies.
Why to use ```sudo apt-get install -f``` after ```sudo dpkg -i /path/to/deb/file``` (mentioned in first method).

```shell
 -f, --fix-broken
```

This attempt to correct a system with broken dependencies in place.
When dpkg install a package and package dependency is not satisfied, it leaves the package in unconfigured state and that package is considered as broken.

```shell
sudo apt-get install -f 
```

##### command tries to fix this broken package by installing the missing dependency.



## Key/PPA Error

Install ```dirmngr ``` using 

```shell
sudo apt-get install dirmngr
```



## Switching to another DE

- Installed ```lightdm``` and ```sudo dpkg--reconfigure lightdm```.
  - Not working. Login as lightdm but after that gdm3 starts action.
  - Removed lightdm and later fixed the issue
  - Now using lightdm and i3wm
- Trying KDE
  - KDE sucks my RAM. Also very bulky and buggy.
  - Not recommended as a DE.
- Using ``i3-wm`` with extra plugins.
  - Fell in :heart:. 
  - Never going to change it.



## Disabling Bluetooth at Startup

```shell
# Disable at startup
sudo systemctl disable bluetooth.service
# Check status at next restart
sudo systemctl status bluetooth.service
# Renable at startup
sudo systemctl enable bluetooth.service
```



## Kill your Boot Time!

**Tricks**

1. ```systemd-analyze``` to instrument the boot.
2. ```systemctl```  to exclude processes from boot.
3. ```bootchart```  is still useful for drawing pretty graphs
4. Don’t enable services you don’t need.

Links : [4-second boot](https://mike42.me/blog/how-to-boot-debian-in-4-seconds) / [Systemd Optimisation](https://freedesktop.org/wiki/Software/systemd/Optimizations/)

**What did I do?**

- Reduce boot loader delay.

  ```shell
  sudo nano /etc/default/grub
  ```

  Set GRUB_TIMEOUT = 0

  ```shell
  sudo update-grub2
  ```

- Looking at systemd.

  - Analyse the boot timing.

    ```shell
    systemd-analyze plot > plot.svg
    # Gives a Damn Beautiful Graph
    ```

  - For stopping  ```NetworkManager-wait-online``` service

    ```shell
    $ sudo systemctl disable NetworkManager-wait-online.service
    Removed /etc/systemd/system/network-online.target.wants/NetworkManager-wait-online.service.
    ```

  - Change Time syncing.

    ```shell
    sudo systemctl disable systemd-timesyncd.service
    sudo apt-get install chrony
    sudo systemctl enable chrony
    ```


## Git-ing the SSH

First create a rsa key using ``ssh-keygen`` and add it to the github account
Then, you are all set!!

For the already existing repository using HTTP protocol use the below one liner
Only for **github** users!!

```bash
git remote set-url origin $(git remote show origin | grep "Fetch URL" | sed 's/ *Fetch URL: //' | sed 's/https:\/\/github.com\//git@github.com:/')
```



## $PATH

This is bash script to check the given directory is in your PATH. In this case, we are checking ```~/bin``` is a PATH variable.

```bash
[[ ":$PATH:" == *":$HOME/bin:"* || ":$PATH:" == *":~/bin:"* ]] && echo "~/bin is in PATH" || echo "~/bin is not in PATH"
```

If not, we can add it to the PATH by the following script.

```bash
echo 'export PATH=~/bin:$PATH' >> ~/.bash_profile
source ~/.bash_profile
```

