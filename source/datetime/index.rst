.. _datetime-tutorial:

============
``datetime``
============

Par Mohamed Jmaa 

Introduction
============

Le module :py:mod:`datetime` fournit des classes pour manipuler des dates et des heures de manière simple et complexe. Bien que l'arithmétique 
de la date et de l'heure est prise en charge, l'accent est mis sur l'extraction efficace des attributs pour le formatage et la manipulation des sorties.

Il existe deux types d'objets date et heure: “naive” et “aware”.

Un objet aware possède une connaissance suffisante des ajustements de temps algorithmiques et politiques applicables, tels que le fuseau horaire et 
les informations sur l'heure d'été, pour se situer par rapport à d'autres objets conscients.

Un objet naive ne contient pas suffisamment d'informations pour se localiser sans ambiguïté par rapport à d'autres objets date / heure.

Pour les applications nécessitant des objets conscients, les objets de :py:mod:`datetime` et :py:mod:`time` ont un attribut d'information de fuseau horaire 
optionnel, :py:mod:`tzinfo`, qui peut être configuré sur une instance d'une sous-classe de la classe abstraite :py:mod:`tzinfo`. Ces objets tzinfo capturent des 
informations sur le décalage par rapport à l'heure UTC, le nom du fuseau horaire et si l'heure d'été est en vigueur. Notez qu'une seule classe :py:mod:`tzinfo` 
concrète, la classe de :py:class:`timezone`, est fournie par le module :py:mod:`datetime`. La classe de :py:class:`timezone` peut représenter des fuseaux horaires simples 
avec un décalage fixe de l'UTC, tel que l'UTC ou les fuseaux horaires nord-américains EST et EDT. Le soutien des fuseaux horaires à des niveaux plus 
détaillés dépend de l'application. Les règles pour l'ajustement du temps à travers le monde sont plus politiques que rationnelles, changent fréquemment, 
et il n'y a pas de norme appropriée pour chaque application en dehors de l'UTC.

Types disponibles
=================

	Classe :py:class:`datetime.date`
	Une date naïve idéale, en supposant que le calendrier grégorien actuel ait toujours été et sera toujours en vigueur. Attributs: :py:attr:`year`, :py:attr:`month` et :py:attr:`day`.

	Classe :py:class:`datetime.time`
	Un temps idéalisé, indépendant de n'importe quel jour particulier, en supposant que tous les jours aient exactement 24 * 60 * 60 secondes (il n'y a aucune notion de «secondes de saut» ici). Attributs: :py:attr:`hour`, :py:attr:`minute`, :py:attr:`second`, :py:attr:`microsecond` et :py:attr:`tzinfo`.

	Classe :py:class:`datetime.datetime`
	Une combinaison d'une date et d'une fois. Attributs: :py:attr:`year`, :py:attr:`month` et :py:attr:`day`, :py:attr:`hour`, :py:attr:`minute`, :py:attr:`second`, :py:attr:`microsecond` et :py:attr:`tzinfo`.

	Classe :py:class:`datetime.timedelta`
	Une durée exprimant la différence entre deux instances de :py:class:`date`, :py:class:`time` ou de :py:class:`datetime` à une résolution de microseconde.

	Classe :py:class:`datetime.tzinfo`
	Une classe de base abstraite pour les objets d'information de fuseau horaire. Ceux-ci sont utilisés par les cours de :py:class:`datetime` et :py:class:`time` pour fournir une notion personnalisable d'ajustement de temps (par exemple, pour tenir compte du fuseau horaire et / ou de l'heure d'été).

	Classe :py:class:`datetime.timezone`
	Une classe qui implémente la classe de base abstraite :py:class:`tzinfo` comme un décalage fixe de l'UTC.

Objet :py:obj:`timedelta` :
===========================

Un objet :py:obj:`timedelta` représente une durée, la différence entre deux dates ou fois.

class :py:class:`datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)`

Tous les arguments sont facultatifs et par défaut à 0. Les arguments peuvent être des entiers ou des flotteurs, et peuvent être positifs ou négatifs.

Seuls les jours, les secondes et les microsecondes sont stockés en interne. Les arguments sont convertis en ces unités:

- Une milliseconde est convertie en 1000 microsecondes.

- Une minute est convertie en 60 secondes.

- Une heure est convertie en 3600 secondes.

- Une semaine est convertie en 7 jours.

Class attributes:
----------------

