# weewx_direwolf
Code added to get weewx to talk to direwolf APRS client using kiss to send weather reports via APRS using RF as the interface

This directory contains the files used on my raspberry pi (ws-2) for APRS weatherstations.

# Hardware/Software Configuration
-Raspberry pi 4
-RTL-SDR dongle to pull weatherstations via the use of RTL_433 decoder
-weewx weatherstation software (starts rtl_433 and is filtered for my external WS-1080 weatherstation)
-WS-1080 based weatherstation (cheap but works)
-direwolf software TNC configured to automatically start via a cronjob refering the dw-start.sh script
-direwolf uses a Wouxon 2m Amateur handheld radio as the RF interfaace, using a usb audio dongle and a relay connected to GPIO to trigger the PTT.

# Weatherstation operation
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

weewx
