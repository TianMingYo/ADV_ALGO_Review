.PHONY: pdf clean

pdf:
	xelatex -interaction=nonstopmode -halt-on-error main.tex
	xelatex -interaction=nonstopmode -halt-on-error main.tex

clean:
	rm -f main.aux main.log main.out main.toc main.fdb_latexmk main.fls main.synctex.gz
