all : cbmc_copy cbmc_llvm.bc

include ../../../s2n.mk

CBMC_SRC= \
  ../../cbmc/proofs/s2n_stuffer_erase_and_read_bytes/s2n_stuffer_erase_and_read_bytes_harness.c \
  ../../cbmc/proofs/s2n_stuffer_read_bytes/s2n_stuffer_read_bytes_harness.c \
  ../../cbmc/proofs/s2n_stuffer_skip_read/s2n_stuffer_skip_read_harness.c \
  ../../cbmc/proofs/s2n_stuffer_copy/s2n_stuffer_copy_harness.c \
  ../../cbmc/proofs/s2n_stuffer_reread/s2n_stuffer_reread_harness.c \
  ../../cbmc/proofs/s2n_stuffer_erase_and_read/s2n_stuffer_erase_and_read_harness.c \
  ../../cbmc/proofs/s2n_stuffer_wipe/s2n_stuffer_wipe_harness.c \
  ../../cbmc/proofs/s2n_stuffer_init/s2n_stuffer_init_harness.c \
  ../../cbmc/proofs/s2n_stuffer_wipe_n/s2n_stuffer_wipe_n_harness.c \
  ../../cbmc/proofs/s2n_stuffer_free/s2n_stuffer_free_harness.c \
  ../../cbmc/proofs/s2n_stuffer_raw_read/s2n_stuffer_raw_read_harness.c \
  ../../cbmc/proofs/s2n_stuffer_alloc/s2n_stuffer_alloc_harness.c \
  ../../cbmc/proofs/s2n_stuffer_read/s2n_stuffer_read_harness.c \
  ../../cbmc/source/cbmc_utils.c \
  ../../cbmc/source/proof_allocators.c \
  ../../cbmc/source/make_common_datastructures.c \
  ../../cbmc/stubs/abort_override_assert_false.c

CBMC_COPY = $(notdir $(CBMC_SRC))
CBMC_BC = $(patsubst %.c,%.bc,$(CBMC_COPY))

.PHONY : cbmc_copy all clean

clean :
	- rm *.c
	- rm *.bc
	- rm *.ll

cbmc_copy :
	cp $(CBMC_SRC) .

%.bc:%.c
	$(CLANG) $(CFLAGS_LLVM) -I../../cbmc/include -I../../.. -I../../../api -I. -Wno-error -DCBMC -DCBMC_OBJECT_BITS=6 -o $@ $<

cbmc_llvm.bc : $(CBMC_BC)
	make -C .. bitcode
	${LLVMLINK} $(CBMC_BC) ../bitcode/all_llvm.bc -o $@
