# Modbus-GoodWe-DT
Protocol description for modbus Goodwe DT inwerters


# Setup connection:
Conection is made with parameters : 9600 baud , 8 data bits , 1 stop bit , parity None.

# Description
In DT series of GoodWe inwerters there is most probably standard modbus implemented. So to receive a full data we have to send to inverter's addres 247 (standard for GoodWe,changable in menu) a command for requesting the content of analog output holding registers ( function 03)  #768  to 891 ( in total 124 registers) a request: \

F7 03 0300 007C 50F9

where: \
F7 - adress o inwerter (247 in HEX: F7) \
03 - Function Code 3 (read Analog Output Holding Registers) \
0300: The Data Address of the first register requested.( 0300 hex = 768)\
007C: The total number of registers requested. (read 124 registers 768 to 891) \
50F9: The CRC16-MODBUS  (cyclic redundancy check) for error checking 

Responce will be similar to:

F7 03 F8 18 38 18 5B 00 1B 00 1C 09 0E 09 63 09 57 00 33 00 32 00 31 13 86 13 86 13 87 0D E9 00 01 01 AD 00 00 00 00 00 00 01 16 00 00 00 17 07 7F 00 00 00 6B 00 00 01 A6 02 98 19 1B 0C 74 00 00 00 06 00 2A 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 14 07 12 09 0F 28 00 00 00 3B 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 0C 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 27 10 00 00 47 57 32 35 4B 2D 44 54 20 20 40 30
 

|Register # | Byte# | Possible Value | Value Decrypt | Value Description |
| :---- | ----- | ----- |----- | ----:|
| - | 00 | F7 | F7 hex = 247  | Addres from where this responce recived  |
| - | 01 | 03 | Function Code 3 | read Analog Output Holding Registers |
| 768 | 02 03 | 18 38 | 1838 hex = 6200  620.0V | DC Voltage on PV1 |
| 769 | 04 05 | 18 5B | 185B hex = 6235  623.5V | DC Voltage on PV2 |
| 770 | 06 07 | 00 1B | 001B hex = 27  2.7A | DC Current on PV1 |
| 771 | 08 09 | 00 1C | 001C hex = 28  2.8A | DC Current on PV2 |
| 772 | 10 11 | 09 0E | 090E hex = 2318  231.8V | AC Voltage on L1 |
| 773 | 12 13 | 09 63 | 0963 hex = 2403  240.3V | AC Voltage on L2 |
| 774 | 14 15 | 09 57 | 0957 hex = 2391  239.1V | AC Voltage on L3 |
| 775 | 16 17 | 00 33 | 0033 hex = 51  5.1A | AC Current on L1 |
| 776 | 18 19 | 00 32 | 0032 hex = 50  5.0A | AC Current on L2 |
| 777 | 20 21 | 00 31 | 0031 hex = 49  4.9A | AC Current on L3 |
| 778 | 22 23 | 13 86 | 1386 hex = 4998 49.98Hz | AC Frequency on L1 |
| 779 | 24 25 | 13 86 | 1386 hex = 4998 49.98Hz | AC Frequency on L2 |
| 780 | 26 27 | 13 87 | 1387 hex = 4999 49.99Hz | AC Frequency on L3 |
| 781 | 28 29 | 0D E9 | 0DE9 hex = 3561 3561W | Actual Power |
| 782 | 30 31 | 00 01 | 0001 hex = 1 | Status ;0-Pause/Waiting , 1-Working , 2 - Error |
| 783 | 32 33 | 01 AD | 01AD hex = 429 42.9deg | Inner Temperature |
| 784 | 34 35 | 00 00 |  | Error Message H |
| 785 | 36 37 | 00 00 |  | Error Message L |
| 786 | 38 39 | 00 00 |  | Energy Total H ? |
| 787 | 40 41 | 01 16 | 0116 hex = 278 kWh  | Energy Total L |
| 788 | 42 43 | 00 00 |  | Working Hours Total H ? |
| 789 | 44 45 | 00 17 | 0017 hex = 23h  | Working Hours Total L |
| 790 | 46 47 | 07 7F | 077F hex = 1919 191.9deg  | TemperatureFaultValue |
| 791 | 48 49 | 00 00 |  | PV1FaultVault |
| 792 | 50 51 | 00 6B | 006B hex = 107 0.7V  | PV2FaultVault |
| 793 | 52 53 | 00 00 |  | Line1VFaultValue |
| 794 | 54 55 | 01 A6 | 01A6 hex = 422 42.2V | Line2VFaultValue |
| 795 | 56 57 | 02 98 | 0298 hex = 664 66.4V | Line3VFaultValue |
| 796 | 58 59 | 19 1B | 191B hex = 6427 64.27Hz | Line1FFaultValue |
| 797 | 60 61 | 0C 74 | 0C74 hex = 3188 31.88Hz | Line2FFaultValue |
| 798 | 62 63 | 00 00 |  | Line3FFaultValue |
| 799 | 64 65 | 00 06 | 0006 hex = 6 6mA | GFC1FaultValue |
| 800 | 66 67 | 00 2A | 002A hex = 42 4.2kWh | Energy Today |
| 801 - 826 | 66 - 119 | 00 00 | | 0 bytes |
| 827 | 120 121 | 14 07 |  | Date ?|
| 828 | 122 123 | 12 09 |  | Time ?|
| 829 | 124 125 | 0F 28 |  | Time ?|
| 830 | 126 - 127 | 00 00 | | 0 bytes |
| 831 | 128 - 129 | 00 3B | | Safety Country code ? |
| 830 - 851 | 130 - 169 | 00 00 | | 0 bytes |
| 852 | 170 171 | 00 0C | 000C hex = 12 | ? |
| 853 - 884 |172 - 235 | 00 00 | | 0 bytes|
| 885 | 236 - 237 | 27 10 | 2710 hex = 10 000 W | Inverter power |
| 886 |238 239 | 00 00 | | 0 bytes|
|887 - 891| 240 - 249 | 47 57 32 35 4B 2D 44 54 20 20 | 475732354B2D44542020 hex to ASCI gives GW25K-DT | Model name |
| - | 250-251| 40 30 | CRC16-MODBUS |

