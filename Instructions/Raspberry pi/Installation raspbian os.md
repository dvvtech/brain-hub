1. Raspbery pi imager, скачиваем утилиту
https://github.com/raspberrypi/rpi-imager/releases
Тут можно выбрать ОС и некоторые настройки
2. Если разрешение экрана не то то прав. кн. мыши свойства и изменить разрешение экрана
3. в командной строке 
sudo raspi-config
3 Interface options
Camera
Yes
4. чтоб сделать фото
libcamera-still -o test.jpg


///////////////////////////////////////////

прошивка для ip camera
https://github.com/motioneye-project/motioneyeos
https://www.youtube.com/watch?v=JXxGwSLLAGs


https://www.youtube.com/watch?v=ybQzlvukSBo
https://github.com/jawsper/motioneyeos/releases/tag/20220119-dev

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
ssid="ASUSWiFi"
psk="Akvalang741"
key_mgmt=WPA-PSK
}


настройка
https://www.youtube.com/watch?v=Gl864ofx4TU
https://www.youtube.com/watch?v=gYo7WPQDlyM

meye
http://192.168.1.126/
http://192.168.1.165/

Камера 1 
http://192.168.1.126:8081/
http://94.19.16.117:8082/

Камера 1 (webcam)
http://192.168.1.126:8082/
http://94.19.16.117:8083/


![[Pasted image 20240222210649.png]]