:py:attr:`date.min`
La première date représentable, ``date(MINYEAR, 1, 1)``.

:py:attr:`date.max`
La dernière date représentable, ``date(MAXYEAR, 12, 31)``.

:py:attr:`date.resolution`
La plus petite différence possible entre les objets de date non égale, ``timedelta(days = 1)``.

Attributs d'instance (en lecture seule):
----------------------------------------

+--------------+--------------------------------------+ 
| Attribut     | valeur 			      | 
+==============+======================================+ 
| days 	       |Entre -999999999 et 999999999 inclus  | 
+--------------+--------------------------------------+ 
| seconds      | Entre 0 et 86399 inclus	      | 
+--------------+--------------------------------------+ 
| microseconds | Entre 0 et 999999 inclus	      | 
+--------------+--------------------------------------+

Méthodes d'instance:
--------------------

:py:meth:`timedelta.total_seconds()`
Renvoie le nombre total de secondes contenues dans la durée.

Exemple d'utilisation:
----------------------

.. code-block:: python3

	>>> from datetime import timedelta
	>>> year = timedelta(days=365)
	>>> another_year = timedelta(weeks=40, days=84, hours=23,
	...                          minutes=50, seconds=600)  # adds up to 365 days
	>>> year.total_seconds()
	31536000.0
	>>> year == another_year
	True
	>>> ten_years = 10 * year
	>>> ten_years, ten_years.days // 365
	(datetime.timedelta(3650), 10)
	>>> nine_years = ten_years - year
	>>> nine_years, nine_years.days // 365
	(datetime.timedelta(3285), 9)
	>>> three_years = nine_years // 3;
	>>> three_years, three_years.days // 365
	(datetime.timedelta(1095), 3)
	>>> abs(three_years - ten_years) == 2 * three_years + year
	True

Objet date :
============

Un objet de date représente une date (année, mois et jour) dans un calendrier idéalisé, le calendrier grégorien actuel indéfiniment étendu dans les deux sens.

class :py:class:`datetime.date(year, month, day)`
Tous les arguments sont nécessaires. Les arguments peuvent être des nombres entiers, dans les plages suivantes:

- ``MINYEAR <= year <= MAXYEAR``

- ``1 <= month <= 12``

- ``1 <= day <= Nombre de jours dans le mois et l'année donné``


Autres constructeurs, toutes les méthodes de classe:
----------------------------------------------------

classmethod :py:meth:`date.today()`
Renvoie la date locale actuelle. 

classmethod :py:meth:`date.fromtimestamp(timestamp)`
Renvoie la date locale correspondant à timestamp POSIX.

classmethod :py:meth:`date.fromordinal(ordinal)`
Renvoie la date correspondant à l'ordinal grégorien proléptique, où le 1er janvier de l'année 1 a ordinal 1.


:py:attr:`date.year`
Entre MINYEAR et MAXYEAR inclus.

:py:attr:`date.month`
Entre 1 et 12 inclus.

:py:attr:`date.day`
Entre 1 et le nombre de jours dans le mois donné de l'année donnée.

Supported operations:
--------------------

+---------------------------+------------------------------------------------------------------------------------------------+ 
| Opération  		    | Resultats 					       				             | 
+===========================+================================================================================================+ 
| date2 = date1 + timedelta | date2 est timedelta.days jours supprimés de date1.  			       		     | 
+---------------------------+------------------------------------------------------------------------------------------------+ 
| date2 = date1 - timedelta | date2 est timedelta.days jours supprimés de date1.					     | 
+---------------------------+------------------------------------------------------------------------------------------------+
| timedelta = date1 - date2 | 												     | 
+---------------------------+------------------------------------------------------------------------------------------------+ 
| date1 < date2 	    | Date1 est considéré comme inférieur à la date2 lorsque la date1 précède la date2 dans le temps | 
+---------------------------+------------------------------------------------------------------------------------------------+

Instance methods:
-----------------

:py:meth:`date.replace(year=self.year, month=self.month, day=self.day)`
	Renvoie une date avec la même valeur, à l'exception de ces paramètres qui donnent de nouvelles valeurs selon les arguments de mots clés spécifiés.

:py:meth:`date.timetuple()`
	Retournez time.struct_time tel que retourné par ``time.localtime()``.

:py:meth:`date.toordinal()`
	Retournez l'ordinal grégorien proleptique de la date, où le 1er janvier de l'année 1 a ordinal 1.

