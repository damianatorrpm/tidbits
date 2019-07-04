# Silverblue tidbits
Some things I do on Fedora Silverblue (best Linux desktop btw) to get software I want on the host without package overlaying. This is entirely for myself, but maybe others might find it useful.

- Contents
  - [Nano](#nano)
  - [Android Tools (adb, fastboot etc)](#android-tools-adb-fastboot-etc)
  - [MS Core Fonts](#ms-core-fonts)

### Nano

#### ncurses

```bash
wget https://ftp.gnu.org/pub/gnu/ncurses/ncurses-6.1.tar.gz
tar -xvf ncurses-6.1.tar.gz && cd ncurses-6.1
./configure --prefix=/usr/local
make -j4
sudo make install
```

#### nano
```bash
wget https://www.nano-editor.org/dist/v4/nano-4.3.tar.gz
tar -xvf nano-4.3.tar.gz && cd nano-4.3
CFLAGS="-I/usr/local/include/ncurses" ./configure --prefix=/usr/local
make -j4
sudo make install
```
---

### Android Tools (adb, fastboot etc)

```bash
wget https://dl.google.com/android/repository/platform-tools-latest-linux.zip
unzip platform-tools-latest-linux.zip
sudo cp -v platform-tools/adb platform-tools/fastboot platform-tools/mke2fs* platform-tools/e2fsdroid /usr/local/bin
```

#### Device rules (to use without root)

```bash
git clone https://github.com/M0Rf30/android-udev-rules.git
cd android-udev-rules
sudo cp -v 51-android.rules /etc/udev/rules.d/51-android.rules
sudo chmod a+r /etc/udev/rules.d/51-android.rules
sudo groupadd adbusers
sudo usermod -a -G adbusers $(whoami)
sudo systemctl restart systemd-udevd.service
adb kill-server
adb devices
```
---

### MS Core Fonts

#### cabextract
```bash
wget https://www.cabextract.org.uk/cabextract-1.9.1.tar.gz
tar -xvf cabextract-1.9.1.tar.gz
cd cabextract-1.9.1
./configure --prefix=/usr/local && make
sudo make install
```

#### mscorefonts.sh

```bash
wget https://raw.githubusercontent.com/p1u3o/tidbits/master/mscorefonts.sh
sh mscorefonts.sh
```
##### To install user-wide (Flatpak will see these)
```bash
mkdir ~/.local/share/fonts
mkdir ~/.local/share/fonts/mscorefonts
cp -v fonts/*.ttf fonts/*.TTF ~/.local/share/fonts/mscorefonts/
```
##### To install system-wide (Flatpak might not see these yet)
```bash
sudo mkdir /usr/local/share/fonts/
sudo mkdir /usr/local/share/fonts/mscorefonts/
sudo cp -v fonts/*.ttf fonts/*.TTF /usr/local/share/fonts/mscorefonts/
```

