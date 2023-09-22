# Using Weekly Coverage Report Diff Scripts
I use this script to generate a weekly email report that shows a diff of the current coverage with some older coverage (I have it set to 1 week older). This is very much a prototype script so don't expect it to be very finely polished but feel free to build on it.

## Instructions
0. Python packages necessary:
- Pandas
- Argparse

1. Copy the following scripts to your home area:
- `/home/cfaber/repos/ampere-github-stash/scripts/parse_groups.py`
- `/home/cfaber/repos/ampere-github-stash/scripts/GenerateDiffCoverageReport.py`
- `/home/cfaber/repos/ampere-github-stash/scripts/WeeklyCoverageDiffReport.sh`

2. Open WeeklyCoverageDiffReport.sh, and edit the following variables to your own workspace:
- `WORKSPACE` - The workspace you will run verdi from
- `LOCAL_COV` - Where to save artifacts the script will generate
- `SCRIPTS` - Where you saved the scripts you copied above

3. Run `WeeklyCoverageDiffReport.sh <COVERAGE_SCOPE> <VDB_FILE>`
The coverage scope is just a name you give the folder to work in. For example memc_lsu_merged_1w.
The first time this runs, it won't have anything to diff with so it will just create the folder `LOCAL_COV/COVERAGE_SCOPE` and create `old.pkl` from VDB file you gave it and diff against itself. It may produce something but it is mostly meaningless.
The next time you run, say at a later date, if you give it the same coverage scope, it will use that same folder and diff the coverage against the *old* coverage you previously ran the script with and print out some metrics.

I have this set up in a webcron job so that it runs every Monday and produces diff reports for 1W and 4W coverage that look like the following:
```
Weekly Coverage Report for memc_lsu_merged_1w
============================================
Date : September 21, 2023

Old Avg: SCORE    91.95237
New Avg: SCORE    90.465512
Diff: SCORE   -1.486858

Coverage Which Decreased >5% From Last Week
-----------------------------------------------------------

                                GROUP_NAME  SCORE_x  SCORE_y
17                    lsu_sobwake_count_cg   100.00    80.70
24                         stored_edwar_cg    43.90    41.46
25                           stored_far_cg    43.90    41.46
32                       llb_with_fills_cg    60.00     0.00
36                    lsu_sea_fault_cpm_cg    66.67    56.86
44         speculative_fault_sob_update_cg    73.68    63.16
45                              lsu_cmo_cg    74.09    56.96
48                         l2c_lsu_resp_cg    75.00    69.12
59                            pteatomic_cg    86.21    78.16
60                            pteatomic_cg    89.66    78.16
63   lsu_remote_tlbsync_and_lsu_quiesce_cg    87.50    62.50
68                      lsu_llb_ld_pipe_cg    89.47    31.58
91                           gretch_spr_cg    96.97    12.12
100                    lsu_chicken_bits_cg    99.06    72.77
134               force_nc_all_memtypes_cg   100.00     0.00
179               lsu_cpm_ld_wake_state_cg   100.00    90.91
218                     lsu_llb_st_pipe_cg   100.00    28.57
231                         lsu_mmu_req_cg   100.00    88.41
236           lsu_ofbreq_one_entry_mode_cg   100.00    64.29
280       lsu_tlbsync_fsm_special_cases_cg   100.00    76.92
288                 mlb_llb_interaction_cg   100.00    83.33
296               non_spec_store_sta_ld_cg   100.00    50.00
395                           st_fwd_nv_cg   100.00    85.71
412                        stored_hpfar_cg   100.00    64.58
414                  tag_zero_unchecked_cg   100.00     0.00
421                             uops_nt_cg   100.00    62.50

Coverage Less than 10%
--------------------------

                   GROUP_NAME  SCORE_x  SCORE_y
0                gretch_pq_cg     0.00     0.00
1            gretch_random_cg     0.00     0.00
6                  mte_spr_cg     0.21     0.21
7               mte_ata_el_cg     0.58     0.58
8    dtlb_parity_injection_cg     3.21     3.26
18               gretch_rt_cg     7.14     7.14
32          llb_with_fills_cg    60.00     0.00
134  force_nc_all_memtypes_cg   100.00     0.00
414     tag_zero_unchecked_cg   100.00     0.00

Coverage Removed Since Last Week
--------------------------------

                     GROUP_NAME  SCORE_x  SCORE_y
265  lsu_sobwake_count_STG_RETI    100.0      NaN

Coverage Added Since Last Week
-------------------------------

Empty DataFrame
Columns: [GROUP_NAME, SCORE_x, SCORE_y]
Index: []
```

The thresholds are all configurable as well. Ask me if you have any questions regarding this or if something doesn't work.