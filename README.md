# Encoding contact predictions in Stockholm alignments

This document is here to start a discussion on how we might add predictions of inter-residue contacts into a [STOCKHOLM formatted](https://en.wikipedia.org/wiki/Stockholm_format) multiple sequence alignment. 

Usually, a "contact" is defined as where two C-beta atoms are within 8 Angstoms of each other (and often contacts are only considered between residues that are at least 6 residues apart in sequence).

In this context, contact predictions are being made at the alignment level - ie predicting that alignment position `i` is in contact with alignment position `j`. So, positions `i` and `j` are numeric from 1 - n (where n is the number of positions in the alignment). The alignment itself could be used to map these numbers to a particular residue in a particular sequence.

### Suggestion 1:

If we are only considering this in a boolean context (`i` and `j` are either in contact or they are not) then a reasonably compact way of representing this sparse half-matrix could be: 

```
#=GF CP     <i>; <j>,<k>,...
#=GF CP     10; 25,37,...
```

Which would represent `10<->25`, `10<->37`, `...` with the convention being that contacts are ordered according to ascending values of `i`.

### Suggestion 2:

However, rather than a simple true/false, groups might wish to add meta-data to each contact prediction - eg predicted distance? likelihood?

If the keys used in this meta data are standard and unlikely to be changed (ie they are always *distance* and *probability*), then this could be represented like:

```
#=GF CP   <i> <j>   <distance>   <probability>
#=GF CP   10 25   8.0   0.85
#=GF CP   10 37   5.6   0.45
```

### Suggestion 3:

However, there might be advantages in keeping this more flexible by using a generally agreed upon set of keys (eg `DS=distance`, `PR=probability`)

```
#=GF CP   <i> <j> <key>=<value>,<key>=<value>
#=GF CP   10 25  DS=8.0,PR=0.85
#=GF CP   10 37  DS=5.6,PR=0.45
```


