# Makefile for jflex documentation (ps + pdf + html)

#VERSION=1.6.1
VERSION=1.7.0-SNAPSHOT
RELEASE_DATE=16 March 2015
UNICODE_VER=7.0

all: pdf html

build: realclean all

pdf: manual.pdf
html: manual.html

md/manual.md: md/head.md md/intro.md md/installing.md md/ant-task.md md/example.md md/lex-specs.md \
              md/encodings.md md/performance.md md/porting-and-parsers.md md/end.md
	cat $^ > $@

manual.html: manual.mdx
	pandoc -S --css manual.css --filter pandoc-citeproc -f markdown $< -o $@
#	pandoc --filter pandoc-citeproc -f markdown --toc --toc-depth=2 $< -o $@

#dependency manual.tex would fire manual.md -> .tex conversion
xmanual.pdf: xmanual.tex intro.tex installing.tex example.tex lex-specs.tex \
             encodings.tex performance.tex porting-and-parsers.tex end.tex \
             ant-task.tex
	pdflatex xmanual
	bibtex xmanual
	pdflatex xmanual
	pdflatex xmanual

manual.pdf: xmanual.pdf
	cp $< $@

# variable substitutions:
%.mdx: md/%.md Makefile
	perl -pi -e 's/\$$VERSION/$(VERSION)/g' < $< | \
	perl -pi -e 's/\$$RELEASE_DATE/$(RELEASE_DATE)/g' | \
	perl -pi -e 's/\$$UNICODE_VER/$(UNICODE_VER)/g' > $@

%.tex: %.mdx
	pandoc -f markdown -t latex --biblatex $< -o $@

clean:
	rm -f *~ *.aux *.log log *.toc *.out *.bbl *.blg *.bcf

realclean: clean 
	rm -f *.mdx
	rm -f manual.html manual.pdf md/manual.md xmanual.pdf
	rm -f intro.tex installing.tex example.tex lex-specs.tex ant-task.tex
	rm -f encodings.tex performance.tex porting-and-parsers.tex end.tex
