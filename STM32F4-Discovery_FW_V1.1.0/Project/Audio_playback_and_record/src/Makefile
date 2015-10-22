# Put your stlink folder here so make burn will work.
STLINK=/home/gjehle/Development/stlink

SRCS=  main.c  stm32f4xx_it.c  system_stm32f4xx.c usb_bsp.c  usbh_usr.c  waveplayer.c  waverecorder.c
#SRCS= audio_sample.c  main.c  stm32f4xx_it.c  system_stm32f4xx.c waveplayer.c waverecorder.c

# Binaries will be generated with this name (.elf, .bin, .hex, etc)
PROJ_NAME=audio_rec_repl

#######################################################################################

STM_COMMON=../../..

CC=arm-none-eabi-gcc
OBJCOPY=arm-none-eabi-objcopy

CFLAGS  = -g -O2 -Wall -Tstm32_flash.ld 
CFLAGS += -DUSE_STDPERIPH_DRIVER -DUSE_USB_OTG_FS -DSTM32F4XX -DMEDIA_USB_KEY 
CFLAGS += -mlittle-endian -mthumb -mcpu=cortex-m4 -mthumb-interwork
CFLAGS += -mfloat-abi=softfp -mfpu=fpv4-sp-d16
CFLAGS += -I. -I../inc

# Include files from STM libraries
CFLAGS += -I$(STM_COMMON)/Libraries/CMSIS/ST/STM32F4xx/Include
CFLAGS += -I$(STM_COMMON)/Libraries/CMSIS/Include 
CFLAGS += -I$(STM_COMMON)/Utilities/Third_Party/fat_fs/inc
CFLAGS += -I$(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/inc
CFLAGS += -I$(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Core/inc
CFLAGS += -I$(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Class/MSC/inc
CFLAGS += -I$(STM_COMMON)/Libraries/STM32_USB_OTG_Driver/inc
CFLAGS += -I$(STM_COMMON)/Utilities/STM32F4-Discovery


#-I C:\Keil_v5\ARM\RV31\INC 
#-I C:\Keil_v5\ARM\CMSIS\Include 
#-I C:\Keil_v5\ARM\Inc\ST\STM32F4xx 

# add startup file to build
SRCS += $(STM_COMMON)/Libraries/CMSIS/ST/STM32F4xx/Source/Templates/gcc_ride7/startup_stm32f4xx.s 
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_rcc.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_tim.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_spi.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_gpio.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/misc.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_dac.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_i2c.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_adc.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_syscfg.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_flash.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_exti.c
SRCS += $(STM_COMMON)/Libraries/STM32F4xx_StdPeriph_Driver/src/stm32f4xx_dma.c
SRCS += $(STM_COMMON)/Utilities/STM32F4-Discovery/stm32f4_discovery.c
SRCS += $(STM_COMMON)/Utilities/STM32F4-Discovery/stm32f4_discovery_lis302dl.c
SRCS += $(STM_COMMON)/Utilities/STM32F4-Discovery/stm32f4_discovery_audio_codec.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_OTG_Driver/src/usb_hcd_int.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_OTG_Driver/src/usb_hcd.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_OTG_Driver/src/usb_core.c
SRCS += $(STM_COMMON)/Utilities/Third_Party/fat_fs/src/fattime.c
SRCS += $(STM_COMMON)/Utilities/Third_Party/fat_fs/src/ff.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Core/src/usbh_core.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Core/src/usbh_ioreq.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Core/src/usbh_stdreq.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Core/src/usbh_hcs.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Class/MSC/src/usbh_msc_scsi.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Class/MSC/src/usbh_msc_bot.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Class/MSC/src/usbh_msc_core.c
SRCS += $(STM_COMMON)/Libraries/STM32_USB_HOST_Library/Class/MSC/src/usbh_msc_fatfs.c
#vpath %.c $(LIBSRC)
#SRCS += $(shell find $(LIBSRC) -name '*.c')
OBJS = $(SRCS:.c=.o)
.PHONY: proj

all: proj

proj: $(PROJ_NAME).elf

$(PROJ_NAME).elf: $(SRCS)
	$(CC) $(CFLAGS) $^ libPDMFilter_GCC.a -o $@ 
	$(OBJCOPY) -O ihex $(PROJ_NAME).elf $(PROJ_NAME).hex
	$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin

clean:
	rm -f *.o
	rm -f $(PROJ_NAME).elf
	rm -f $(PROJ_NAME).hex
	rm -f $(PROJ_NAME).bin


# Flash the STM32F4
burn: proj
	$(STLINK)/st-flash write $(PROJ_NAME).bin 0x8000000
