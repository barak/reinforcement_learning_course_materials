# build.py edited the \includeonly line in main.tex in place,
# called pdflatex -synctex=1 -interaction=nonstopmode main.tex
# and moved main.pdf to the appropriate target location.

# Here we use latexmk, and we create a bunch of "stub" files in order
# to avoid mutation of tracked files and also allow parallelism in
# their processing.

all: handouts slides
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

LECS = $(shell cd tex && echo *.tex)

built/%.pdf: build-%.pdf
	ln --force $< $@

BLECS = $(patsubst %,build-%,$(LECS))
slides: $(patsubst %,built/%,$(LECS:.tex=.pdf))
slides: built/all.pdf
.PHONY: slides

$(BLECS):
	echo "\includeonly{tex/$(subst build-,,$@)}\input{main.tex}" > $@

.PRECIOUS: $(BLECS:.tex=.pdf)
.PRECIOUS: build-all.pdf

build-handout-%.tex: build-%.tex
	echo "\PassOptionsToClass{handout}{beamer}\input{build-$*}" > $@

.PRECIOUS: $(patsubst %,build-handout-%,$(LECS))
.PRECIOUS: $(patsubst %,build-handout-%,$(LECS:.tex=.pdf))
.PRECIOUS: build-handout-all.tex
.PRECIOUS: build-handout-all.pdf

handouts: $(patsubst %,built/handout-%,$(LECS:.tex=.pdf))
handouts: built/handout-all.pdf
.PHONY: handouts
