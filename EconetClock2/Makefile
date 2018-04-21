AVR ?= /Applications/Arduino.app/Contents/Java/hardware/tools/avr

default: EconetClock2 myelin_ecohub

# Firmware for the EconetClock2 board
EconetClock2:
	${AVR}/bin/avr-gcc -I${AVR}/avr/include -DNDEBUG -fpack-struct -fshort-enums -Wall -Os -mmcu=attiny45 -o main.o -c main.c
	${AVR}/bin/avr-gcc -w -Os -g -flto -fuse-linker-plugin -Wl,--gc-sections -mmcu=attiny45 -o $@.elf main.o -lm
	${AVR}/bin/avr-objcopy -O ihex -R .eeprom $@.elf $@.hex

# Firmware for https://github.com/google/myelin-acorn-electron-hardware/tree/master/econet_hub
# This board has no DIP switches, so the clock is hardcoded to 200kHz (5us period, 1us mark).
myelin_ecohub:
	${AVR}/bin/avr-gcc -I${AVR}/avr/include -DNDEBUG -DHARDCODE_DIP_SWITCH=0 -fpack-struct -fshort-enums -Wall -Os -mmcu=attiny85 -o main.o -c main.c
	${AVR}/bin/avr-gcc -w -Os -g -flto -fuse-linker-plugin -Wl,--gc-sections -mmcu=attiny85 -o $@.elf main.o -lm
	${AVR}/bin/avr-objcopy -O ihex -R .eeprom $@.elf $@.hex

upload_myelin_ecohub:
	${AVR}/bin/avrdude -C ${AVR}/etc/avrdude.conf -c usbtiny -p attiny85 -U flash:w:myelin_ecohub.hex -U lfuse:w:0xe2:m -U hfuse:w:0xdc:m -U efuse:w:0xff:m

clean:
	rm -vf *.hex *.o *.elf
