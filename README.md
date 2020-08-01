# Modbus-GoodWe-DT
Protocol description for modbus Goodwe DT inwerters


# Setup connection:
Conection is made with parameters : 9600 baud , 8 data bits , 1 stop bit , parity None.

# Description
In DT series of GoodWe inwerters there isstandard modbus implemented (in opposite to other GoodWe inverters with their own protocol). So to receive a full data we have to send to inverter's addres 247 (standard for GoodWe,changable in menu) a command for requesting the content of analog output holding registers ( function 03)  #768  to #891 ( in total 124 registers) a request: 

F7 03 0300 007C 50F9

where: \
F7 - inwerter adress (247 in HEX: F7) \
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
| - | 02 | F8 | Data length (Packet length = Data length + 5) |  |
| 768 | 03 04 | 18 38 | 1838 hex = 6200  620.0V | DC Voltage on PV1 |
| 769 | 05 06 | 18 5B | 185B hex = 6235  623.5V | DC Voltage on PV2 |
| 770 | 07 08 | 00 1B | 001B hex = 27  2.7A | DC Current on PV1 |
| 771 | 09 10 | 00 1C | 001C hex = 28  2.8A | DC Current on PV2 |
| 772 | 11 12 | 09 0E | 090E hex = 2318  231.8V | AC Voltage on L1 |
| 773 | 13 14 | 09 63 | 0963 hex = 2403  240.3V | AC Voltage on L2 |
| 774 | 15 16 | 09 57 | 0957 hex = 2391  239.1V | AC Voltage on L3 |
| 775 | 17 18 | 00 33 | 0033 hex = 51  5.1A | AC Current on L1 |
| 776 | 19 20 | 00 32 | 0032 hex = 50  5.0A | AC Current on L2 |
| 777 | 21 22 | 00 31 | 0031 hex = 49  4.9A | AC Current on L3 |
| 778 | 23 24 | 13 86 | 1386 hex = 4998 49.98Hz | AC Frequency on L1 |
| 779 | 25 26 | 13 86 | 1386 hex = 4998 49.98Hz | AC Frequency on L2 |
| 780 | 27 28 | 13 87 | 1387 hex = 4999 49.99Hz | AC Frequency on L3 |
| 781 | 29 30 | 0D E9 | 0DE9 hex = 3561 3561W | Actual Power |
| 782 | 31 32 | 00 01 | 0001 hex = 1 | Status: 0-Pause/Waiting , 1-Working , 2 - Error |
| 783 | 33 34 | 01 AD | 01AD hex = 429 42.9deg | Inner Temperature |
| 784 | 35 36 | 00 00 |  | Error Message H |
| 785 | 37 38 | 00 00 |  | Error Message L |
| 786 | 39 40 | 00 00 |  | Energy Total H ? |
| 787 | 41 42 | 01 16 | 0116 hex = 278 27,8kWh | Energy Total L |
| 788 | 43 44 | 00 00 |  | Working Hours Total H ? |
| 789 | 45 46 | 00 17 | 0017 hex = 23h  | Working Hours Total L |
| 790 | 47 48 | 07 7F | 077F hex = 1919 191.9deg  | TemperatureFaultValue |
| 791 | 49 50 | 00 00 |  | PV1FaultVault |
| 792 | 51 52 | 00 6B | 006B hex = 107 0.7V  | PV2FaultVault |
| 793 | 53 54 | 00 00 |  | Line1VFaultValue |
| 794 | 55 56 | 01 A6 | 01A6 hex = 422 42.2V | Line2VFaultValue |
| 795 | 57 58 | 02 98 | 0298 hex = 664 66.4V | Line3VFaultValue |
| 796 | 59 60 | 19 1B | 191B hex = 6427 64.27Hz | Line1FFaultValue |
| 797 | 61 62 | 0C 74 | 0C74 hex = 3188 31.88Hz | Line2FFaultValue |
| 798 | 63 64 | 00 00 |  | Line3FFaultValue |
| 799 | 65 66 | 00 06 | 0006 hex = 6 6mA | GFC1FaultValue |
| 800 | 67 68 | 00 2A | 002A hex = 42 4.2kWh | Energy Today |
| 801 - 826 | 69 - 120 | 00 00 | | 0 bytes |
| 827 | 121 122 | 14 07 |14hex = 20 2020year 07hex - 07 month  | Date |
| 828 | 123 124 | 12 09 |12hex = 18 day 09hex= 09 hour  | Date/Time |
| 829 | 125 126 | 0F 28 | 0Fhex =  16 min 28hex = 40s  | Time |
| 830 | 127 - 128 | 00 00 | | 0 bytes |
| 831 | 129 - 130 | 00 3B | 003B hex = 59 | Measurement during start, leackage current ? |
| 830 - 851 | 131 - 170 | 00 00 | | 0 bytes |
| 852 | 171 172 | 00 0C | 000C hex = 12 | Constans value , ? |
| 853 - 884 |173 - 236 | 00 00 | | 0 bytes|
| 885 | 237 - 238 | 27 10 | 2710 hex = 10 000 W | Inverter power |
| 886 |239 240 | 00 00 | | 0 bytes|
|887 - 891| 241 - 250 | 47 57 32 35 4B 2D 44 54 20 20 | 475732354B2D44542020 hex to ASCII gives GW25K-DT | Model name |
| - | 251-252| 40 30 | CRC16-MODBUS | |

Error masages (784+785 combined) on first look maches with standard GoodWe protocol.

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
