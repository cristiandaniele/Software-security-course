#Readme

This repository contains the material needed to prepare for the exam of Software Security at Radboud University (and much more).

**Erik Poll** (http://www.cs.ru.nl/~erikpoll/), a teacher of Software Security and an Associate professor at Radboud University, created these contents. 

The goal is to keep this repo updated through the years, **also with the help of the students**.

#How to contribute (and please, do it!)

You can contribute of course by fixing typos and by clarifying notions. 
However, the main contribution would be sharing with the community your experience and your sleepless nights letting the fuzzers work. 
So, you could:
1) **Create a new tutorial** (*like afl_osx.md*) that carefully explains how to install the fuzzer *in a specific SO*.
2) **Share your project using Docker**: would be amazing to collect all of your projects here making them reproducible!
   ```PS: Also an easy docker's guide is still missing!```
3) **By sending your ideas to improve the project**: do you have ideas, hints, or something you think could help your colleagues (and the community)? Shout them out!
So, dear student, feel free to push the edits or drop me an e-mail at cristian.daniele@ru.nl.

#Repository overview
## Let's keep this structure
```mermaid
flowchart TB
subgraph to keep updated!!!
    Project_guidelines --> azure_setup & project_guidelines

    Projects --collection of all the projects--> *folder_with_target_and_fuzzer_name*
    Tutorials --collection of all the tutorials--> *folder_with_fuzzer_name*
    end
```


    