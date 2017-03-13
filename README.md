<!-- README.md is generated from README.Rmd. Please edit that file -->
hexSticker: create hexagon sticker in R
=======================================

I write a script to create [ggtree sticker](https://github.com/jotsetung/BioC-stickers/tree/master/ggtree) purely in `R`. Laurent create the [make\_sticker](https://github.com/jotsetung/BioC-stickers/issues/12) function based on my script and pack it to the [sticker](https://github.com/lgatto/sticker) package.

I have some new ideas to add new features:

-   support base plot
-   support image file
-   etc.

To avoid breaking their existing code to make stickers, I create this package for my own needs.

Examples
--------

``` r
library(ggbio)
library(biovizBase)
library(Homo.sapiens)

library(TxDb.Hsapiens.UCSC.hg19.knownGene)
txdb <- TxDb.Hsapiens.UCSC.hg19.knownGene

data(genesymbol, package = "biovizBase")
wh <- genesymbol[c("BRCA1", "NBR1")]
wh <- range(wh, ignore.strand = TRUE)

gr.txdb <- crunch(txdb, which = wh)
## change column to  model
colnames(values(gr.txdb))[4] <- "model"
grl <- split(gr.txdb, gr.txdb$tx_id)
## fake some randome names
set.seed(2016-10-25)
names(grl) <- sample(LETTERS, size = length(grl), replace = TRUE)


## the random tree

library(ggtree)
n <- names(grl) %>% unique %>% length
set.seed(2016-10-25)
tr <- rtree(n)
set.seed(2016-10-25)
tr$tip.label = sample(unique(names(grl)), n)

p <- ggtree(tr, color='grey') + geom_tiplab(align=T, linesize=.05, linetype='dashed', size=1.2, color='grey')

##  align the alignment with tree
p2 <- facet_plot(p, 'Alignment', grl, geom_alignment, inherit.aes=FALSE, alpha=.6, size=.1, mapping=aes(), color='grey', extend.size=.1)
p2 <- p2 + theme_transparent() + theme(strip.text = element_blank())+xlim_tree(3.4)


#################################
library(hexSticker)
sticker(p2, package="ggtree", p_x=1, p_y=1.5, p_size=9, s_x=.85, s_y = .68, s_width=.95, s_height=.65)
```

> the produced file has dimension exactly for printing according to <http://hexb.in/sticker.html>

<img src="ggtree.png" height=300"/>