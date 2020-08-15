# Home-Assistant EPEver eBox-Wifi-01 using MODBUS over TCP
How to extract data from the EPEver TRACER-AN using the eBox-Wifi-01 with Home Assistant.
+ Make sure you're able to connect your eBox-Wifi-01 to your WiFi network, so your Home Assistant can reach it!
+ Follow my quick guide here: https://github.com/gabrielpc1190/eBox-Wifi-01-hacks
+ Some of the registers that worked came from this PDF: http://www.solar-elektro.cz/data/dokumenty/1733_modbus_protocol.pdf

# modbus integration section on configuration.yaml
```
#EPEver eBox-Wifi-01 modbus
modbus:
  - type: tcp
    host: 172.16.10.98
    port: 8088
    name: hub1
    timeout: 2
#  - type: serial
#    name: hub2
#    method: rtu
#    port: /dev/ttyUSB0
#    baudrate: 115200
#    stopbits: 1
#    bytesize: 8
#    parity: N
  - type: rtuovertcp
    host: 172.16.10.98
    port: 8088
    name: hub1
```

# sensor section on configuration.yaml
```
# modbus sensors for EPEver
  - platform: modbus
    scan_interval: 10
    registers:
    - name: EPEver_Battery
      hub: hub1
      unit_of_measurement: V
      slave: 01
      register: 13082
      register_type: input
      scale: 0.01
      precision: 2
    - name: EPEver_Solar #3100
      hub: hub1
      unit_of_measurement: V
      slave: 1
      register: 12544
      register_type: input
      scale: 0.01
    - name: EPEver_Solar_Power_L # 3102
      hub: hub1
      unit_of_measurement: W
      slave: 01
      register: 12546
      register_type: input
      scale: 0.01
    - name: EPEver_Solar_Current # 3101
      hub: hub1
      unit_of_measurement: A
      slave: 01
      register: 12545
      register_type: input
      scale: 0.01
      precision: 2
    - name: EPEver_Load # 310E
      hub: hub1
      unit_of_measurement: W
      slave: 01
      register: 12558
      register_type: input
      scale: 0.01
    - name: EPEver_Battery_Temperature
      hub: hub1
      unit_of_measurement: Celsius
      slave: 01
      register: 12560
      register_type: input
    - name: EPEver_Battery_SOC
      hub: hub1
      unit_of_measurement: Percent
      slave: 01
      register: 12570
      register_type: input
```
# More registers for future testing:
```
            #modbus:
            #  - type: tcp
            #    host: 172.16.10.98
            #    port: 8088
            #    name: hub1
            #  - platform: modbus
            #    registers:
            #      - name: PV array rated voltage 
            #        hub: hub1
            #        unit_of_measurement: V
            #        slave: 01
            #        register: 0x3000 #need to be converted from hex to decimal
            #        register_type: input
            
            #      - name: PV array rated current 
            #        hub: hub1
            #        unit_of_measurement: A
            #        slave: 01
            #        register: 0x3001 #need to be converted from hex to decimal
            #        register_type: input
            
            #      - name: PV array rated power (low 16 bits)
            #        hub: hub1
            #        unit_of_measurement: W
            #        slave: 01
            #        register: 0x3002 #need to be converted from hex to decimal
            #        register_type: input
            
            #      - name: PV array rated power (high 16 bits)
            #        hub: hub1
            #        unit_of_measurement: W
            #        slave: 01
            #        register: 0x3003 #need to be converted from hex to decimal
            #        register_type: input
            
            #      - name: Bank 1 Voltage
            #        hub: hub1
            #        unit_of_measurement: V
            #        slave: 01
            #        register: 0x3004 #need to be converted from hex to decimal
            #        register_type: output
            
            #      - name: Rated charging current to battery 
            #        hub: hub1
            #        unit_of_measurement: A
            #        slave: 01
            #       register: 0x3005 #need to be converted from hex to decimal
            #        register_type: output
            
            #      - name: Rated charging power to battery 
            #        hub: hub1
            #        unit_of_measurement: W
            #        slave: 01
            #        register: 0x3006 #need to be converted from hex to decimal
            #        register_type: output
```
