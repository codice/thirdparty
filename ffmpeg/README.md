# ffmpeg Compilation Guide
The following instructions are based off of the [official ffmpeg compilation guide](https://trac.ffmpeg.org/wiki/CompilationGuide).
If problems are encountered during this guide, please refer to the official guide.

This guide sets up [Oracle's VirtualBox](https://www.virtualbox.org/wiki/Downloads) VMs to compile the ffmpeg library, with the exception of MacOS X.
That way:

1. Most environmental inconsistencies are mitigated
1. Cleanup isn't needed.

> NOTE
>
> If not specified, assume the instructions to be for the host machine.


### Linux:

1. Download the [Ubuntu 64 bit iso](https://www.ubuntu.com/download/desktop).

1. Create a new Ubuntu 64 bit virtual machine in VirtualBox.
If your host machine is Windows, you may have to [disable Hyper-V](https://www.poweronplatforms.com/enable-disable-hyper-v-windows-10-8/) to run 64-bit virtual machines in VirtualBox.

1. Start up the new virtual machine and select the Ubuntu 64 bit iso to boot from. Follow the installation instructions to complete the installation.

1. On the virtual machine, open the terminal. This will be our terminal for the rest of this guide.

1. On the virtual machine, install compilation dependencies by running

    ```
    sudo apt-get update -qq && sudo apt-get -y install \
    autoconf \
    automake \
    build-essential \
    cmake \
    git \
    libtool \
    pkg-config \
    texinfo \
    wget \
    yasm \
    zlib1g-dev
    ```

1. On the virtual machine, clone and cd into the [ffmpeg GitHub repository](https://github.com/ffmpeg/ffmpeg) by running

    ```
    git clone https://github.com/ffmpeg/ffmpeg && \
    cd ffmpeg
    ```

1. Checkout the release you want to compile by running

    ```
    git fetch --all --tags --prune && \
    git checkout tags/<tag_name> -b <branch_name>
    ```
    where `<tag_name>` is the name of the tag you want to checkout, and `<branch_name>` is the name of a new local branch to checkout that commit on.

1. On the virtual machine, configure the source for compilation & make by running

    ```
    ./configure \
    --disable-shared \
    --enable-static \
    --pkg-config-flags="--static" \
    --extra-cflags="-static" \
    --extra-ldflags="-static" \
    --enable-version3 && \
    make
    ```

1. The root directory of the cloned ffmpeg repo will now have the desired binary `ffmpeg`.

### MacOS X
> Note
>
> FFMPEG was compiled directly on a MacOS X system rather than a VM because it was simpler.

1. Install compilation dependencies by running

    ```
    brew install automake fdk-aac git lame libass libtool libvorbis libvpx \
    opus sdl shtool texi2html theora wget x264 x265 xvid nasm
    ```

1. Clone and cd into the [ffmpeg GitHub repository](https://github.com/ffmpeg/ffmpeg) by running

       ```
       git clone https://github.com/ffmpeg/ffmpeg && \
       cd ffmpeg
       ```

1. Checkout the release you want to compile by running

   ```
   git fetch --all --tags --prune && \
   git checkout tags/<tag_name> -b <branch_name>
   ```
   where `<tag_name>` is the name of the tag you want to checkout, and `<branch_name>` is the name of a new local branch to checkout that commit on.

1. Configure the source & make by running

    ```
      ./configure \
     --arch=x86_64 \
     --target-os=darwin \
     --enable-cross-compile \
     --pkg-config-flags="--static" \
     --disable-shared \
     --enable-static \
     --enable-version3 && \
     make
    ```

1. The root directory of the cloned ffmpeg repo will now have the desired binary `ffmpeg`.

### Windows:

1. [Install Vagrant](https://www.vagrantup.com/downloads.html).
1. Clone and cd into this [Windows Vagrant GitHub repository](https://github.com/oconnormi/vagrant-windows) made by Michael O'Connor by running

    ```
    git clone https://github.com/oconnormi/vagrant-windows
    ```
1. Run `vagrant up`.
1. Run `vagrant rdp`.
1. On the virtual machine, [download MSYS2 32 bit](http://repo.msys2.org/distrib/i686/msys2-i686-20161025.exe).
1. On the virtual machine, start `mingw32.exe` found under `C:\msys32`. This will be our terminal for the remainder of this guide.
1. On the virtual machine, install compilation dependencies by running

    ```
    pacman -S make \
    diffutils \
    yasm \
    mingw-w64-i686-gcc
    ```
1. On the virtual machine, clone and cd into the [ffmpeg GitHub repository](https://github.com/ffmpeg/ffmpeg) by running

    ```
    git clone https://github.com/ffmpeg/ffmpeg && \
    cd ffmpeg
    ```
Note that mingw32 doesn't have git installed. You can open `Git Bash` to use git, but make sure to switch back to mingw32 for the remainder of this guide.
1. Checkout the release you want to compile by running

    ```
    git fetch --all --tags --prune && \
    git checkout tags/<tag_name> -b <branch_name>
    ```
    where `<tag_name>` is the name of the tag you want to checkout, and `<branch_name>` is the name of a new local branch to checkout that commit on.
1. On the virtual machine, configure the source & make by running

    ```
    ./configure \
    --disable-shared \
    --enable-static \
    --pkg-config-flags="--static" \
    --extra-cflags="-static" \
    --extra-ldflags="-static" \
    --enable-version3 && \
    make
    ```
1. The root directory of the cloned ffmpeg repo will now have the desired binary `ffmpeg.exe`.

### Testing
To test the binary run the following command

```
./ffmpeg -i <MPG_FILE> -vf thumbnail,scale=200:-1 -frames:v 1 -vsync vfr thumbnail_%d.png
```
where `<MPG_FILE>` is a .mpg file of your choosing.

If successful, there should be console output and a thumbnail named "thumbnail_x.png" in the current directory.
