# Measurements

## Test Setup
<a href="url"><img src="GL-inet-test-setup.jpg" height="768" width="576" ></a>

## Scenarios

### Instantaneous Power Draw
|Activity|Uplink|Draw 4.89V|
|:------:|:------:|:--------:|
|File Transfer|Ethernet|350 mA|
|File Transfer|4G|650 mA|

### Idle
|Activity|Ethernet|WiFi|4G|lighthttpd|Consumption 4.89V|
|:------:|:------:|:--:|:-:|:-:|:--------:|
|idle    |off     |off|off|off|140 mAh/30 min|
|idle    |on     |off|on|off|166 mAh/30 min|
|idle    |on     |on|on|on|182 mAh/30 min|

#### Disable Ethernet 
Unplug Ethernet 

#### Disable WiFi
Disable WiFi interface in ```/etc/config/wireless```:
```
config wifi-device 'radio0'
    option disabled '1'
```

#### Disable 4G
Disable the 4G modem through the console
```
AT+QPOWD=1
```


