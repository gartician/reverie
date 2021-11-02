---
layout: page
title: Projects
permalink: /projects/
---

| Project                                                      | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [GoPeaks](https://github.com/maxsonBraunLab/gopeaks)         | Window-based peak caller written in Go for CUT&TAG data. Shout out to [jakevc](https://www.jakevc.com/about/) for writing most of it! |
| [CUT&TAG](https://github.com/maxsonBraunLab/cutTag-pipeline) | Align reads, call peaks, and find differential peaks across all unique combinations of conditions in CUT&TAG data. |
| [GoPeaks-Compare\*](https://github.com/maxsonBraunLab/gopeaks-compare) | Benchmark GoPeaks with peak callers MACS2 and SEACR for CUT&TAG data [Insert DOI]. |
| [ATAC-Seq\*](https://github.com/maxsonBraunLab/atac_seq)      | Find differential open-chromatin regions between all unique combinations of conditions in paired-end ATAC-Seq data. Both DiffBind and default DESeq2 are used in case some experiments do not fit DESeq2's assumptions (e.g. global change in ATAC signal). |
| [CITE-Seq\*](https://github.com/maxsonBraunLab/cite_seq)      | Import output of CellRanger into this secondary analysis pipeline to assess quality control, integrate samples together, cluster with UMAP, and test for differential expression + GO with various schemes. |
| [RNA-Seq](https://github.com/maxsonBraunLab/Bulk-RNA-seq-pipeline-PE) | Assess differential gene expression in RNA-Seq data. Shout out to [JEstabrook](https://github.com/JEstabrook) for writing most of it! |
| [HINT-ATAC\*](https://github.com/maxsonBraunLab/hint-atac-old) | Implementation of [HINT-ATAC](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1642-2) by Zhijian, Li et. al. to find transcription factor footprints in ATAC-Seq data. This is deprecated. |

In addition to work-related projects, I try to contribute work to open-source projects like:

| Project                                                      | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [MultiQC Sambamba MarkDup](https://github.com/ewels/MultiQC/pull/1421) | Calculate duplication rate for paired and single-end sequencing libraries. |
| [MultiQC GoPeaks](https://github.com/ewels/MultiQC/pull/1562) | Parse the output log file for GoPeaks to display peak counts per sample to the user. |
| [HINT-ATAC](https://github.com/CostaLab/reg-gen/issues/164)  | Fixed sequencing depth normalization problems for ATAC-Seq footprinting. |

\*Most of this project was written by me.

_Projects are ranked from newest to oldest_