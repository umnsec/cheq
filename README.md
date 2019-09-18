# CheQ: Automatically Identifying Security Checks in OS Kernels

OS kernels enforce a large number of security checks to validate
system states. Security checks are in fact very informative in
inferring critical semantics in OS kernels. For example, 
security checks can reveal (1) whether an operation or a variable is
critical but can be erroneous, (2) what particular errors may occur,
and (3) constraints that should be enforced for the uses of a
variable or a function. Such information is particularly
valuable for detecting kernel semantic bugs because the detection
typically requires understanding critical semantics. 

The tool, CheQ, can help automatically identify security checks in OS
kernels.  We have used CheQ to detect hundreds of new bugs in the
Linux kernel.

We've upgraded CheQ to use LLVM 10.0.0 and Linux 5.3.0.
The kernel can now be compiled with O2 (the paper used O0) with
inlining completely disabled.  The code was tested on Ubuntu 18.04
64-bit.

## How to use CheQ

### Build LLVM 
```sh 
	$ cd llvm 
	$ ./build-llvm.sh 
	# The installed LLVM is of version 10.0.0 
```

### Build the CheQ analyzer 
```sh 
	# Build the analysis pass of CheQ 
	$ cd ../analyzer 
	$ make 
	# Now, you can find the executable, `kanalyzer`, in `build/lib/`
```
 
### Prepare LLVM bitcode files of OS kernels

* Replace error-code definition files of the Linux kernel with the ones
in "encoded-errno"
* The code should be compiled with the built LLVM
* Compile the code with options: -O0 or -O2, -g, -fno-inline
* Generate bitcode files
	- We have our own tool to generate bitcode files: https://github.com/sslab-gatech/deadline/tree/release/work. Note that files (typically less than 10) with compilation errors are simply discarded
	- We also provided the pre-compiled bitcode files - https://github.com/umnsec/linux-bitcode

### Run the CheQ analyzer
```sh
	# To analyze a single bitcode file, say "test.bc", run:
	$ ./build/lib/kanalyzer -sc test.bc
	# To analyze a list of bitcode files, put the absolute paths of the bitcode files in a file, say "bc.list", then run:
	$ ./build/lib/kalalyzer -sc @bc.list
```

## More details
* [The CheQ paper (ESORICS'19)](https://www-users.cs.umn.edu/~kjlu/papers/cheq.pdf)
```sh
@inproceedings{cheq-esorics19,
  title        = {{Automatically Identifying Security Checks for Detecting Kernel Semantic Bugs}},
  author       = {Kangjie Lu and Aditya Pakki and Qiushi Wu},
  booktitle    = {Proceedings of the 24th European Symposium on Research in Computer Security (ESORICS)},
  month        = September,
  year         = 2019,
  address      = {Luxembourg},
}
```
