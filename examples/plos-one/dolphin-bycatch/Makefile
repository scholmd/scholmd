.PHONY: all
all: dolphins.pdf

dolphins.pdf: dolphins.tex	
	sed -i 's/\\includegraphics{}//' $^
	pdflatex $^

dolphins.tex: dolphins.md plos-one.latex
	pandoc --filter pandoc-citeproc -S --standalone --template=plos-one.latex -o $@ $<
