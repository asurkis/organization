build: pdf

pdf: main.tex out
	pdflatex --shell-escape --output-directory=out $<

out:
	mkdir -p out

clean:
	rm -rf out
