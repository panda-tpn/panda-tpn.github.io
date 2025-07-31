---
title: Examples
layout: default
nav_order: 3
last_modified_date: 2025-07-31
---

## Default command

The default call to `panda` generates the SSCG for a Petri net in .net format
and prints some statistics on the resulting graph: number of classes; number of
different markings and domains; the number of transitions; and the total
computation time (in seconds). With option -v, we also print a textual
representation of the graph.

The PN models used in our examples are available on the [model
page](/docs/models/).

```console
panda -v docs/nets/abp.net
```

Output:

```console
class 0
  marking: p1 p5
  domain:
   0 ≤ t1 < ∞
  successors: t1:"i"/1

class 1
  marking: p2 p9 p5
  domain:
   0 ≤ t2 ≤ 0
   0 ≤ t7 ≤ 0
   0 ≤ t13 ≤ 0
  successors: t7:"i"/2 t13:"i"/3

class 2
  marking: p2 p6
  domain:
   0 ≤ t2 ≤ 1
   0 ≤ t8 ≤ 0
  successors: t8:"i"/4

[...]

16 classe(s), 14 marking(s), 6 domain(s), 22 transition(s)
0.000s
```

## Output in the Aldebaran (.aut) format

The following call generates the SCG in AUT format, using the labels of
transitions instead of their names. Files in the AUT format can be displayed by
the Tina toolbox editor, called nd. With option `--aut=2`, the result also
encodes state properties (typically the marking of places).

```console
panda --aut --lt docs/nets/mutex.net
```

Output:

```console
des(0,83,47)
(0,"a",1)
(0,"b",2)
(1,"",3)
(2,"",4)
(3,"",5)
(3,"b",6)
(4,"a",7)
(4,"c",8)
[...]
```

## Building the Modified SCG

To generate the MSCG we can use option `--mscg` (short form `-M`). Option
`--climit` (equivalently `-c`) can be used to stop the exploration after a fixed
number of classes.

For the MSCG, the statistics also include an extra pair of values. The first,
called max persistence, is the longest sequence of transitions during which the
same transition remained persistent. The second, between parenthesis, is the
size of the longest persistence history.

```console
panda -M -c=32 docs/nets/ifip.net -v
```

Output:

```console
class 0
  marking: p2*2 p1
  domain:
   4 ≤ t1 ≤ 9
   ———
   4 ≤ Δ(0,t1) ≤ 9
  successors: t1:"i"/1

class 1
  marking: p3 p5 p4
  domain:
   0 ≤ t2 ≤ 2
   1 ≤ t3 ≤ 3
   0 ≤ t4 ≤ 2
   0 ≤ t5 ≤ 3
   ———
   0 ≤ Δ(1,t2) ≤ 2
   1 ≤ Δ(1,t3) ≤ 2
   0 ≤ Δ(1,t4) ≤ 2
   0 ≤ Δ(1,t5) ≤ 2
  successors: t2:"i"/2 t3:"i"/3 t4:"i"/4 t5:"i"/5

[...]
```

## Building the subgraph matching a scenario

With a scenario file, we only explore the subset of the State Class Graph that
is compatible with the given sequence of events (or labels).

```console
panda --scn=docs/scn/mutex.scn docs/nets/mutex.net
```

When using the Modified SCG (options `-M` or `-R`), it is possible to print the
list of matching traces using the `--plan` (short `-P`) option. Together with
the verbose output (option `-v`), the tool also prints the associated timing
constraints for each trace.

```console
panda --scn=docs/scn/mutex.scn docs/nets/mutex.net -MPv
```

Output:

```console
reached state(s): 10, 11, 12, 13
traces:
  t1@1 t2 t4@5 t5 t6@6 $9   UNSAT
  constraints:
1 ≤ z1 ≤ 1
0 ≤ z2
1 ≤ z2 - z1 ≤ 3
5 ≤ z3 ≤ 5
0 ≤ z3 - z2 ≤ 3
0 ≤ z4
    z4 - z2 ≤ 4
1 ≤ z4 - z3
6 ≤ z5 ≤ 6
    z5 - z2 ≤ 4
0 ≤ z5 - z4 ≤ 3
9 ≤ zc ≤ 9
    zc - z2 ≤ 4
0 ≤ zc - z5 ≤ 3
  t1@1 t2 t3 t4@5 t5 t6@6 $9   SAT
  feasible: [z1: 1, z2: 4, z3: 5, z4: 5, z5: 6, z6: 6, zc: 9]
  constraints:
1 ≤ z1 ≤ 1
0 ≤ z2
1 ≤ z2 - z1 ≤ 3
0 ≤ z3

[...]
```

## Computing classes reachable with a given time horizon

Another possibility to limit the exploration of the state space is to specify a
time horizon (option `--date=[int]`, or `-d` for short). This is only available
when building the Modified SCG.

```console
panda -M docs/nets/simple_abp.net -d=3
```

It is possible to print the list of traces compatible with the given duration
using the `--plan` (short `-P`) option. Together with the verbose output (option
`-v`), the tool also prints the associated timing constraints for each trace.

```console
panda -M docs/nets/simple_abp.net -d=3 -P
```

Output:

```console
reached state(s): 1, 2, 3, 4, 5, 6
traces:
  t1
  t1 t4
  t1 t6
  t1 t7
  t1 t6 t8
  t1 t7 t8
9 classe(s), 5 marking(s), 6 dbm(s), 11 transition(s), 3 max. persistence
0.000s
```

## Graphical Output

For state graphs of limited size (typically less than 100 classes), it is
possible to display a graphical representation inside a Web browser; based on a
translation into a Mermaid diagram.

```console
panda -M -v --serve docs/nets/simple_1train.net
```

Output:

```console
7 classe(s), 5 marking(s), 4 dbm(s), 8 transition(s), 1 max. persistence
0.000s
Web Server is available at http://localhost:1313 (bind address 127.0.0.1)
Press Ctrl+C to stop
```
