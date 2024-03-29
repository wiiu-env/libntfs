#---------------------------------------------------------------------------------
# Clear the implicit built in rules
#---------------------------------------------------------------------------------
.SUFFIXES:
#---------------------------------------------------------------------------------
ifeq ($(strip $(DEVKITPPC)),)
$(error "Please set DEVKITPPC in your environment. export DEVKITPPC=<path to>devkitPPC")
endif

ifeq ($(PLATFORM),wiiu)
    include $(DEVKITPRO)/wut/share/wut_rules
    MACHDEP += -D__WIIU__
endif

ifeq ($(PLATFORM),wii)
    include $(DEVKITPPC)/wii_rules
endif

ifeq ($(PLATFORM),cube)
    include $(DEVKITPPC)/gamecube_rules
endif

#---------------------------------------------------------------------------------
# BUILD is the directory where object files & intermediate files will be placed
# SOURCES is a list of directories containing source code
# INCLUDES is a list of directories containing extra header files
#---------------------------------------------------------------------------------
BUILD		?=	wiiu_release
SOURCES		:=	.
INCLUDES	:=	../include
LIBDIR		:=	../lib

#---------------------------------------------------------------------------------
# options for code generation
#---------------------------------------------------------------------------------
CFLAGS		=	-Wall -ffast-math -pipe  $(MACHDEP) $(INCLUDE) -ffunction-sections -DHAVE_CONFIG_H
CXXFLAGS	=	$(CFLAGS)
ASFLAGS		:=	-g
export NTFSBIN	:= $(LIBDIR)/$(PLATFORM)/libntfs.a

ifeq ($(BUILD),cube_debug)
CFLAGS += -DDEBUG
CXXFLAGS += -DDEBUG
endif

ifeq ($(BUILD),wii_debug)
CFLAGS += -DDEBUG
CXXFLAGS += -DDEBUG
endif

ifeq ($(BUILD),wiiu_debug)
CFLAGS += -DDEBUG
CXXFLAGS += -DDEBUG
endif

#---------------------------------------------------------------------------------
# any extra libraries we wish to link with the project
#---------------------------------------------------------------------------------
LIBS		:= -lwut

#---------------------------------------------------------------------------------
# list of directories containing libraries, this must be the top level containing
# include and lib
#---------------------------------------------------------------------------------
ifeq ($(PLATFORM),wiiu)
LIBDIRS		:= $(WUT_ROOT)/usr $(WUT_ROOT)
endif

#---------------------------------------------------------------------------------
# no real need to edit anything past this point unless you need to add additional
# rules for different file extensions
#---------------------------------------------------------------------------------
ifneq ($(BUILD),$(notdir $(CURDIR)))
#---------------------------------------------------------------------------------

export DEPSDIR	:=	$(CURDIR)/$(BUILD)

export VPATH	:=	$(foreach dir,$(SOURCES),$(CURDIR)/$(dir)) \
			$(foreach dir,$(DATA),$(CURDIR)/$(dir))

CFILES		:=	$(foreach dir,$(SOURCES),$(notdir $(wildcard $(dir)/*.c)))
CPPFILES	:=	$(foreach dir,$(SOURCES),$(notdir $(wildcard $(dir)/*.cpp)))
SFILES		:=	$(foreach dir,$(SOURCES),$(notdir $(wildcard $(dir)/*.s)))
BINFILES	:=	$(foreach dir,$(DATA),$(notdir $(wildcard $(dir)/*.*)))


export OFILES	:=	$(addsuffix .o,$(BINFILES)) \
			$(CPPFILES:.cpp=.o) $(CFILES:.c=.o) $(SFILES:.s=.o)

export INCLUDE	:=	$(foreach dir,$(INCLUDES),-I$(CURDIR)/$(dir)) \
			$(foreach dir,$(LIBDIRS),-I$(dir)/include) \
			$(foreach dir,$(LIBDIRS),-I$(dir)/include) \
            -I$(CURDIR)/$(BUILD) -I$(PORTLIBS)/include

export LIBPATHS	:= $(foreach dir,$(LIBDIRS),-L$(dir)/lib) $(PORTLIBS)/lib

.PHONY: $(BUILD) clean

#---------------------------------------------------------------------------------
$(BUILD):
	@[ -d $@ ] || mkdir -p $@
	@$(MAKE) --no-print-directory -C $(BUILD) -f $(CURDIR)/Makefile

#---------------------------------------------------------------------------------
clean:
	@echo clean ...
	@rm -fr wii_debug wii_release wiiu_release cube_debug cube_release $(LIBDIR)

all: $(NTFSBIN)

cube-install:
	cp ../include/ntfs.h $(DEVKITPRO)/libogc/include
	cp ../lib/cube/libntfs.a $(DEVKITPRO)/libogc/lib/cube

wii-install:
	cp ../include/ntfs.h $(PORTLIBS)/include
	cp ../lib/wii/libntfs.a $(PORTLIBS)/lib

wiiu-install:
	cp ../include/ntfs.h $(WUT_ROOT)/usr/include
	cp ../lib/wiiu/libntfs.a $(WUT_ROOT)/usr/lib

#---------------------------------------------------------------------------------
else

DEPENDS	:=	$(OFILES:.o=.d)

#---------------------------------------------------------------------------------
# main targets
#---------------------------------------------------------------------------------
$(NTFSBIN): $(OFILES) $(LIBDIR)/$(PLATFORM)
	@rm -f "../$(NTFSBIN)"
	@$(AR) rcs "../$(NTFSBIN)" $(OFILES)
	@echo built ... $(notdir $@)

$(LIBDIR)/$(PLATFORM):
	mkdir -p ../$(LIBDIR)/$(PLATFORM)

-include $(DEPENDS)

#---------------------------------------------------------------------------------------
endif
#---------------------------------------------------------------------------------------

