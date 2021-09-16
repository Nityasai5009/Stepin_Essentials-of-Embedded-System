PROJ_NAME = SeatHeatApp
BUILD_DIR = Build
# All Source code files
SRC = SeatHeatingApp.c\
src/activity1.c\
src/activity2.c\
src/activity3.c\

#Object copy to create hexfile
OBJCOPY = avr-objcopy.exe
#Avrdude
AVRDUDE := avrdude
#Options for HEX file generation
HFLAGS = -j .text -j .data -O ihex
# All header file paths
INC = -I inc
# Find out the OS and configure the variables accordingly
ifdef OS	# All configurations for Windwos OS
   # Name of the compiler used
   CC = avr-gcc.exe
   # Correct the path based on OS
   FixPath = $(subst /,\,$1)
   # Name of the elf to hex file converter used
   AVR_OBJ_CPY = avr-objcopy.exe
   # Delete command 
   RM = del /q
else #All configurations for Linux OS
   ifeq ($(shell uname), Linux)
   	  # Delete command
      RM = rm -rf				
	  # Correct the path based on OS
      FixPath = $1				
	  # Name of the elf to hex file converter used
	  AVR_OBJ_CPY = avr-objcopy 
	  # Name of the compiler used
	  CC = avr-gcc
   endif
endif

# Command to make to consider these names as targets and not as file names in folder
.PHONY:all analysis clean doc

all:$(BUILD_DIR)
	# Compile the code and generate the ELF file
	$(CC) -g -Wall -Os -mmcu=atmega328 -DF_CPU=16000000UL $(INC) $(SRC) -o $(call FixPath,$(BUILD_DIR)/$(PROJ_NAME).elf)
	
hex: $(call FixPath,$(BUILD_DIR)/$(PROJ_NAME).elf)
	#create hex file
	$(OBJCOPY) $(HFLAGS) $< $(call FixPath,$(BUILD_DIR)/$(PROJ_NAME).hex)
	
doc:
	#Build the code code documentation using Doxygen command line utility
	make -C documentation

clean:
	# Remove all the build files and generated document files
	$(RM) $(call FixPath,$(BUILD_DIR)/*)
	make -C documentation clean
	rmdir $(BUILD_DIR)
$(BUILD_DIR):
	# Create directory to store the built files
	mkdir $(BUILD_DIR)

analysis: $(SRC)
	#Analyse the code using Cppcheck command line utility
	cppcheck --enable=all $^