:py:meth:`date.weekday()`
	Retournez le jour de la semaine en nombre entier, où le lundi est 0 et le dimanche 6.

:py:meth:`date.isoweekday()`
	Retournez le jour de la semaine en entier, où le lundi est 1 et le dimanche a 7 ans.

:py:meth:`date.isocalendar()`
	Retourner un 3-tuple (année ISO, numéro de semaine ISO, jour de semaine ISO).
	Le calendrier ISO est une variante largement utilisée du calendrier grégorien. https://www.staff.science.uu.nl/~gent0113/calendar/isocalendar.htm

:py:meth:`date.isoformat()`
	Renvoie une chaîne représentant la date au format ISO 8601, 'AAAA-MM-JJ'.

:py:meth:`date.__str__()`
	Pour une date d, ``str(d)`` équivaut à ``d.isoformat()``

:py:meth:`date.ctime()`
	Renvoie une chaîne représentant la date, par exemple la ``date(2002, 12, 4) .ctime () == 'Wed Dec 4 00:00:00 2002'``.

:py:meth:`date.strftime(format)`
	Renvoie une chaîne représentant la date, contrôlée par une chaîne de format explicite.

:py:meth:`date.__format__(format)`
	Identique à :py:meth:`date.strftime()`.

Exemple de compter les jours d'un événement:
--------------------------------------------

.. code-block:: python3

	>>> import time
	>>> from datetime import date
	>>> today = date.today()
	>>> today
	datetime.date(2007, 12, 5)
	>>> today == date.fromtimestamp(time.time())
	True
	>>> my_birthday = date(today.year, 6, 24)
	>>> if my_birthday < today:
	...     my_birthday = my_birthday.replace(year=today.year + 1)
	>>> my_birthday
	datetime.date(2008, 6, 24)
	>>> time_to_birthday = abs(my_birthday - today)
	>>> time_to_birthday.days
	202

Objet datetime :
================

Un objet :py:obj:`datetime` est un objet unique contenant toutes les informations à partir d'un objet :py:obj:`date` et d'un objet :py:obj:`time`.

Constructeur:
-------------

class :py:class:`datetime.datetime(year, month, day, hour=0, minute=0, second=0, microsecond=0, tzinfo=None, *, fold=0)`

Les arguments de l'année, du mois et du jour sont nécessaires. ``tzinfo`` peut être None ou une instance d'une sous-classe ``tzinfo``. 
Les arguments restants peuvent être des nombres entiers, dans les plages suivantes:

- ``MINYEAR <= year <= MAXYEAR``,

- ``1 <= month <= 12``,

- ``1 <= day <= Nombre de jours dans le mois et l'année donné``,

- ``0 <= hour < 24``,

- ``0 <= minute < 60``,

- ``0 <= second < 60``,

- ``0 <= microsecond < 1000000``,

- ``fold in [0, 1]``.

Autres constructeurs, toutes les méthodes de classe:
----------------------------------------------------

classmethod :py:meth:`datetime.today`
	Renvoie l'heure de date locale actuelle, sans tzinfo .

classmethod :py:meth:`datetime.now(tz=None)`
	Renvoie la date et l'heure locale actuelle. Si l'argument optionnel tz est ``None`` ou n'est pas spécifié, c'est comme :py:func:`today()`.

classmethod :py:meth:`datetime.utcnow()`
	Renvoie la date et l'heure UTC actuelles, sans tzinfo.

classmethod :py:meth:`datetime.fromtimestamp(timestamp, tz=None)`
	Renvoie la date et l'heure locales correspondant à timestamp POSIX, tel que renvoyé par :py:func:`time.time()`.

classmethod :py:meth:`datetime.utcfromtimestamp(timestamp)`
	Renvoie datetime de la date UTC correspondant à timestamp POSIX, sans tzinfo.

classmethod :py:meth:`datetime.fromordinal(ordinal)`
	Renvoie datetime de la date correspondant à l'ordinal grégorien proléptique, où le 1er janvier de l'année 1 a ordinal 1.

classmethod :py:meth:`datetime.combine(date, time, tzinfo=self.tzinfo)`
	Renvoie un nouvel objet datetime dont les composants de date sont égaux à l'objet de date donnée et dont les composants de temps sont égaux aux objets de time donné.

