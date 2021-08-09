---
layout: post
title: Dealing with ambiguous functions in R
categories: [R, Bioinformatics]
---

This month will be a short update, but I hope it’ll be a useful one.

Recently I encountered upon a strange error while knitting an RMarkdown document for an analysis report. It read:

```R
Quitting from lines 489-527 (tmpq5_o6n7f.generate_report.Rmd)
Error: All arguments must be named
Backtrace:
     x
  1. +-rmarkdown::render(...)
  2. | \-knitr::knit(...)
  3. |   \-knitr:::process_file(text, output)
  4. |     +-base::withCallingHandlers(...)
  5. |     +-knitr:::process_group(group)
  6. |     \-knitr:::process_group.block(group)
  7. |       \-knitr:::call_block(x)
  8. |         \-knitr:::block_exec(params)
  9. |           +-knitr:::in_dir(...)
 10. |           \-knitr:::evaluate(...)
 11. |             \-evaluate::evaluate(...)
 12. |               \-evaluate:::evaluate_call(...)
 13. |                 +-evaluate:::timing_fn(...)
 14. |                 +-base:::handle(...)
 15. |                 +-base::withCallingHandlers(...)
 16. |                 +-base::withVisible(eval(expr, envir, enclos))
 17. |                 \-base::eval(expr, envir, enclos)
 18. |                   \-base::eval(expr, envir, enclos)
 19. \-`%>%`(...)
 20.   +-base::withVisible(eval(quote(`_fseq`(`_lhs`)), env, env))
 21.   \-base::eval(quote(`_f
Execution halted

```

The “Error: All arguments must be named” silently pinged at the back of my mind as I got home from work and went to sleep. I mean, I did give this function an argument didn’t I? It’s definitely worked before, so why is it crashing now? Long story short, this error came from conflicting function names from different packages.

For those not familiar with coding, the premise is simple. You load 2 different libraries in R, and they both share a function with the same name. From R’s point of view, which do you prioritize? This is not too different from grabbing two different bags of tools, and they both contain slightly different hammers. For example, one could only hammer nails in, while the other has an additional function to remove nails from a board.

In my case, does R prioritize select() from the dplyr or annotationDB library? In this specific use, I was selecting columns from a dataframe so the dplyr select() would be most useful. I could append the library name to the function like dplyr::select() at every select() instance, but I knew I exclusively use dplyr anyways. I also exclusively used purrr’s reduce function similarly, so I’ll combine them together as shown:

```R
# install required libraries
library(devtools)
devtools::install_github("r-lib/conflicted")
library(conflicted)

# load libraries normally
library(here)
library(dplyr)
library(annotationDB)
library(Rcpp) # etc. 

# define function-library priorities
# structure: conflict_prefer(function, winning_package, losing_package = optional)
conflict_prefer(select, dplyr, AnnotationDbi)
conflict_prefer(reduce, purrr, IRanges)

```

This works because my R environment used AnnotationDbi’s select(), which takes in more arguments than dplyr’s, and thus throws the error “Error: All arguments must be named.” I was using reduce() from purrr to join many data frames together, and it doesn’t make sense to use IRange’s version.

You can proactively search for these redundant function names using conflict_scout:

```R
> conflict_scout(pkgs = c("dplyr", "AnnotationDbi"))
1 conflict:
* `select`: dplyr, AnnotationDbi

> conflict_scout(pkgs = c("purrr", "IRanges"))
1 conflict:
* `reduce`: purrr, IRanges

```

And if you don’t know a priori the possible combinations of packages, conflicted will announce any ambiguities like:

```R
> select
Error: [conflicted] `select` found in 2 packages.
Either pick the one you want with `::` 
* dplyr::select
* AnnotationDbi::select
Or declare a preference with `conflict_prefer()`
* conflict_prefer("select", "dplyr")
* conflict_prefer("select", "AnnotationDbi")

```

Running into ambiguous functions is something every computer programmer and bioinformatician will run into. Hopefully this can point you in a direction to solve your problems and make your code easier to read.