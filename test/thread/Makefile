include ../common.mak

ifneq (,$(findstring win,$(OS)))
TESTS:=
else
TESTS:=fiber_guard_page external_threads
endif

.PHONY: all clean
all: $(addprefix $(ROOT)/,$(addsuffix .done,$(TESTS)))

# segfault || bus error (OSX)
$(ROOT)/fiber_guard_page.done: $(ROOT)/%.done : $(ROOT)/%
	@echo Testing $*
	$(QUIET)$(TIMELIMIT)$(ROOT)/$* $(RUN_ARGS); rc=$$?; [ $$rc -eq 139 ] || [ $$rc -eq 138 ]
	@touch $@

$(ROOT)/external_threads.done: $(ROOT)/%.done : $(ROOT)/%
	@echo Testing $*
	$(QUIET)$(TIMELIMIT)$(ROOT)/$*
	@touch $@

$(ROOT)/%: $(SRC)/%.d
	$(QUIET)$(DMD) $(DFLAGS) -of$@ $<

clean:
	rm -rf $(GENERATED)
