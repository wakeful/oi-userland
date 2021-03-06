#
# This file and its contents are supplied under the terms of the
# Common Development and Distribution License ("CDDL"), version 1.0.
# You may only use this file in accordance with the terms of version
# 1.0 of the CDDL.
#
# A full copy of the text of the CDDL should have accompanied this
# source.  A copy of the CDDL is also available via the Internet at
# http://www.illumos.org/license/CDDL.
#

#
# Copyright 2013 Andrzej Szeszo.  All rights reserved.
# Copyright 2016 Aurelien Larcher. All rights reserved.
#

include ../../../make-rules/shared-macros.mk

COMPONENT_NAME= gcc
COMPONENT_VERSION= 6.3.0
COMPONENT_REVISION= 3
COMPONENT_MAJOR_VERSION=	$(shell echo $(COMPONENT_VERSION) | $(GSED) -e 's/\([0-9]\+\)\.[0-9]\+.*/\1/')
THIS_GCC_PKGVERSION= $(COMPONENT_VERSION)-OI-$(COMPONENT_REVISION)
COMPONENT_FMRI= developer/gcc-6
COMPONENT_CLASSIFICATION= Development/C
COMPONENT_SUMMARY= GNU Compiler Collection
MPFR_NAME= mpfr
MPFR_VERSION=3.1.5
MPC_NAME=mpc
MPC_VERSION=1.0.3
GMP_NAME=gmp
GMP_VERSION=6.1.2
COMPONENT_SUMMARY= GNU Compiler Collection
COMPONENT_SRC= $(COMPONENT_NAME)-$(COMPONENT_VERSION)
COMPONENT_ARCHIVE= $(COMPONENT_SRC).tar.bz2
COMPONENT_ARCHIVE_HASH= \
  sha256:f06ae7f3f790fbf0f018f6d40e844451e6bc3b7bc96e128e63b09825c1f8b29f
COMPONENT_ARCHIVE_URL= \
  http://ftp.gnu.org/gnu/gcc/gcc-$(COMPONENT_VERSION)/$(COMPONENT_ARCHIVE)
COMPONENT_PROJECT_URL = http://gcc.gnu.org/
PATCH_EACH_ARCHIVE=1
PATCHDIR_PATCHES = $(shell find $(PATCH_DIR) $(PARFAIT_PATCH_DIR) -type f -name '$(PATCH_PATTERN)' \
                                2>/dev/null | sort) $(EXTRA_PATCHES)

COMPONENT_SRC_1=    $(MPFR_NAME)-$(MPFR_VERSION)
COMPONENT_ARCHIVE_1=    $(COMPONENT_SRC_1).tar.bz2
COMPONENT_ARCHIVE_URL_1= http://www.mpfr.org/$(COMPONENT_SRC_1)/$(COMPONENT_ARCHIVE_1)
COMPONENT_ARCHIVE_HASH_1= \
    sha256:ca498c1c7a74dd37a576f353312d1e68d490978de4395fa28f1cbd46a364e658
CLEAN_PATHS += $(COMPONENT_SRC_1)
COMPONENT_POST_UNPACK_ACTION_1 += ( $(RM) -r $(COMPONENT_SRC)/$(MPFR_NAME) && $(CP) -r $(COMPONENT_SRC_1) $(COMPONENT_SRC)/$(MPFR_NAME) )

COMPONENT_SRC_2= $(MPC_NAME)-$(MPC_VERSION)
COMPONENT_ARCHIVE_2= $(COMPONENT_SRC_2).tar.gz
COMPONENT_ARCHIVE_URL_2=  http://www.multiprecision.org/mpc/download/$(COMPONENT_ARCHIVE_2)
COMPONENT_ARCHIVE_HASH_2= \
    sha256:617decc6ea09889fb08ede330917a00b16809b8db88c29c31bfbb49cbf88ecc3
CLEAN_PATHS += $(COMPONENT_SRC_2)
COMPONENT_POST_UNPACK_ACTION_2 += ( $(RM) -r $(COMPONENT_SRC)/$(MPC_NAME) && $(CP) -r $(COMPONENT_SRC_2) $(COMPONENT_SRC)/$(MPC_NAME) )

COMPONENT_SRC_3= $(GMP_NAME)-$(GMP_VERSION)
COMPONENT_ARCHIVE_3= $(COMPONENT_SRC_3).tar.bz2
COMPONENT_ARCHIVE_URL_3=  http://ftp.gnu.org/gnu/gmp/$(COMPONENT_ARCHIVE_3)
COMPONENT_ARCHIVE_HASH_3= \
    sha256:5275bb04f4863a13516b2f39392ac5e272f5e1bb8057b18aec1c9b79d73d8fb2