classmethod :py:meth:`datetime.strptime(date_string, format)`
	Renvoie un datetime correspondant à date_string, parsé selon le format.

Attributs de classe:
--------------------

:py:attr:`datetime.min`
	le :py:class:`datetime` représentable le plus tôt possible, ``datetime(MINYEAR, 1, 1, tzinfo=None)``.

:py:attr:`datetime.max`
	le :py:class:`datetime` représentable le plus tard possible, ``datetime(MAXYEAR, 12, 31, 23, 59, 59, 999999, tzinfo=None)``.
	
:py:attr:`datetime.resolution`
	La plus petite différence possible entre les objets :py:class:`datetime` non égaux, ``timedelta(microseconds=1)``.

Attributs d'instance (en lecture seule):
---------------------------------------

:py:attr:`date.year`
	Entre :py:const:`MINYEAR` et :py:const:`MAXYEAR` inclus.

:py:attr:`date.month`
	Entre 1 et 12 inclus.

:py:attr:`date.day`
	Entre 1 et le nombre de jours dans le mois donné de l'année donnée.

:py:attr:`datetime.hour`
	Dans ``range(24)``.

:py:attr:`datetime.minute`
	Dans ``range(60)``.

:py:attr:`datetime.second`
	Dans ``range(60)``.

:py:attr:`datetime.microsecond`
	In ``range(1000000)``.

:py:attr:`datetime.tzinfo`
	L'objet est passé comme l'argument tzinfo au constructeur de :py:class:`datetime`, ou ``None`` si aucun n'a été transmis.

:py:attr:`datetime.fold`
	Dans ``[0, 1]``, Utilisé pour désambiguiser les temps du mur pendant un intervalle répété.

Méthodes d'instance:
--------------------

datetime.date()
###############
	renvoie un objet :py:obj:`date` avec la même année, mois et jour.

datetime.time()
###############
	renvoie un objet :py:obj:`time` avec la même heure, minute, seconde, microseconde et pli.

datetime.timetz()
#################
	renvoie un objet time avec la même heure, minute, seconde, microseconde, pli et les attributs tzinfo.

datetime.replace(year=self.year, month=self.month, day=self.day, hour=self.hour, minute=self.minute, second=self.second, microsecond=self.microsecond, tzinfo=self.tzinfo, * fold=0)
###########################
	Renvoie un datetime avec les mêmes attributs, à l'exception de ces attributs donnés de nouvelles valeurs selon les arguments de mots clés spécifiés.

datetime.astimezone(tz=None)
###########################
	Renvoie un objet py:obj:`datetime` avec un nouvel attribut :py:class:`tzinfo` tz, en ajustant les données date et time afin que le résultat soit le même temps UTC, mais dans le temps locale de tz.

datetime.utcoffset()
####################
	Si :py:class:`tzinfo` est ``None``, renvoie ``None``, sinon renvoie ``self.tzinfo.utcoffset(self)`` et genere une exception si celle-ci ne renvoie pas ``None``, Ou un objet :py:obj:`timedelta` 
	représentant un nombre entier de minutes avec une grandeur inférieure à un jour.

datetime.dst()
##############
	Si :py:obj:`tzinfo` est ``None``, renvoie ``None``, sinon renvoie ``self.tzinfo.utcoffset(self)`` et genere une exception si celle-ci ne renvoie pas None,Ou un objet :py:obj:`timedelta`
	représentant un nombre entier de minutes avec une grandeur inférieure à un jour.

datetime.tzname()
#################
	Si :py:obj:`tzinfo` est ``None``, renvoie ``None``, sinon renvoie ``self.tzinfo.tzname(self)``, genere une exception si celle-ci ne renvoie pas None ou un objet :py:obj:`string`,

datetime.timetuple()
####################
	Renvoie ``time.struct_time`` tel que retourné par ``time.localtime()``.

datetime.utctimetuple()
#######################
	Si l'instance de `py:class:`datetime` d est naïve, ceci est identique à ``d.timetuple()`` sauf que tm_isdst est forcé à 0 indépendamment de ce que ``d.dst()`` retourne.

datetime.toordinal()
####################
	Renvoie l'ordinal grégorien proleptique de la date. La même chose que ``self.date().toordinal()``.

datetime.timestamp()
###########################
	Renvoie timeslamp POSIX correspondant à l'instance py:class:`datetime`. La valeur de retour est un flot similaire à celui renvoyé par ``time.time()``.

