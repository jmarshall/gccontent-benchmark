# Comparing string processing performance of programming languages

... using a simple bioinformatics task: Computing the GC fraction of DNA. It is based on the [GC content problem at Rosalind](http://rosalind.info/problems/gc/).

## Usage

```
make all
cat report.csv
```

## More info

This is a continuation of a previous benchmarking project, covered in [this blog post](http://saml.rilspace.com/moar-languagez-gc-content-in-python-d-fpc-c-and-c).

The idea is to compare the string processing performance of different programming languages
by implementing a very small a very simple algorithm and task: Read a [specific file](http://ftp.ensembl.org/pub/release-67/fasta/homo_sapiens/dna/Homo_sapiens.GRCh37.67.dna_rm.chromosome.Y.fa.gz)
containing DNA sequence in the [FASTA format](https://en.wikipedia.org/wiki/FASTA_format),
and compute the GC content in this file.

Two requirements apply:

1. The file must be read line by line (since DNA files are in reality ofter
   bigger than RAM, and this also helps make the implementations remotely
   comparable)
2. For each line, the program has to check if it starts with a `>` character,
   which if so means it is a header row and should be skipped.

The FASTA file can contain DNA letters (A,C,G,T) or unknowns (N), or new-lines
(Unix style `\n` ones).

This is it. Please have a look in the Makefile, and the various implementations
in the code directories, or send a pull request with your own implementation
(if the language already exists, increase the number one step, so for a new Go
implementation, you would create a `golang.001` folder, optionally with some
tag appended to it, like: `golang.001.table-optimized`, etc).

## Current results

These are some results (Execution times in seconds, smaller is better) from
running some of the tests in the Makefile, on a Dell Inspiron laptop with an
Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz, with Xubuntu 18.04 Bionic LTS 64bit
as operating system:

| Language                                                                                        | Execution time (s) | Compiler versions                                                         |
|-------------------------------------------------------------------------------------------------|-------------------:|---------------------------------------------------------------------------|
| [Rust.001](rust.001/src/main.rs)<br>Contributed by [@sstadick](https://github.com/sstadick)     |              0.067 | Rust 1.52.0-nightly (152f66092 2021-02-17)                                |
| [C](c/gc.c)                                                                                     |              0.080 | gcc (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0                                   |
| [D](d/gc.d)                                                                                     |              0.080 | The LLVM D compiler (1.22.0) (LLVM 10.0.0)                                |
| [Rust](rust/src/main.rs)<br>With improvements contributed by [@rob-p](https://github.com/rob-p) |              0.100 | Rust 1.52.0-nightly (152f66092 2021-02-17)                                |
| [Go](go/gc.go)                                                                                  |              0.110 | Go 1.14.4 linux/amd64                                                     |
| [C++](cpp/gc.cpp)                                                                               |              0.157 | g++ (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0                                   |
| [Crystal](crystal/gc.cr)                                                                        |              0.213 | Crystal 0.35.1 [5999ae29b] (2020-06-19) LLVM: 8.0.0                       |
| [PyPy](pypy/gc.py)                                                                              |              0.227 | PyPy 5.10.0 with GCC 7.3.0 (Python 2.7.13)                                |
| [Nim](nim/gc.nim)                                                                               |              0.243 | Nim Compiler Version 0.17.2 (2018-02-05)                                  |
| [FPC](gc.pas)                                                                                   |              0.350 | Free Pascal Compiler version 3.0.4+dfsg-18ubuntu2 [2018/08/29] for x86_64 |
| [Cython](cython/gc.pyx)                                                                         |              0.710 | Cython version 0.29.17                                                    |
| [Python](python/gc.py)                                                                          |              0.747 | Python 3.7.0                                                              |

## Acknowledgements

I got tons of help with the [previous blog post](http://saml.rilspace.com/moar-languagez-gc-content-in-python-d-fpc-c-and-c),
and I'm afraid I might miss to mention some people here who have helped out,
but see the below list as an (incomplete) start at collecting contributors, and
feel free to add any missing info, including yourself, here.

## Incomplete list of contributions

- [Seth "ducktape programmer"](https://github.com/sstadick)
  [contributed](https://github.com/samuell/gccontent-benchmark/pull/2) the top
  Rust version ([rust.001](rust.001/src/main.rs)).
- [Rob Patro](https://github.com/rob-p) contributed
  [improvements](https://github.com/samuell/gccontent-benchmark/pull/1) to the
  main Rust version, making it bump upward the list a few steps.
- Daniel Spångberg (working at UPPMAX HPC center at the time) contributed
  numerous, extremely fast implementations in C, including the one above (c),
  which is constrained by the requirement to process the file line by line.
- [Roger Peppe](https://github.com/rogpeppe)
  ([twitter](https://twitter.com/rogpeppe)) contributed the fastest Go
  implementation, including pointers in combination with a table lookup.
- [Mario Ray Mahardhika (aka leledumbo)](https://github.com/leledumbo)
  contributed the fastest FreePascal implementation, which is the one above
  (fpc.000).
- [Harald Achitz](https://www.linkedin.com/in/harald-achitz-860657139/)
  provided the C++ implementation used above (cpp.000).
- (Who is missing here?)