CLEAN_PATHS += $(COMPONENT_SRC_3)
COMPONENT_POST_UNPACK_ACTION_3 += ( $(RM) -r $(COMPONENT_SRC)/$(GMP_NAME) && $(CP) -r $(COMPONENT_SRC_3) $(COMPONENT_SRC)/$(GMP_NAME) )

include $(WS_MAKE_RULES)/prep.mk
include $(WS_MAKE_RULES)/configure.mk
include $(WS_MAKE_RULES)/ips.mk

CC_BITS=
CFLAGS=         -g -O2

CONFIGURE_PREFIX= /usr/gcc/$(COMPONENT_MAJOR_VERSION)
PATH=/usr/gnu/bin:/usr/bin:/usr/perl5/bin

PARCH = $(MACH:i386=i386-pc)
GNU_ARCH = $(PARCH:sparc=sparc-sun)-solaris$(SOLARIS_VERSION)
PKG_MACROS += GNU_ARCH="$(GNU_ARCH)"

# Build recipe borrowed from OmniOS
COMMON_ENV=  LD_OPTIONS="-zignore -zcombreloc -i"
COMMON_ENV+= LD_FOR_TARGET=/usr/bin/ld
COMMON_ENV+= LD_FOR_HOST=/usr/bin/ld
COMMON_ENV+= LD=/usr/bin/ld

CONFIGURE_ENV+= $(COMMON_ENV)
COMPONENT_BUILD_ENV+= $(COMMON_ENV)
COMPONENT_INSTALL_ENV+= $(COMMON_ENV)

CONFIGURE_OPTIONS+= --sbindir=$(CONFIGURE_BINDIR.$(BITS))
CONFIGURE_OPTIONS+= --libdir=$(CONFIGURE_LIBDIR.$(BITS))
CONFIGURE_OPTIONS+= --libexecdir=$(CONFIGURE_LIBDIR.$(BITS))
CONFIGURE_OPTIONS+= --host $(GNU_ARCH)
CONFIGURE_OPTIONS+= --build $(GNU_ARCH)
CONFIGURE_OPTIONS+= --target $(GNU_ARCH)
#CONFIGURE_OPTIONS+= --with-boot-ldflags=-R$(CONFIGURE_PREFIX)/lib
CONFIGURE_OPTIONS+= --with-pkgversion="OpenIndiana $(THIS_GCC_PKGVERSION)"
CONFIGURE_OPTIONS+= --with-bugurl="https://bugs.openindiana.org"
CONFIGURE_OPTIONS+= --enable-plugins
CONFIGURE_OPTIONS+= --enable-objc-gc
CONFIGURE_OPTIONS+= --enable-initfini-array
CONFIGURE_OPTIONS+= --enable-languages=c,c++,fortran,lto,objc
CONFIGURE_OPTIONS+= --without-gnu-ld
CONFIGURE_OPTIONS+= --with-ld=/usr/bin/ld
CONFIGURE_OPTIONS+= --with-build-time-tools=/usr/gnu/$(GNU_ARCH)/bin
CONFIGURE_OPTIONS+= --disable-libitm

# On SPARC systems, use Sun Assembler
CONFIGURE_OPTIONS.sparc+= --without-gnu-as --with-as=/usr/bin/as
CONFIGURE_OPTIONS.i386+= --with-gnu-as --with-as=/usr/bin/gas
CONFIGURE_OPTIONS+= $(CONFIGURE_OPTIONS.$(MACH))

CONFIGURE_OPTIONS+= LDFLAGS="-R$(CONFIGURE_PREFIX)/lib"

COMPONENT_POST_INSTALL_ACTION = \
  $(RM) -r $(PROTO_DIR)$(CONFIGURE_PREFIX)/lib/gcc/$(GNU_ARCH)/$(COMPONENT_VERSION)/include-fixed

build: $(BUILD_32)

install: $(INSTALL_32)

test: $(TEST_32)

REQUIRED_PACKAGES += SUNWcs
REQUIRED_PACKAGES += system/library
REQUIRED_PACKAGES += system/library/math
# Required by test suite
REQUIRED_PACKAGES+= developer/test/dejagnu
REQUIRED_PACKAGES+= developer/build/autogen
