Weather Station is a plugin for both users and developers, to display and share weather data. Run it from the plugin tab in the right click menu, search your location, and it will update the weather every hour and send the data to all open ghosts. It will also show the current weather/forecast data in the plugin menu. Developers are welcome to make use of the weather data in their ghosts, and even bundle this plugin with their ghosts if they would like.

Ghosts cannot see your location data unless you explicitly turn this setting on in the settings menu. Therefore, many ghosts can have weather information without you needing to trust each one.

Please note: I have had trouble running this plugin on my Windows 7 computer, but someone else tried it on Windows 7 and it worked fine. If it will not show any results when you search your location, please get in contact! I would love to try and figure out why this happens and if I can fix it.


Weather Station v1.0.1
Made by Zichqec https://zichqec.github.io/s-the-skeleton/
Using Weather API https://www.weatherapi.com/

—————————— Developer Info ——————————

Information on the references for each event can be found on the external SHIORI page of Ukadoc ( http://ssp.shillest.net/ukadoc/manual/list_shiori_event_ex.html ), or in events_reference.txt. 

After the user has entered their location, this plugin will send the event OnWeatherStation.Weather every hour on the hour, every time a ghost boots, and any time a ghost calls it with raiseplugin. Note that it is always sent to all open ghosts, no matter how it is called. I do not recommend putting dialogue in here! Just use it to update variables in your ghost.

OnWeatherStation.Weather contains all the data for the current weather at the user's location, as well as the chance of rain/snow for the day. If you desire additional data besides this, such as forecast data, weather alerts, and astronomy data, you will need to call those events specifically using the raiseplugin command. For example, you can use \![raiseplugin,Weather Station,OnWeatherStation.Astro,0] to get the astronomy data for the current day.

When calling the OnWeatherStation.Astro, OnWeatherStation.Forecast.Day, and OnWeatherStation.Forecast.Hourly events, you will have to send an additional argument to specify which day's data you want. 0 is for the current day, 1 is for tomorrow, and 2 is for the day after tomorrow. If you do not specify, it will default to the current day.

The hourly forecast data is sent as 24 arrays, one for each hour. The reference number matches the hour the array goes with, in 24 hour time. So reference0 is 00:00/12AM, reference 14 is 14:00/2PM, etc.

The weather alert data is also sent as a series of arrays, though the amount changes depending on how many alerts are available. Fields of the arrays are separated by a 1 byte value, since the descriptions of the weather alerts often contain commas. (Note: I'm not sure if 1 byte value is a good name for this. It's what comes up when running Ukadoc through machine translation. If you're using AYA/YAYA, this value can be written with %(CHR(1)). ) In YAYA, you can separate these values by using a command such as SPLIT(reference.raw[0],C_BYTE1). Note the reference.raw there instead of a normal reference0. This is because if you use a normal reference and you haven't turned off auto type convert, it will automatically change all 1 byte values to commas. Unfortunately, if you're using AYA, you're a bit out of luck with this one since AYA does not have reference.raw and does not have the option to turn off auto type convert as far as I know. Sorry!

You may want to have your ghost check if the plugin is installed before attempting to get the weather data. If you're on AYA/YAYA, here's some code you can use to do so:

YAYA version:

On_installedplugin
{
    pluginlist = IARRAY
    foreach reference; _ref
    {
        pluginlist ,= _ref[0]
    }
}

AYA version:

//Modified version of the code that makes ghostexlist
On_installedplugin
{
    pluginlist = IARRAY
    
    _i = 0
    while _i >= 0
    {
        _ghostname = NAMETOVALUE("reference%_i")
        if _ghostname != ""
        {
            pluginlist ,= _ghostname[0]
            _i++
        }
        else; _i = -1
    }
}

Whichever version you use, you'll also need this:

WeatherStationInstalled
{
    if ASEARCH("Weather Station",pluginlist) != -1; 1
    else; 0
}

And then you can write your checks as:  if WeatherStationInstalled == 1


If there is any sort of error, such as the location not being valid or the API not responding, the event OnWeatherStation.Error will be sent instead of any of the other events. You can use this to clear your weather related variables so that your ghost doesn't continue to give out of date weather data.

A full list of events sent by the plugin, as well as the references associated with them, is available in events_reference.txt.

Please note: I only get 1 million free API calls a month. If you're a developer and that limit worries you, please check the note near the top of main.dic about it. My code uses weatherapi.com to get the weather, and you are welcome to use anything you find in main.dic and envelopes.dic! I would appreciate a link back to me if you use my code. You are also welcome to use my moon icons to display the moon phase, if you'd like.



—————————— Version History ——————————


—v1.0.1—

• Fixed a bug where if a weather alert contained an apostrophe, it would break it and make ghosts read out part of the weather alert when loaded in.


—v1.0.0—

• Initial release