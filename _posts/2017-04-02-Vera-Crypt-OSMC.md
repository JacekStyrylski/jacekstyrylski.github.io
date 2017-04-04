---
layout: post
title: VeraCrypt on OSMC
subtitle: VeraCrypt is an industry standard disk encryption. This tutorial will focus on command line usage based on Raspberry 3 with OSMC distribution.
comments: true
tags: VeraCrypt, encryption, OSMC, Raspberry Pi
#header-img: img/posts/2017-04/veracrypt.jpg
---
![ss]({{site.url}}/img/posts/2017-04/veracrypt.jpg)

If you'd like to use it with gui, there are some good tutorials on how to use it with GUI, for example this one:

[http://www.linuxandubuntu.com/home/encrypt-data-in-linux-with-veracrypt-an-alternative-to-truecrypt](http://www.linuxandubuntu.com/home/encrypt-data-in-linux-with-veracrypt-an-alternative-to-truecrypt)

## Installation

Based on: [fredfire1](https://fredfire1.wordpress.com/2016/02/04/install-veracrypt-debianwindows/)

1. Dependencies

    ```bash
    sudo apt-get update
    sudo apt-get install libfuse-dev makeself libwxbase3.0-0
    ```

2. Download:

    The newest version might be found here: [VeraCrypt - Downloads](https://veracrypt.codeplex.com/wikipage?title=Downloads)

    ```bash
    cd ~
    mkdir veracryptfiles
    cd veracryptfiles
    wget -L -O veracrypt-1.19-raspbian-setup.tar.bz2 https://launchpad.net/veracrypt/trunk/1.19/+download/veracrypt-1.19-raspbian-setup.tar.bz2
    ```

3. Extract:

    ```bash
    tar -vxjf ./veracrypt-1.19-raspbian-setup.tar.bz2
    chmod +x veracrypt-1.19-setup-*
    ./veracrypt-1.19-setup-console-armv7
    ```

4. Install:

    Run installation script:

    ```bash
    ./veracrypt-1.19-setup-console-armv7
    ```

    Go according those steps:

        
    ```bash
    VeraCrypt 1.19 Setup
    ____________________
    Installation options:
    1) Install veracrypt_1.19_console_armv7.tar.gz
    2) Extract package file veracrypt_1.19_console_armv7.tar.gz and place it to /tmp

    To select, enter 1 or 2: '1'
    ```
    ```bash
    Before you can use, extract, or install VeraCrypt, you must accept the
    terms of the VeraCrypt License.
    
    Press Enter to display the license terms... 'Enter'
    ```
    ```bash
    Press Enter or space bar to see the rest of the license.
    
    VeraCrypt License
    Software distributed under this license is distributed on an AS
    IS BASIS WITHOUT WARRANTIES OF ANY KIND. THE AUTHORS AND
    DISTRIBUTORS OF THE SOFTWARE DISCLAIM ANY LIABILITY. ANYONE WHO
    USES, COPIES, MODIFIES, OR (RE)DISTRIBUTES ANY PART OF THE
    SOFTWARE IS, BY SUCH ACTION(S), ACCEPTING AND AGREEING TO BE
    BOUND BY ALL TERMS AND CONDITIONS OF THIS LICENSE. IF YOU DO NOT
    ACCEPT THEM, DO NOT USE, COPY, MODIFY, NOR (RE)DISTRIBUTE THE
    SOFTWARE, NOR ANY PART(S) THEREOF.
    
    VeraCrypt is multi-licensed under Apache License 2.0 and
    the TrueCrypt License version 3.0, a verbatim copy of both
    licenses can be found below.
    
    : 'q'
    ```
    ```bash
    Do you accept and agree to be bound by the license terms? (yes/no): 'yes'
    ```
    ```bash
    Uninstalling VeraCrypt:
    -----------------------
    
    To uninstall VeraCrypt, please run 'veracrypt-uninstall.sh'.
    
    Installing package...
    [sudo] password for '[your user]':'*******'
    ```
    ```bash
    usr/
    usr/share/
    usr/share/veracrypt/
    usr/share/veracrypt/doc/
    usr/share/veracrypt/doc/License.txt
    usr/share/veracrypt/doc/VeraCrypt User Guide.pdf
    usr/share/pixmaps/
    usr/share/pixmaps/veracrypt.xpm
    usr/share/applications/
    usr/share/applications/veracrypt.desktop
    usr/bin/
    usr/bin/veracrypt
    usr/bin/veracrypt-uninstall.sh
    
    Press Enter to exit... 'Enter'
    ```

5. Erase installation files:

    ```bash
    rm -r veracryptfiles
    ```

## Volume creation

There are two ways of creating encrypted containers using VeraCrypt:

1. Within the file.
2. Encrypting complete partition/disk.

There is useful help command:

```bash
veracrypt --help
```

## Volume within a file

Follow these steps:

1. Create mounting point:

    ```bash
    sudo mkdir /media/vera-test
    sudo chown osmc:osmc /media/vera-test
    ```

2. Create a volume:

    *Assuming you've external drive connected and mounted already to `/media/storage` which is connected to device*.

    ```bash
    veracrypt -t -c
    ```

    > 1. Press `1` *(unless you need hidden volume)*.
    > 2. Write `/media/storage/new-volume` press `enter`.
    > 3. Write `500M` *(will create 500 M volume) press `enter`.
    > 4. Press `enter` *(unless you want to use different encryption algorithm)*.
    > 5. Press `enter` *(unless you want to use different hash algorithm)*.
    > 6. Press `enter` *(unless you want to use different file system)*
    > 7. Write your password and press `enter` *(it should be larger than 20 characters to block > against brute force techniques)*.
    > 8. Press `enter` *(What is PIM: [https://sourceforge.net/p/veracrypt/discussion/general/thread/e51e51fe/#663e](https://sourceforge.net/p/veracrypt/discussion/general/thread/e51e51fe/#663e))*.
    > 9. Press `enter`.
    > 10. Write on keyboard at least 320 random characters and press `enter`.

3. Mounting a volume:

    ```bash
    veracrypt /media/storage/new-volume /media/vera-test
    ```

    > 1. Enter password written in step 7 and press `enter`.
    > 2. Press `enter`.
    > 3. Press `enter`.
    > 4. Press `enter`.

    That's it, your volume should be visible in `/media/vera-test`.

## Encrypting complete parition/disk

If you need to encrypt entire drive, the only difference is that in step 2.2 instead of file name `/media/storage/new-volume` you need to put device file name like that: `/dev/sda`.

## Dismounting volume

After you stopped using encrypted volume, you should dismount it:

```bash
varacrypt -d
```

## Creating aliases

If you wish, you can create handy aliases for mounting and dismounting your VeraCrypt volume:

Mounting alias:
```bash
printf "\nalias storage=\"veracrypt /dev/sda /media/storage --pim=0 --keyfiles= --protect-hidden=no\"" >> .bashrc 
```

Unmounting alias:

```bash
printf "\nalias unstorage=\"veracrypt -d /dev/sda\"" >> .bashrc 
```

After adding aliases