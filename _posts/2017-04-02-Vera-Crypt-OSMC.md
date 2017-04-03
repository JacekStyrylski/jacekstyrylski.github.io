---
layout: post
title: VeraCrypt on OSMC
subtitle: VeraCrypt is an industry standard disk encryption. This tutorial will focus on command line usage based on Raspberry 3 with OSMC distribution.
comments: true
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

    ```bash
    ./veracrypt-1.19-setup-console-armv7
    ```

    > 1. Press `1`
    > 2. Press `Enter`
    > 3. Press `q` (only if you already read license agreement)
    > 4. Write `yes` and press `enter`
    > 5. Press `enter`

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

    *Assuming you've external drive connected and mounted already to `/media/storage`*.

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