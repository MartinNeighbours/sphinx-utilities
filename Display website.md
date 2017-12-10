Working on html from an existing project.

https://help.github.com/articles/creating-project-pages-manually
https://stackoverflow.com/questions/8446218/how-to-see-an-html-page-on-github-as-a-normal-rendered-html-page-to-see-preview

Steps
-----

1. Fork a copy of the project
2. Clone the forked copy
3. Go to the local cloned copy root directory
   - git checkout --orphan gh-pages
   - git push -u origin gh-pages
   
To access a page (use copypath), just prefix with:   
https://\<user-name>.github.io/<path>

So a page with the path:   
nilmtk.github.io/nilmtk/master/index.html

Becomes::   
https://MartinNeighbours.github.io/nilmtk.github.io/nilmtk/master/index.html
