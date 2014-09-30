# Installing NLTK on the IDL Servers



1. Make sure you’ve got python-dev:
`sudo apt-get install python-dev`

2. Install Setuptools: [http://pypi.python.org/pypi/setuptools](http://pypi.python.org/pypi/setuptools)

3. Install Pip: `run sudo easy_install pip`

4. Install Numpy (optional): `run sudo pip install -U numpy`

5. Install PyYAML and NLTK: r`un sudo pip install -U pyyaml nltk`

6. Test installation: run `python` then type `import nltk`

7. Then, get all the NLTK data:
`sudo python -m nltk.downloader -d /usr/share/nltk_data all`

_This has been done on **idlstage**, but not **idlrack**, and is a prerequisite to deploying Matchmaking_