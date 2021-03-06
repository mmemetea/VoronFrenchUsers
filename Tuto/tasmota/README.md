Pour pouvoir allumer et eteindre le PSU 24v donc les deux MCU de la Voron depuis Mainseal avec un device tasmota (Sonoff Basic R2 chez moi)

Installer git sur le Pi
```
sudo apt-get install git -y
```

Pour faire apparaitre le bouton dans mainseal :
```
cd
sudo nano /home/pi/moonraker.conf
```
Et ajouter dans le fichier :
```
[power]
devices: printer
#   A comma separated list of devices you wish to control. Device names may not
#   contain whitespace.  This parameter must be provided.
#
# Each device specified in "devices" should define its own set of the below
# options:
printer_name: PSU24V
#   An optional alias for the device. The default is the name specifed in
#   "devices".
printer_pin: 4
#   The sysfs GPIO pin number you wish to control.  This parameter must be
#   provided.
printer_active_low: False
#   When set to true the pin signal is inverted.  Default is False.
```

Maintenant ce bouton apparait

![power_off_switch](https://github.com/elpopo-eng/VoronFrenchUsers/blob/main/Tuto/tasmota/psu24v.png)

Il faut installer powermanager
```
cd
git clone https://github.com/Raabi91/powermanager
cd powermanager
sh ./scripts/install.sh
```

Si tu as un Raspberry Pi 4 comme moi il faut faure une mise a jour manuelle de wiringpi 2.52

```
cd /tmp
wget https://project-downloads.drogon.net/wiringpi-latest.deb
sudo dpkg -i wiringpi-latest.deb
```

Dans les fichier de configuration de mainseal on a "powermanager_config.sh"

```
#!/bin/sh
# Only: 1=on/off/autoshutdown 0=only autoshutdown
only="1"
# printer_off = shell command to switch printer off
printer_off="curl http://192.168.1.132/cm?cmnd=Power%20off"
# printer_on = shell command to switch printer on
printer_on="curl http://192.168.1.132/cm?cmnd=Power%20on"
# pin_printer = the same pin as in the power section for printer
pin_printer="4"
# pin_autosh = the same pin as in the power section for autoshutdwon
pin_autosh="5"
# Time = Time after print end before shutdown in seconds (do not use under 60 sec)
time="300"
# Port from moonraker
port="7125"
```
Faut mettre l'ip de votre device tasmota

On fini avec un reboot du pi et c'est bon le switch fonctionne !
```
sudo reboot
````
