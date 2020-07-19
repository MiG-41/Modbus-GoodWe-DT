# Modbus-GoodWe-DT
Protocol description for modbus Goodwe DT inwerters


# Setup connection:
Conection is made with parameters : 9600 baud , 8 data bits , 1 stop bit , parity None.

# Description
In DT series of GoodWe inwerters there is most probably standard modbus implemented. So to receive a full data we have to send to inverter's addres 247 (standard for GoodWe,changable in menu) a command for requesting the content of analog output holding registers ( function 03)  # 768  to #892 ( in total 124 registers) a request:

F7 03 0300 007C 50F9

where:
F7 - adress o inwerter (247 in HEX: F7)
03 - Function Code 3 (read Analog Output Holding Registers)
0300: The Data Address of the first register requested.( 0300 hex = 768)
007C: The total number of registers requested. (read 124 registers 768 to 892) 
50F9: The CRC16-MODBUS  (cyclic redundancy check) for error checking.


Responce will be similar to:

F7 03 F8 18 38 18 5B 00 1B 00 1C 09 0E 09 63 09 57 00 33 00 32 00 31 13 86 13 86 13 87 0D E9 00 01 01 AD 00 00 00 00 00 00 01 16 00 00 00 17 07 7F 00 00 00 6B 00 00 01 A6 02 98 19 1B 0C 74 00 00 00 06 00 2A 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 14 07 12 09 0F 28 00 00 00 3B 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0C 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 27 10 00 00 47 57 32 35 4B 2D 44 54 20 20 40 30

|Register # | Byte# | Possible Value | Value Decrypt | Value Description |
| :---- | ----- | ----- |----- | ----:|
| - | 00 | F7 | F7 hex = 247  | Adres from where this responce recived  |
| - | 01 | 03 | Function Code 3 | read Analog Output Holding Registers |
| 768 | 02 03 | 18 38 | 1838 hex = 6200  620.0V | DC Voltage on PV1 |
| 769 | 04 05 | 18 5B | 185B hex = 6235  623.5V | DC Voltage on PV2 |
| 770 | 06 07 | 00 1B | 001B hex = 27  2.7A | DC Current on PV1 |
| 771 | 08 09 | 00 1C | 001C hex = 28  2.8A | DC Current on PV2 |
| 772 | 10 11 | 09 0E | 090E hex = 2318  231.8V | AC Voltage on L1 |
| 773 | 12 13 | 09 63 | 0963 hex = 2403  240.3V | AC Voltage on L2 |
| 774 | 14 15 | 09 57 | 0957 hex = 2391  239.1V | AC Voltage on L3 |
| 775 | 16 17 | 09 0E | 090E hex = 2318  231.8V | AC Voltage on L1 |
