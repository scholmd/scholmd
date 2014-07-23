.PHONY: all
all: dolphins.pdf submitted.pdf

dolphins.pdf: dolphins.tex	
	sed -i 's/\\includegraphics{}//' $^
	pdflatex $^

dolphins.tex: dolphins.md resources/plos-one.latex
	pandoc --filter pandoc-citeproc -S --standalone --template=resources/plos-one.latex -o $@ $<

%.pdf: %.tex
	pdflatex $^
	bibtex $(basename $^)
	pdflatex $^
	pdflatex $^

