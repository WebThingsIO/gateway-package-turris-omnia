## Install dependencies needed for turris build

```
sudo apt install ca-certificates git build-essential zlib1g-dev gawk libssl-dev subversion unzip libncurses-dev wget python file rsync
sudo apt install ccache intltool
```

## Build and install usign

```
git clone https://git.openwrt.org/project/usign.git
cd usign
mkdir build
cd build
cmake ..
make
sudo make install
```

## Get the turris-build repository

The branch that you checkout here should correspond to the BRANCH used in the
[make-medkit.sh](https://github.com/mozilla-iot/turris-medkit/blob/master/make-medkit.sh#L7) script.
See the [WORKFLOW.asciidoc](https://gitlab.labs.nic.cz/turris/turris-build/blob/master/WORKFLOW.asciidoc)
for the appropriate correspondence.

So far, I've only been able to get clean builds using the master branch from gitlab
and BRANCH="hbd", but there still seem be a number of issues (see below).

```
git clone https://gitlab.labs.nic.cz/turris/turris-build.git
cd turris-build
git checkout master
```

## Edit the feeds.conf file and add this line to the end

```
src-git moziot https://github.com/mozilla-iot/gateway-package-turris-omnia.git
```

If you forget to do the above (before building the toolchain), then you can add
that line to build/feeds.conf after building the toolchain.

## Build the toolchain and other required tools

```
./compile_pkgs -f -t omnia -j8 prepare compile_tools compile_target
```

This will create a directory called build and build the appropriate toolchain
pieces.

## Update the package lists and install all of the packages

```
cd build
./scripts/feeds/update -a
```
If you get git merge commits just do :wq in vi to accept the merge commit
```
./scripts/feeds/install -a
```
You can verify that node-mozilla-iot-gateway now exists in the package/feeds/moziot directory.
```
export TERM=xterm   # Makes the menuconfig console display look good
make menuconfig
exit menuconfig and save the changes (you don't need to change anything)
```
Verify node-mozilla-iot-gateway was enabled in .config
```
grep node-mozilla-iot-gateway .config
```
You should see:
```
CONFIG_PACKAGE_node-mozilla-iot-gateway=m
```
## Build the .ipk files for the gateway and misc dependencies

From within the build directory:
```
make package/node-mozilla-iot-gateway/compile
cd ..
```

## Create a repository for the .ipk files and update it

```
git clone git@github.com:mozilla-iot/turris-medkit.git moziot
cd moziot
./update-repo.sh
./update-aws.sh
```

## Create a medkit

Make sure that the BRANCH specified in the make-medkit.sh lines up with the
branch of turris-build used above.

```
./make-medkit.sh
```

Make sure that the version of python used to build the python-gateway-addon
is the same as the version of python installed in the medkit.

```
tar tf omnia-medkit-moziot-0.9.3-1.tar.gz | grep gateway_addon-
```
You should see something like this:
```
./usr/lib/python3.7/site-packages/gateway_addon-0.10.0-py3.7.egg-info/
./usr/lib/python3.7/site-packages/gateway_addon-0.10.0-py3.7.egg-info/dependency_links.txt
./usr/lib/python3.7/site-packages/gateway_addon-0.10.0-py3.7.egg-info/PKG-INFO
./usr/lib/python3.7/site-packages/gateway_addon-0.10.0-py3.7.egg-info/SOURCES.txt
./usr/lib/python3.7/site-packages/gateway_addon-0.10.0-py3.7.egg-info/requires.txt
./usr/lib/python3.7/site-packages/gateway_addon-0.10.0-py3.7.egg-info/top_level.txt
```

and
```
tar tf omnia-medkit-moziot-0.9.3-1.tar.gz --list | grep /usr/bin/python
```
```
lrwxrwxrwx root/root         0 2019-10-09 06:31 ./usr/bin/python3 -> python3.7
-rwxr-xr-x root/root      4099 2019-10-09 06:31 ./usr/bin/python2.7
-rwxr-xr-x root/root      4099 2019-10-09 06:31 ./usr/bin/python3.7
lrwxrwxrwx root/root         0 2019-10-09 06:31 ./usr/bin/python2 -> python2.7
lrwxrwxrwx root/root         0 2019-10-09 06:31 ./usr/bin/python -> python2.7
```
We can see that we've got python3.7 and the gateway_addon was built for 3.7
so we're good.

## Issues

The master branch of turris-build should correspond to "hbd" aka "Here be Dragons".
See: https://gitlab.labs.nic.cz/turris/turris-build/blob/master/WORKFLOW.asciidoc
for up-to-date details.

I tried to build node-mozilla-iot-gateway using the v4.0 branch (hbk) but ran
into some issues about dependencies. I believe that this is actually an issue
with the OpenWRT 18.06 branch.

I wrote this on Oct 15.

* The overlay/etc/uci-defaults file gets copied to the image and its supposed
  to be executed when the turris boots up, but it doesn't seem to be. The
  files from /etc/uci-defaults/ should get removed when they're executed.

  This might me related to the random number generator. It seems that when I
  start typing on the console then all of the sudden it decides that it has
  enough entropy and the continues. Everything starts happening when I see the
  message:

```
random: crng init done
```

* All of the pre-installed plugins come up disabled by default. This probably
  needs to be tweaked in the make-medkit.sh script.

* I didn't see any errors when loading the speedtest addon but I couldn't seem
  to get it to work.

