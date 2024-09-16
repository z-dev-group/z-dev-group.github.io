z: z
	sed 's/https:\/\/cdn.jsdelivr.net\/npm\/katex@0.16.11\/dist\/katex.min.css/.\/static\/css\/katex.min.css/g' index.html > temp1
	sed 's/<title>index<\/title>/<title>z语言文档<\/title>/g' temp1 > temp
	mv temp index.html
	rm temp1
clean:
	rm -rf index.html