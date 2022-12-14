Vytvorenie aplikácie
======================

V rámci projektu si pre poriadok vytvoríme separátnu aplikáciu `blog`.

Zapnite si virtuálne prostredie (venv) a presuňte sa v termináli do zložky `django_project/`.

Windows / macOS / Linux
```console
$ python manage.py startapp blog
```

Vytvorí sa nová zložka `blog` a obsahuje ďalšie súbory. Zložka s našim projektom by mala vyzerať takto:

```
django_project
├── blog
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── db.sqlite3
├── manage.py
├── pyladies_project
│   ├── asgi.py
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── requirements.txt
```

Vytvorenú aplikáciu `blog` pridáme do zoznamu `INSTALLED_APPS` v nastavení projektu `django_project/pyladies_project/settings.py` aby Django vedelo, že ju má použiť. Bude to vyzerať takto:

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```

Čo je Django?
===============

- vytvorené ako interný projekt pre vývoj webových aplikácií v Lawrence Journal-World v rokoch 2003-2005 (prvý oficiálny release 2008)
- populárny python framework využívaný pre web development
- Django sa drží princípu MVC (Model-View-Controller), ktorý je štandardom pre vývoj webových aplikácií
- opensource, zdarma, má veľkú a aktívnu komunitu, dobrú dokumentáciu a rozličné možnosti rozšírenia (väčšina je zdarma)

Pre MVC princíp má Django vlastnú konvenciu MVT (Model-View-Template)

{{ figure(
    img=static('django.png'),
    alt="Architecture"
) }}
[Zdroj: https://pythonguides.com/wp-content/uploads/2021/08/Django-MVT-architecture.png]

Model - je vrstva určená k správe dát, predstavuje prepojenie celej architektúry s databázou; 1 model = 1 tabuľka v databáze.

View - je určené pre definovanie cekovej logiky datového toku. Zároveň zaisťuje odosielanie response (odpovede) danému užívateľovi.

Template - prezentačná vrstva ktorá sa stará o interakcie s užívateľom.

User request -> Django controller -> urls.py -> views.py (models + html templates) -> template response

Vytvorenie modelu pre príspevok na blogu
========================================

Model v Django je špeciálny druh objektu – je uložený v databáze. Databáza je súbor údajov. Je to miesto kam budete ukladať informácie o užívateľoch, príspevky na blogu atď. Na ukladanie údajov budeme používať databázu SQLite (defaultné nastavenie v Django, pre naše potreby postačí).

Model v databáze si môžete predstaviť ako tabuľku so stĺpcami a riadkami.

Aké atribúty a metódy by taký príspevok na blogu mohol mať? (len do PPT)

V súbore `blog/models.py` budeme definovať všetky objekty nazvané modely, medzi nimi aj príspevok.

```python
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```

Viac informácií k typom polí v modeloch nájdete v [dokumentácii Django](https://docs.djangoproject.com/en/4.1/ref/models/fields/#module-django.db.models.fields).

Pre vytvorenie tabuľky pre model v databáze je potrebné Django najskôr informovať o tom, že máme nejaké zmeny.

```console
$ python manage.py makemigrations blog
Migrations for 'blog':
  blog/migrations/0001_initial.py
    - Create model Post
```

Django vytvorilo súbor s migráciou ktorý chceme aplikovať do našej databáze.

```console
$ python manage.py migrate blog
Operations to perform:
  Apply all migrations: blog
Running migrations:
  Applying blog.0001_initial... OK
