# Experiment 1: Out order

```sim-outorder: SimpleScalar/Alpha Tool Set version 3.0 of August, 2003.
Copyright (c) 1994-2003 by Todd M. Austin, Ph.D. and SimpleScalar, LLC.
All Rights Reserved. This version of SimpleScalar is licensed for academic
non-commercial use.  No portion of this work may be used by any commercial
entity, or for any commercial purpose, without the prior written permission
of SimpleScalar, LLC (info@simplescalar.com).

warning: section `.comment' ignored...
sim: command line: ./sim-outorder -issue:inorder false cc1.alpha -O 1stmt.i 

sim: simulation started @ Fri Mar 19 14:33:23 2021, options follow:

sim-outorder: This simulator implements a very detailed out-of-order issue
superscalar processor with a two-level memory system and speculative
execution support.  This simulator is a performance simulator, tracking the
latency of all pipeline operations.

# -config                     # load configuration from a file
# -dumpconfig                 # dump configuration to a file
# -h                    false # print help message    
# -v                    false # verbose operation     
# -d                    false # enable debug message  
# -i                    false # start in Dlite debugger
-seed                       1 # random number generator seed (0 for timer seed)
# -q                    false # initialize and terminate immediately
# -chkpt               <null> # restore EIO trace execution from <fname>
# -redir:sim           <null> # redirect simulator output to file (non-interactive only)
# -redir:prog          <null> # redirect simulated program output to file
-nice                       0 # simulator scheduling priority
-max:inst                   0 # maximum number of inst's to execute
-fastfwd                    0 # number of insts skipped before timing starts
# -ptrace              <null> # generate pipetrace, i.e., <fname|stdout|stderr> <range>
-fetch:ifqsize              4 # instruction fetch queue size (in insts)
-fetch:mplat                3 # extra branch mis-prediction latency
-fetch:speed                1 # speed of front-end of machine relative to execution core
-bpred                  bimod # branch predictor type {nottaken|taken|perfect|bimod|2lev|comb}
-bpred:bimod     2048 # bimodal predictor config (<table size>)
-bpred:2lev      1 1024 8 0 # 2-level predictor config (<l1size> <l2size> <hist_size> <xor>)
-bpred:comb      1024 # combining predictor config (<meta_table_size>)
-bpred:ras                  8 # return address stack size (0 for no return stack)
-bpred:btb       512 4 # BTB config (<num_sets> <associativity>)
# -bpred:spec_update       <null> # speculative predictors update in {ID|WB} (default non-spec)
-decode:width               4 # instruction decode B/W (insts/cycle)
-issue:width                4 # instruction issue B/W (insts/cycle)
-issue:inorder          false # run pipeline with in-order issue
-issue:wrongpath         true # issue instructions down wrong execution paths
-commit:width               4 # instruction commit B/W (insts/cycle)
-ruu:size                  16 # register update unit (RUU) size
-lsq:size                   8 # load/store queue (LSQ) size
-cache:dl1       dl1:128:32:4:l # l1 data cache config, i.e., {<config>|none}
-cache:dl1lat               1 # l1 data cache hit latency (in cycles)
-cache:dl2       ul2:1024:64:4:l # l2 data cache config, i.e., {<config>|none}
-cache:dl2lat               6 # l2 data cache hit latency (in cycles)
-cache:il1       il1:512:32:1:l # l1 inst cache config, i.e., {<config>|dl1|dl2|none}
-cache:il1lat               1 # l1 instruction cache hit latency (in cycles)
-cache:il2                dl2 # l2 instruction cache config, i.e., {<config>|dl2|none}
-cache:il2lat               6 # l2 instruction cache hit latency (in cycles)
-cache:flush            false # flush caches on system calls
-cache:icompress        false # convert 64-bit inst addresses to 32-bit inst equivalents
-mem:lat         18 2 # memory access latency (<first_chunk> <inter_chunk>)
-mem:width                  8 # memory access bus width (in bytes)
-tlb:itlb        itlb:16:4096:4:l # instruction TLB config, i.e., {<config>|none}
-tlb:dtlb        dtlb:32:4096:4:l # data TLB config, i.e., {<config>|none}
-tlb:lat                   30 # inst/data TLB miss latency (in cycles)
-res:ialu                   4 # total number of integer ALU's available
-res:imult                  1 # total number of integer multiplier/dividers available
-res:memport                2 # total number of memory system ports available (to CPU)
-res:fpalu                  4 # total number of floating point ALU's available
-res:fpmult                 1 # total number of floating point multiplier/dividers available
# -pcstat              <null> # profile stat(s) against text addr's (mult uses ok)
-bugcompat              false # operate in backward-compatible bugs mode (for testing only)

  Pipetrace range arguments are formatted as follows:

    {{@|#}<start>}:{{@|#|+}<end>}

  Both ends of the range are optional, if neither are specified, the entire
  execution is traced.  Ranges that start with a `@' designate an address
  range to be traced, those that start with an `#' designate a cycle count
  range.  All other range values represent an instruction count range.  The
  second argument, if specified with a `+', indicates a value relative
  to the first argument, e.g., 1000:+100 == 1000:1100.  Program symbols may
  be used in all contexts.

    Examples:   -ptrace FOO.trc #0:#1000
                -ptrace BAR.trc @2000:
                -ptrace BLAH.trc :1500
                -ptrace UXXE.trc :
                -ptrace FOOBAR.trc @main:+278

  Branch predictor configuration examples for 2-level predictor:
    Configurations:   N, M, W, X
      N   # entries in first level (# of shift register(s))
      W   width of shift register(s)
      M   # entries in 2nd level (# of counters, or other FSM)
      X   (yes-1/no-0) xor history and address for 2nd level index
    Sample predictors:
      GAg     : 1, W, 2^W, 0
      GAp     : 1, W, M (M > 2^W), 0
      PAg     : N, W, 2^W, 0
      PAp     : N, W, M (M == 2^(N+W)), 0
      gshare  : 1, W, 2^W, 1
  Predictor `comb' combines a bimodal and a 2-level predictor.

  The cache config parameter <config> has the following format:

    <name>:<nsets>:<bsize>:<assoc>:<repl>

    <name>   - name of the cache being defined
    <nsets>  - number of sets in the cache
    <bsize>  - block size of the cache
    <assoc>  - associativity of the cache
    <repl>   - block replacement strategy, 'l'-LRU, 'f'-FIFO, 'r'-random

    Examples:   -cache:dl1 dl1:4096:32:1:l
                -dtlb dtlb:128:4096:32:r

  Cache levels can be unified by pointing a level of the instruction cache
  hierarchy at the data cache hiearchy using the "dl1" and "dl2" cache
  configuration arguments.  Most sensible combinations are supported, e.g.,

    A unified l2 cache (il2 is pointed at dl2):
      -cache:il1 il1:128:64:1:l -cache:il2 dl2
      -cache:dl1 dl1:256:32:1:l -cache:dl2 ul2:1024:64:2:l

    Or, a fully unified cache hierarchy (il1 pointed at dl1):
      -cache:il1 dl1
      -cache:dl1 ul1:256:32:1:l -cache:dl2 ul2:1024:64:2:l



sim: ** starting performance simulation **
warning: partially supported sigaction() call...
 label_rtx emit_jump expand_label expand_goto expand_goto_internal expand_fixup fixup_gotos expand_asm expand_asm_operands expand_expr_stmt clear_last_expr expand_start_stmt_expr expand_end_stmt_expr expand_start_cond expand_end_cond expand_start_else expand_end_else expand_start_loop expand_start_loop_continue_elsewhere expand_loop_continue_here expand_end_loop expand_continue_loop expand_exit_loop expand_exit_loop_if_false expand_exit_something expand_null_return expand_null_return_1 expand_return drop_through_at_end_p tail_recursion_args expand_start_bindings use_variable use_variable_after expand_end_bindings expand_decl expand_decl_init expand_anon_union_decl expand_cleanups fixup_cleanups move_cleanups_up expand_start_case expand_start_case_dummy expand_end_case_dummy pushcase pushcase_range check_for_full_enumeration_handling expand_end_case do_jump_if_equal group_case_nodes balance_case_nodes node_has_low_bound node_has_high_bound node_is_bounded emit_jump_if_reachable emit_case_nodes get_frame_size assign_stack_local put_var_into_stack fixup_var_refs fixup_var_refs_insns fixup_var_refs_1 fixup_memory_subreg walk_fixup_memory_subreg fixup_stack_1 optimize_bit_field max_parm_reg_num get_first_nonparm_insn parm_stack_loc assign_parms get_structure_value_addr uninitialized_vars_warning setjmp_protect expand_function_start expand_function_end
time in parse: 0.000000
time in integration: 0.000000
time in jump: 0.000000
time in cse: 0.000000
time in loop: 0.000000
time in cse2: 0.000000
time in flow: 0.000000
time in combine: 0.000000
time in sched: 0.000000
time in local-alloc: 0.000000
time in global-alloc: 0.000000
time in sched2: 0.000000
time in dbranch: 0.000000
time in shorten-branch: 0.000000
time in stack-reg: 0.000000
time in final: 0.000000
time in varconst: 0.000000
time in symout: 0.000000
time in dump: 0.000000
warning: partially supported sigprocmask() call...

sim: ** simulation statistics **
sim_num_insn              337317730 # total number of instructions committed
sim_num_refs              121890069 # total number of loads and stores committed
sim_num_loads              83200039 # total number of loads committed
sim_num_stores         38690030.0000 # total number of stores committed
sim_num_branches           58868344 # total number of branches committed
sim_elapsed_time                199 # total simulation time in seconds
sim_inst_rate          1695063.9698 # simulation speed (in insts/sec)
sim_total_insn            395832281 # total number of instructions executed
sim_total_refs            143427922 # total number of loads and stores executed
sim_total_loads           100697753 # total number of loads executed
sim_total_stores       42730169.0000 # total number of stores executed
sim_total_branches         68748010 # total number of branches executed
sim_cycle                 273291935 # total simulation time in cycles
sim_IPC                      1.2343 # instructions per cycle
sim_CPI                      0.8102 # cycles per instruction
sim_exec_BW                  1.4484 # total instructions (mis-spec + committed) per cycle
sim_IPB                      5.7300 # instruction per branch
IFQ_count                 621771764 # cumulative IFQ occupancy
IFQ_fcount                137618012 # cumulative IFQ full count
ifq_occupancy                2.2751 # avg IFQ occupancy (insn's)
ifq_rate                     1.4484 # avg IFQ dispatch rate (insn/cycle)
ifq_latency                  1.5708 # avg IFQ occupant latency (cycle's)
ifq_full                     0.5036 # fraction of time (cycle's) IFQ was full
RUU_count                2301604105 # cumulative RUU occupancy
RUU_fcount                 66079313 # cumulative RUU full count
ruu_occupancy                8.4218 # avg RUU occupancy (insn's)
ruu_rate                     1.4484 # avg RUU dispatch rate (insn/cycle)
ruu_latency                  5.8146 # avg RUU occupant latency (cycle's)
ruu_full                     0.2418 # fraction of time (cycle's) RUU was full
LSQ_count                 836259446 # cumulative LSQ occupancy
LSQ_fcount                 30797731 # cumulative LSQ full count
lsq_occupancy                3.0599 # avg LSQ occupancy (insn's)
lsq_rate                     1.4484 # avg LSQ dispatch rate (insn/cycle)
lsq_latency                  2.1127 # avg LSQ occupant latency (cycle's)
lsq_full                     0.1127 # fraction of time (cycle's) LSQ was full
sim_slip                 3273063551 # total number of slip cycles
avg_sim_slip                 9.7032 # the average slip between issue and retirement
bpred_bimod.lookups        72298952 # total number of bpred lookups
bpred_bimod.updates        58868344 # total number of updates
bpred_bimod.addr_hits      50988826 # total number of address-predicted hits
bpred_bimod.dir_hits       52581208 # total number of direction-predicted hits (includes addr-hits)
bpred_bimod.misses          6287136 # total number of misses
bpred_bimod.jr_hits         4974498 # total number of address-predicted hits for JR's
bpred_bimod.jr_seen         6303237 # total number of JR's seen
bpred_bimod.jr_non_ras_hits.PP       534787 # total number of address-predicted hits for non-RAS JR's
bpred_bimod.jr_non_ras_seen.PP      1713297 # total number of non-RAS JR's seen
bpred_bimod.bpred_addr_rate    0.8662 # branch address-prediction rate (i.e., addr-hits/updates)
bpred_bimod.bpred_dir_rate    0.8932 # branch direction-prediction rate (i.e., all-hits/updates)
bpred_bimod.bpred_jr_rate    0.7892 # JR address-prediction rate (i.e., JR addr-hits/JRs seen)
bpred_bimod.bpred_jr_non_ras_rate.PP    0.3121 # non-RAS JR addr-pred rate (ie, non-RAS JR hits/JRs seen)
bpred_bimod.retstack_pushes      5889100 # total number of address pushed onto ret-addr stack
bpred_bimod.retstack_pops      5595384 # total number of address popped off of ret-addr stack
bpred_bimod.used_ras.PP      4589940 # total number of RAS predictions used
bpred_bimod.ras_hits.PP      4439711 # total number of RAS hits
bpred_bimod.ras_rate.PP    0.9673 # RAS prediction rate (i.e., RAS hits/used RAS)
il1.accesses              427337385 # total number of accesses
il1.hits                  415514727 # total number of hits
il1.misses                 11822658 # total number of misses
il1.replacements           11822146 # total number of replacements
il1.writebacks                    0 # total number of writebacks
il1.invalidations                 0 # total number of invalidations
il1.miss_rate                0.0277 # miss rate (i.e., misses/ref)
il1.repl_rate                0.0277 # replacement rate (i.e., repls/ref)
il1.wb_rate                  0.0000 # writeback rate (i.e., wrbks/ref)
il1.inv_rate                 0.0000 # invalidation rate (i.e., invs/ref)
dl1.accesses              128826601 # total number of accesses
dl1.hits                  125361955 # total number of hits
dl1.misses                  3464646 # total number of misses
dl1.replacements            3464134 # total number of replacements
dl1.writebacks               901809 # total number of writebacks
dl1.invalidations                 0 # total number of invalidations
dl1.miss_rate                0.0269 # miss rate (i.e., misses/ref)
dl1.repl_rate                0.0269 # replacement rate (i.e., repls/ref)
dl1.wb_rate                  0.0070 # writeback rate (i.e., wrbks/ref)
dl1.inv_rate                 0.0000 # invalidation rate (i.e., invs/ref)
ul2.accesses               16189113 # total number of accesses
ul2.hits                   15272778 # total number of hits
ul2.misses                   916335 # total number of misses
ul2.replacements             912239 # total number of replacements
ul2.writebacks               207917 # total number of writebacks
ul2.invalidations                 0 # total number of invalidations
ul2.miss_rate                0.0566 # miss rate (i.e., misses/ref)
ul2.repl_rate                0.0563 # replacement rate (i.e., repls/ref)
ul2.wb_rate                  0.0128 # writeback rate (i.e., wrbks/ref)
ul2.inv_rate                 0.0000 # invalidation rate (i.e., invs/ref)
itlb.accesses             427337385 # total number of accesses
itlb.hits                 427220840 # total number of hits
itlb.misses                  116545 # total number of misses
itlb.replacements            116481 # total number of replacements
itlb.writebacks                   0 # total number of writebacks
itlb.invalidations                0 # total number of invalidations
itlb.miss_rate               0.0003 # miss rate (i.e., misses/ref)
itlb.repl_rate               0.0003 # replacement rate (i.e., repls/ref)
itlb.wb_rate                 0.0000 # writeback rate (i.e., wrbks/ref)
itlb.inv_rate                0.0000 # invalidation rate (i.e., invs/ref)
dtlb.accesses             129850867 # total number of accesses
dtlb.hits                 129802651 # total number of hits
dtlb.misses                   48216 # total number of misses
dtlb.replacements             48088 # total number of replacements
dtlb.writebacks                   0 # total number of writebacks
dtlb.invalidations                0 # total number of invalidations
dtlb.miss_rate               0.0004 # miss rate (i.e., misses/ref)
dtlb.repl_rate               0.0004 # replacement rate (i.e., repls/ref)
dtlb.wb_rate                 0.0000 # writeback rate (i.e., wrbks/ref)
dtlb.inv_rate                0.0000 # invalidation rate (i.e., invs/ref)
sim_invalid_addrs                 0 # total non-speculative bogus addresses seen (debug var)
ld_text_base           0x0120000000 # program text (code) segment base
ld_text_size                1564672 # program text (code) size in bytes
ld_data_base           0x0140000000 # program initialized data segment base
ld_data_size                 277104 # program init'ed `.data' and uninit'ed `.bss' size in bytes
ld_stack_base          0x011ff9b000 # program stack segment base (highest address in stack)
ld_stack_size                 16384 # program initial stack size
ld_prog_entry          0x0120025f70 # program entry point (initial PC)
ld_environ_base        0x011ff97000 # program environment base address address
ld_target_big_endian              0 # target executable endian-ness, non-zero if big endian
mem.page_count                  786 # total number of pages allocated
mem.page_mem                  6288k # total size of memory pages allocated
mem.ptab_misses              661679 # total first level page table misses
mem.ptab_accesses        2665173713 # total page table accesses
mem.ptab_miss_rate           0.0002 # first level page table miss rate
```