# WebThings Empty Project

**WebThing Empty Project** is a starting point for building webThing device on ESP32 processor.

This software is prepared for ESP32 CPUs and uses esp-idf environment (current stable version: 4.1).

To build webThing device follow these steps:

1. download ```WebThing Empty Project```
2. in project directory create ```components``` folder
3. into ```components``` folder download ```web_thing_server``` from [webthings-components](https://github.com/KrzysztofZurek1973/webthings-components) repository
4. in the directory ```components``` put code of your webThing, it should have ```init_the_thing``` function which returns pointer to the webThing (see ```web_thing_server``` documentation and [wethings-node-example-project](https://github.com/KrzysztofZurek1973/webthings-node-example-project) for information about how to create webThing software)
5. for tests (in point 3) you can download [thing_button](https://github.com/KrzysztofZurek1973/webthings-components/tree/master/thing_button) and/or [thing_blinking_led](https://github.com/KrzysztofZurek1973/webthings-components/tree/master/thing_blinking_led) into components and add things to the server using ```add_thing_to_server(init_button())``` and/or ```add_thing_to_server(init_blinking_led())```
6. include header file (e.g. ```thing_button.h```) of your thing
7. if necessary configure project by ```idf.py menuconfig```
8. compile by ```idf.py build```
9. upload into flash memory by ```idf.py flash```

In order to fully use the potential of IoT, and to have access to *webThings* from the Internet, it is best to run [WebThing Gateway](https://webthings.io/gateway/) in the same WiFi network.

### Power-up

By the first power-up the node starts in **AP** mode with SSID: ```iot-node-ap``` and password: ```htqn9Fzv```. After loggin into this WiFi network load the main page by typing in browser ```iot-node-ap.local:8080/``` (it does not work on Android) or ```192.168.4.1:8080/```.

On the page write SSID and password of the WiFi network where the node will work. In the last position enter the local node's name.

Pressing *submit* causes writing data into flash and restart of the node in *station* mode. If you want to change the WiFi SSID or password later just connect ```GPIO27``` to GND, this causes node's restart in **AP** mode as in the previous paragraph.

If *WebThing Gateway* runs in the same WiFi network go to *Things* category on gateway and press *plus* sign. Gateway shoud find new things, press *Save* and *Done*.

Now you have 2 new things in your IoT network and you can control them from anywhere you are.

### Start-up step by step description

After power-up information about CPU is sent to serial port (e.g. /dev/ttyUSB0 in Linux). Serial port speed is set on 115200 bps.

```init_reset_button``` - initialization of ```GPIO27``` for restarting in **AP** mode.

```init_things``` - things are added here (see things documentation).

```init_nvs``` - checks if WiFi SSID and password are saved in flash, if so WiFi initialization takes place in *station* mode (```wifi_init_sta```) and the node connects to the WiFi network.

If SSID or pasword were not found node starts in *softAP* mode (```wifi_init_softap```).

In the main loop information about free heap memory is sent every 2 seconds on serial port (for tests only).

*Web Thing Server* starts running after event ```IP_EVENT_STA_GOT_IP```. Server runs on port 8080  (more information in *Web Thing Server* documentation).

After that the sntp client (```init_sntp```) is started.

### Project Configuration

```
idf.py menuconfig
```

In the *WebThing Empty Project* menu item, you can set the ```GPIO_A``` (default 26) and ```GPIO_B``` (default 22) positions. They will be visible for compiler as ```CONFIG_GPIO_A``` and ```CONFIG_GPIO_B``` constants.

### Build and Flash


Build: ```idf.py build```

Flash: ```idf.py flash``` and press boot button.

## Links

* [WebThing Gateway](https://webthings.io/gateway/) - https://webthings.io/gateway/
* [Web Thing API](https://webthings.io/api/) - https://webthings.io/api/
* [esp-idf](https://github.com/espressif/esp-idf) - https://github.com/espressif/esp-idf

## License

This project is licensed under the MIT License.

## Authors

* **Krzysztof Zurek** - [github](https://github.com/KrzysztofZurek1973)
