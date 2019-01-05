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
#      Build - Builds and links all source files
#      Clean - Removes all generated files
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
CC = arm-none-eabi-gcc
LD = arm-none-eabi-ld
TARGET = executable
LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
CFLAGS = -mcpu=$(CPU) -march=$(ARCH) -mthumb -mfloat-abi=$(FLOAT_ABI) \
	 -mfpu=$(FPU) --specs=$(SPECS) -Wall -Werror -g -O0 -std=c99 \
	 -D$(PLATFORM)

OBJS = $(SRCS:.c=.o)

%.o : %.c
	$(CC) -c $< $(CFLAGS) $(INCLUDES) -o $@

.PHONY: build
build: $(TARGET).out

$(TARGET).out : $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(INCLUDES) $(LDFLAGS) -o $@

.PHONY:clean
clean:
	rm -f $(OBJS) executable.map executable.out
