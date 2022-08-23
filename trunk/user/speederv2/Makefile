SRC_NAME=UDPspeeder
STAGING_DIR=$(STAGEDIR)

all:
	$(MAKE) -C $(SRC_NAME) mips24kc_le

clean:
	$(MAKE) -C $(SRC_NAME) clean

romfs:
	$(STRIP) $(SRC_NAME)/speederv2_mips24kc_le
	$(ROMFSINST) $(SRC_NAME)/speederv2_mips24kc_le /usr/bin/speederv2
#	upx -9 -o $(ROMFSDIR)/usr/bin/speederv2 $(SRC_NAME)/speederv2_mips24kc_le
