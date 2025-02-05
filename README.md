# Nissan LEAF CAN bus messages (in .DBC format!)

This is an evolution of the spreadsheet found at https://docs.google.com/spreadsheets/d/1EHa4R85BttuY4JZ-EnssH4YZddpsDVu6rUFm0P7ouwg/edit#gid=1

Proper CAN database files are also found in this repository along with the original excel spreadsheet. The .dbc files are easier to use when working with CAN messages. Please note that there are major differences between ZE0,AZEO and ZE1 LEAFs

Please fork the repo and make changes in your repo before creating a pullrequest, or message me if you have some CAN findings to share.

## How to browse the database?

Download 'Kvaser Database Editor 3' https://www.kvaser.com/download/
With this tool you can open the .DBC files and explore the frames and the position of the data inside the frame.
![alt text](https://github.com/dalathegreat/leaf_can_bus_messages/blob/master/DatabaseEditor.PNG)

DBC files are also extremely useful for reverse engineering, if you playback a CAN log these files can be used directly for translating the data.

## Some notes about CRC/CSUM/MPRUN

Nissan places a few error checking methods into the communication. This video explains the functions of these:
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/oENNNfy5GSM/0.jpg)](https://www.youtube.com/watch?v=oENNNfy5GSM)

## What about active CAN-polling?

Actively asking the different control units for info is another thing. The database files here won't help you (those are for passive listening), but here is a list of the different control units query and response IDs.

| Control Unit  |    ID Query   |  ID Response  |
| ------------- | ------------- | ------------- |
|   Consult3+   |     0x7D2     |               |
|      VCM      |     0x797     |     0x79A     |
|      BCM      |     0x743     |     0x763     |
|      ABS      |     0x740     |     0x760     |
|      LBC      |     0x79B     |     0x7BB     |
|     INV/MC    |     0x784     |     0x78C     |
|     Meter     |     0x745     |     0x765     |
|     HVAC      |     0x744     |     0x764     |

Here's an example of a request, checking which gear is selected from the VCM:

Description: Gear position (1=Park, 2=Reverse, 3=Neutral, 4=Drive, 7=Eco). The fifth reply byte value is
the gear value. In the example gear is 1.

Query: 0x797 03 22 11 56 00 00 00 00

Answer: 0x79A 04 62 11 56 **01** 00 00 00
