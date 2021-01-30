# A linux battery life saver

This is a quick and dirty solution for saving battery life of Linux laptops.

When working in a laptop that is continuosliy connected to the power grid
the battery keeps charged at 100% all the time and this damages the battery
shortening its life span.

To solve this problem I bought a Tp-link HS-100 that is a Wifi controllable
power switch that can be easily managed using Python as there is a library,
Kasa, that allows direct control of the switch from the computer.

With this I have built a simple script that reads battery status and then turns
off the switch when battery charge is above a value and back on when the
discharge reaches a certain point.

The script is controlled with a systemd's timer that runs periodically the
check.

## Installation

First edit the script that has the switch ALIAS and charge percentages hard coded
in the header of the script. And copy the file to local bin directory.

``` 
sudo cp battery-saver /usr/local/bin
```

For the script to work  you need the library Kasa, installed with pip.

```
sudo pip install kasa
```

Then copy all systemd unit files into the configuration folder

```
sudo cp systemd/battery-saver.* /etc/systemd/system
```

Enable the service and the timer

```
sudo systemctl enable battery-saver.service
sudo systemctl enable battery-saver.timer
```

Finally start the timer

```
sudo systemctl start battery-saver.timer
```

## Turning off the switch when halting the system.

To ensure that the laptop is not charging the battery after shutting down the
system I built a service file that switches off the current when that happens.
To install it copy the systemd service file to systemd's folder.

```
sudo cp systemd/battery-saver-halt.service /etc/systemd/system
```

And now enable and start it.

```
sudo systemctl enable battery-saver-halt
sudo systemctl start battery-saver-halt
```
