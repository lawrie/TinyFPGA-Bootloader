# Makefile borrowed from https://github.com/cliffordwolf/icestorm/blob/master/examples/icestick/Makefile
#
# The following license is from the icestorm project and specifically applies to this file only:
#
#  Permission to use, copy, modify, and/or distribute this software for any
#  purpose with or without fee is hereby granted, provided that the above
#  copyright notice and this permission notice appear in all copies.
#
#  THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
#  WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
#  MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
#  ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
#  WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
#  ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
#  OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

PROJ = bootloader

PIN_DEF = pins.pcf
DEVICE = lp8k
PKG = cm81

all: $(PROJ).rpt fw.bin

%.blif: %.v ../../common/*.v 
	yosys -p 'synth_ice40 -top $(PROJ) -blif $@' $^

%.asc: $(PIN_DEF) %.blif
	arachne-pnr -d 8k -P $(PKG) -o $@ -p $^

%.bin: %.asc
	icepack $< $@

fw.bin: bootloader.bin
	cp bootloader.bin bootloader_0.bin 
	cp bootloader.bin bootloader_1.bin 
	cp bootloader.bin bootloader_2.bin 
	cp bootloader.bin bootloader_3.bin 
	icemulti -v -o fw.bin -a15 -p0 bootloader_0.bin bootloader_1.bin bootloader_2.bin bootloader_3.bin

%.rpt: %.asc
	icetime -d $(DEVICE) -mtr $@ $<

%_syn.v: %.blif
	yosys -p 'read_blif -wideports $^; write_verilog $@'

clean:
	rm -f $(PROJ).blif $(PROJ).asc $(PROJ).rpt $(PROJ).bin $(PROJ)_0.bin $(PROJ)_1.bin fw.bin

.SECONDARY:
.PHONY: all clean
