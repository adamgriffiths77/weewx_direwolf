# Description

This directory contains the files that are modified for Weewx to correctly obtain data from the weatherstation using RTL_433 and also export the data into the listed text files.

## Weewx.conf
- see the section of the configuration file under SDR for hte RTL_433 configuration and mapping of sensors.  note the sensor name (22).  I found that I could pick up quite a few devices transmitting data via 433Mhz so sometimes you'll need to run RTL_433 seperately to identify the specific weatherstation that you need data from.  NOTE: this doesn't seem the change once the weatherstation is turned on but I do check this every time I change the batteries just to make sure
- see the section at the bottom of the configuration file for the APRS configuration which specifies the output directory and comments which is appended to the APRS packet for station description (helps people if they need to contact me if my station is doing something weird) 


# /user/aprs.py

This is a modified version of the aprs driver that is provided with weewx.  The included driver assumes that the APRS message is being sent to a full external TNC (which provides the callsign and path data) hence this driver is modified to including the callsign, destination and path info.  It is also modified (at teh bottom) to output the completed APRS packet to the output file specified in weewx.conf rather than a serial port.  This is the file which is then picked up by direwolf to transmit the packet.
NOTE: I have noticed that there's a couple of hardcoded variables (ie callsign and path), which I should move to a variable when I get a chance. 5 min job but not important give it works pretty much perfectly.
