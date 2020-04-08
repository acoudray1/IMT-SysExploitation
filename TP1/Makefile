

OS=$(shell uname)
COMPILATEUR=CPP_$(OS)
LIEUR=LD_$(OS)
COMPILATEUR_OPTION=COMPOP_$(OS)
LIEUR_OPTION=LIEUR_$(OS)
QEMU=$(QEMU_$(OS))

#-------------------
##Pour Mac
CPP_Darwin=i386-elf-g++
LD_Darwin=i386-elf-ld
#QEMU_Darwin=/sextant/qemu/Q.app/Contents/MacOS/i386-softmmu.app/Contents/MacOS/i386-softmmu
QEMU=qemu-system-i386 

#-------------------
##Pour Linux
CPP_Linux=g++
LD_Linux=ld
COMPOP_Linux=-fno-stack-protector -m32
LIEUR_Linux=-m elf_i386

#-------------------
##POUR WINDOWS
CPP_WindowsNT=g++
LD_WindowsNT=ld
 
#-------------------
## Partie commune a toutes les configurations


CPPFLAGS  = -gdwarf-2 -g3 -Wall -fno-builtin -fno-rtti -fno-exceptions -nostdinc $($(COMPILATEUR_OPTION))
LDFLAGS = --warn-common -nostdlib $($(LIEUR_OPTION))

PWD :=.
DELE = rm -rf
MV = mv -f


KERNEL_OBJ   = sextant.elf


# Main target
all: $(KERNEL_OBJ)


OBJECTS = 	hal/boot.o							\
	  		sextant/main.o
	  		
OBJ_FILES = $(wildcard build/all-o/*.o)

$(KERNEL_OBJ): $(OBJECTS)
	echo 'Votre compilateur $(COMPILATEUR) et votre lieur $(LIEUR)'
	$($(LIEUR)) $(LDFLAGS) -T ./support/linker.ld -o build/boot/$@ $(OBJ_FILES)

# Create objects from C source code
%.o: %.cpp
	$($(COMPILATEUR)) -I$(PWD) -c $< $(CPPFLAGS) -o $@
	$(MV) $@ build/all-o
	
# Create objects from assembler (.S) source code
%.o: %.S
	$($(COMPILATEUR)) -I$(PWD)  -c $< $(CPPFLAGS) -DASM_SOURCE=1 -o $@
	$(MV) $@ build/all-o


run:
	$(QEMU) -net nic,model=ne2k_isa -net user,tftp=./build/boot -cdrom ./build/boot/grub.iso

# Clean directory
clean:
	$(DELE) build/all-o/*.o
	$(DELE) build/boot/*.elf
	
