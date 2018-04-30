# ENVs
CCX_VER = 2.14
PROGSDIR = ${HOME}/Calculix
MY_LIB = ${PROGSDIR}/lib
MY_INCLUDE =${PROGSDIR}/include

CFLAGS = -w -O3 -L $(MY_LIB) -I $(MY_INCLUDE) -DARCH="Linux" -DMATRIXSTORAGE -lm -lc -lpthread -ldl
FFLAGS = -w -O3 -fopenmp -m64 $(OPTIONS)
LIBS = 

# Spooles
CFLAGS += -I $(MY_INCLUDE)/spooles -DSPOOLES
LIBS += $(MY_LIB)/spooles.a

# Multithreaded spooles
CFLAGS += -DUSE_MT
LIBS += $(MY_LIB)/spoolesMT.a

# ARPACK
LIBS += $(MY_LIB)/libarpack_linux.a
CFLAGS += -DARPACK

CC=gcc
FC=gfortran

OPTIONS = -w -fno-second-underscore -fcray-pointer -x f77-cpp-input
FPPSTOP=-x none

.SUFFIXES: .o .c .cu
.c.o :
	$(CC) $(CFLAGS) -c $<
.f.o :
	$(FC) $(FFLAGS) -c $<

include Makefile.inc

SCCXMAIN = ccx_$(CCX_VER).c

# OCCXCU = $(SCCXCU:.cu=.o)
OCCXF = $(SCCXF:.f=.o)
OCCXC = $(SCCXC:.c=.o)
OCCXMAIN = $(SCCXMAIN:.c=.o)

ccx_$(CCX_VER): $(OCCXMAIN) ccx_$(CCX_VER).a \
    ./date.pl; $(CC) $(CFLAGS) -c ccx_$(CCX_VER).c; \
    $(FC) $(FFLAGS) -o $@ $(FPPSTOP) $(OCCXMAIN) ccx_$(CCX_VER).a \
    -Wl,--start-group \
	$(LIBS) \
    -Wl,--end-group

ccx_$(CCX_VER).a: $(OCCXCU) $(OCCXF) $(OCCXC)
	ar vr $@ $?
