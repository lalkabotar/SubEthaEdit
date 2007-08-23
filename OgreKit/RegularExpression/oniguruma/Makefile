ONIGURUMA_VERSION=onig-4.0.1
OS_VERSION=$(shell uname -r)
UNIVERSAL_DIR=universal
INTEL_DIR=intel
PPC_DIR=ppc
INTEL_CONFIGURE_FLAGS=--disable-dependency-tracking --host=i686-apple-darwin$(OS_VERSION) --build=i686-apple-darwin$(OS_VERSION)
PPC_CONFIGURE_FLAGS=--disable-dependency-tracking --host=ppc-apple-darwin$(OS_VERSION) --build=i686-apple-darwin$(OS_VERSION)
INTEL_SDK=$(SDKROOT)
INTEL_CC=gcc -isysroot $(SDKROOT)
PPC_SDK=$(SDKROOT)
PPC_CC=gcc -arch ppc -isysroot $(SDKROOT) 
ONIG_TARGET=libonig.la
LIBS_DIR=.libs
ONIGURUMA=libonig.a

all: $(UNIVERSAL_DIR)/$(ONIGURUMA)

$(UNIVERSAL_DIR)/$(ONIGURUMA): $(UNIVERSAL_DIR) $(PPC_DIR)/$(ONIG_TARGET) $(INTEL_DIR)/$(ONIG_TARGET)
	lipo -create $(INTEL_DIR)/$(LIBS_DIR)/$(@F) $(PPC_DIR)/$(LIBS_DIR)/$(@F) -output $(UNIVERSAL_DIR)/$(@F)

$(INTEL_DIR)/$(ONIG_TARGET): $(INTEL_DIR)
	cd $(@D);\
	CC="$(INTEL_CC)" ./configure $(INTEL_CONFIGURE_FLAGS);\
	perl -pi -e 's@CFLAGS =@CFLAGS = -isysroot $(INTEL_SDK) -arch i386@' Makefile;\
	perl -pi -e 's@-dynamiclib@-dynamiclib -arch i386@g' libtool;\
	make $(@F)

$(PPC_DIR)/$(ONIG_TARGET): $(PPC_DIR)
	export;\
	cd $(@D);\
	CC="$(PPC_CC)" ./configure $(PPC_CONFIGURE_FLAGS);\
	perl -pi -e 's@CFLAGS =@CFLAGS =@' Makefile;\
	perl -pi -e 's@-dynamiclib@-dynamiclib -arch ppc@g' libtool;\
	make $(@F)

$(INTEL_DIR):
	tar xzvf $(ONIGURUMA_VERSION).tar.gz
	mv $(ONIGURUMA_VERSION) $@

$(PPC_DIR):
	tar xzvf $(ONIGURUMA_VERSION).tar.gz
	mv $(ONIGURUMA_VERSION) $@

$(UNIVERSAL_DIR):
	mkdir $@

clean:
	rm -rf $(INTEL_DIR)
	rm -rf $(PPC_DIR)
	rm -rf $(UNIVERSAL_DIR)