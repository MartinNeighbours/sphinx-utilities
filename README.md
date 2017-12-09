# sphinx-utilities

I have spent many years acquiring expertise in many different programming languages - as a relative newbie to python, I have discovered that it is sometimes difficult to quickly acquire the knowledge that you need.  I have a simple philosphy: I know what I need to do, find a way to do it and then build on this.  The best way to do this is by example.  I've grown to really appreciate the open source solutions offered by python, but am often frustrated by the length of time it takes to deliver some of what should be the simplest tasks and the half solutions offered by expert users that assume knowledge that I don't have.  I hope I won't create the same issues for others.

This utility was created as part of a review of the [nilmtk](https://github.com/nilmtk/nilmtk) package.  Starting fresh, I needed to understand what the package did and how it worked.  The tutorials were great, but only covered a small proportion of the functionality.  To understand it better I needed to understand the application and modules.  The API documentation had been created using sphinx directly from the docstrings using the sphinx-bootstrap theme.

It was often difficult to understand where you were in the documentation or to explore other methods and functions that seemed to be useful, with few code examples.

Many other applications had documentation which was easier to navigate such as [scipy](http://scikit-learn.org/stable/modules/classes.html) which I felt was good for discovery and navigation.

So to build on this I set out to combine the best of both worlds:
1. keep the automated process to eleminate effort as the application evolves or fixes are applied as no further development is planned for the application
2. provide summaries in each module and place the modules into separate pages for easier navigation
3. incorporate code examples and the 'copybutton' to make it simple to copy and paste sample code
4. Eliminate some of the section headings which appeared redundant in intermediate packages which provided structure to the application, but contained no modules i.e. Modules sections which were created with automodule, but were empty

Sphinx-autosummary is great and uses templates to deliver (2), but falls down on (1) as the autosummary needs to be explicit for each module and each class, creating a new rst file which can be included with the module file.  Whilst this process can be automated, it's messy and creates a lot of files, a better solution was required.

The autoautosummary solution offered by [clonker](https://stackoverflow.com/questions/20569011/python-sphinx-autosummary-automated-listing-of-member-functions) to automatically summarise class members is quite neat, but still falls down on (1) and whilst it could be automated, limits the ability to control whether summaries are provided for modules that don't really need it i.e. no point in providing a summary for a module that only has 1 or 2 public functions.

Autosummary contains a bug which prevents the documentation of class attributes

After much research (3) is quite simple and just requires javascript document and instruction in config.py to incorporate it.

(4) can be delivered through the solution to (2) and (1)

Solution
--------

The utilitity automatically documents the API code, including new features:

* add the copybutton, to remove/include the >>> from code examples (included in the conf.py file and copybutton.js in source/static)
* a template based summary table for modules with the following features:
   * automates the creation of rst files - only for separate members
   * parameterised to set a minimum number of members for summary tables
   * include module summary and class summaries (where present) with the module code, with each module on a separate page
      * jinja templates to allow for tailoring of the summaries
   * remove the Modules section from intermediate packages and subpackages
   
Credits
=======

The base code for creating autosummaries has been created from [sphinx-autosummary generate.py](https://github.com/sphinx-doc/sphinx/blob/master/sphinx/ext/autosummary/generate.py)

copybutton code is from [scitkit learn](https://github.com/scikit-learn/scikit-learn/tree/master/doc/themes/scikit-learn/static/js)

Sphinx intro
============

The simplest way to describe the workflow for sphinx is:

1. Create the environment
2. Create the rst files
3. Create html files

1. Create the environment
-------------------------

To do this, use:

sphinx-quickstart

This creates a series of prompts to provide an initial configuration (a good intro is from [Sam Nicholls](https://samnicholls.net/2016/06/15/how-to-sphinx-readthedocs/)

The configuration options populate config.py (in docs/source) and make.bat and makefile in the docs directory.

2. Create the rst files
-----------------------

sphinx-apidoc {opts}

Again to get going refer to [Sam Nicholls](https://samnicholls.net/2016/06/15/how-to-sphinx-readthedocs/), but ignore the section on make autodoc actually work and see below

3. create html files
--------------------

config.py
---------

Sphinx is parameterised by the conf.py file - this is just a python script that sets everything up for the sphinx application.

This scans through the app and creates .rst files for each module and package in the docs/source directory.  These are then used by the make command.

It's also possible to alter the directives in the sphinx application which can be done in the conf.py file.  copybutton is an example of this.

make.bat or makefile
--------------------

Make.bat (windows) and makefile (unix/Mac OS) do the same thing and simply run sphinx to generate outputs of different types (e.g. html/xml).  Both can be run with the relevant options from the command lines.  The build files will be generated in source/_build unless a different directory is specified in make.bat or makefile.

Note that if you install sphinx into an environment, first use::

Unix/Mac OS
source activate [environment]

Windows
activate [environment]

Before running make.


Paths
-----

One of the areas that appears to cause a great deal of consternation is why sphinx just can't seem to find the modules. 

The file that creates the html output is in a docs directory

The source files such as config.py and the rst files are in docs/source

If the application is in a different directory that is not in the standard path, then the path needs to be amended to refer to it.  This can be done with an absoluate or relative path.

As an example if I have the structure github/nilmtk/docs and gitub/nilmtk/nilmtk (which is where the application is located) then we need to include a path from source to github/nilmtk in the conf.py file i.e.:

sys.path.insert(0, os.path.abspath('..'))

This takes us from githib/nilmtk/doc to github/nilmtk - the nilmtk application is then read from this.


   
Jinja templates
===============

If, like me, reading reams of documentation on something that you just want to do a job is not your thing, then here's a quick intro to Jinja base on a shallow understanding - the best way is to learn by experimentation/example.  The start point has been the module.rst and class.rst templates that autosummary uses.  It looks pretty much like python enclosed in {% %} separated by text - which is pretty much what it is.  

The template is a mixture of code instructions and actual text to be included - where you put the code instruction on a particular line doesn't matter, what does is beginning and ending if statements, for loops or blocks.  It's a whole lot easier to read the template if you indent!

Where you put the text does matter and needs to follow the indent from the controlling e.g. autoclass unless the object is referred to with it's full name.  If you use the full name in autosummary, then this will be shown in the table which uses valuable real-estate - it's far better to just use the class name.


