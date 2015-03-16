# Example 1: Greetings, program! #

This is the simplest example. We just want our homepage to show:

```
Greetings, program!
```

This example introduces:

  1. wiring simplates up to Django
  1. default documents


### settings.py ###

```
SIMPLATE_DIRS = (
     '/path/to/simplates',
)
SIMPLATE_DEFAULTS = ('index.html',)
```


### urls.py ###

```
from django.conf.urls.defaults import *
from simplates.views import direct_to_simplate


urlpatterns = patterns('',
    (r'^$', direct_to_simplate),
)
```


### simplate at /path/to/simplates/index.html ###

This is just a straight Django template. We could have used `direct_to_template`, except that doesn't give us default documents.

```
<p>Greetings, program!</p>
```


## Next Example: [Greetings, program! (Dynamic)](EgGreetingsProgramDynamic.md) ##