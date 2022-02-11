ssh pi@192.168.0.101 -p 2222
sudo mkdir /usr/local/qt-raspi3b-kit 
sudo chown -R pi:pi /usr/local/qt-raspi3b-kit 

sudo mkdir ~/qt-raspi3b
sudo mkdir ~/qt-raspi3b/build
sudo mkdir ~/qt-raspi3b/tools
sudo mkdir ~/qt-raspi3b/sysroot
sudo mkdir ~/qt-raspi3b/sysroot/usr
sudo mkdir ~/qt-raspi3b/sysroot/opt
sudo chown -R 1000:1000 ~/qt-raspi3b
cd ~/qt-raspi3b

rsync -avz -e "ssh -p 2222"  --rsync-path="sudo rsync" pi@192.168.0.101:/lib sysroot/
rsync -av -e "ssh -p 2222"  --rsync-path="sudo rsync" pi@192.168.0.101:/usr/include sysroot/usr/
rsync --ignore-existing --progress -avz -e "ssh -p 2222"  --rsync-path="sudo rsync" pi@192.168.0.101:/usr/lib sysroot/usr/
rsync -avz -e "ssh -p 2222"  --rsync-path="sudo rsync" pi@192.168.0.101:/opt/vc sysroot/opt/


cd build
../qt-everywhere-src-5.15.2/configure -release  -egl -eglfs  -opengl es2  -device linux-rasp-pi3-g++ -device-option CROSS_COMPILE=~/qt-raspi3b/tools/gcc-linaro-7.4.1-2019.02-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf- -sysroot ~/qt-raspi3b/sysroot -prefix /usr/local/qt-raspi3b-kit -extprefix ~/qt-raspi3b/qt-raspi3b-kit -opensource -confirm-license -skip qtscript -skip qtwayland -skip qtwebengine -nomake tests -make libs -pkg-config -no-use-gold-linker -v -recheck

rsync -avz -e "ssh -p 2222" --rsync-path="sudo rsync" =~/qt-raspi3b/qt-raspi3b-kit pi@192.168.0.101:/usr/local/


