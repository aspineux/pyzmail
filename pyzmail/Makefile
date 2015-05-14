epydoc_target=docs/build/html/api
python3=/usr/bin/python3.4
2to3=/usr/bin/2to3-3.4

epydoc:
	mkdir -p ${epydoc_target}
	#--google-analytics "UA-21029552-4"
	epydoc -v -c epydoc.css --google-analytics "UA-21029552-4" -u ../index.html -n "pyzmail homepage" --html pyzmail -o ${epydoc_target}

sphinx:
	cd docs ; make html man
	cp ./docs/build/man/* man
	cp ./docs/build/html/man/* man

test:
	python setup.py test

test3:
	sudo rm -rf build ; ${python3} setup.py test

py3:
	mkdir -p py3k
	rm -rf py3k/*
	cp scripts/* py3k
	${2to3}  --no-diffs --write --nobackups py3k/*
	find py3k -type f -exec perl -pi -e "s=#!/bin/env python=#!${python3}=g" {} \;
	chmod a+x py3k/*
	cp -av pyzmail py3k
	${2to3}  --no-diffs --write --nobackups py3k

all: epydoc sphinx upload

docs: epydoc sphinx

clean:
	rm -rf build
	rm -rf docs/build
	rm -rf py3k
	rm -rf pyzmail/__pycache__
	rm -rf pyzmail/tests/__pycache__
	find . -name "*.pyc" -exec rm {} \;

upload:
	cp ./docs/build/html/_static/favicon32.ico docs/build/html/favicon.ico
	lftp -f upload.lftp

sdist: sphinx
	python setup.py sdist

pypi: sphinx
	python setup.py sdist upload

