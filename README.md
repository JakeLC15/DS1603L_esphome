# DS1603L_esphome
DS1603L Ultrasonic Sensor External Component with Level and Volume

Use a DS1603L ultrasonic sensor without modification.
These sensors read from the bottom of a tank and need to have some kind of couplant for them to work. There can be no air gap between sensor and bottom of container. I use clear silicone grease, easily found at most automotive or hardware stores.

Example

```yaml
external_components:
  - source: github://JakeLC15/DS1603L_esphome/DS1603L/
    components: [ DS1603L ]

  ############
  # Use this if dowloaded locally to "esphome/my_components" folder  
  #  - source:
  #    type: local
  #    path: my_components/
  ############
    
sensor: 

##############
### Tank 1 ###
##############
  - platform: DS1603L 
    uart_id: tank1                       # must match uart config
    min_level: 0.0                       # set minimum level in mm  default 0.0
    max_level: 600.0                     # set maximum level in mm  default 1000.0
    min_volume: 0.0                      # set minimum volume in whatever unit_of_measurement you prefer  default 0.0
    max_volume: 55.0                     # set maximum volume in same unit as min_volume  default 1000.0
    liquid_level:                        # get level in mm
      state_class: measurement           # for HA
      device_class: distance             # for HA
      name: "Gray Distance"              # name for HA
      unit_of_measurement: "mm"          # sensor outputs mm, can be set to anything but will need filtering to convert
      icon: mdi:arrow-expand-vertical    # icon for HA
      filters:                           # some filters I found useful
        - filter_out: 0
        - filter_out: nan
        - delta: 1.0
    liquid_volume:
      name: "Gray Volume"
      unit_of_measurement: "gal"
      filters:
        - filter_out: 0
        - filter_out: nan
        - delta: 1.0
##############
### Tank 2 ###
##############
  - platform: DS1603L
    uart_id: tank2
    min_level: 0.0
    max_level: 600.0
    min_volume: 0.0
    max_volume: 55.0
    liquid_level:
      name: "Black Distance"
      state_class: measurement 
      device_class: distance 
      unit_of_measurement: "mm"
      icon: mdi:arrow-expand-vertical 
      filters:
        - filter_out: 0
        - filter_out: nan
        - delta: 0.1
    liquid_volume:
      name: "Black Volume"
      unit_of_measurement: "gal"
      filters:
        - filter_out: 0
        - filter_out: nan
        - delta: 1.0
# optional
  - platform: internal_temperature
    name: ${name} internal temperature
# optional  
  - platform: wifi_signal 
    name: ${name} wifi signal 
    update_interval: 60s

switch: # optional
  - platform: restart 
    name: ${name} restart

#required for sensor
uart: 
  - id: tank1
    tx_pin: GPIO17
    rx_pin: GPIO16
    baud_rate: 9600 
    
  - id: tank2
    tx_pin: GPIO1    
    rx_pin: GPIO3 
    baud_rate: 9600 
