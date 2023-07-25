:manuscript:
  :title: Hello, RSM!

# What is this?

ReStructured Manuscripts (RSM) is a set of tools for building web-first scientific manuscripts.  At the core of the RSM system lies the RSM markup language.  The text you see to your left is in RSM markup.

Writing RSM markup instead of LaTeX, Markdown, or ReST allows you to focus on your manuscript's content and leave the layout, styles, and display to RSM.

::

# Features

## Handrail

On the left-hand side of every heading you will see a gray border that we call /the handrail/.  The handrail shows a context menu for that specific part of the manuscript.  Many other parts of the manuscript have handrails\:

:remark:
  :label: rmk

This is a remark, and it has a handrail.

::

::

## Tooltips

Any part of the manuscript can be referenced.  For example, :ref:rmk,this:: refers to the remark above.  If you hover over the link text, you will see a tooltip showing the referenced part.

::

## Bibliography

RSM accepts bibliography items in BibTex syntax.  Here are a few examples :cite:knuth,turing::.  Note you can hover on the citation number to see the reference without scrolling the page. The reference list at the bottom has backlinks over which you can also hover, as well as links to DOI URLs, if available.

::

::

# More!

See :url:https://docs.write-rsm.org, the documentation:: to learn more about RSM.

::

:bibliography: ::

::


:bibtex:

@book{knuth,
  title={Art of computer programming, Volume 2, Seminumerical algorithms},
  author={Knuth, Donald E},
  year={2014},
  publisher={Addison-Wesley Professional},
  doi = {10.1137/1012065},
}

@article{turing,
  author = {Turing, Alan M},
  title = {Computing machinery and intelligence},
  journal = {Mind},
  year = {1950},
  doi = {10.1093/mind/LIX.236.433},
}

::
