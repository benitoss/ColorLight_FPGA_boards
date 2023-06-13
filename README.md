# ColorLight_FPGA_boards
IceStudio already supports ColorLight 5A-75X, i5 and ICeSugar Pro FPGA boards in in Windows, Mac and Linux versions.

## New supported boards in IceStudio and Apio

- Colorlight 5A-75B
- Colorlight 5A-75F
- Colorlight i5
- IceSugar Pro 

Below you can see the new boards added and the supported versions. In parenthesis you can see the acepted programmers for each board. If there is not a JTAG programmer in parenthesis, the default JTAG programmer is FT2232H.

There is another utility for ColorLight i5 and iCESugar-Pro called [ECPDAP](https://github.com/adamgreig/ecpdap) added in the toolchain, but that program is too slow to program the board. Other JTAG programmer as STM32 with the [DirtyJTAG firmware](https://github.com/jeanthom/DirtyJTAG) has the same problem. It is recommendable to use an external JTAG programmer as FT2232, FT232H or USB-Blaster to program these boards.

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/icestudio_boards.png)

**NOTE: At 7/15/2021 the las update of the toolchain-ecp5 of the FPGAWArs groups already has support to the ColorLight and IceSugar Pro boards in Windows, Mac and Linux versions of Icestudio, consequently we don't need to add any file to get this support**

# JTAG Programmers

Because the Colorlight 5A-75B and 5A-75E not include a JTAG programmer, you will need an external JTAG programmer
In case of Colorlight i5 or ICeSugar Pro use a STM32 based programmer that is too slow (It spends more than 1 minute), so it is recomended to use an external JTAG programmer too

The recomended JTAG programmers are:
1) FT2232H  from here --> https://www.aliexpress.com/item/1005002448969402.html
2) FT232H  from here -->  https://www.aliexpress.com/item/33052982174.html
3) USB-Blaster from here --> https://www.aliexpress.us/item/3256802725775256.html

I have a FT2232H programmer as the first recommended option:

<img src="https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/ft2232h_1.png" alt="FT2322H Board" width="450" height="342">

Because I use the same programmer in other Xilinx Boards I have applied a mod to this programmer (If you only are going to use for Lattice  ECP5 board you don't need to do it)
The mod is based in change the content of its EPROM , here you have the instructions  -->  https://gist.github.com/rikka0w0/24b58b54473227502fa0334bbe75c3c1

if you adquire the FT232H the mod to convert in Digilent HS2 Cable compatible with Vivado is --> https://github.com/erd0spy/ft232h_to_digilent_fpga_programmer
And here you have the other possible configurations for the FT232H board, as a STM1 for example --> https://github.com/vanbwodonk/ft2232hl-jtag-clone/tree/main/eeprom_binary

# Install LibUSBK to work in Windows (Linux user don't need this part)

Working in windows you have to install the LibUSBk driver to get the programmer be recognized by the programmer software included in Apio

In the same way of other boards in IceStudio, you can use Zadig to install the LibUsbK driver if you want to use the programmer in IceStudio/Apio.

If you applied the mod of the EPROM in your ft2232H board and you install the Xilinx Drivers, in Zadig you will be the FT2232H board as Digilent Adept USB Device (Interface 0) and you can install the driver libUsbK

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/zadig.png)

You have more information how to use ZADIG in this link -->  https://github.com/foss-for-synopsys-dwc-arc-processors/arc_gnu_eclipse/wiki/How-to-Use-OpenOCD-on-Windows

But in my case, because I use the board for Lattice ECP5 and for Xilinx boards in ISE/Vivado, I don't use Zadig to install the LibUsbK driver because the FT2232H programmer will not work with Xilinx IDE. So, to recover easily the original driver that works in Xilinx, I use an alternative method.  First, I install the Xilinx driver of the FT2232H board after modifying the EPROM and later I use the alternative method to modify the driver.

**How to install and change the LibUSbK driver (Alternative Method to Zadig. It allows recover the driver easily)**

I use the utility USB Driver Tool, you can download from https://visualgdb.com/UsbDriverTool/  The last version is here --> https://sysprogs.com/getfile/1372/UsbDriverTool-2.1.exe
In case you use the FT2232H JTAG programmer, you will see the "USB Serial Converter A" with VID 0403 and VPI 6010. With FT232H is similar and with USB-Blaster you will see "Altera USB Blaster"

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/USB_Driver_Tool_1.jpg)

So, You have to press right mouse button over it and select " Install Libusb-WinUSB"

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/USB_Driver_Tool_2.jpg)

You will see in the line of "USB Srial Converter A"  in parenthesis Libusb-WinUSB. Now, You have the drivers already installed

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/USB_Driver_Tool_3.jpg)

If you want to revert the original driver of the FT2232H in order to use with Xilinx ISE or Vivado IDE , you only has to press right button over "USB Serial Converter A" again and select "Restore default driver", the driver will be restored to the original.

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/USB_Driver_Tool_4.jpg)

And you will see the original state with the device name without nothing in parenthesis . Now the board has recovered the original Xilinx sriver and I can use it in ISE and Vivado Xilinx IDEs 

In this way you can use the FT2232H JTAG programmer with any board  
The way to install the driver is equivalent for FT232H or USB-Blaster programmers

# Connections for FT2232H

Here you have the JTAG connections of the FT2232H board to program the board. Note: It is not necessary connect the VCC connector. Do not forget to connect the GND to the board

- ADBUS0 --> TCLK
- ADBUS1 --> TDI
- ADBUS2 --> TDO
- ADBUS3 --> TMS

If you apply the MOD of the EPROM, you will have an USB-UART converter in the Second port "USB Serial Converter B" and you can use as a traditional USB-UART convertor

- CDBUS0 --> TX
- CDBUS1 --> RX

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/ft2232h_2.png)

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/ft2323h_3.jpg)

# Connections for FT232H

Note: Do not forget to connect the GND to the board

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/ft232h_1.png)

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/ft232h_2.png)

- ADBUS0 --> TCLK
- ADBUS1 --> TDI
- ADBUS2 --> TDO
- ADBUS3 --> TMS

# Connections for USB-Blaster

The USB-Blaster connector is shown here

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/USB-Blaster.jpg)

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/jtag_USB_BLASTER.png)


# Connections in Colorlight 5A-75X boards

Eventually you can follow the fantastic info from chubby75 [Github](https://github.com/q3k/chubby75) to Know the JTAG pins in the Cololight Boards.

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/jtag.jpg)

For the Colorlight 5A-75E v8.0 here is the list of the JTAG connections. Thanks to cdwijs [Github](https://github.com/cdwijs)

- J30 - TDO - 3
- J32 - TDI - 9
- J31 - TMS - 5
- J27 - TCK - 1
- J33 - 3v3 - 7
- J34 - GND - 2,10

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/jtag_80.jpg)

# Connections in Colorlight i9-v7.2 board
Thanks to https://github.com/wuxx/Colorlight-FPGA-Projects

![alt text](https://github.com/benitoss/ColorLight_FPGA_boards/blob/main/images/i9_v7.2_pinout.png)
