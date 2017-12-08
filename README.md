# sphinx-utilities

Created as part of a review of the [nilmtk](https://github.com/nilmtk/nilmtk) package.  The utilitity automatically documents the API code, including new features:

* add the copybutton, to remove the >>> from code examples (included in the conf.py file
* a template based summary table for modules with the following features:
   * automates the creation of rst files - only for separate members
   * parameterised to set a minimum number of members for summary tables
   * include module summary and class summaries (where present) with the module code, with each module on a separate page
      * jinja templates to allow for tailoring of the summaries
   * remove the Modules section from intermediate packages and subpackages
   
Credits
=======

The base code for creating autosummaries has been created from sphinx-autosummary generate.py

copybutton code is from [scitkit learn](https://github.com/scikit-learn/scikit-learn/tree/master/doc/themes/scikit-learn/static/js)

Sphinx intro
============

Sphinx is parameterised by the conf.py file - this is just a python script that sets everything up for the sphinx application.

It's also possible to alter the directives in the sphinx application which can also be done in the conf.py file.

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


