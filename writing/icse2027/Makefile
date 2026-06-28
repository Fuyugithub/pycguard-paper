MAIN = main
LATEXMK ?= /Library/TeX/texbin/latexmk

.PHONY: all clean distclean

all:
	$(LATEXMK) -pdf $(MAIN).tex

clean:
	$(LATEXMK) -c $(MAIN).tex

distclean:
	$(LATEXMK) -C $(MAIN).tex