datetime.weekday()
##################
	Renvoie le jour de la semaine en entier, où lundi est 0 et dimanche est 6. Même que ``self.date().weekday()``.

datetime.isoweekday()
#####################
	Renvoie le jour de la semaine en entier, où le lundi est 1 et le dimanche est 7. Le même que ``self.date().isoweekday()``.

datetime.isocalendar()
######################
	Renvoie un 3-tuple (année ISO, numéro de semaine ISO, jour de semaine ISO). La même chose que ``self.date().isocalendar()``.

datetime.isoformat(sep='T', timespec='auto')
############################################
	Renvoie une chaîne représentant la date et l'heure au format ISO 8601, AAAA-MM-DDTHH: MM: SS.mmmmmm ou, si la microseconde est de 0, AAAA-MM-DDTHH: MM: SS

datetime.__str__()
##################
	Pour une instance de :py:class:`datetime` d, ``str(d)`` équivaut à ``d.isoformat('')``.

datetime.ctime()
################
	Renvoie une chaîne représentant la date et l'heure

datetime.strftime(format)
#########################
	Renvoie une chaîne représentant la date et l'heure, contrôlée par une chaîne de format explicite.

datetime.__format__(format)
###########################
	Même chose que ``datetime.strftime()``.
	
Exemples de travail avec des objets datetime:
---------------------------------------------

.. code-block:: python3

	>>> from datetime import datetime, date, time
	>>> # Using datetime.combine()
	>>> d = date(2005, 7, 14)
	>>> t = time(12, 30)
	>>> datetime.combine(d, t)
	datetime.datetime(2005, 7, 14, 12, 30)
	>>> # Using datetime.now() or datetime.utcnow()
	>>> datetime.now()   
	datetime.datetime(2007, 12, 6, 16, 29, 43, 79043)   # GMT +1
	>>> datetime.utcnow()   
	datetime.datetime(2007, 12, 6, 15, 29, 43, 79060)
	>>> # Using datetime.strptime()
	>>> dt = datetime.strptime("21/11/06 16:30", "%d/%m/%y %H:%M")
	>>> dt
	datetime.datetime(2006, 11, 21, 16, 30)
	>>> # Using datetime.timetuple() to get tuple of all attributes
	>>> tt = dt.timetuple()
	>>> for it in tt:   
	...     print(it)
	...
	2006    # year
	11      # month
	21      # day
	16      # hour
	30      # minute
	0       # second
	1       # weekday (0 = Monday)
	325     # number of days since 1st January
	-1      # dst - method tzinfo.dst() returned None
	>>> # Date in ISO format
	>>> ic = dt.isocalendar()
	>>> for it in ic:   
	...     print(it)
	...
	2006    # ISO year
	47      # ISO week
	2       # ISO weekday
	>>> # Formatting datetime
	>>> dt.strftime("%A, %d. %B %Y %I:%M%p")
	'Tuesday, 21. November 2006 04:30PM'
	>>> 'The {1} is {0:%d}, the {2} is {0:%B}, the {3} is {0:%I:%M%p}.'.format(dt, "day", "month", "time")
	'The day is 21, the month is November, the time is 04:30PM.'

Objet time :
============

Un objet :py:obj:`time` représente une heure (locale), indépendamment de n'importe quel jour particulier, et peut être réglée par un objet  :py:obj:`tzinfo`.

class :py:class:`datetime.time(hour=0, minute=0, second=0, microsecond=0, tzinfo=None, *, fold=0)`

Tous les arguments sont facultatifs. tzinfo peut être ``None`` ou une instance d'une sous-classe  :py:obj:`tzinfo`. Les arguments restants peuvent 
être des nombres entiers, dans les plages suivantes:

- ``0 <= hour < 24``,

- ``0 <= minute < 60``,

- ``0 <= second < 60``,

- ``0 <= microsecond < 1000000``,

- ``fold in [0, 1]``.

Attributs de classe:
--------------------

:py:attr:`time.min`
	le :py:obj:`time` représentable le plus tôt possible, ``time(0, 0, 0, 0)``.

:py:attr:`time.max`
	le :py:obj:`time` représentable le plus tard possible, ``time(23, 59, 59, 999999)``.
	
:py:attr:`time.resolution`
	La plus petite différence possible entre les objets :py:obj:`time` non égaux, ``timedelta(microseconds=1)``.

