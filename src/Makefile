#******************************************************************************
# Copyright (C) 2019 by Jagadheesvaran T S
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Jagadheesvaran T S is not liable for any misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# Complex makefile for host as well as cortex-m4 build system
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.i - Generates <FILE>.i preprocessed output file
#      <FILE>.asm - Generates <FILE>.asm assembly output file
#      <FILE>.o - Builds <FILE>.o object file
#      compile-all - Builds all object files
#      build - Builds and links all source files
#      clean - Removes all generated files
#
# Platform Overrides:
#      PLATFORM - HOST or MSP432 
#
#
#------------------------------------------------------------------------------
include sources.mk

# Architectures Specific Flags
LINKER_FILE = ../msp432p401r.lds
CPU = cortex-m4
ARCH = armv7e-m
SPECS = nosys.specs
FLOAT_ABI = hard
FPU = fpv4-sp-d16

# Compiler Flags and Defines
ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	TARGET = c1m2
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
	CFLAGS = -mcpu=$(CPU) -march=$(ARCH) -mthumb -mfloat-abi=$(FLOAT_ABI) \
	 -mfpu=$(FPU) --specs=$(SPECS) -Wall -Werror -g -O0 -std=c99 \
	 -D$(PLATFORM)
	OBJDUMP = arm-none-eabi-objdump
else ifeq ($(PLATFORM),HOST)
	CC = gcc
	LD = ld
	TARGET = c1m2
	LDFLAGS = -Wl,-Map=$(TARGET).map
	CFLAGS = -Wall -Werror -g -O0 -std=c99 -D$(PLATFORM)
	OBJDUMP = objdump
else
$(warning Enter a valid platform (HOST or MSP432))
endif

OBJS = $(SRCS:.c=.o)

%.o : %.c
	$(CC) -c $< $(CFLAGS) $(INCLUDES) -o $@

DEPS = $(SRCS:.c=.d)

%.d : %.c
	$(CC) $< $(CFLAGS) $(INCLUDES) -MM -o $@

PREPS = $(SRCS:.c=.i)

%.i : %.c
	$(CC) $< $(CFLAGS) $(INCLUDES) -E -o $@

ASMS = $(SRCS:.c=.asm)

$(TARGET).asm : $(TARGET).out
	$(OBJDUMP) $< -D > $@

%.asm : %.c
	$(CC) $< $(CFLAGS) $(INCLUDES) -S -o $@

.PHONY : compile-all
compile-all : $(OBJS)
	$(info IF ERROR ARISES, TRY OUT "make clean" COMMAND BEFORE THIS COMMAND)

.PHONY : build
build : $(TARGET).out $(DEPS)

$(TARGET).out : $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(INCLUDES) $(LDFLAGS) -o $@
	$(info IF ERROR ARISES, TRY OUT "make clean" COMMAND BEFORE THIS COMMAND)


.PHONY:clean
clean:
	rm -f *.o *.d *.asm *.i c1m2.map c1m2.out
