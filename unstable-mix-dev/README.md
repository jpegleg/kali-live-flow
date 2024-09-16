# Kali: unstable mix with Debian Sid +

This build of Kali linux adds three additional debian repositories.
The first one is to add Debian Sid (Debian Unstable), but only update
the kernel to the chosen Sid build. 

## Warning

This is not an appropriate build for a production server.

The intent is for research, development, and integration purposes.

This is maybe an appropriate build for a virtual workstation used in continuous integration and development (testing) of software, or components of kali, debian, or the linux kernel itself.

## But why was this made?

<b>To shrink the OS footprint of kali but leverage it, drift and tune the kernel, open wide the compiler and library possibilities.</b>

This is a setup intended for software developers. It has little 
from the kali repo compared to the default install, but leverages
the position of kali in the software development lifecycle of Debian
to create a stabalized base to mix with Sid. Mixing Stable with Unstable
is not typically a good idea and is rarely useful. 

But Testing mixed with Unstable can be good with some care.
Kali is downstream of Testing, but Testing-like, and provides
a reliable starting point for mixing.

In this case, we are mixing the kernel from Sid. This can be useful
to get newer kernel features and support, as well as to do security
research, testing, and development for the linux kernel.

The scripts and packages are detailed and specific, but they don't have to be.
Change it as you will to add or remove the tools and apps.

## Details about packages and repositories

<i>See the (COMING SOON) scripts for installing the app and library packages and tools.</i>

Update the kernel and then freeze Sid:

```
vim /etc/apt/sources.list

deb https://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
deb https://deb.debian.org/debian sid main non-free-firmware
deb-src http://deb.debian.org/debian side main non-free-firmware

```

The example uses `aptitude`, but the other variants can be used instead of course.

```
sudo aptitude update # NOT UPGRADE here, just pull the packages
...
sudo aptitude search linux-image 
...
sudo aptitude install linux-image-6.10.9-amd64
...
sudo vim /etc/apt/preferences

Package: *
Pip: release a=unstable
Pin-Priority: 100

```

Now we'll have access to many packages, but we won't install from unstable by default when kali (testing+) has a package for it.

```
sudo aptitude update
...
sudo aptitude upgrade
```

The second two repositories are added with `kali-tweaks`: the Test Staging repo and the Experimental repo.

Additionally with `kali-tweaks`, we remove all of the metapackages. This can be an important step in the
unstable mix building, depending on what the intent is. 

Executing `kali-tweaks` can be from the launcher panel in (XFCE), or on the CLI. We can use the TUI to
deselect the metapackages. This is also just nice for a number of reaons, including the installation disk usage down to less than 8GB for the core and less
than 10 GB after installing all the development tools. This size is from an XFCE desktop build of this type tested on 2024 09 15.

What is left is the core tools for debugging and hacking, and development, and a shiny newer kernel build.


## The other actions in the scripts for this build

We then can tune or harden the kernel settings, and configure the development environments we want to use.

This build applies development for `Rust`, `Go`, `Python`, `R`, `C`, `C++`, shell scripting, as well as the added shells `elvish` and `nu`.
Java developers can select the upstream they want/need to use, only the java runtime is included by default.

The included scripts use `rustup` setting the default build to nightly.

Additionally this build adds `vim`, `obsidian`, and `Rstudio` for the various text editing/development/writing/notes tools.
The programs `Inkscape` and `Gimp` are included for additional media and image processing.

As per jpegleg style, scripts include the dwarven toolbox project in the build: https://github.com/jpegleg/dwarven-toolbox
Also per jpegleg style, installing the rust CLI tool `AES256CTR` from https://github.com/jpegleg/file_encryption_AES256
and `fms` from https://github.com/jpegleg/rocky-bastard

Wezterm and classic xterm are both installed for terminal emulators.

The program `yara-x` (new YARA) is installed via `cargo`. Also installing `cross` for musl libc and other architecture targets like ARM64.

The a reference for installing the java program `dbeaver` for database browsing is included.

Clang is included, and a wide range of clang versions become available with the repo modifications.

Docker is installed from docker.io to keep inline with Debian.
