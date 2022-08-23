SRC_NAME=udp2raw-tunnel
STAGING_DIR=$(STAGEDIR)

all:
	$(MAKE) -C $(SRC_NAME) mips24kc_le_asm_aes

clean:
	$(MAKE) -C $(SRC_NAME) clean

romfs:
	$(STRIP) $(SRC_NAME)/udp2raw_mips24kc_le_asm_aes
	$(ROMFSINST) $(SRC_NAME)/udp2raw_mips24kc_le_asm_aes /usr/bin/udp2raw
#	upx -9 -o $(ROMFSDIR)/usr/bin/udp2raw $(SRC_NAME)/udp2raw_mips24kc_le
