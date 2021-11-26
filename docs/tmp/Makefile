# Makefile for Sphinx documentation
#

# You can set these variables from the command line, and also
# from the environment for the first two.
SPHINXOPTS    ?=
SPHINXBUILD   ?= sphinx-build
SOURCEDIR     = source
#BUILDDIR      = build
BUILDDIR      = ../fab/doc


# Put it first so that "make" without argument is like "make help".
oldhelp:
	@echo ""
	@echo ""
	@echo "  Command 'make help' for documentation generation commands"
	@echo "  -------             -------------------------------------"
	@echo ""
	@$(SPHINXBUILD) -M help "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

.PHONY: help Makefile

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
%: Makefile
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

help:
	@echo " "
	@echo "*** Commands for main documentation :"
	@echo " "
	@echo "  - clean : "
	@echo "    make clean "
	@echo "    make cleanref "
	@echo " "
	@echo "  - generation : "
	@echo "    make ref "
	@echo "    make html "
	@echo "    make pdf "
	@echo " "

#
# ref (cleanref) added to build (clean) the documentation source of source code
#
REFPROJECTDIR      = source/ref/autogen/project
REFAPPSDIR         = source/ref/autogen/APPS

ref:
	sphinx-apidoc -F -H ws -A INRAE -f -o $(REFPROJECTDIR) ..
	sphinx-apidoc -F -H ws-apps -A INRAE -f -o $(REFAPPSDIR) ../ws/apps

cleanref:
	-rm -rf $(REFPROJECTDIR)
	-rm -rf $(REFAPPSDIR)

clean:
	-rm -rf $(BUILDDIR)/*
	echo "empty" > $(BUILDDIR)/__init__.py

