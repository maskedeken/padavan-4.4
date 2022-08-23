SRC_NAME=bind-9.18.5

all: extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar -xf $(SRC_NAME).tar.xz; \
		patch -d $(SRC_NAME) -p1 < bind9-orig.patch ; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	autoreconf -fi ; \
	./configure \
		--prefix=$(STAGEDIR) \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) \
		--with-libxml2=no \
		--enable-epoll=yes --with-gssapi=no --with-readline=no \
		--without-lmdb --without-cmocka \
		--disable-doh \
		--disable-geoip \
		--disable-linux-caps \
		PKG_CONFIG_PATH="$(STAGEDIR)/lib/pkgconfig" \
		OPENSSL_CFLAGS="-I$(STAGEDIR)/include" \
		OPENSSL_LIBS="-L$(STAGEDIR)/lib -lssl -lcrypto" \
		LIBUV_CFLAGS="-I$(STAGEDIR)/include" \
		LIBUV_LIBS="-L$(STAGEDIR)/lib -luv" ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

install:

romfs:
	cp -fP $(SRC_NAME)/lib/dns/.libs/libdns*.so $(ROMFSDIR)/lib
	cp -fP $(SRC_NAME)/lib/irs/.libs/libirs*.so $(ROMFSDIR)/lib
	cp -fP $(SRC_NAME)/lib/bind9/.libs/libbind9*.so $(ROMFSDIR)/lib
	cp -fP $(SRC_NAME)/lib/ns/.libs/libns*.so $(ROMFSDIR)/lib
	cp -fP $(SRC_NAME)/lib/isccfg/.libs/libisccfg*.so $(ROMFSDIR)/lib
	cp -fP $(SRC_NAME)/lib/isc/.libs/libisc.so $(SRC_NAME)/lib/isc/.libs/libisc-9*.so $(ROMFSDIR)/lib
	$(ROMFSINST) $(SRC_NAME)/bin/dig/.libs/dig /usr/bin/dig

