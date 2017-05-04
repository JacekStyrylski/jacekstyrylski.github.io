---
layout: post
title: PostgreSQL on OSMC
subtitle: PostgreSQL is object-relational database (RDBMS), I'll explain how to install it on Raspbery Pi with OSMC Linux distribution on board. 
comments: true
tags: VeraCrypt, encryption, OSMC, Raspberry Pi
#header-img: img/posts/2017-04/veracrypt.jpg
---

## Installation 
    
Installation of PostgreSQL is quite straightforward since on most Linux distributions it is accessible through apt-get package manager. Nevertheless always remember to first call `apt-get update` in order to download new package lists of the newest packages.  

```bash
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib               
```

## Configuration

1. Introduction:

    Most basic way to connect PostgreSQL db is to use `psql` command prompt tool which should be available after installation. Let's try that:
    ```bash
    psql

    psql: Critical:  role "osmc" do not exits
    ```
    It seems like we do not have permissions, let's try with sudo.
    ```bash
    sudo psql

    psql: Critical:  role "root" does not exits
    ```
    It looks like neither user osmc nor root have proper role created. Why do we need to care about roles in PostgreSQL DB?  
    For the purpose of authorization and authentication in Postgres, there is `role` concept implemented. By default it is configured as `ident` what means, that it will associate every role with exact Linux user name. Currently there is no role for your default user created, that is why it is not possible to login simply using `psql` or using `sudo psql`. We will change that, but first we need to set up postgres user.

2. Set up postgres user:

    After installation there is a dedicated user created with proper roles to connect to PostgreSQL - `postgres`. On OSMC Linux distribution, it is not possible for osmc user (default osmc user) to log in as postgres user. In order to do so we will modify `/etc/sudoers` using `visudo` as follows:  

    First let's open suders editor:

    ```bash
    sudo visudo                                                                 
    ```

    Next add following line to the file:

    `osmc    ALL=(postgres) ALL`

    So after modification it will look as follows:

    ```bash
    #
    # This file MUST be edited with the 'visudo' command as root.
    #
    # Please consider adding local content in /etc/sudoers.d/ instead of
    # directly modifying this file.
    #
    # See the man page for details on how to write a sudoers file.
    #
    Defaults        env_reset
    Defaults        mail_badpass
    Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

    # Host alias specification

    # User alias specification

    # Cmnd alias specification

    # User privilege specification
    root    ALL=(ALL:ALL) ALL

    # ~~~ New Configuration ~~~
    # Below line allows osmc to login to postgres user:
    osmc    ALL=(postgres) ALL
    
    # Allow members of group sudo to execute any command
    %sudo   ALL=(ALL:ALL) ALL

    # See sudoers(5) for more information on "#include" directives:

    #includedir /etc/sudoers.d
    ```

    Now we should be able to connect to Postgres prompt using sudo:

    ```bash
    sudo -i -u postgres psql                                            
    psql (9.4.10)
    Type "help" for help.

    postgres=# 
    ```

    If you wish to exit Postgres prompt just write:

    ```bash
    postgres=# \q                                                       
    ```

3. Adding new role:

    Now that we can finally connect to postgres db let's create a new role.

    ```sql
    create role                                                          
        osmc 
    with
        SUPERUSER LOGIN;
    ```

    If you've done mistake, you can remove role like that

    ```sql
    drop role                                                           
        osmc;
    ```

    Now let's disconnect:

    ```bash
    \d                                                                  
    ```

4. Create test database:

    In order to create db first we will connect to postgres db which by default exists after installation. Now that we've created osmc role, we can simply log, even without sudo as follows:

    ```bash
    psql -d postgres                                                    
    ```

    Let's create test db:

    ```sql
    create database testPi with owner=osmc;                             
    ```

    Now we can disconnect and connect to our new db:

    ```bash
    \q

    psql -d testpi
    ```

## Summary

After above configuration, it should be possible to use `psql` as db management tool by selected user. Probably, it would be a lot of easier to use Postgres out of the box if default db role would be root, unfortunately this is not the case. I hope that above tutorial will help some of you.