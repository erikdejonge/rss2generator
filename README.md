# PyRSS2Gen-1.1

Forked from [http://dalkescientific.com/Python/PyRSS2Gen.html](http://dalkescientific.com/Python/PyRSS2Gen.html)

## A Python library for generating RSS 2.0 feeds.

Requires at least Python 2.3.  (Uses the datetime module for timestamps.)
Also works under Python 3.x



To install:

``` bash
% python setup.py install
```

This uses the standard Python installer.  For more details, read

[http://docs.python.org/inst/inst.html](http://docs.python.org/inst/inst.html)

(And there's only one file, so you could just copy it wherever you
need it.)

## Specs

[http://blogs.law.harvard.edu/tech/rss](http://blogs.law.harvard.edu/tech/rss)



## Example

### program

```python
import datetime
import PyRSS2Gen

rss = PyRSS2Gen.RSS2(
    title = "Andrew's PyRSS2Gen feed",
    link = "http://www.dalkescientific.com/Python/PyRSS2Gen.html",
    description = "The latest news about PyRSS2Gen, a "
                  "Python library for generating RSS2 feeds",

    lastBuildDate = datetime.datetime.now(),

    items = [
       PyRSS2Gen.RSSItem(
         title = "PyRSS2Gen-0.0 released",
         link = "http://www.dalkescientific.com/news/030906-PyRSS2Gen.html",
         description = "Dalke Scientific today announced PyRSS2Gen-0.0, "
                       "a library for generating RSS feeds for Python.  ",
         guid = PyRSS2Gen.Guid("http://www.dalkescientific.com/news/"
                          "030906-PyRSS2Gen.html"),
         pubDate = datetime.datetime(2003, 9, 6, 21, 31)),
       PyRSS2Gen.RSSItem(
         title = "Thoughts on RSS feeds for bioinformatics",
         link = "http://www.dalkescientific.com/writings/diary/"
                "archive/2003/09/06/RSS.html",
         description = "One of the reasons I wrote PyRSS2Gen was to "
                       "experiment with RSS for data collection in "
                       "bioinformatics.  Last year I came across...",
         guid = PyRSS2Gen.Guid("http://www.dalkescientific.com/writings/"
                               "diary/archive/2003/09/06/RSS.html"),
         pubDate = datetime.datetime(2003, 9, 6, 21, 49)),
    ])

rss.write_xml(open("pyrss2gen.xml", "w"))
```

### Output
The output does not contain newlines, so if you want to read it, you'll need to use your favorite XML tools to reformat it.

### Extend
RSS is not a fixed format.  People are free to add various metadata, like Dublin Core elements.

The RSS objects are converted to XML using the 'publish' method, which takes a **SAX2 ContentHandler**.  If you want different output, implement your own 'publish'.  The "simple" data types which takes a string, int, or date, can be replaced with a publishable object, so you can add metadata to, say, the "description" field.  To support new elements for RSS and RSSItem, derive from them and use the 'publish_extensions" hook.  To add your own attributes (needed for namespace declarations), redefine 'element_attrs' or 'rss_attrs' in your subclass.

### encoding
To use a different encoding, create your own ContentHandler instead of using the helper methods 'to_xml' and 'write_xml.'  You'll need to make sure the 'characters' method in the handler does the appropriate translation.


### categories
The "categories" list is somewhat special.  It needs to be a list and doesn't have a publish method.  That's because the RSS spec doesn't have an explicit concept for the set of categories -- an RSS2 channel can have 0 or more 'category' elements, but doesn't have a "list of categories" -- my "categories" attribute is an API fiction.

### License

This is copyright (c) by Andrew Dalke Scientific, AB (previously 'Dalke Scientific Software, LLC') and released under the BSD license. 

### Log

- #### changes for 1.1: Released August 25, 2012

  - Ported to Python 3.x. Thanks to Graham Bell for the initial patch.

- #### changes for 1.0: released november 6, 2005

  - Many people (Richard Chamberlain, Daniel Hsu, Leonart Richardson and Daniel Holth) pointed out that Guid sets "isPermaLink" (with a "L" not "l").  Fixed, and changed it so the isPermaLink RSS attribute is always either "true" or "false" instead of assuming empty means false.
  - Added patches from Erik de Jonge and MATSUNO Tokuhiro to set the output encoding.
  - Implemented a suggestion by Daniel Hoth to convert the enclosure length to a string.


- #### changes for 0.1.1: released in september 2003

  - retroactively renamed "0.0" to "0.1"
  - fixed bug in Image height.  Patch thanks to Edward Dale.

