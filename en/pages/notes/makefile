# Produce HTML files from all Markdown files in a dir
# Uses grip; install from python-pip

# List all files to be made by finding all *.md and appending .html
HTMLs := $(patsubst %.md,%.md.html,$(wildcard *.md))


all : $(HTMLs)

# gen rule accepts html targets with corresponding md source and makes
%.md.html : %.md
	grip $< --export $@ --user yuhuanq

# remove all html outputs
clean :
	rm *.html

# remove all html outputs then build again
rebuild : clean all

