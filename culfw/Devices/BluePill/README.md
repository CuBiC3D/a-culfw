# Firmware für BluePill

## Benötigte Tools
- STM32 Flash loader demonstrator: http://www.st.com/en/development-tools/flasher-stm32.html
- oder alternativ stm32flash : https://sourceforge.net/p/stm32flash/wiki/Home
- DFU Bootloader: https://github.com/rogerclarkmelbourne/STM32duino-bootloader/blob/master/binaries/generic_boot20_pc13.bin
- dfu-util: http://dfu-util.sourceforge.net/
- oder alternativ MSC bootloader: https://github.com/Telekatz/MSC-stm32f103-bootloader/releases
- a-culfw: https://www.mediafire.com/folder/iuf7lue8r578c/a-culfw
- USB zu TTL Adapter. 
- Funktionierende toolchain: `gcc-arm-none-eabi-6-2017-q2-update`

## Bootloader flashen
Der Prozess über den internen UART Bootloader ist hier beschrieben: https://medium.com/@paramaggarwal/programming-an-stm32f103-board-using-usb-port-blue-pill-953cec0dbc86. Falls vorhanden, kann auch ein ST-LINK, J-Link oder beliebiger anderer
Debugger eingesetzt werden.


## a-culfw flashen

### DFU Bootloader
- Reset an der BluePill drücken.
- Während die LED schnell blinkt (ca. 4Hz) dfu-util starten:
`dfu-util --verbose --device 1eaf:0003 --cfg 1 --alt 2 --download MapleCUNx4_W5100_BL.bin`
- Durch setzen des Boot1 Jumpers, kann der Bootloader-Modus nach Reset dauerhaft aktiviert werden.

### MCS Bootloader
- Reset an der BluePill zweimal innerhalb einer Sekunde drücken.
- Die Firmware Datei auf das vom Bootloader bereitgestellte USB-Laufwerk kopieren.

## Hinweis zum Flash
Der STM32F103C8T6 der BluePill besitzt offiziell einen 64k großen Flash. In der Regel sind in den Chips trotzdem 128k verbaut. Um den 128k Flash mit dieser Firmware nutzen zu können, muss bei bedarf in den beiden Linker Files (`.ld`) folgendes angepasst werden:
```
MEMORY
{
    RAM (xrw)      : ORIGIN = 0x20000000, LENGTH = 20K
    FLASH (rx)      : ORIGIN = 0x8000000, LENGTH = 128K
}
```
bzw. mit Bootloader:
```
MEMORY
{
    RAM (xrw)      : ORIGIN = 0x20000000, LENGTH = 20K
    FLASH (rx)      : ORIGIN = 0x8002000, LENGTH = 120K
}
```


Hinweis: Für eine BluePillCUL ist es egal, ob die Datei BluePillCUNx4_W5100_BL.bin oder BluePillCUNx4_W5500_BL.bin verwendet wird.
