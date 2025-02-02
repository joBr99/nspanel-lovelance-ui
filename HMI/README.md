# NSPanel Lovelace UI

The HMI Project of this project is only used to display stuff, navigation ist mostly up to the backend. This allows to be way more flexible.

Messages to the Panel can be send through the Command `CustomSend`, which is implemented in the berry driver.
You can issue this command through MQTT by sending messages to the `cmnd/XXX/CustomSend` Topic.
Messages from the Panel are send to the `tele/XXX/RESULT` Topic, encoded in json `{"CustomRecv":"message_from_screen"}`


# Table of contents

  - [Startup](#startup)
    - [Some preperation before we are acually navigating away:](#some-preperation-before-we-are-acually-navigating-away)
    - [Navigate from the startup page to the screensaver, by sending this command to the CustomSend Topic.](#navigate-from-the-startup-page-to-the-screensaver-by-sending-this-command-to-the-customsend-topic)
    - [Exit Screensaver](#exit-screensaver)
  - [Messages to Nextion Display](#messages-to-nextion-display)
    - [General Commands, implemented on all pages](#general-commands-implemented-on-all-pages)
    - [screensaver page](#screensaver-page)
    - [cardEntities Page](#cardentities-page)
    - [cardGrid Page](#cardgrid-page)
    - [cardMedia](#cardmedia)
    - [cardThermo](#cardthermo)
    - [cardAlarm](#cardalarm)
    - [cardQR](#cardqr)
    - [cardPower](#cardpower)

## Startup

On startup the panel will send `{"CustomRecv":"event,startup,39,eu"}` every few seconds.

```
event,   #Every message from the screen will start with `event`
startup, #Startup Event
39,      #Current HMI Project Version
eu       #Current HMI Project Model
```

You can answer this message in many different ways, but in general the goal is to navigate way from the startup page. In the following example we will navigate to the screensaver page.

Send the following messages to the CustomSend Topic. (You can also send them on tasmota console for testing)

### Some preperation before we are acually navigating away:

Send this every minute: `time~18:17`

Send this at least once at midnight: `date~Donnerstag, 25. August 2022`

Send theese message once after receiving the startup event (parameters will be explained later):

`timeout~20`

`dimmode~10~100~6371`

### Navigate from the startup page to the screensaver, by sending this command to the CustomSend Topic.

`pageType~screensaver`

After sending this command you should already see the time and date.
To also show weather data you have to send them with weatherUpdate, but we will skip this for now.

### Exit Screensaver

Touching the panel on the screensaver will result in this MQTT Message on the result topic:

`event,buttonPress2,screensaver,bExit,1`

You can answer this by sending theese commands to the CustomSend Topic.

`pageType~cardEntities`

`entityUpd~test~~button~navigate.prev~<~65535~~~button~navigate.next~>~65535~~~~light~light.schreibtischlampe~X~17299~Schreibtischlampe~0~text~sensor.server_energy_power~Y~17299~Server ENERGY Power~155 W~shutter~cover.rolladenfenster_cover_1~Z~17299~Fenster Eingang~A|B|C|disable|enable|enable~switch~switch.bad~D~63142~Bad~1`

## Messages to Nextion Display

### General Commands, implemented on all pages

set brightness of screensaver and active-brightness:

`dimmode~0~100 - (screen off)`

`dimmode~100~100 - (screen on with full brightness)`

set current time:

`time~22:26`

set current date:

`date~Di 24. Februar`

set screensaver timeout (set time in sec~ max 65):

`timeout~15 - timeout after 15 seconds`

`timeout~0 - disable screensaver`

change the page type:

`pageType~pageStartup`

`pageType~cardEntities`

`pageType~cardThermo`

`pageType~cardMedia`

`pageType~popupLight~Schreibtischlampe~light.schreibtischlampe`

`pageType~popupNotify`

`pageType~screensaver`
### Icon
Go to https://docs.nspanel.pky.eu/icon-cheatsheet.html
Click on the little (U)-Symbol right beside the icon and paste the code in the field for icon.
![image](https://github.com/joke24/nspanel-lovelace-ui/assets/19669309/04a6cdff-54b3-4eea-84b2-1cb89dc4983d)

### Icon Color 
For icons RGB565 color is used. Use color picker from e.g. https://rgbcolorpicker.com/565 and paste the value in the field for icon color.

### screensaver page

<table>
<thead>
  <tr>
    <th>Parameter Number</th>
    <th>Category</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td>instruction</td>
    <td></td>
    <td>instruction</td>
    <td>weatherupdate</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td></td>
    <td rowspan="6">Main Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>2</td>
    <td></td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>3</td>
    <td></td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>4</td>
    <td></td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td></td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>6</td>
    <td></td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>7</td>
    <td></td>
    <td rowspan="6">First Forecast Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td></td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>9</td>
    <td></td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>10</td>
    <td></td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td></td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td></td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>13</td>
    <td></td>
    <td rowspan="6">Second Forecast Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td></td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>15</td>
    <td></td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>16</td>
    <td></td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>17</td>
    <td></td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td></td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>19</td>
    <td></td>
    <td rowspan="6">Third Forecast Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>20</td>
    <td></td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>21</td>
    <td></td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>22</td>
    <td></td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>23</td>
    <td></td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>24</td>
    <td></td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>25</td>
    <td></td>
    <td rowspan="6">Fourth Forecast Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>26</td>
    <td></td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>27</td>
    <td></td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>28</td>
    <td></td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>29</td>
    <td></td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>30</td>
    <td></td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>31</td>
    <td></td>
    <td rowspan="6">Alternative Layout Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>32</td>
    <td></td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>33</td>
    <td></td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>34</td>
    <td></td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>35</td>
    <td></td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>36</td>
    <td></td>
    <td>optionalValue</td>
    <td></td>
  </tr>
</tbody>
</table>

`color~background~tTime~timeAMPM~tDate~tMainText~tForecast1~tForecast2~tForecast3~tForecast4~tForecast1Val~tForecast2Val~tForecast3Val~tForecast4Val~bar~tMainTextAlt2~tTimeAdd`

<table>
<thead>
  <tr>
    <th>Parameter Number</th>
    <th>Category</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td colspan="3">instruction</td>
    <td>color</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td></td>
    <td></td>
    <td></td>
    <td>background</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tTime</td>
    <td></td>
  </tr>
  <tr>
    <td>3</td>
    <td></td>
    <td></td>
    <td></td>
    <td>timeAMPM</td>
    <td></td>
  </tr>
  <tr>
    <td>4</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tDate</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tMainText</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast1</td>
    <td></td>
  </tr>
  <tr>
    <td>7</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast2</td>
    <td></td>
  </tr>
  <tr>
    <td>8</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast3</td>
    <td></td>
  </tr>
  <tr>
    <td>9</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast4</td>
    <td></td>
  </tr>
  <tr>
    <td>10</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast1Val</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast2Val</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast3Val</td>
    <td></td>
  </tr>
  <tr>
    <td>13</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tForecast4Val</td>
    <td></td>
  </tr>
  <tr>
    <td>14</td>
    <td></td>
    <td></td>
    <td></td>
    <td>bar</td>
    <td></td>
  </tr>
  <tr>
    <td>15</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tMainTextAlt2</td>
    <td></td>
  </tr>
  <tr>
    <td>16</td>
    <td></td>
    <td></td>
    <td></td>
    <td>tTimeAdd</td>
    <td></td>
  </tr>
</tbody>
</table>

`notify~heading~text`

`statusUpdate~icon1~icon1Color~icon2~icon2~icon2color~icon1font~icon2font`

### cardEntities Page

Structure (Category): `entityUpd~title~[navigation]~[entity_information]`
Example with 4 Entities: 
```
entityUpd~LightTest~button~navigate.prev~<~65535~~~button~navigate.next~>~65535~~~light~light.bed_light~A~17299~Bed Light~0~light~light.ceiling_lights~B~52231~Ceiling Lights~1~switch~switch.ac~C~17299~AC~0~switch~switch.decorative_lights~D~65222~Decorative Lights~1
```

Possible entities on cardEntities/cardGrid:

`~light~light.entityName~1~17299~Light1~0`

`~shutter~cover.entityName~0~17299~Shutter2~iconUp|iconStop|iconDown`

`~delete~~~~~`

`~text~sensor.entityName~3~17299~Temperature~content`

`~button~button.entityName~3~17299~bt-name~bt-text`

`~switch~switch.entityName~4~17299~Switch1~0`

`~number~input_number.entityName~4~17299~Number123~value|min|max`

`~input_sel~input_select.entityName~3~17299~sel-name~sel-text`


<table>
<thead>
  <tr>
    <th>Number</th>
    <th>Category</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td>instruction</td>
    <td></td>
    <td>instruction</td>
    <td>entityUpd</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td rowspan="12">Navigation</td>
    <td rowspan="6">Upper Left Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>3</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>4</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>7</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td rowspan="6">Upper Right Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>9</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>10</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>13</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td rowspan="36">Entities</td>
    <td rowspan="6">First Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>15</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>16</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>17</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>19</td>
    <td colspan="2">optionalValue</td>
  </tr>
  <tr>
    <td>20</td>
    <td rowspan="6">Second Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>21</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>22</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>23</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>24</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>25</td>
    <td colspan="2">optionalValue</td>
  </tr>
  <tr>
    <td>26</td>
    <td rowspan="6">Thrid Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>27</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>28</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>29</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>30</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>31</td>
    <td colspan="2">optionalValue</td>
  </tr>
  <tr>
    <td>32</td>
    <td rowspan="6">Forth Entiry</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>33</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>34</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>35</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>36</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>37</td>
    <td colspan="2">optionalValue</td>
  </tr>
  <tr>
    <td>38</td>
    <td rowspan="6">Fifth Entiy (US Portrait&nbsp;&nbsp;&nbsp;Version)</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>39</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>40</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>41</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>42</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>43</td>
    <td colspan="2">optionalValue</td>
  </tr>
  <tr>
    <td>44</td>
    <td rowspan="6">Sixth Entiy (US Portrait&nbsp;&nbsp;&nbsp;Version)</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>45</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>46</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>47</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>48</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>49</td>
    <td colspan="2">optionalValue</td>
  </tr>
</tbody>
</table>

### cardGrid Page

cardGrid is using the exact same messageformat like cardEntities does. The only difference is, it ignores the information supplied in optionalValue, because it isn't needed for cardGrid.

<table>
<thead>
  <tr>
    <th>Parameter&nbsp;&nbsp;&nbsp;Number</th>
    <th>Category</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td>instruction</td>
    <td></td>
    <td>instruction</td>
    <td>entityUpd</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td rowspan="12">Navigation</td>
    <td rowspan="6">Upper Left Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>3</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>4</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>7</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td rowspan="6">Upper Right Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>9</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>10</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>13</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td rowspan="36">Entities</td>
    <td rowspan="6">First Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>15</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>16</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>17</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>19</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>20</td>
    <td rowspan="6">Second Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>21</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>22</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>23</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>24</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>25</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>26</td>
    <td rowspan="6">Thrid Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>27</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>28</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>29</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>30</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>31</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>32</td>
    <td rowspan="6">Forth Entiry</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>33</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>34</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>35</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>36</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>37</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>38</td>
    <td rowspan="6">Fifth Entiy (US Portrait&nbsp;&nbsp;&nbsp;Version)</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>39</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>40</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>41</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>42</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>43</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>44</td>
    <td rowspan="6">Sixth Entiy (US Portrait&nbsp;&nbsp;&nbsp;Version)</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>45</td>
    <td colspan="2">intNameEntity</td>
  </tr>
  <tr>
    <td>46</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>47</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>48</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>49</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
</tbody>
</table>

### cardMedia

Example without icons in bottom row: `entityUpd~Kitchen~button~navigation.up~U~65535~~~delete~~~~~~media_player.kitchen~I'm a Hurricane~~Wellmess~~100~A~64704~B~media_pl~media_player.kitchen~C~17299~Kitchen~`

<table>
<thead>
  <tr>
    <th>Parameter&nbsp;&nbsp;&nbsp;Number</th>
    <th>Category</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td>instruction</td>
    <td></td>
    <td>instruction</td>
    <td>entityUpd</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td rowspan="12">Navigation</td>
    <td rowspan="6">Upper Left Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>3</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>4</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>7</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td rowspan="6">Upper Right Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>9</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>10</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>13</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td rowspan="9">cardMedia specific</td>
    <td></td>
    <td rowspan="9">cardMedia specific</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>15</td>
    <td rowspan="2">1st text row</td>
    <td>title</td>
    <td></td>
  </tr>
  <tr>
    <td>16</td>
    <td>titleColor</td>
    <td></td>
  </tr>
  <tr>
    <td>17</td>
    <td rowspan="2">2nd text row</td>
    <td>author</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td>authorColor</td>
    <td></td>
  </tr>
  <tr>
    <td>19</td>
    <td>slider</td>
    <td>volume</td>
    <td>0-100</td>
  </tr>
  <tr>
    <td>20</td>
    <td>icon middle</td>
    <td>playPauseIcon</td>
    <td></td>
  </tr>
  <tr>
    <td>21</td>
    <td>icon right side</td>
    <td>onOffBtn</td>
    <td>"disable" or color</td>
  </tr>
  <tr>
    <td>22</td>
    <td>icon left side</td>
    <td>iconShuffle</td>
    <td>"disable" or icon</td>
  </tr>
  <tr>
    <td>23</td>
    <td rowspan="36">Entities</td>
    <td rowspan="6">upper left corner media&nbsp;&nbsp;&nbsp;icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>24</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>25</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>26</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>27</td>
    <td>displayName</td>
    <td>only used for popups</td>
  </tr>
  <tr>
    <td>28</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>29</td>
    <td rowspan="6">First Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>30</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>31</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>32</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>33</td>
    <td>displayName</td>
    <td>only used for popups</td>
  </tr>
  <tr>
    <td>34</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>35</td>
    <td rowspan="6">Second Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>36</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>37</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>38</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>39</td>
    <td>displayName</td>
    <td>only used for popups</td>
  </tr>
  <tr>
    <td>40</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>41</td>
    <td rowspan="6">Thrid Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>42</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>43</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>44</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>45</td>
    <td>displayName</td>
    <td>only used for popups</td>
  </tr>
  <tr>
    <td>46</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>47</td>
    <td rowspan="6">Forth Entiry</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>48</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>49</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>50</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>51</td>
    <td>displayName</td>
    <td>only used for popups</td>
  </tr>
  <tr>
    <td>52</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>53</td>
    <td rowspan="6">Fifth Entiy</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>54</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>55</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>56</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>57</td>
    <td>displayName</td>
    <td>only used for popups</td>
  </tr>
  <tr>
    <td>58</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
</tbody>
</table>

### cardThermo

Serial Protocol of cardThermo is about to change; table will be completed later

<table>
<thead>
  <tr>
    <th>Parameter Number</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td></td>
    <td>instruction</td>
    <td>entityUpd</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td rowspan="6">Upper Left Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>3</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>4</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>7</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td rowspan="6">Upper Right Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>9</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>10</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>13</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td></td>
    <td></td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>15</td>
    <td colspan="2">2nd text box</td>
    <td>currentTemp</td>
    <td></td>
  </tr>
  <tr>
    <td>16</td>
    <td colspan="2">target temperature</td>
    <td>dstTemp</td>
    <td>multiplied by 10</td>
  </tr>
  <tr>
    <td>17</td>
    <td colspan="2">Text 4th Box Left Side</td>
    <td>status</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td colspan="2">Min Temp</td>
    <td>minTemp</td>
    <td>multiplied by 10</td>
  </tr>
  <tr>
    <td>19</td>
    <td colspan="2">Max Temp</td>
    <td>maxTemp</td>
    <td>multiplied by 10</td>
  </tr>
  <tr>
    <td>20</td>
    <td colspan="2">Temperature Steps</td>
    <td>tempStep</td>
    <td>multiplied by 10</td>
  </tr>
  <tr>
    <td>21</td>
    <td rowspan="4">bottom hvac_action 1</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>22</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>23</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>24</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>25</td>
    <td rowspan="4">bottom hvac_action 2</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>26</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>27</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>28</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>29</td>
    <td rowspan="4">bottom hvac_action 3</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>30</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>31</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>32</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>33</td>
    <td rowspan="4">bottom hvac_action 4</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>34</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>35</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>36</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>37</td>
    <td rowspan="4">bottom hvac_action 5</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>38</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>39</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>40</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>41</td>
    <td rowspan="4">bottom hvac_action 6</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>42</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>43</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>44</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>45</td>
    <td rowspan="4">bottom hvac_action 7</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>46</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>47</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>48</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>49</td>
    <td rowspan="4">bottom hvac_action 8</td>
    <td rowspan="4">Hvac Action</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>50</td>
    <td>iconColorActive</td>
    <td></td>
  </tr>
  <tr>
    <td>51</td>
    <td>buttonState</td>
    <td></td>
  </tr>
  <tr>
    <td>52</td>
    <td>intName</td>
    <td></td>
  </tr>
  <tr>
    <td>53</td>
    <td colspan="2">Currently Label 1th Text Box</td>
    <td>tCurTempLbl</td>
    <td></td>
  </tr>
  <tr>
    <td>54</td>
    <td colspan="2">State Label 3th Text Box</td>
    <td>tStateLbl</td>
    <td></td>
  </tr>
  <tr>
    <td>55</td>
    <td></td>
    <td></td>
    <td>tALbl</td>
    <td>deprecated; ignored</td>
  </tr>
  <tr>
    <td>56</td>
    <td colspan="2">Temperature Unit (Celcius/Farhenheit)</td>
    <td>tCF</td>
    <td></td>
  </tr>
  <tr>
    <td>57</td>
    <td>Second Destination Tempature (Heat/Cool)</td>
    <td></td>
    <td>second temp</td>
    <td>; multiplied by 10</td>
  </tr>
  <tr>
    <td>58</td>
    <td colspan="2">additonal detail button to open another page</td>
    <td>btDetail</td>
    <td>"1" to hide</td>
  </tr>
</tbody>
</table>

### cardAlarm

<table>
<thead>
  <tr>
    <th>Parameter Number</th>
    <th>Category</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td>instruction</td>
    <td></td>
    <td>instruction</td>
    <td>entityUpd</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td rowspan="12">Navigation</td>
    <td rowspan="6">Upper Left Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>3</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>4</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>7</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td rowspan="6">Upper Right Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>9</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>10</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>13</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td rowspan="16">cardAlarm specific</td>
    <td colspan="2">card intNameEntity</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>15</td>
    <td colspan="2" rowspan="2">1st button right side</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>16</td>
    <td>intId</td>
    <td></td>
  </tr>
  <tr>
    <td>17</td>
    <td colspan="2" rowspan="2">2nd button right side</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td>intId</td>
    <td></td>
  </tr>
  <tr>
    <td>19</td>
    <td colspan="2" rowspan="2">3rd button right side</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>20</td>
    <td>intId</td>
    <td></td>
  </tr>
  <tr>
    <td>21</td>
    <td colspan="2" rowspan="2">4th button right side</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>22</td>
    <td>intId</td>
    <td></td>
  </tr>
  <tr>
    <td>23</td>
    <td colspan="2" rowspan="2">icon next to code display</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>24</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>25</td>
    <td colspan="2">numpad</td>
    <td>numpadStatus</td>
    <td>"disable" or "enable"</td>
  </tr>
  <tr>
    <td>26</td>
    <td colspan="2">flashing of icon next to code</td>
    <td>flashing status</td>
    <td>"enable" or "disable"</td>
  </tr>
  <tr>
    <td>27</td>
    <td colspan="2" rowspan="3">button bottom left corner</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>28</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>29</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
</tbody>
</table>


### cardQR

Example: `entityUpd~Guest Wifi~button~navigate.prev~<~65535~~~button~navigate.next~>~65535~~~WIFI:S:test_ssid;T:WPA;P:test_pw;;~text~iText.test_ssid~���~17299~Name~test_ssid~text~iText.test_pw~���~17299~Password~test_pw`

<table>
<thead>
  <tr>
    <th>Parameter&nbsp;&nbsp;&nbsp;Number</th>
    <th>Category</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td>instruction</td>
    <td></td>
    <td>instruction</td>
    <td>entityUpd</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td rowspan="12">Navigation</td>
    <td rowspan="6">Upper Left Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>3</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>4</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>7</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td rowspan="6">Upper Right Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>9</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>10</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>13</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td colspan="3">cardQR specific</td>
    <td>qrcode text</td>
    <td></td>
  </tr>
  <tr>
    <td>15</td>
    <td rowspan="12">Entities</td>
    <td rowspan="6">1st Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>16</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>17</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>19</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>20</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>21</td>
    <td rowspan="6">2nd Entity</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td></td>
  </tr>
  <tr>
    <td>22</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>23</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>24</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>25</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>26</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
</tbody>
</table>


### cardPower

```entityUpd~PowerTest~x~navUp~A~65535~~~delete~~~~~~text~sensor.power_consumption~B~17299~Power consumption~100W~1~text~sensor.power_consumption~C~17299~Power consumption~100W~1~text~sensor.today_energy~D~17299~Total energy 1~5836.0kWh~0~delete~~~~~~0~text~sensor.today_energy~E~17299~Total energy 1~5836.0kWh~-30~delete~~~~~~0~text~sensor.today_energy~F~65504~Total energy 1~5836.0kWh~90~text~sensor.today_energy~G~17299~Total energy 1~5836.0kWh~10```

<table>
<thead>
  <tr>
    <th>Parameter Number</th>
    <th>Location</th>
    <th>Type</th>
    <th>Field</th>
    <th>Addional Information</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>0</td>
    <td></td>
    <td>instruction</td>
    <td>entityUpd</td>
    <td></td>
  </tr>
  <tr>
    <td>1</td>
    <td>title</td>
    <td>title</td>
    <td>title</td>
    <td></td>
  </tr>
  <tr>
    <td>2</td>
    <td rowspan="6">Upper Left Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>3</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>4</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>5</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>6</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>7</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>8</td>
    <td rowspan="6">Upper Right Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>(ignored)¹</td>
  </tr>
  <tr>
    <td>9</td>
    <td>intNameEntity</td>
    <td></td>
  </tr>
  <tr>
    <td>10</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>11</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>12</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>13</td>
    <td>optionalValue</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>14</td>
    <td rowspan="7">Home Icon / Value below Home Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>15</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>16</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>17</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>18</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>19</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>20</td>
    <td colspan="2">speed</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>21</td>
    <td rowspan="7">Value above Home Icon</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>22</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>23</td>
    <td>icon</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>24</td>
    <td>iconColor</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>25</td>
    <td>displayName</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>26</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>27</td>
    <td colspan="2">speed</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>28</td>
    <td rowspan="7">1st Item Upper Left</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>29</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>30</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>31</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>32</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>33</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>34</td>
    <td colspan="2">speed</td>
    <td>numbers (between -120 and 120)</td>
  </tr>
  <tr>
    <td>35</td>
    <td rowspan="7">2nd Item Middle Left</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>36</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>37</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>38</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>39</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>40</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>41</td>
    <td colspan="2">speed</td>
    <td>numbers (between -120 and 120)</td>
  </tr>
  <tr>
    <td>42</td>
    <td rowspan="7">3rd Item Bottom Left</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>43</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>44</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>45</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>46</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>47</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>48</td>
    <td colspan="2">speed</td>
    <td>numbers (between -120 and 120)</td>
  </tr>
  <tr>
    <td>49</td>
    <td rowspan="7">4th Item Upper Right</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>50</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>51</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>52</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>53</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>54</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>55</td>
    <td colspan="2">speed</td>
    <td>numbers (between -120 and 120)</td>
  </tr>
  <tr>
    <td>56</td>
    <td rowspan="7">5th Item Middle Right</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>57</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>58</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>59</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>60</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>61</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>62</td>
    <td colspan="2">speed</td>
    <td>numbers (between -120 and 120)</td>
  </tr>
  <tr>
    <td>63</td>
    <td rowspan="7">6th Item Bottom Right</td>
    <td rowspan="6">Entity Definition</td>
    <td>type</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>64</td>
    <td>intNameEntity</td>
    <td>ignored</td>
  </tr>
  <tr>
    <td>65</td>
    <td>icon</td>
    <td></td>
  </tr>
  <tr>
    <td>66</td>
    <td>iconColor</td>
    <td></td>
  </tr>
  <tr>
    <td>67</td>
    <td>displayName</td>
    <td></td>
  </tr>
  <tr>
    <td>68</td>
    <td>optionalValue</td>
    <td></td>
  </tr>
  <tr>
    <td>69</td>
    <td colspan="2">speed</td>
    <td>numbers (between -120 and 120)</td>
  </tr>
</tbody>
</table>

### cardChart Page
`entityUpd~heading~navigation~color~yAxisLabel~yAxisTick:[yAxisTick]*[~value[:xAxisLabel]?]*`  

`entityUpd~Chart Demo~~button~navigate.prev~<~65535~~~button~navigate.next~>~65535~~~~6666~Gas [kWh]~20:40:60:80:100~10~7^2:00~7~6^4:00~6~7^6:00~0~7^8:00~5~1^10:00~1~10^12:00~5~6^14:00~8`


### popupLight Page

`entityUpdateDetail~entityName~*ignored*~*iconColor*~*buttonState*~*sliderBrightnessPos*~*sliderColorTempPos*~*colorMode*~*color_translation*~*color_temp_translation*~*brightness_translation*`

`entityUpdateDetail~1~17299~1~100~78~enable`

`entityUpdateDetail~1~17299~1~100~disable`

### popupShutter Page

`entityUpdateDetail~entityName~*sliderPos*~2ndrow~textPosition~icon1~iconUp~iconStop~iconDown~iconUpStatus~iconStopStatus~iconDownStatus~textTilt~iconTiltLeft~iconTiltStop~iconTiltRight~iconTiltLeftStatus~iconTiltStopStatus~iconTiltLeftStatus~tiltPos`

`entityUpdateDetail~1~77`

### popupNotify Page

`entityUpdateDetail~*internalName*~*tHeading*~*tHeadingColor*~*b1*~*tB1Color*~*b2*~*tB2Color*~*tText*~*tTextColor*~*sleepTimeout*~*font*~*alt_icon*~*altIconColor*`

`exitPopup`

### popupThermo Page

`entityUpdateDetail~{entity_id}~{icon_id}~{icon_color}~{heading}~{mode}~mode1~mode1?mode2?mode3~{heading}~{mode}~mode1~mode1?mode2?mode3~{heading}~{mode}~mode1~mode1?mode2?mode3~`

### popupInSel Page (input_select detail page)

`entityUpdateDetail2~*entity_id*~~*icon_color*~*input_sel*~*state*~*options*`

options are ? seperated

### popupTimer

editable is 0 or 1

action fields are in the answer on the button press

in case action is empty the button will be hidden

`entityUpdateDetail~{entity_id}~~{icon_color}~{entity_id}~{min_remaining}~{sec_remaining}~{editable}~{action1}~{action2}~{action3}~{label1}~{label2}~{label3}`





## Messages from Nextion Display

`event,buttonPress2,pageName,bNext`

`event,buttonPress2,pageName,bPrev`

`event,buttonPress2,pageName,bExit,number_of_taps`

`event,buttonPress2,pageName,sleepReached`


### startup page

`event,startup,version,model`

### screensaver page

`event,buttonPress2,screensaver,exit` - Touch Event on Screensaver

`event,screensaverOpen` - Screensaver has opened


### cardEntities Page

`event,*eventName*,*entityName*,*actionName*,*optionalValue*`

`event,buttonPress2,internalNameEntity,up`

`event,buttonPress2,internalNameEntity,down`

`event,buttonPress2,internalNameEntity,stop`

`event,buttonPress2,internalNameEntity,OnOff,1`

`event,buttonPress2,internalNameEntity,button`

### popupLight Page

`event,pageOpenDetail,popupLight,internalNameEntity`

`event,buttonPress2,internalNameEntity,OnOff,1`

`event,buttonPress2,internalNameEntity,brightnessSlider,50`

`event,buttonPress2,internalNameEntity,colorTempSlider,50`

`event,buttonPress2,internalNameEntity,colorWheel,x|y|wh`

### popupShutter Page

`event,pageOpenDetail,popupShutter,internalNameEntity`

`event,buttonPress2,internalNameEntity,positionSlider,50`

### popupNotify Page

`event,buttonPress2,*internalName*,notifyAction,yes`

`event,buttonPress2,*internalName*,notifyAction,no`

### cardThermo Page

`event,buttonPress2,*entityName*,tempUpd,*temperature*`

`event,buttonPress2,*entityName*,hvac_action,*hvac_action*`

### cardMedia Page

`event,buttonPress2,internalNameEntity,media-back`

`event,buttonPress2,internalNameEntity,media-pause`

`event,buttonPress2,internalNameEntity,media-next`

`event,buttonPress2,internalNameEntity,volumeSlider,75`

### cardAlarm Page

`event,buttonPress2,internalNameEntity,actionName,code`


# Custom Protocol

```
55 BB [payload length] [payload length] [payload] [crc] [crc]
```

Payload length contains the number of bytes of the payload.

CRC is "CRC-16 (MODBUS) Big Endian" calculated over the whole message

This protocol does not try to implement broken JSON Commands with a specified type (lol).
Instead the commands are plain text commands with parameters.

## Example for valid Message

This message has to be generated for the Message "1337" (1337 is not a valid command~ this is just an example)

```
55 BB  04 00  31 33 33 37  5F 5B
```
