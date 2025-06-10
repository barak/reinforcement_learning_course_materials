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

handout-%.tex:
	echo "\PassOptionsToClass{handout}{beamer} \input{$(notdir $*)}" > $@

.PRECIOUS: $(patsubst %,handout-build-%,$(LECS))
.PRECIOUS: $(patsubst %,handout-build-%,$(LECS:.tex=.pdf))
.PRECIOUS: handout-build-all.tex
.PRECIOUS: handout-build-all.pdf

built/handout-%.pdf: handout-build-%.pdf
	ln --force $< $@

all: $(patsubst %,built/handout-%,$(LECS:.tex=.pdf))
all: built/handout-all.pdf
