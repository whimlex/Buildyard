#!gmake
.PHONY: debug release ninja build clean clobber package tests

ifeq ($(wildcard Makefile), Makefile)
all:
	@$(MAKE) --no-print-directory -f Makefile $(MAKECMDGOALS)

clean:
	@$(MAKE) --no-print-directory -f Makefile $(MAKECMDGOALS)

.DEFAULT:
	@$(MAKE) --no-print-directory -f Makefile $(MAKECMDGOALS)

else

BUILD ?= Build

normal: $(BUILD)/Makefile
	@$(MAKE) --no-print-directory -C $(BUILD) AllBuild pngs

build: $(BUILD)/Makefile
	@$(MAKE) --no-print-directory -C $(BUILD)

all: debug release
clean:
	@-$(MAKE) --no-print-directory -C Build clean cleans
	@-$(MAKE) --no-print-directory -C Debug clean cleans
	@-$(MAKE) --no-print-directory -C Release clean cleans

packages: Release/Makefile
	@$(MAKE) --no-print-directory -C Release packages

tests: $(BUILD)/Makefile
	@$(MAKE) --no-print-directory -C $(BUILD) tests
endif

clobber:
	rm -rf Debug Release Build Ninja

debug: Debug/Makefile
	@$(MAKE) --no-print-directory -C Debug

Debug/Makefile:
	@mkdir -p Debug
	@cd Debug; cmake .. -DCMAKE_BUILD_TYPE=Debug

release: Release/Makefile | debug
	@$(MAKE) --no-print-directory -C Release

Release/Makefile:
	@mkdir -p Release
	@cd Release; cmake .. -DCMAKE_BUILD_TYPE=Release

ninja: Ninja/Makefile
	ninja $(MAKECMDFLAGS) -C Ninja

Ninja/Makefile:
	@mkdir -p Ninja
	@cd Ninja; cmake .. -DCMAKE_BUILD_TYPE=Debug -G Ninja

%/Makefile:
	@mkdir -p $*
	@cd $*; cmake ..

ifneq ($(wildcard Makefile), Makefile)

${BUILD}/projects.make: $(BUILD)/Makefile

-include ${BUILD}/projects.make

.DEFAULT:
	@$(MAKE) --no-print-directory $(BUILD)/Makefile
	@$(MAKE) --no-print-directory -C $(BUILD) $(MAKECMDGOALS)
endif
