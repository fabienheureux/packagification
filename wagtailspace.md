---
title: Packagification how we turned a Wagtail website into a Wagtail package
author: Fabien
---

# Hi

Since 2016  
Freelance software engineer  
Public sector, NGOs

<!-- 
speaker_note: |
Discovered Wagtail in 2016, initially as backend for next.js, quickly used it headless
Then went back from this approach and using it as a monolith since 2018
Worked with torchbox for a few months
Working for french government agencies since 2022
-->

<!-- end_slide -->

# Why
Que Faire de Mes Objets et Déchets  
- 2021 : original static gatsby website
- 2022 : the djangbo-based data-platform
- 2024 : migrate the legacy website into the django app
- 2025 : adopt Sites Conformes for a landing page
- 2026 : include Sites Conformes as a django dependency

<!-- 
speaker_note: |
Frontend coupled to a data-platform that helps citizens with RRR : reuse, recycle, repair (or trash)
We aggregate all french locations related to this topic, categorized by objects and type of service provided by these actors.
It is developped since 2022, based on django. 
Coupled to basic content pages, with fixed list of charfields in django.

Over the time, users wanted to add an image field.
That should be orderable. 
In the mean time, DINUM developped Sites Conformes.
In the mean time, our content editor shipped a Sites Conformes, as a landing page for the project. 

I suggested "let's add Wagtail, on top of django. Content editors will be at ease"
So we added a streamfield, with a single imagefield

Then, my content editors asked me "could we do things in the Que Faire that we do in Sites Conformes".
That got me thinking. 
Let's add Sites Conformes to our django project
-->


<!-- end_slide -->

# How 
I initially decided to simply declare Sites Conformes as a python package, following Wagtail docs pattern 
Then I realised that django app names was going to be an issue : 

```
sites_conformes
| .git
| blog
| content_manager
| events
| forms
```

We already had a forms app...
Same issue for the templates

```
sites_conformes
| .git
| templates
| | base.html
```

I then checked how it is done in github.com/wagtail/wagtail

```
sites_conformes
| .git
| sites_conformes
| | blog
| | content_manager
| | events
| | forms
| | templates
| | | sites_conformes_core/base.html
```

```py
# sites_conformes/blog/apps.py
from django.apps import AppConfig

# Before
class BlogConfig(AppConfig):
    default_auto_field = "django.db.models.BigAutoField"
    name = "blog"
    label = "blog"

# After
class BlogConfig(AppConfig):
    default_auto_field = "django.db.models.BigAutoField"
    name = "sites_conformes.blog"
    label = "sites_conformes_blog"
```

A big search and replace later, things started to work...

<!-- end_slide -->

...until Sites Conformes released a new version. That included a new `proconnect` app. 
Things broke. I tried to redo the search and replace, but I piled up lots and lots of replacements, that I lost track of. 
So I decided to re-read the diff and extract some patterns. 
Thanks to AI, things w

# What worked well 
Do not fork 
`uv add --editable ../sites_conformes` worked really well for me 
Adopt a determinist approach : every run of the script replayed all replacements in the source code






# How what went wrong 



# What's next


