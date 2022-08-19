SRC_NAME=tcpdump-4.99.1
SRC_URL=https://www.tcpdump.org/release/$(SRC_NAME).tar.gz

CFLAGS += -Os -ffunction-sections -fdata-sections -fvisibility=hidden
LDFLAGS += -Wl,--gc-sections

all: download_test extract_test config_test build

download_test:
	( if [ ! -f $(SRC_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).tar.gz $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar zxf $(SRC_NAME).tar.gz; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	ac_cv_lib_rpc_main=no \
        ac_cv_path_PCAP_CONFIG="$(STAGEDIR)/bin/pcap-config" \
	./configure \
		--prefix=/usr \
		--without-crypto \
		--without-smi \
		--disable-smb \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

build:
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

clean:
	rm -f config_done
	rm -f tcpdump
	rm -rf $(SRC_NAME)

romfs:
	cp $(SRC_NAME)/tcpdump .
	$(STRIP) tcpdump
	$(ROMFSINST) /usr/sbin/tcpdump

