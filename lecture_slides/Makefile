# build.py edited the \includeonly line in main.tex in place,
# called pdflatex -synctex=1 -interaction=nonstopmode main.tex
# and moved main.pdf to the appropriate target location.

# Here we use latexmk, and we create a bunch of "stub" files in order
# to allow parallelism in their processing.

LECS = $(shell cd tex; echo *.tex)
BLECS = $(patsubst %,build-%,$(LECS))

all: $(patsubst %,built/%,$(LECS:.tex=.pdf))
all: built/all.pdf
.PHONY: all

# The "FORCE" trick here causes latexmk to be run even when the .pdf
# file seems up to date, because latexmk knows about all the
# dependencies, like figure graphics and included files.
LATEXMK = latexmk
LATEXMKFLAGS =
%.pdf: %.tex FORCE
	$(LATEXMK) $(LATEXMKFLAGS) -pdf $<
FORCE:
.PHONY: FORCE

$(BLECS):
	echo "\includeonly{tex/$(subst build-,,$@)}\input{main.tex}" > $@

.PRECIOUS: $(BLECS:.tex=.pdf)
.PRECIOUS: build-all.pdf

built/%.pdf: build-%.pdf
	ln --force $< $@
