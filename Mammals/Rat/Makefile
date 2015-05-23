all:: pics

# Optionally some PICS could be ignored. By default XXX ones.
# PICS_IGNORE must contain a rule for grep
PICS_IGNORE ?= "XXX"

# For every .svg we must have a pdf
PICS=$(shell find . -iname \*svg \
	| sed -e 's/svg/pdf/g' -e 's/\([^\]\)\([ \t:]\)/\1\\\\\2/g' \
	| grep -v -e $(PICS_IGNORE))

# Converted
PICS_CONVERTED=
SVGIS=$(shell  find . -iname \*.svgtune | sed -e 's/.svgtune/_tuned/g')

pics: $(SVGIS) $(PICS)

clean::
	-rm $(PICS_CONVERTED)
	for p in *.svg; do rm -f $${p%*.svg}.{pdf,eps,png}; done
	rm -fr *_tuned

.PHONY: ignore-%
ignore-%:
	@grep -q "^$*$$" .gitignore || { \
	  echo "$*" >> .gitignore; echo "Ignore $@"; }


#
# SVGTune
#
%_tuned: %.svgtune %.svg
	@echo "Splitting SVG using $<"
# Use gqview/geeqie to prevew svgs -- quite nice
	@svgtune $<
# Touch it to adjust the timestamp so make does not think that we are
# out of date later on
	@touch "$@"
# And assure that we have ignore ... cannot be in deps since would cause
# regeneration over and over again
	@$(MAKE) ignore-$@

#
# Inkscape rendered figures
#
%.pdf: %.svg ignore-%.pdf
	@echo "Rendering $@"
	@inkscape -z -f "$<" -A "$@"

%.eps: %.svg ignore-%.eps
	@echo "Rendering $@"
	@inkscape -z -T -f "$<" -E "$@"

%.png: %.svg ignore-%.png
	@echo "Rendering $@"
	@inkscape -z -f "$<" -e "$@" -d 150



.PHONY: all pics
