# Example 3: Redirecting to Another Page #

This example introduces two things:

  1. the _import_ section of a simplate
  1. defining your own `response` object
  1. using `raise SystemExit`

As a contrived example, we will redirect from [/old.html](http://www.example.com/old.html) to [/new.html](http://www.example.com/new.html) after a delay of three seconds.


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
    (r'^.*\.html$', direct_to_simplate),
)

```


### simplate at /path/to/simplates/old.html ###

This simplate has three sections, all separated by this line:

```
#<!--===BREAK===-->
```

[Again](EgGreetingsProgramDynamic.md), this token was chosen so that it is syntax-highlighted as a comment in your editor whether you are using HTML or Python highlighting.

The first section is only run once, when the simplate is hit for the first time. Use it for doing Python imports and defining constants. The second section is a Django view, and the third section is a Django template. Any objects you define in the first (_import_) or second (_view_) sections become part of the context for the third (_template_) section.

Here is the content of our simplate for this example:

```
from django.http import HttpResponse


#<!--===BREAK===-->

response = HttpResponse()
response['Refresh'] = "3; url=/new.html"
raise SystemExit(response)


#<!--===BREAK===-->

<p>Redirecting to <a href="/new.html">new.html</a>.</p>
```

Notice here that we `raise SystemExit`. This always terminates execution of the _view_ section immediately. If you don't provide any arguments to `SystemExit`, then simplate processing proceeds as usual (an implicit `HttpResponse` is created, templating is applied, etc.). However, if you do provide an argument to `SystemExit`, it must be a Django `HttpResponse`. This object is then used to render the response back to the browser, giving you control over the response when you need it.

## Next Example: [Processing a Form](EgFormProcessing.md) ##