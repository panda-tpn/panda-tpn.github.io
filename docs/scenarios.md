---
title: Scenario files
layout: default
nav_order: 4
last_modified_date: 2025-07-29
---

## Scenario files

Scenario files can be used to limit state space generation to only the classes
needed for matching a given set of labeled transition sequences, together with
option `--scn` in PaNDA.  

Scenarios are based on a modified version of the `scn` format of Tina in which
we can use labels in addition to transition names, and where we can specify an
optional delay at the end of the scenario.

## Syntax

Scenarios are text files, with a `.scn` extension, that defines a sequence of
blocks, each formed from *atoms*.

### Atoms

The only form admitted for labels and transition names is any non empty string
of letters, digits, primes ('), and underscores (_), similar to what is called
ANAME in Tina formats.

The difference between names and labels is made using quotes, so "a" is a label,
whereas t1 is a transition name. The special label "i", used for silent,
unlabelled transitions, cannot be used in an atom.

    atom ::= ANAME | "ANAME"

### Blocks

A `.scn` file is basically a sequence of blocks of the following shape (all blocks
in a file have same shape):

```bnf
    block ::=  atom | atom@<time> | $<time>
````

where `<time>` is a nonnegative integer. Time information is used to specify an
absolute date since the start of execution.

`$<time>` should only appear as the last block in a scenario, and is optional.
It can be used to specify a current observation date. A nul observation delay,
`$0`, is treated the same has no block declaration. Every block after the first
occurrence of an observation time block, `$<time>`, is ignored.

All the blocks in a scenario should mention the same category of events, either
all are about labels, or all use transition names. In the case of an empty
scenario or if we only have a `$<time>` block, then we assume that the scenario
is labeled and we match all the classes reachable from the initial state by a
sequence of silent transitions.

Empty lines and lines beginning with `#` are considered comments.

Spaces and tabs are considered separators. Line breaks are significant in Tina,
since they are interpreted as marks by the *nd* stepper, but they have no
particular interpretation in PaNDA.

## Examples

<table>
  <tr>
    <th>scn file</th>
    <th>Comment</th>
  </tr>

  <tr>
<td markdown="1">
```bnf
t1@1 t3 t2 t5@3 t1@4
```
</td>
<td>An example of scenario matching transition names, where the first transition matched should fire at time 1 and have name t1.</td>
  </tr>

  <tr>
<td markdown="1">
```bnf
"a" "b"@3 $5
```
</td>
<td>An example of a labeled scenario matching sequences reachable at time 5, with
observable sequence of labels <code>a b</code>.</td>
  </tr>

</table>

## The scn format in Tina

The original `scn` format is used by several tools inside the Tina toolbox, see
the [Tina Formats manual
page](https://projects.laas.fr/tina/manuals/formats.html#14).

* the nd stepper to store/load histories;
* the selt model checker to save counter examples for replay by the stepper;
* the plan application, to analyse path timing.
