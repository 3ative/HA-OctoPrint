# HA-OctoPrint
Everything you need to add a RPi and Octo Print to a 3D Printer, and Add it to Home Assistant

## Extra Code-y Bits for Smart Plugs:

```yaml
substitutions:
  ## OctoPrint Sensors
  is_printing: binary_sensor.ender_3_v2_printing
  bed_temp: sensor.ender_3_v2_actual_bed_temp
  shutdown_temp: "55"
  shutdown_button: button.ender_3_v2_shutdown_system
```

```yaml
binary_sensor:
  ############## Extra Priner Automation ##############
  - platform: homeassistant
    id: is_printing
    entity_id: $is_printing
    internal: true
  ############## Extra Priner Automation ##############
```

```yaml
sensor:
  ############## Extra Priner Automation ##############
  - platform: homeassistant
    id: bed_temp
    entity_id: $bed_temp
    on_value:
      - if:
          condition:
            and:
              - binary_sensor.is_off: is_printing
              - switch.is_off: master
              - lambda: "return id(bed_temp).state < $shutdown_temp;"
          then:
            - homeassistant.service:
                action: button.press
                data:
                  entity_id: $shutdown_button
            - delay: 15s
            - switch.turn_off: relay
  ############## Extra Priner Automation ##############
```

```yaml
switch:
  ############## Extra Priner Automation ##############
  - platform: template
    name: "Master"
    id: master
    icon: mdi:printer
    optimistic: true
    on_turn_on:
      - switch.turn_on: relay
  ############## Extra Priner Automation ##############
```




___

#### 💖 Found this useful, want to say '*Thanks*' and support my efforts. *CHEERS*🍺
| Buy me a Coffee | PATREON |
|-----------------|---------|
| https://www.buymeacoffee.com/3ative | https://www.patreon.com/3ative |
