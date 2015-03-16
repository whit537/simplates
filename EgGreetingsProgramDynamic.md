# Example 2: Greetings, program! (Dynamic) #

This example is more complicated. It introduces:

  1. the concept of 'sections' in simplates
  1. serving multiple URLs from a single simplate
  1. accessing variables captured in a regex in urls.py

For this example, we want these:

```
http://www.example.com/greetings/python/
http://www.example.com/greetings/django/
http://www.example.com/greetings/perl/
```

to all be served by a single simplate at:

```
/path/to/simplates/greetings/program.html
```

And we want the output to vary based on the URL, like so:

```
Greetings, Python!
Greetings, Django!
Greetings, Perl!
```


How do we do this with simplates? Let's see ...


### settings.py ###

```
SIMPLATE_DIRS = (
     '/path/to/simplates',
)
```


### urls.py ###

```

from django.conf.urls.defaults import *
from simplates.views import direct_to_simplate


urlpatterns = patterns('',
    ( r'^greetings/(?P<program>[^/]+)/$'
    , direct_to_simplate
    , {'simplate':'greetings/program.html'}
     )
)

```


### simplate at /path/to/simplates/greetings/program.html ###

Our simplate has two sections, separated by this line:

```
#<!--===BREAK===-->
```

Simplates uses this token because it is syntax-highlighted as a comment in your editor whether you are using HTML or Python highlighting. (Note: to work efficiently with simplates, you'll want to set up a key mapping in your editor to quickly switch between these highlighting modes.)

Think of the first section as a _Django view_, and the second section as a _Django template_. Any objects you define in the first section (the view) become part of the context for the second (the template).

Here is the content of the simplate:

```
program = params['program'] # this is how you access urls.py regex parameters
program = program.title()   # convert to Title Case


#<!--===BREAK===-->

<p>Greetings, {{ program }}!</p>
```

You can see that we access the second URL part as captured in `urls.py`, and transform it to title case before inserting it into our template.


## Next Example: [Redirecting to Another Page](EgRedirect.md) ##