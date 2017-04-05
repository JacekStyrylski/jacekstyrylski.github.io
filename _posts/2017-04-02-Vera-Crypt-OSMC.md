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

In order to create volume within a file, follow these steps:

1. Create mounting point:

    ```bash
    sudo mkdir /media/vera-test
    sudo chown osmc:osmc /media/vera-test
    ```

2. Create a volume:

    *Assuming you've external drive connected and mounted already to `/media/storage`*.

    ```bash
    veracrypt -t -c
    ```

    Select volume type, hidden volumes provide even higher security, however for now, we will just create normal volume.

    ```bash
    Volume type:
     1) Normal
     2) Hidden
    Select [1]: '1'
    ```

    Enter filename, within which volume will be created.

    ```bash
    Enter volume path:  '/media/Backup/vera-test-volume
    ```

    Define volume size. If you need 10 kilo, mega or giga bytes it will look as follows.

   - 10K (10 Kilobytes)
   - 10M (10 Megabytes)
   - 10G (10 Gigabytes)

    We will use 10 Megabytes.

    ```bash
    Enter volume size (sizeK/size[M]/sizeG): '10M'
    ```

    Select encryption algorithm, people who tries to do brute force attack on encrypted volumes, usually assumes AES algorithm hence it is recommended to select different one.

    ```bash
    Encryption Algorithm:
    1) AES
    2) Serpent
    3) Twofish
    4) Camellia
    5) Kuznyechik
    6) AES(Twofish)
    7) AES(Twofish(Serpent))
    8) Serpent(AES)
    9) Serpent(Twofish(AES))
    10) Twofish(Serpent)
    Select [1]: '3'
    ``` 

    Select hashing algorithm.

    ```bash
    Hash algorithm:
     1) SHA-512
     2) Whirlpool
     3) SHA-256
     4) Streebog
    Select [1]: '1'
    ```

    Select file system, for simplicity it can be FAT but if you need file permission even Ext4 might be used.

    ```bash
    Filesystem:
     1) None
     2) FAT
     3) Linux Ext2
     4) Linux Ext3
     5) Linux Ext4
     6) NTFS
     7) exFAT
    Select [2]: '2' 
    ```

    Password selection, it is recommended to use password consisting of more than 20 alpha numeric characters, due to possibility of brute force attack.

    ```bash
    Enter password: '[your password]'
    Re-enter password: '[your password]'
    ```

    PIM stands for Personal Iterations Multiplier.

    > It is a value that controls the number of iterations used by the header key derivation
    > following the formulas:
    >
    > - For system encryption: Iterations = PIM x 2048
    > - For non-system encryption and file containers: Iterations = 15000 + (PIM x 1000)
    >
    > If PIM value is high, iterations are also high and this implies a better security but a slower
    > mounting/booting. 
    > If PIM value is small, iterations count is also small and this implies quicker 
    > mounting/booting but it brings a decreases security.

    *[Explanation done by Mounir IDRASSI.](https://sourceforge.net/p/veracrypt/discussion/general/thread/e51e51fe/#663e)*


    We will select default value so, just press `enter`.

    ```bash
    Enter PIM: 
    ```

    If you want to use keyfile instead of password, here you can define keyfile. We will not do that so again you can just press `enter`.

    ```bash
    Enter keyfile path [none]: 
    ```

    Here is the funny part, you need to type at least 320 random characters since this will be base for your encryption key.

    ```bash
    Please type at least 320 randomly chosen characters and then press Enter:
    ```

    That's it, now you need to wait, even hours (for about 500GB drive) for encryption to process.

3. Mounting a volume:

    **Run following command:**

    ```bash
    veracrypt /media/storage/new-volume /media/vera-test
    ```

    **Steps:**

    ```bash
    Enter password for /media/storage/new-volume: '[volume password]'
    ```

    Enter PIM but, if you've used default PIM value, just press `enter`.

    ```bash
    Enter PIM for /media/storage/new-volume:
    ```

    Enter keyfile, if you've used password you can just hit `enter`.

    ```bash
    Enter keyfile [none]:
    ```

    If you've created normal, not hidden volume, just hit `enter` again.

    ```bash
    Protect hidden volume (if any)? (y=Yes/n=No) [No]:
    ```

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

After adding aliases you can call `storage` for mounting and `unstorage` for dismounting your volume.