## Linux boot time analsysis


`for file in ./*.txt; do cat $file | ./filter > $file.tsv;  done
`

Relatated changes

 - [Kernel changes:](https://github.com/Ngalstyan4/linux4.14.72/commit/305535d39dc20457a917aae26dc38261ab148c69)

 - [Firecracker changes:](https://github.com/princeton-sns/firecracker/commit/b525345001c16bacd1d848253be786f73441c142)
 
### Files:
- [boottime/data:](/measurements/boottime/data) Curated firecracker run logs that have info about runtime of each tracked function
- [boottime/\[executable\]](/measurements/boottime/):
  [measure](/measurements/boottime/measure) script is used to run firecracker, collect logs and kill it afterwards
  the other scripts are helpers
  After collecting all the necessary data with `measure`, run:
  ```bash
  for file in ./*.txt; do cat $file | ./filter > $file.tsv;  done
  ```
  to clean the output into TSV files
- [boottime/analysis.ipynb](/measurements/boottime/analysis.ipynb): Used for data analysis
### Findings: 
 - ubuntu kernel boot is about 2.5 times slower (91571ms vs 230000ms at `sfi_init_late`*)
 
      *ubuntu init script does not write to the special magic port so firecracker does not know when that happened
 - `setup_arch` and `check_bugs` (and `console_init` when it is included) are individual slowest function calls in the boot process
 - More detailed boot timing info can be found in [boottime/analysis.ipynb](/measurements/boottime/analysis.ipynb) or in [raw data tsv files](/measurements/boottime/data)
