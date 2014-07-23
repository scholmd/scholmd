This example was produced as part of the 
[Mozilla Science Lab Global Sprint 2014](http://mozillascience.org/)

## For the impatient

Run `make` to build two versions of the same manuscript. One will be called dolphins.pdf
and one submitted.pdf. The submitted.pdf is produced from the latex that was submitted to
Plos One for [this article](http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0064438)

The markdown derived version is quite similar to the submitted version, but currently fails in
a few areas, specifically in the construction of tables and the handling of internal
references. As things currently (July 2014) stand it would be challenging to author an entire
paper in markdown, although it would certainly be possible to author a significant fraction.

## Details

In this context _markdown_ refers to [Pandoc](http://johnmacfarlane.net/pandoc/README.html)
markdown.

Perhaps the key motivation for authoring scholarly papers in markdown is to
decouple the actual authoring from fiddling with the layout/presentation. But 
another important goal is to enable content to be authored once and made available
in a range of different formats. Pandoc is an excellent tool with which to try
and achieve this goal. However, it is not quite there yet so in this example 
when workarounds are considered the goal is to produce both a pdf and an html
version of this article.

### Requirements

This requires both latex and pandoc to be installed.

#### Ubuntu 14.04

    sudo apt-get install texlive-full haskell-plaform

A newer version of pandoc-citeproc is needed so you might as well install pandoc too
    
    sudo apt-get install build-essential
    cabal install pandoc pandoc-citeproc -j
    
If the cabal directory is not on the path:

    export PATH=$HOME/.cabal/bin:$PATH

#### Other platforms

Unfortunately I have not had the opportunity to test this on other platforms, but I
expect that it will be fairly straight forward on other linux platforms, reasonable on
a Mac and harder (but not impossible) on windows.

### The process

- __Create a YAML header__ Markdown files support a yaml header for metadata
  about the file. This is essentially a requirement for scholarly markdown.
  Use the header to include information such as:

    - Title
    - Authors
    - Addresses
    - Bibliography file
    - Journal specifc CSL (Citation formatting) file

    See more detail [here](https://github.com/scholmd/scholmd/wiki/Article-metadata)

- __Create a latex template__ The pandoc default latex template unlikely to be
  sufficient for a journal submission. This example uses a [template](resources/plos-one.latex)
  derived from the Plos One latex template 

- __Write an article__ Some of the markdown features used here are:

    - `# Section {-}` the `-` in braces will suppres section numbering
    - `[@citekey]` for citations
    - `&deg;` for degrees unicode symbol
    - Numbered lists `(@)` can have a tag `(@foo)` which can be used to refer
      to the item elsewhere.

### Issues

#### Tables

- Pandoc does not give you much control over the way in which tables are formatted.

- Pandoc markdown does not appear to support multi-column cells for multiple row headers

__Workaround__ Almost all tables will need to be included as both inline latex and
inline html if you wish to be able to output both latex and html.

#### Internal references

- Pandoc markdown does not _quite_ support all the syntax needed to cross-reference
  tables, figures and equations.

__Workaround__ There is a lengthly discussion about this in a 
[pandoc issue](https://github.com/jgm/pandoc/issues/813),
but until this is resolved it will be necessary to handle this manually.
Equations can be cross-referenced using the numbered list tag syntax, but
this only places numbers on the left of the equation.

#### Figures without captions

- The Plos One submission uses separate images and when you create and image 
  in markdown without an image file pandoc emits and empty includegraphics call
  which breaks the latex compilation.

__Workaround__ Use sed to replace remove the empty includegraphics calls before 
compiling to pdf.

#### Bibilography placement

- The bibliography is always placed at the end of the document and this is not actually
  what we want in this case.

__Workaround__ Manual copy and paste before submission? ...