Error masages (784+785 combined) on first look maches with standard GoodWe protocol. (generated  only  Utility loss and Insulation failure errors)
|Bit # | Error Message |  Description |
| :---- | ----- | ----:|
| Bit31 | Internal Communication Failure | Communication between microcontrollers is failure  |
| Bit30 | EEPROM R/W Failure | EEPROM cannot be read or written |
| Bit29 | Fac Failure | The grid frequency is out of tolerable range |
| Bit28 | TBD | N/A |
| Bit27 | TBD | N/A |
| Bit26 | TBD | N/A |
| Bit25 | Relay Check Failure | Relay check is failure |
| Bit24 | TBD | N/A |
| Bit23 | Vac Consistency Failure | Different value between Master and Slave for grid voltage |
| Bit22 | Fac Consistency Failure | Different value between Master and Slave for grid frequency |
| Bit21 | TBD | N/A |
| Bit20 | TBD | N/A |
| Bit19 | DC Injection High | The DC injection to grid is too high  |
| Bit18 | Isolation Failure | Isolation resistance of PV-plant out of tolerable range |
| Bit17 | Vac Failure | Grid voltage out of tolerable range  |
| Bit16 | Fan Failure | Fan Lock |
| Bit15 | PV Over Voltage | Pv input voltage is over the tolerable maximum value |
| Bit14 | Auto Test Failure | Auto test failure |
| Bit13 | Over Temperature | Temperature is too high |
| Bit12 | Internal Version Unmatch | Master and slave firmware version is unmatch |
| Bit11 | DC Bus High | Dc bus is too high |
| Bit10 | Gournd I Failure | Ground current is too high |
| Bit9 | Utility Loss | Utility is unavailable |
| Bit8 | TBD | N/A |
| Bit7 | TBD | N/A |
| Bit6 | TBD | N/A |
| Bit5 | TBD | N/A |
| Bit4 | GFCI Consistency Failure | Different value between Master and Slave for GFCI |
| Bit3 | DCI Consistency Failure | Different value between Master and Slave for output DC current |
| Bit2 | TBD | N/A |
| Bit1 | AC HCT Failure | The output current sensor is abnormal |
| Bit0 | GFCI Device Failure | The GFCI detecting circuit is abnormal |
