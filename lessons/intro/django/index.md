Inštalácia: Django
======================

Vo vašom adresári Pyladies, tam kde máte vytvorené zložky k jednotlivým lekciám si vytvorte zložku pre Django aplikáciu a prejdite do nej.

```console
$ mkdir django_project
$ cd django_project/
```

Zapnite si virtuálne prostredie (venv).

V zložke `django_project/` si vytvorte súbor `requirements.txt` do ktorého zapíšte:
```console
Django~=4.0.8
```

Nainštalujte Django pomocou zadania príkazu do terminálu:
```console
pip install -r requirements.txt
```
Na konci výpisu v terminále by ste mali vidieť:

```console
Successfully installed Django-4.0.8
```

Django projekt
======================

Prvým krokom pri tvorbe projektu v Django je vytvorenie jeho základnej kostry zložiek a súborov pomocou Django skriptov.
Tieto zložky a súbory budeme neskôr používať, ich umiestnenie a pomenovanie sú pre Django dôležité, preto ich nepresúvajte
ani nepremenovávajte. Nezabudnite, že je potreba byť v zložke `django_project/`a mať zapnutý venv (platí aj pre ďalšie príkazy
do terminálu v rámci tohoto nastavenia).

Bodka (tečka) na konci príkazu je dôležitá.

Windows
```console
$ django-admin.exe startproject pyladies_blog .
```

macOS / Linux
```console
$ django-admin startproject pyladies_blog .
```

`django-admin.py` je skript ktorý vytvorí súbory a zložky. Štruktúra by mala vyzerať takto:

```
django_project
├── manage.py
├── pyladies_blog
│   ├── asgi.py
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── requirements.txt
```

Súbor `manage.py` je skript ktorý slúži k správe webovej aplikácie.
Pomocou neho si budeš (okrem iného) môcť spusiť web server na svojom počítači bez nutnosti inštalovať čokoľvek iné.

Súbor `settings.py` obsahuje konfiguráciu tvojej aplikácie.

Súbor `urls.py` obsahuje zoznam schémat na základe ktorých sa párujú Python triedy (tzv. views) s URL cestami.


Úprava nastavenia projektu
======================

Do súboru `settings.py` je potrebné pridať nakoniec:
```
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'static'
```

Je možné zmeniť si jazyk vašej aplikácie pomocou nastavenia v `LANGUAGE_CODE`,
ideálne si ale ponechajte angličtinu (pozn. pre češtinu by bola hodnota `cs-CZ`).

Nastavenie databáze
======================
Je niekoľko databázových software-ov pre uloženie dát o vašej aplikácii, my budeme používať
defaultné `sqlite3`. Nastavenie je už súčasťou `settings.py`.

Pre vytvorenie databáze pre vašu aplikáciu je potrebné spustiť následovné (zo zložky `django_project/`):
```console
python manage.py migrate
```

Mali by ste v prípade úspechu vidieť takýto výpis:
```console
(venv) ~/pyladies/django_project$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
```

Hotovo :)
