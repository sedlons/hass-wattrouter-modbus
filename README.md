# hass-wattrouter-modbus
Modbus configuration in Home Assistant for Wattrouter. No addon needed.

Important Setup Notes:
 - Update your wattrouter to latest firmware. 
 - IP Address: Replace 192.168.1.100 in the code with the actual IP address of your WATTrouter.
 - Units: The registers state values in "Tens of W" or "Tens of Wh". Therefore, scale: 10 is used in the configuration so that HA displays the correct values in W and Wh.

### Modbus definition and sensors (reading values, statistics, and text fields)
Add modbus section to configuration.yaml. You can find source files in wattrouter_Mx or wattrouter_M.

### Binary sensors (breaking down status registers into individual bits – errors and info)
Modbus reads a whole integer (word), but individual bits have distinct meanings. This is done in template section in your configuration.yaml (or your template file). These sensors decode the values from Wattrouter Error Word Raw and Wattrouter Info Word Raw.

### Control elements (writing to registers to switch outputs)
If you use the number.wattrouter_switch_* entities to switch on an output (writing to registers 0-5 or 8-13):
The manual states:
"When switched/restricted externally, the output keeps the switched/restricted state for only 30 s, then for safety reasons the output will be switched off... If you want to switch/restrict the output in this way for a longer time, you must send message 06/16 repeatedly, with a period of eg 10 s." 

#### Important Warning Regarding Output Control 
Home Assistant sends the value only once upon change by default. If you want permanent switching via Modbus, you must create an automation in HA that will repeatedly send the value (e.g., using the number.set_value or modbus.write_register service) as long as the device is supposed to be on.
