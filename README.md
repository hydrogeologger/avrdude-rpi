Using avrdude with the Raspberry Pi
===================================

Since the Raspberry Pi lacks a DTR pin that makes it oh-so-easy to upload your hex files into
the avr, we need this hack to make it just as easy.  When you wire up your atmega chip, be sure
to connect one of the digital gpio pins to the reset pin, and then you'll be able to use avrdude
as if your serial cable actually had a dtr pin.

Instructions:
-------------

Copy both files into your /usr/bin directory, then rename the original avrdude to avrdude-original
and symlink avrdude-autoreset to become avrdude.
```bash
cp autoreset /usr/bin
cp avrdude-autoreset /usr/bin
```
```bash
# Backing up original avrdude, make sure you need to use this command or else you will replace backup with our own avrdude-autoreset
mv /usr/bin/avrdude /usr/bin/avrdude-original
```
```bash
# Symlink avrdude to avrdude-autoreset
ln -s /usr/bin/avrdude-autoreset /usr/bin/avrdude
```

Modify the autoreset script to use the pin that you wired up to the reset pin.  See the line in
autoreset where we do "pin = 11" and change the 11 to your gpio pin number.

Now when you run avrdude from anywhere (including via arduino's normal UI) it will flag dtr when
it is about to upload hex data.