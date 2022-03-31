# sudoku.py by Jupyter Notebook 

Description
===========

The sudoku.ipynb notebook has been written from the existing sudoku.py file
(see wcsp/tutorials/sudoku/sudoku.py) where some code (print...) has been
added in order to describe the sudoku problem, instance and resolution.

Conversions from sudoku.ipynb : sudoku.html, sudoku.pdf.

*************
BASE_DIR=$PWD
*************

Installs
========

Install Jupyter Notebook
------------------------

- pyvenv virtual environment creation :

cd $BASE_DIR

python3 -m venv ${BASE_DIR}/pyvenv

source ${BASE_DIR}/pyvenv/bin/activate

+ to solve "invalid command 'bdist_wheel'" problem :

pip3  install --upgrade pip wheel setuptools

pip3 install -r ${BASE_DIR}/requirement.txt

Required for conversions
------------------------

- Pandoc :

sudo apt-get install pandoc

- Linux: TeX Live

sudo apt-get install texlive-xetex texlive-fonts-recommended

sudo apt-get install texlive-generic-recommended

Install toulbar2 and pytoulbar2
-------------------------------

1. See wcsp/ways_to_install_toulbar2/build_commands.txt

=>

__PATH__TO__/toulbar2/build/bin/Linux/toulbar2

__PATH__TO__/toulbar2/build/lib/Linux/pytoulbar2.cpython-36m-x86_64-linux-gnu.so

2. Add pytoulbar2 and CFN.py :

source ${BASE_DIR}/pyvenv/bin/activate

cd ${BASE_DIR}/pyvenv/lib/python3.6/site-packages

ln -s __PATH__TO__/toulbar2/build/lib/Linux/pytoulbar2.cpython-36m-x86_64-linux-gnu.so pytoulbar2.cpython-36m-x86_64-linux-gnu.so


cd ${BASE_DIR}

ln -s __PATH__TO__/toulbar2/web/TUTORIALS/CFN.py CFN.py

Use Jupyter Notebook
====================

- pyvenv virtual environment activation

source ${BASE_DIR}/pyvenv/bin/activate

- run Jupyter Notebook, open sudoku.ipynb

cd $BASE_DIR

jupyter-notebook sudoku.ipynb

- conversions :

jupyter nbconvert --to html sudoku.ipynb

mv sudoku.html conversions/.

jupyter nbconvert --to pdf sudoku.ipynb

mv sudoku.pdf conversions/.

