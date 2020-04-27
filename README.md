# pack_Qt5_on_ubuntu
---
Checked on Ubuntu18.04. If you checked on other linux os, please issue. Thx :D.
##  opencv with video
- 1 . Install pre :
```bash
sudo apt-get install build-essential 
sudo apt-get install libswscale-dev libavcodec-dev libavcodec53 libavformat53 libavformat-dev libswscale-dev libgtk2.0-dev 
```
- 2 . compile opencv with cmake. 
- 3 . Install dir: /usr/local/lib
- 4 . Config ld .so.conf.d 
```
sudo echo /usr/local/lib >>  /etc/ld.so.conf.d/opencv.conf.
sudo ldconfig
```
## QT
- 1 . install qt with linux binary
- 2 . config ~/.bashrc
```
export $LD_LIBRARY_PATH=/home/{YOUR USER NAME}/Qt5.12.0/5.12.0/gcc_64/lib:$LD_LIBRARY_PATH
```
- 3 . config qmake
```
sudo gedit /usr/lib/x86_64-linux-gnu/qt-default/qtchooser/default.conf 

add:
/home/{YOUR USER NAME}/Qt5.12.0/5.12.0/gcc_64/lib
/home/{YOUR USER NAME}/Qt5.12.0/5.12.0/gcc_64/bin

check qmake:
qmake -v
```
## linuxdeployqt
- 1 . install pre :
```
sudo apt-get install git patchelf
```
- 2 . clone linuxdeployqt
```
git clone https://github.com/probonopd/linuxdeployqt.git
```
- 3 . modify	 "/tools/linuxdeployqt/main.cpp"
```
// openSUSE Leap 15.0 uses glibc 2.26 and is used on OBS
        /*if (strverscmp (glcv, "2.27") >= 0) {
            qInfo() << "ERROR: The host system is too new.";
            qInfo() << "Please run on a system with a glibc version no newer than what comes with the oldest";
            qInfo() << "currently still-supported mainstream distribution (xenial), which is glibc 2.23.";
            qInfo() << "This is so that the resulting bundle will work on most still-supported Linux distributions.";
            qInfo() << "For more information, please see";
            qInfo() << "https://github.com/probonopd/linuxdeployqt/issues/340";
            return 1;
        }*/
```
- 4 . compile 
```
qmake 
make -j8
cd bin
chmod a+x linuxdeployqt
sudo cp linuxdeployqt /usr/local/bin/
sudo wget -c "https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage" -O /usr/local/bin/appimagetool
sudo chmod a+x /usr/local/bin/appimagetool
```
- 5 . pack 
```
linuxdeployqt {YOUR APP NAME} -appimage
```
- 6 . add a "runApp.sh"
```
gedit runApp.sh

add:
#!/bin/bash
export LD_LIBRARY_PATH=/app/lib:$LD_LIBRARY_PATH
export QT_PLUGIN_PATH=/app/plugins:$QT_PLUGIN_PATH
export QML2_IMPORT_PATH=/app/qml:$QML2_IMPORT_PATH
./{YOUR APP NAME}
```
