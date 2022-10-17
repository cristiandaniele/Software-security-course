# Fuzzing group project

## Intro

Goal of this project is to become familiar with fuzzing in combination with memory sanatisers as way to do security testing. Trying out different fuzzers, with and without sanatisers, should also provide some comparison of the pros and cons and effectiveness of various approaches.

- **Step 1: Choose a target**
  For the assignment can target any software you want as the System-Under-Test (SUT). Only constraint is that it should be C(++), open source, and take some input in a complicated data or file format.  You can choose some old outdated software, where it should be easier to find bugs, or some newer up-to-date software, where finding bugs may be harder but also more interesting. To keep things simple, choose some software that can take a single file as input on the command line.
To make things rewarding, choose some software that takes a complex input format as input: the more complex the input format, the more likely that there are bugs in input handling and the more likely that fuzzing will detect them. One obvious choice here is software that handles graphical data.

- **Step 2: Get fuzzing**
  Try the fuzzers below, initially without but then with sanitizers, on your chosen application. 

Each group should at least try out

1. **afl**, or rather the more up-to-date version **afl++**, as the leading tool around;

2. one of the dumber fuzzers **Radamsa** or **zzuf**; and 

3. **HonggFuzz**, as another well-known smart fuzzer.

As sanatiser, at least try out **ASan**, but you can also try **MSan** or **valgrind**.

You are more than welcome to try out other fuzzers that you happen to know and are curious about. This project assignment is fairly open-ended. Make sure you all spend at least one full afternoon per week on it for the next 2 months. We will discuss progress and experiences in class, so that we can still shift or focus efforts depending on the results so far, or maybe cut things short if spending time does not seem that interesting. Final reports with a summary of your experiments and reflection on the tools will be due at the end of November/early December, so that we can still discuss this at one of the last lectures in December.

> NB: Document your experiments carefully When you run your experiments, document the set-up and input files precisely. Ideally you should be able to re-run experiment and reproduce the exact same results.

Optional things to try, depending on the experience with your specific
target applications, include:

- investigate (and fix?) any bugs you found;
- check bugs found against known CVEs;
- introduce bugs and see if fuzzers can find these;
- if you do not find bugs: test older releases;
- try different settings of the tools, or different initial seeds;

or other creatives ideas of your own!

## Fuzzers

