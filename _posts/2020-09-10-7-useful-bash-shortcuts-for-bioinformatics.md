---
layout: post
title:  7 useful bash shortcuts for bioinformatics
categories: [Bash, Bioinformatics]
---

# Monitor SLURM jobs in real time

```bash
watch squeue -u $(whoami)

```

# Monitor file sizes in real time

```bash
watch du -shc *
```
# Monitor files as they are made or edited

```bash
watch tree .
```

# List files in long format, organized by time of edit, in human read-able format

```bash
ls -lrth
# l = long list format
# r = reverse order
# t = organize by time
# h = memory in human-readable format
```

# Count number of reads in all bam files in a directory

```bash
# in parallel
ls *.bam | parallel "echo {} && samtools view -c {}"

# in serial
for i in $(ls *.bam); do echo $i && samtools view -c $i; done

```

# Index all bam files in a directory

```bash
for i in $(ls *.bam); do echo "indexing $i" && samtools index -@ 4 -b $i; done
```

# Symlink many files to a directory

```bash
for i in $(find /path/of/interest/ -name *.gz | grep "more_filter_terms" | sort); do ln -s $i .; done
```
