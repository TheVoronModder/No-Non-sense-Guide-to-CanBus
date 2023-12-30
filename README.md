# No-Non-sense-Guide-to-CanBus
No Nonsense guide to CanBus installation

YouTube Video here:

https://www.youtube.com/watch?v=N9FpPlgPhpY

STEP 1.
SSH into your pi
```
sudo nano /etc/network/interfaces.d/can0
```
Once done you will CTRL+C these and Right Mouse Click in the terminal to paste
```
allow-hotplug can0
  Iface can0 can static
  Bitrate 1000000
  Up ip link set can0 txqueuelen 1024
```
STEP 2.
Now to verify your U2C is in DFU mode
```
lsusb
```
Flash the U2C
```
cd ~/
wget https://github.com/Esoterical/voron_canbus/raw/main/can_adapter/BigTreeTech%20U2C%20v2.1/G0B1_U2C_V2.bin
dfu-util -D ~/G0B1_U2C_V2.bin -a 0 -s 0x08000000:leave
```
NOTE: dfu-util: Error during download get_status is normal FYI!
```
ip addr
```
STEP 3. 
Now
```
lsusb
```
Now we need to install more stuff
```
cd~
git clone https://github.com/Arksine/katapult
cd katapult
make menuconfig
```
Now we make the firmware
```
make
```
Once its done installing run this
```
dfu-util -R -a 0 -s 0x08000000:force:mass-erase:leave -D ~/katapult/out/katapult.bin -d 0483:df11
```
Step 4
```
cd ~/klipper/
```
```
git pull
```
```
make clean
```
```
make menuconfig
```
```
make
```
```
~/klippy-env/bin/python3 ~/klipper/scripts/canbus_query.py can0
```
Use your Specific Address with this:

```
python3 ~/katapult/scripts/flash_cany.py -I can0 -u YOURIDHERE -f -/klipper/out/klipper.bin
```
