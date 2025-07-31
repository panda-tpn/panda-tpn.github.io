---
title: Home
layout: home
nav_order: 1
last_modified_date: 2025-07-31
---

# PåNĐA: time Petri Nets Diagnosability Analyzer

PaNDA is a formal verification tool for the state estimation, prognosis and
diagnosability of time Petri nets (TPN).

![Demo with charmbracelet VHS](/assets/img/demo.gif){:style="display:block; margin-left:auto margin-right:auto"}

## Downloads

Release 0.3 (July 2025), see the [Download page](/docs/download/).  

|---------|--------|
| [panda_Darwin_arm64.tar.gz](/assets/releases/panda_Darwin_arm64.tar.gz)   | For macOS 13 (Ventura) or later, on Apple Silicon |
| [panda_Darwin_x86_64.tar.gz](/assets/releases/panda_Darwin_x86_64.tar.gz) | For 64-bit macOS 10.14 (Mojave) or later, on Intel Macs |
| [panda_Linux_arm64.tar.gz](/assets/releases/panda_Linux_arm64.tar.gz)     | For 64-bit Linux, ARM aarch64 executable, statically linked |
| [panda_Linux_x86_64.tar.gz](/assets/releases/panda_Linux_x86_64.tar.gz)   | For 64-bit Linux, x86-64 executable, statically linked |
| [panda_Windows_arm64.zip](/assets/releases/panda_Windows_arm64.zip)       | For 64 bit Windows PE32+ executable (console) on Aarch64|
| [panda_Windows_x86_64.zip](/assets/releases/panda_Windows_x86_64.zip)     | For 64 bit Windows, PE32+ executable (console) on x86-64 |

## Usage

By default, panda builds the strong State Class Graph (SCG) of a Time Petri net,
according to the technique implemented in the tool
[Tina](https://projects.laas.fr/tina/) (with option -S). The SSCG is an
abstraction of the state space of the net that preserves reachable markings and
traces, meaning the set of (untimed) execution sequences.

```shell
panda [flags] file
```

Panda takes a file, in the .net textual format used by Tina, and prints its
result on the standard output. We read the content of the file from the standard
input when using the special file name `-`. Panda can handle Petri nets with read
and inhibitor arcs but does not support priorities yet.

The basic flags are:

    -h, --help
        Print usage information.

    --version
        Print version number and generation date.

    -q, --quiet
        Block the display of statistics information.

    -v, --verbose int[=1]
        Textual output (use -v=2 for more info) (default none).

Build flags, to select the SCG construction:

    -S, --strong
        Generate the strong SCG, preserving states and LTL (default).

    -W, --linear
        Linear SCG, preserving LTL.

    -M, --mscg
        Modified SCG.

    -R, --rmscg
        Reduced version of the MSCG (prototype).

Stopping test and limit flags (mutually exclusive flags):

    -c, --climit int
        Limit on number of explored classes (default none).

    -s, --scn string
        Name of scenario file, e.g. --scn=aa.scn. Limit the generation of
        the SCG to the transition sequences matching the scenario.

    -d, --date int
        Time horizon for fault prognosis (default none).

Output format and options flags:

    -P, --plan
        List traces matching a scenario or a time horizon (only with -M and -R). 
        Add option -v to print the associated constraints systems.

    --aut int[=1]
       Output LTS in .aut format, as read by Aldebaran. Option --aut=2
       encodes state properties, see the manual pages of tina for option -wp.

    --lt
       Output labelled transition properties with --aut.

    --serve string[="1313"]
       Display LTS inside a Web page (default https://localhost:1313).

files:

* **infiles:** input file is stdin when using -

* **outfile:** output is always on stdout

* **errorfile:** error are always printed on stderr