```

Django admin
=============

Pre tvorbu, editáciu a zmazanie príspevkov využijeme Django admin (administrátorské rozhranie).

Otvoríme si súbor `blog/admin.py` a vložíme do neho:

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```
Viac informácii k rozhraniu admin nájdete v [dokumentácii Django](https://docs.djangoproject.com/en/4.1/ref/contrib/admin/).

Spustíme si webserver
```
$ python manage.py runserver
```

V prehliadači si otvoríme stránku `http://127.0.0.1:8000/admin/`.

{{ figure(
    img=static('login.png'),
    alt="Login"
) }}

Pre prihlásenie do rozhrania je potrebné vytvoriť si tzv. superuser-a, účet ktorý má v administrátorskom rozhraní plnú kontrolu.
Otvoríme si nový terminál (webserver nám musí stále bežať) v ktorom máme zapnutý venv a zadáme do neho:

```
$ python manage.py createsuperuser
```

Zadáme údaje (heslo sa pri písaní nezobrazuje). Napr.:
```
Username: alby
Email address: alby@example.com
Password:
Password (again):
Superuser created successfully.
```

Vrátime sa späť do prehliadača a údaje použijeme pre prihlásenie. Mali by sme sa po prihlásení dostať do administrátorského rozhrania:

{{ figure(
    img=static('admin.png'),
    alt="Admin"
) }}

Vytvoríme si nejaké príspevky pomocou sekcie Posts.

{{ figure(
    img=static('post.png'),
    alt="Post"
) }}


Django ORM (Object-Relational-Mapper)
=============

Umožňuje interakcie s databázou.

Otvoríme si Django konzolu
```
$ python manage.py shell
```

Naimportujeme si náš model

```python
>>> from blog.models import Post
```

Výpis všetkých objektov:

```python
>>> Post.objects.all()
<QuerySet [<Post: my post title>, <Post: another post title>]>
```

V konzoli si môžeme tvoriť nové objekty:

```python
>>> Post.objects.create(author=me, title='Sample title', text='Test')
```

V konzoli uvidíme chybovú hlášku. Čo nám hovorí?

Premenná `me` je neznáma a aby sme mohli vytvoriť `Post`, potrebujeme do poľa `author` predať objekt užívateľa.

Naimportujeme si Django model `User`:

```python
>>> from django.contrib.auth.models import User
```

Vypíšeme si existujúcich užívateľov.

```python
>>> User.objects.all()
<QuerySet [<User: alby>]>
```

Do premennej si uložíme svojho užívateľa:

```python
>>> me = User.objects.get(username='alby')
```

Vyskúšame vytvoriť znova:

```python
>>> Post.objects.create(author=me, title='Sample title', text='Test')
<Post: Sample title>
```

Z existujúcich príspevkov môžeme filtrovať:

```python
>>> Post.objects.filter(title__contains='title')
<QuerySet [<Post: Sample title>, <Post: 4th title of post>]>
```


Skúsime si vyfiltrovať publikované:

```python
>>> from django.utils import timezone
>>> Post.objects.filter(published_date__lte=timezone.now())
<QuerySet []>
```

Zatiaľ nemáme žiadne publikované články, môžeme nejaké publikovať pomocou metódy `publish()`:

```python
>>> post = Post.objects.get(title="Sample title")
>>> post.publish()
```

Vo vyfiltrovaných publikovaných príspevkoch teraz uvidíme náš príspevok:

```python
>>> Post.objects.filter(published_date__lte=timezone.now())
<QuerySet [<Post: Sample title>]>
```

Objekty si môžeme zoradiť:

```python
>>> Post.objects.order_by('created_date')
<QuerySet [<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]>
```

Prípadne reversne:

```python
>>> Post.objects.order_by('-created_date')
<QuerySet [<Post: Sample title>, <Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>]>
```

Reťazenie:

```python
>>> Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
<QuerySet [<Post: Post number 2>, <Post: My 3rd post!>, <Post: 4th title of post>, <Post: Sample title>]>
```

Konzolu opustíme:

```python
>>> exit()
```

Viac informácii k ORM nájdete v [dokumentácii Django](https://docs.djangoproject.com/en/4.1/topics/db/queries/).

Django URLs
=============

Django využíva URLconf (URL configuration), ktorý obsahuje zoznam schémat na základe ktorých sa párujú Python triedy (tzv. views) s URL cestami.

V `pyladies_project/urls.py` už máme:

```python
"""pyladies_project URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/4.0/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]

```

`path('admin/', admin.site.urls)` znamená, že pre všetky URL, ktoré začínajú na `admin/` Django nájde korešpondujúce view. V tomto prípade to zahŕňa veľké množstvo admin podstránok, ktoré sú zabalené do jedného súboru z modulu admin.

Aby sme do URLs nášho projektu zahrnuli aj schémata z našej aplikácie blog, pridáme ich do `urlpatterns`:

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

Toto zaistí, že pri všetkých requestoch smerujúcich na `http://127.0.0.1:8000/` sa Django pozrie do `blog.urls` a hľadá tam ďalšie inštrukcie.

Vytvorenie vlastných URL pre aplikáciu blog v súbore `blog/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.PostListView.as_view(), name='post_list'),
]
```

K root URL adrese teraz priraďujeme view `PostListView`. Toto URL schéma sa napáruje na prázdny reťazec a Django URL resolver bude ignorovať názov domény (t. j. `http://127.0.0.1:8000/`), ktorý je predponou celej URL. Toto schéma povie Djangu, že PostListView je to správne miesto, kam ísť, ak niekto vstúpi na 'http://127.0.0.1:8000/'.

Posledná časť `name='post_list'` je názov URL, ktorý budeme používať na identifikáciu. Názvy URL budeme ďalej v aplikácii používať, je dôležité aby boli unikátne, jednoduché na zapamätenie a mali výpovednú hodnotu.


Ak by sme si teraz spustili webserver tak by sme v termináli mali vidieť hlášku:

```python
  File "/home/alby/www/django_project/blog/urls.py", line 5, in <module>
    path('', views.PostListView.as_view(), name='post_list'),
AttributeError: module 'blog.views' has no attribute 'PostListView'
```

Tá nám hovorí, že PostListView naša aplikácia nepozná, preto si ju musíme vytvoriť.

Viac informácii k URLs nájdete v [dokumentácii Django](https://docs.djangoproject.com/en/4.1/topics/http/urls/).


Django views
=============

Do súboru `blog/views.py` si pridáme novú view.

```python
from django.shortcuts import render
from django.views.generic import TemplateView


class PostListView(TemplateView):
    template_name = 'blog/post_list.html'

    def get(self, request):
        return render(request, self.template_name, {})
```

Naša view dedí z `TeplateView`, ktoré je vytvorené v Django frameworku a už má v sebe zabudovanú funkcionalitu, ktorú prevezmeme do nášho view (spomeňme si na dedičnosť tried). Do atribútu triedy `template_name` uložíme názov šablóny pre danú stránku.

Do našej triedy pridáme aj metódu `get()`, ktorá si za argument vezme `request` (všetky informácie ktoré dostane od užívateľa pri jeho pristúpení na stránku) a vráti nám hodnotu, ktorú vráti funkcia `render()`.

Funkcia `render()` je v Djangu jedna z najpoužívanejších funkcií, ktorá vezme kombinuje šablónu (hodnota v našej `template_name`) s kontextom a vráti `HttpResponse objekt` s príslušným kontextom.

Viac informácii k views nájdete v [dokumentácii Django](https://docs.djangoproject.com/en/4.1/ref/class-based-views/base/).

HTML šablóny v Django
=======================

HTML šablóna je súbor v jazyku HTML, ktorý sa dá prepoužiť pre zobrazenie rôznych informácií v konzistentnom formáte.

HTML (HyperText Markup Language) je kód interpretovaný prehliadačom pre zobrazenie webovej stránky užívateľovi.

Je zložený z tzv. tagov každý začínajúci na `<` a končiaci na `>`, ktoré reprezentujú elementy.

Pre vytvorenie prvej šablóny si v zložke blog vytvoríme následovné podzložky:

```
blog
└───templates
    └───blog
```

A v zložke `blog/templates/blog` si vytvoríme súbor `post_list.html`.


Pridáme:

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Alby's blog</title>
	</head>
	<body>
	    <p>Hi there!</p>
	    <p>It works!</p>
	</body>
</html>
```

`!DOCTYPE html` - nie je to HTML tag, určuje typ dokumentu.
`html` - ohraničuje obsah stránky.
`head` - obsahuje rôzne informácie pre dokument, ktoré sa užávateľovi nezobrazujú.
`body` - všetko ostatné, čo sa užívateľovi na stráne zobrazuje.

Rôzne HTML tagy nájdte vypísané napr. [tu](https://www.w3schools.com/TAGS/default.asp).

Dôležité je nezabúdať na odsadenie v zanorených tagoch a ukončovací tag (až na výnimky, napr. `<br>`).

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Django Girls blog</title>
    </head>
    <body>
        <header>
            <h1><a href="/">Django Girls Blog</a></h1>
        </header>

        <article>
            <time>published: 14.06.2014, 12:14</time>
            <h2><a href="">My first post</a></h2>
            <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.</p>
        </article>

        <article>
            <time>published: 14.06.2014, 12:14</time>
            <h2><a href="">My second post</a></h2>
            <p>Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut f.</p>
        </article>
    </body>
</html>
```

Zatiaľ však pomocou šablóny zobrazujeme presne stanovené informácie, no šablóny by nám predovšetkým mali umožniť zobrazovať rôzne informácie v rovnakom formáte.

Chceme zobraziť objekty modelu `Post`, ktoré sme si vytvorili.

Dynamické dáta v šablónach
============================

Pre zobrazenie nami vytvorených publikovaných príspevkov (modely uložené do databáze) v našej šablóne využijeme `PostListView()` v súbore `views.py`.

```python
from django.shortcuts import render
from django.views.generic import TemplateView


class PostListView(TemplateView):
    template_name = 'blog/post_list.html'

    def get(self, request):
        return render(request, self.template_name, {})
```

1. Naimportujeme si model `Post()` a modul `timezone`.

```python
from django.utils import timezone
from .models import Post
```

2. Vytvoríme si Queryset (zoznam objektov z databáze, umožňuje čítať, radiť či filtrovať dáta z databáze).

```python
Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
```

3. Uložíme si ho do premennej

```python
posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
```

4. Premennú pridáme do posledného parametru funkcie `render()`, ktorá sa nazýva `context` a nesie v sebe informácie, ktoré chceme šablóne poskytnúť.

```python
from django.shortcuts import render
from django.views.generic import TemplateView
from django.utils import timezone
from .models import Post

class PostListView(TemplateView):
    template_name = 'blog/post_list.html'

    def get(self, request):
        posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
        return render(request, self.template_name, {'posts': posts})
```

5. Pridáme dáta do šablóny.

Vypísanie jednotlivých príspevkov:

{% raw %}
```jinja
{% for post in posts %}
    {{ post }}
{% endfor %}
```
{% endraw %}

Vypísanie jednotlivých príspevkov a ich detailov

{% raw %}
```html+jinja
<header>
    <h1><a href="/">Django Girls Blog</a></h1>
</header>

{% for post in posts %}
    <article>
        <time>published: {{ post.published_date }}</time>
        <h2><a href="">{{ post.title }}</a></h2>
        <p>{{ post.text | linebreaksbr }}</p>
    </article>
{% endfor %}
```
{% endraw %}