More information and pointers about these fuzzers on tools page. In Brightspace there are discussion forums to exchange good and bad experiences on installing and using the tools. You can also have a look at [this very gentle introduction to zuff, ASan and afl](https://fuzzing-project.org/tutorials.html).

- [Radamsa](https://gitlab.com/akihe/radamsa)
Radamsa is a file-based mutational fuzzer written in Owl-Lisp, a dialect of LISP. It takes a set of valid example files as input and then outputs an arbitrary number of invalid files by mutating them. It does not feed these files to the target software. You can feed these files manually to the target software or set up some automation for this.

    *To install*:

    ```
    > git clone https://gitlab.com/akihe/radamsa
    > cd radamsa
    > make
    > sudo make install # optional, you can also launch radamsa by giving the full path to bin/radamsa
    > radamsa --help
    ```

- [zzuf](http://caca.zoy.org/wiki/zzuf)
zzuf is another mutational fuzzer, very similar in style to Radamsa. Indeed, it will be interesting to see if zuff and Radamsa produce very similar results. It is pretty old and it's not clear how actively it is being maintained. But it is simple, lightweight, and deterministic, so still worth a shot to try out.
    *To install*: On Linux, you can simply get it with

    ```sudo apt-get install zzuf```

    Alternatively, unpack the tar-ball from the git repo and run the standard sequence

    ```
    > ./bootstrap 
    >  ./configure 
    >  make
    >  sudo make install
    ```

- [afl](https://github.com/google/AFL) or [afl ++](https://github.com/AFLplusplus/AFLplusplus)
afl by Michael Zalewski is a 'smart' fuzzer that takes evolutionary approach to fuzzing: it mutates sample input files and then observes executions to see which mutations result in different code execution paths, to then prioritise these mutations over others. To observe the code execution path, the target software needs to be instrumented at compile time. (It is possible to use the tool if you do not have access to the source code by running the code in the QEMU emulator, but we're not going to use that option for this project.)
The original afl is no longer maintained, though it should still work. There is a newer daughter project afl++ that might prove more stable.
The [QuickStartGuide.txt](https://lcamtuf.coredump.cx/afl/QuickStartGuide.txt) provides a quick intro. There is a daughter project afl++ which may be easier to install and run than the original afl.
*To install*: The easiest way to install afl is with

  ```sudo apt-get install afl```
If that does not work on your Linux/Mac OS X machine, Peter Guttman's article Fuzzing Code with afl walks through the installation of both clang (as part of LLVM) and afl
The instruction for afl++ are slightly differently.
Unlike with the tools above, for afl you need to recompile your code before you can start fuzzing. This tutorial walks trough a simple example using afl. Peter Guttman's article Fuzzing Code with afl walks through the use of afl in combination with ASan. There is also an afl tutorial at fuzzing-project.org, which also discusses the use of afl with ASan. 
    > **NB Windows users**: Originally afl only ran on Linux and Mac OS X, as it uses *nix features. You can run afl in a VirtualBox VM, with a performance hit of course, but using a memory-instrumentation tool like ASan is then not an option. There is now also a Windows fork of afl. If you have any Linux or Mac OS X users in the group, obvious strategy would be to use afl on these systems and not to try this Windows fork. Or, if you could try winafl for comparison; they had to make some tweaks for winafl, not sure how these will affect results. There is also a recent - still experimental - port of ASan for Windows. By all means give it a try, if you like a challenge, but you'd probably want to ditch it if you hit any snags. Please use the Brightspace forum to report good or bad experiences, so that other groups can benefit from this.
- [CERT BFF](https://github.com/CERTCC/certfuzz)
Playing around with CERT BFF was not a success last year, so better give it a miss. BFF started off as a fuzzing framework built around zzuf, but has since evolved to use a different mutational fuzzer under the hood. BFF has grown out of the merger of two fuzzing tools, BFF for Linux/OSX and FOE for Windows, and includes several features to help not just in finding flaws but also analysing them: it tries to automate some of the configuration of the fuzzer, can try to to reduce fuzzed examples that cause crashes to a minimal size, and collects debugging information about crashes to help in analysis. Not sure if all this complexity makes the tool easier to use for this project than a bare bones tool like zzuf or harder...
*To install*: from the BFF download page you can get OS X and Windows installers and a UbuFuzz virtual machine. Or you can grab the source code from the github, but that is probably not for the faint-hearted.

- [HonggFuzz](https://github.com/google/honggfuzz)
HonggFuzz is another modern fuzzer. It has been reported to outperform afl(++) in many cases: it will be interesting to see if it will also be the case for the target you are looking at.

## Instrumentation tools

Below some instrumentation tools to try out. These should help to spot more errors when fuzzing. At least try out ASan, which will probably give you the best improvements for the effort and overhead. If you have trouble with ASan, you could use valgrind's Memcheck as a fallback.

- [AddressSanitizer (ASan)](https://github.com/google/sanitizers/wiki/AddressSanitizer)
ASan adds memory safety checks to C(++) code, both for spatial and temporal memory flaws. ASan is part of the LLVM clang compiler starting with version 3.1 and a part of GCC starting with version 4.8.

- [MemorySanitizer (MSan)](https://github.com/google/sanitizers/wiki/MemorySanitizer)
MSan adds memory safety checks to C(++) code to detect reading of uninitialised memory. MSan is available as a compile-time option in clang since version 4.0.

- [Valgrind](http://valgrind.org) 
  Valgrind is the classic instrumentation framework for building tools that do dynamic analysis. It provides several detectors to find different classes of bugs. Valgrind's Memcheck detector for memory errors would be interesting to try for these fuzzing experiments. (Valgrind provides many more detectors and tools, for instance detectors to spot thread errors, tools to profile heap usage, and many more.)

### Valgrind vs ASan/MSan

Valgrind is much slower than ASan and MSan, and has a much larger memory overhead. ASan does set up an insane amount of virtual memory (20 TB), but does not use all of it.
ASan can spot some flaws that valgrind's Memcheck cannot. Conversely, Memcheck can spot some flaws that ASan can't. For instance, ASan does not try to spot access to uninitialised memory, which Memcheck, like MSan, does. So using ASan together with MSan comes closer to what Memcheck does.

Memcheck can detect more problems than MSan. For instance, Memcheck tries to detect memory leaks, which MSan does not. ASan has some support to detect memory leaks, with LeakSanitizer, which is not enabled by default on all systems. For this project detecting memory leaks is probably not interesting: memory leaks typically occur when an application runs for a longer time, not when it is used one-off to process a single input file.

### afl-cmin and afl-tmin

afl comes with two helper programs that may be interesting to play with:
- For some initial set of input files (aka the initial test corpus) that you use to get the fuzzers started, afl-cmin tries to determine the smallest subset that still has the same code coverage as the full set. Code coverage here is measured using the instrumentation that afl adds to a program to observe its control flow and distinguish different execution paths. So, for example, if there are two input files that trigger exactly the same execution path, then afl-cmin will remove one of these files from this test corpus. This will speed up fuzzing, not just for afl but also for other mutation-based fuzzers, as mutating one of these files is likely to generate similar inputs as mutating the other, so having both in the test set is likely to double the amount work for the same outcome.
- Given a single input file that triggers a crash, afl-tmin tries to generate a smaller input file that still triggers this crash.

Note that afl-cmin and afl-tmin try very different forms of minimisation: afl-cmin tries to minimise a set of inputs and afl-tmin tries to minimise a single input.

## Report

You final report should:
- summarise the outcomes of your fuzzing experiments
- reflect on these outcomes and the tools

### Outcomes of the fuzzing experiments

For your fuzzing experiments, describe for each tool or each tool combination of fuzzer and sanitiser:

- how many initial files you used;
- how many mutations were generated;
- how much time this took;
- how many -- if any -- flaws were found.

You MUST include some tables to provide an overview of your results
with different tools/sanitiser combinations.  These could look like
the one below, but feel free to add columns and info depending on what
is revelant and interesting for your case study or some specific.
E.g. it may be interesting to also provide "crashes found/million
test cases" or "crashes found/hr" to compare results between
fuzzing campaigns.

| Number       | Tool           | Time    | No of test cases | issues found      |
| ------------ | -------------- | ------- | ---------------- |------------------ | 
| Experiment 1 | afl with ASan  | 2.5 hrs |  2.5 million     | 2 crashes, 1 hang |
| Experiment 2 | zzuf with ASan | 1.5 hrs |  134 k           | nothing           |

You MUST clearly name or number your experiments, so it is easy to see
the link with the text and the tables. This is useful to do in text
anyway, as it allows clear cross-references in the discussion of your
findings.

For any flaws found, possible issues to discuss would be:

- if different tools found the same flaws;
- if the same flaws are found if differents initial files are used or if a different random number seed is used;
- if any of the flaws are known CVEs, or other known bugs not reported as CVE but say reported on the application's website, github repo, or in release notes. Figuring out if some crash responds to some CVE might be really tricky to figure out, so don't feel obliged to dig into this for too long!
- if not, if they should be reported as new bugs.

### Reflection

Some research questions to reflect on are given below. (You do NOT have to stick to following these precise questions, feel free to reflect on other interesting issues that came up, or questions that were unresolved.)

- How good are the tools (or tool combinations) at finding flaws in these 'typical' applications? What does this tell us about the quality of the tools and/or about the quality of the applications tested?
- Is afl as modern evolutionary fuzzer much better than more basic mutation-based fuzzers? Or are the latter still good enough to find flaws in (at least some) applications?
- What are the overheads of instrumentation approaches? Does the improved detection rate with instrumentation outweigh the slower speed?
- How difficult are the fuzzing and instrumentation tools to set up for the application tested?
- How many test cases can be generated is a given time frame? Are there big differences here?
- How sensitive are tools to the initial set of valid inputs, or the initial random number seed? Does a given tool find the same bugs when given different initial inputs? Can different tools find the same bugs when given identical initial inputs? If you have more time to fuzz, it it more effective to simply let the tool run longer or to start with a larger initial sample size?
- Of course, the hope is that the tools do find some flaws. But even if they don't, this result - albeit a negative result - is still interesting.

If you find that the tools do not find flaws in the application you're testing, you may want to switch to an earlier release of the software, or to another application that takes the same input format. If the interfaces and input languages the same, swapping one application for another should be easy to do without having to change the configuration of tools and test set-ups.
