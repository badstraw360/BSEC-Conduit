# BSEC-Conduit Daemon FORK for HASSIO
A first class Systemd process which acts as a conduit between between BSEC-Library
and MQTT. Provides an alternative method of getting data out of an I2C connected
Bosch BME680 sensor and into Home Assistant. Much more accurate than the native
HA BME680 module, as it uses the Bosch Sensortec Environmental Cluster (BSEC)
fusion library to process the raw BME680 sensor readings.


## Requirements
- python-systemd
- paho.mqtt

sudo apt-get install python3-pip`

`sudo apt-get install python3-systemd python3-paho.mqtt`
  *or*
`pip3 install python-systemd paho.mqtt`

## Installation
In this example we'll be installing into a Python venv located at `/opt/bsec` with the
user `pi` on a recent Debian based distro (Raspbian/Hassbian). You can use any location
and user you want, just make sure they are a member of the `i2c` group.
- `sudo mkdir /opt/bsec` Create the directory.
- `sudo chown pi:pi /opt/bsec` Change permissions on the directory.
- `sudo -u pi git clone https://github.com/badstraw360/BSEC-Conduit.git /opt/bsec` Clone the repo into our new directory.
- `sudo -u pi python3 -m venv /opt/bsec` Create our venv.
- `cd /opt/bsec` Change into the directory.
- `source bin/activate` Activate our new venv.
- `sudo -u pi pip3 install systemd-python paho-mqtt` Install required Python modules.
- `sudo python3 install.py` Run the installer.
- `sudo -u pi nano bsec-conduit.ini` Edit the config section at the top of the file. Use `CTRL-X` to save.
- `sudo systemctl start bsec-conduit.service; journalctl -f -u bsec-conduit.service` Start the program and open the log file.

## Usage
Here's a typical log output when started for the first time, stopping and subsequent runs:

`pi@raspberrypi ~# systemctl start bsec-conduit.service`
```
 systemd[1]: Starting BSEC-Conduit Daemon...
 raspberrypi BSEC-Conduit[1234]: BSEC-Conduit v0.3.3
 raspberrypi BSEC-Conduit[1234]: Generated MQTT Client ID: BME680-A12BC3D4
 raspberrypi BSEC-Conduit[1234]: Generated MQTT Base Topic: raspberrypi/BME680
 raspberrypi BSEC-Conduit[1234]: Connected to MQTT Broker.
 raspberrypi BSEC-Conduit[1234]: BSEC-Library executable or hash file not found, starting build process.
 raspberrypi BSEC-Conduit[1234]: BSEC-Library source file not found, writing file: /opt/bsec/BSEC_1.4.7.1_Generic_Release_20180907/bsec-library.c
 raspberrypi BSEC-Conduit[1234]: Detected architecture as ARMv8 64-Bit.
 raspberrypi BSEC-Conduit[1234]: Build process complete.
 raspberrypi BSEC-Conduit[1234]: Created new BSEC-Library configuration [generic_33v_3s_28d].
 raspberrypi BSEC-Conduit[1234]: Created blank BSEC-Library state file.
 raspberrypi BSEC-Conduit[1234]: BSEC-Library started.
 raspberrypi systemd[1]: Started BSEC-Conduit Daemon.
```
`pi@raspberrypi ~# systemctl stop bsec-conduit.service`
```
raspberrypi systemd[1]: Stopping BSEC-Conduit Daemon...
raspberrypi BSEC-Conduit[1234]: Caught Signal 15 (SIGTERM).
raspberrypi BSEC-Conduit[1234]: BSEC-Library stopped.
raspberrypi BSEC-Conduit[1234]: Disconnected from MQTT Broker.
systemd[1]: Stopped BSEC-Conduit Daemon.
```
`pi@raspberrypi ~# systemctl start bsec-conduit.service`
```
 systemd[1]: Starting BSEC-Conduit Daemon...
 raspberrypi BSEC-Conduit[2345]: BSEC-Conduit v0.3.3
 raspberrypi BSEC-Conduit[2345]: Generated MQTT Client ID: BME680-A12BC3D4
 raspberrypi BSEC-Conduit[2345]: Generated MQTT Base Topic: raspberrypi/BME680
 raspberrypi BSEC-Conduit[2345]: Connected to MQTT Broker.
 raspberrypi BSEC-Conduit[2345]: Found existing BSEC-Library executable, skipping build.
 raspberrypi BSEC-Conduit[2345]: Using existing BSEC-Library configuration [generic_33v_3s_28d].
 raspberrypi BSEC-Conduit[2345]: Found existing BSEC-Library state file, skipping creation.
 raspberrypi BSEC-Conduit[2345]: BSEC-Library started.
 raspberrypi systemd[1]: Started BSEC-Conduit Daemon.
```

iT'S VERY IMPORTANT TO ENABLE THE SERVICE ONCE IT'S WORKING:

```
sudo systemctl enable bsec-conduit.service

```
