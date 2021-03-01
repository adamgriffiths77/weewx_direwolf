# weewx_direwolf
Code added to get weewx to talk to direwolf APRS client using kiss to send weather reports via APRS using RF as the interface

This directory contains the files used on my raspberry pi (ws-2) for APRS weatherstations.

# Hardware/Software Configuration
- Raspberry pi 4
- RTL-SDR dongle to pull weatherstations via the use of RTL_433 decoder
- weewx weatherstation software (starts rtl_433 and is filtered for my external WS-1080 weatherstation)
- WS-1080 based weatherstation (cheap but works)
- direwolf software TNC configured to automatically start via a cronjob refering the dw-start.sh script
- cheap USB audio card with custom cable to my radio
- direwolf uses a Wouxon 2m Amateur handheld radio as the RF interfaace, using a usb audio dongle and a relay connected to GPIO to trigger the PTT.

# Weatherstation Operation
1. weewx is configured to obtain data for the WS-1080 weather station using the RTLSDR option.
2. weex outputs the weather data ion APRS format to a text file every 30 mins.  This is a modified version of the APRS client, using learning from the CWOP and YAAC code
3. KissTNC is configured to watch the output directory of the text file and converts any file in that directory to a properly formatted TNC data string and passes it on to direwolf TNC
4. Direwolf transmits the APRS message via the 2M hand-held radio
5. APRS message received by any iGate within radio range.

# modifications from out-of-the-box weewx and direwolf

## direwolf 
-direwolf: the only modification is the cron script to add starting kisstnc.
-direwolf is started with the following cron "* * * * * /root/dw-start.sh >/dev/null 2>&1"
-using the file in the direwolf directory
-others can read the file and figure out where callsign data and PTT GPIO are used and how to wire up a radio so I'm not going to describe that part here (unless I have to rebuild the whole thing and figure out out again...I haven't done the physical part for good 10 years).

## weewx
This directory contains the files that are modified for Weewx to correctly obtain data from the weatherstation using RTL_433 and also export the data into the listed text files.

### Weewx.conf
- see the section of the configuration file under SDR for hte RTL_433 configuration and mapping of sensors.  note the sensor name (22).  I found that I could pick up quite a few devices transmitting data via 433Mhz so sometimes you'll need to run RTL_433 seperately to identify the specific weatherstation that you need data from.  NOTE: this doesn't seem the change once the weatherstation is turned on but I do check this every time I change the batteries just to make sure
- see the section at the bottom of the configuration file for the APRS configuration which specifies the output directory and comments which is appended to the APRS packet for station description (helps people if they need to contact me if my station is doing something weird) 


### /user/aprs.py

This is a modified version of the aprs driver that is provided with weewx.  The included driver assumes that the APRS message is being sent to a full external TNC (which provides the callsign and path data) hence this driver is modified to including the callsign, destination and path info.  It is also modified (at teh bottom) to output the completed APRS packet to the output file specified in weewx.conf rather than a serial port.  This is the file which is then picked up by direwolf to transmit the packet.
NOTE: I have noticed that there's a couple of hardcoded variables (ie callsign and path), which I should move to a variable when I get a chance. 5 min job but not important give it works pretty much perfectly.