:py:attr:`time.hour`
	Dans ``range(24)``.

:py:attr:`time.minute`
	Dans ``range(60)``.

:py:attr:`time.second`
	Dans ``range(60)``.

:py:attr:`time.microsecond`
	In ``range(1000000)``.

:py:attr:`time.tzinfo`
	L'objet est passé comme l'argument tzinfo au constructeur de datetime, ou Aucun si aucun n'a été transmis.

:py:attr:`time.fold`
	Dans ``[0, 1]``, Utilisé pour désambiguiser les temps du mur pendant un intervalle répété.
	
Méthodes d'instance:
--------------------

time.replace(hour=self.hour, minute=self.minute, second=self.second, microsecond=self.microsecond, tzinfo=self.tzinfo, * fold=0)
################################################################################################################################
	Renvoie un time avec la même valeur, à l'exception de ces attributs donnés de nouvelles valeurs par les mots-clés arguments spécifiés.
time.isoformat(timespec='auto')
###############################
	Renvoie une chaîne représentant l'heure au format ISO 8601, HH: MM: SS.mmmmmm ou, si microseconde est 0, HH: MM: SS Si utcoffset() ne retourne 
	None, une chaîne de 6 caractères est ajoutée, donnant l'UTC Décalage dans (signé) heures et minutes: HH: MM: SS.mmmmmm + HH: MM ou, si self.microsecond 
	est 0, HH: MM: SS + HH: MM

time.__str__()
##############
	Pour un temps t, str(t) équivaut à t.isoformat().
	
time.strftime(format)
#####################
	Renvoie une chaîne représentant l'heure, contrôlée par une chaîne de format explicite.

time.__format__(format)
#######################
	Même chose que time.strftime().

time.utcoffset()
################
	Si tzinfo est None, renvoie None, sinon renvoie self.tzinfo.utcoffset(None) et génére une exception si celle-ci ne renvoie pas None ou un objet
	timedelta représentant un nombre entier de minutes avec une grandeur inférieure à un jour.

time.dst()
##########
	Si tzinfo est None, renvoie None, sinon renvoie self.tzinfo.utcoffset(None) et génére une exception si celle-ci ne renvoie pas None ou un objet
	timedelta représentant un nombre entier de minutes avec une grandeur inférieure à un jour.

time.tzname()
#############
	Si tzinfo est None, renvoie None, sinon renvoie self.tzinfo.tzname(None) ou génére une exception si celle-ci ne renvoie pas None ou un objet
	string.

Exemple:
--------

.. code-block:: python3

	>>> from datetime import time, tzinfo, timedelta
	>>> class GMT1(tzinfo):
	...     def utcoffset(self, dt):
	...         return timedelta(hours=1)
	...     def dst(self, dt):
	...         return timedelta(0)
	...     def tzname(self,dt):
	...         return "Europe/Prague"
	...
	>>> t = time(12, 10, 30, tzinfo=GMT1())
	>>> t                               
	datetime.time(12, 10, 30, tzinfo=<GMT1 object at 0x...>)
	>>> gmt = GMT1()
	>>> t.isoformat()
	'12:10:30+01:00'
	>>> t.dst()
	datetime.timedelta(0)
	>>> t.tzname()
	'Europe/Prague'
	>>> t.strftime("%H:%M:%S %Z")
	'12:10:30 Europe/Prague'
	>>> 'The {} is {:%H:%M}.'.format("time", t)
	'The time is 12:10.'

Objet tzinfo :
==============

class :py:class:`datetime.tzinfo`

Il s'agit d'une classe de base abstraite, ce qui signifie que cette classe ne doit pas être instanciée directement. Vous devez dériver
une sous-classe concrète, et (au moins) fournir des implémentations des méthodes standard :py:class:`tzinfo` nécessaires aux méthodes de :py:class:`datetime`
que vous utilisez. Le module datetime fournit une simple sous-classe concrète de :py:class:`tzinfo`, :py:class:`timezone`, qui peut représenter des fuseaux horaires avec un décalage fixe de l'UTC tel que UTC ou EST nord-américain et EDT.

