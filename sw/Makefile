include common/Makefile.common

dirs = $(filter %/, $(wildcard [^_]*/))
SUBDIRS = $(subst /,,$(subst common,,$(dirs)))

.PHONY: $(SUBDIRS) common clean

all: $(SUBDIRS)

$(SUBDIRS): common
	@if [ -f $@/Makefile ]; then \
		$(MAKE) -C $@; \
	fi

common:
	$(MAKE) -C $@

update:
	@for i in $(dirs:%/=%); do \
		if [ -f "$$i/$$i.elf" ]; then \
			echo $$i && cd $$i; \
			$(OBJCOPY) -j .reset -j .text -O binary $$i.elf imem.bin; \
			$(OBJCOPY) -R .reset -R .text -O binary $$i.elf dmem.bin; \
			$(OBJCOPY) -O binary $$i.elf memory.bin; \
			$(OBJDUMP) -d $$i.elf > $$i.dis; \
			$(READELF) -a $$i.elf > $$i.symbol; \
			cd ..; \
		fi \
	done

clean:
	@for i in $(SUBDIRS) common _*; do \
		if [ -f $$i/Makefile ]; then \
			$(MAKE) -C $$i clean; \
		fi; \
	done

distclean: clean