:py:func:`tzinfo.utcoffset(dt)`

		Renvoie l'heure locale par rapport à UTC, en minutes à l'est de UTC. Si l'heure locale est à l'ouest de UTC, cela devrait être
		négatif. Notez que ceci est destiné à être le décalage total de l'UTC; Par exemple, si un objet :py:obj:`tzinfo` représente un fuseau horaire et 
		des ajustements DST, :py:func:`utcoffset()` doit retourner sa somme. Si le décalage UTC n'est pas connu, renvoie ``None``. Sinon, la valeur renvoyée 
		doit être un objet :py:obj:`timedelta` spécifiant un nombre entier de minutes dans la plage -1439 à 1439 inclus (1440 = 24 * 60, l'ampleur du décalage 
		doit être inférieure à un jour).
		
:py:func:`tzinfo.dst(dt)`


		Renvoie l'ajustement de l'heure d'été (heure d'été), en minutes à l'est de UTC, ou None si les informations sur l'heure d'été ne sont pas 
		connues. Renvoie ``timedelta(0)`` si DST n'est pas en vigueur. Si DST est en vigueur, revoie le décalage en tant qu'objet :py:obj:`timedelta`. 
		Notez que le décalage DST, le cas échéant, a déjà été ajouté à l'offset UTC renvoyé par :py:func:`utcoffset()`, il n'est donc pas nécessaire de consulter
		:py:func:`dst()` sauf si vous êtes intéressé à obtenir des informations DST séparément.
		
:py:func:`tzinfo.tzname(dt)`
		Renvoie le nom du fuseau horaire correspondant à l'objet :py:class:`datetime` dt, en tant que chaîne. Rien à propos des noms de chaîne n'est défini par 
		le module :py:mod:`datetime`, et il n'y a aucune exigence que cela signifie quelque chose en particulier.
		
:py:func:`tzinfo.fromutc(dt)`
		Cela s'appelle à partir de l'exécution par défaut :py:func:`datetime.astimezone()`. Lorsqu'il en est appelé, ``dt.tzinfo`` est autonome, et les données 
		de date et d'heure de Dt doivent être considérées comme exprimant une heure UTC. Le but de :py:func:`fromutc()` est d'ajuster les données de date et d'heure,
		en renvoyant une datetime équivalente dans l'heure locale de l'auto.
	
Objet timezone :
================

La classe de :py:class:`timezone` est une sous-classe de :py:class:`tzinfo`, dont chaque instance représente un fuseau horaire défini par un décalage fixe de l'UTC. 
Notez que les objets de cette classe ne peuvent pas être utilisés pour représenter l'information de la fuseau horaire dans les endroits où différents décalages sont utilisés en différents jours de l'année ou où des changements historiques ont été apportés au temps civil.
 
class :py:class:`datetime.timezone(offset, name=None)`
	L'argument offset doit être spécifié comme un objet :py:obj:`timedelta` représentant la différence entre l'heure locale et UTC. Il doit être strictement entre ``-timedelta(heures = 24)`` et ``timedelta(heures = 24)`` et représente un nombre entier de minutes, sinon, ValueError est élevé.
	
	:py:func:`timezone.utcoffset(dt)`
		Renvoie la valeur fixe spécifiée lorsque l'instance de :py:class:`timezone` est construite. L'argument dt est ignoré. La valeur de retour est une instance :py:class:`timedelta` égale à la différence entre l'heure locale et UTC.
	
	:py:func:`timezone.tzname(dt)`
		Renvoie la valeur fixe spécifiée lorsque l'instance de :py:class:`timezone` est construite. Si le nom n'est pas fourni dans le constructeur, le nom renvoyé par ``tzname(dt)``
		est généré à partir de la valeur du décalage comme suit. Si le décalage est ``timedelta(0)``, le nom est "UTC", sinon c'est une chaîne 'UTC ± HH: MM', 
		où ± est le signe du ``offset``, HH et MM sont deux chiffres de ``offset.hours`` et ``offset.minutes`` respectivement.
		
	:py:func:`timezone.dst(dt)`
		Renvoie toujours ``None``.
		
	:py:func:`timezone.fromutc(dt)`
		Renvoie ``dt + offset``. L'argument dt doit être une instance :py:class:`datetime` aware, avec ``tzinfo`` défini à ``self``.
		
Attributs de classe:
--------------------

	:py:func:`timezone.utc`
		La timezone UTC, ``timezone(timedelta(0))``.
		
	
Conclusion
==========
Dans ce module :py:mod:`datetime` nous avons traité toutes les classes que nous pouvons utiliser, le cas d'utilisation, les méthodes, les attributs, avec quelques exemples pour bien comprendre comment peut on utiliser ce module.