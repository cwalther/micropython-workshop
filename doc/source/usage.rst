Python als Taschenrechner
=========================

Um die grundlegende Syntax von Python kennen zu lernen, probieren wir nun einige Dinge auf der REPL aus. Vieles davon stammt von https://docs.python.org/3/tutorial/ und ist dort in noch mehr Detail beschrieben. In den folgenden Beispielen bezeichnet der Prompt ``>>>`` oder ``...`` die Zeilen, die eingetippt werden müssen, Zeilen ohne Prompt sind Ausgaben des Interpreters.

Zahlen
------

Arithmetik mit Zahlen funktioniert wie erwartet. Leerzeichen sind optional. ::

   >>> 50 - 5*6
   20
   >>> (50 - 5*6) / 4
   5.0
   >>> 8 / 5
   1.6

Division mit ``/`` gibt immer eine Fliesskommazahl zurück. Ganzzahldivision (Division mit Rest) geht mit ``//``, den Rest liefert ``%``::

   >>> 17//3
   5
   >>> 17%3
   2

Potenzen berechnet ``**``::

   >>> 5**2
   25

``=`` weist ein Resultat einer Variable zu, die nachher verwendet werden kann. Verwenden einer undefinierten Variable erzeugt einen Fehler::

   >>> width = 3
   >>> height = 5
   >>> width * height
   15
   >>> width * x
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   NameError: name 'x' isn't defined

Es gibt abgekürzte Operatoren für Addition und Subtraktion zu/von Variablen (aber kein ``++`` oder ``--``)::

   >>> width += 1
   >>> height -= 2
   >>> width * height
   12

Auf der REPL kann mit der Tab-Taste der Beginn eines bekannten Wortes vervollständigt werden, darunter fallen u.a. die definierten Variablen. Mit Pfeil-auf und Pfeil-ab werden die vorherigen Eingaben wieder aufgerufen (MicroPython speichert die letzten 10).

Das Resultat des letzten Ausdrucks wird automatisch der speziellen Variable ``_`` zugewiesen::

   >>> 5+2
   7
   >>> 20/_
   2.85714
   >>> round(_, 2)
   2.86

Strings
-------

Strings (Zeichenketten) werden mit einfachen oder doppelten Anführungszeichen geschrieben, wobei die jeweils anderen Anführungszeichen darin vorkommen können, während die gleichen mit ``\`` geschützt werden müssen::

   >>> 'Hello World!'
   'Hello World'
   >>> "Hello World!"
   'Hello World'
   >>> 'Hello "World"!'
   'Hello "World"!'
   >>> 'Hello \'World\'!'
   "Hello 'World'!"

Ein als Resultat ausgegebener String erscheint so, wie er wieder eingegeben werden müsste, mit Anführungszeichen und allem. Mit der eingebauten Funktion ``print()`` kann stattdessen sein Inhalt ausgegeben werden. ``\n`` bezeichnet einen Zeilenumbruch::

   >>> "Hello\nWorld!"
   'Hello\nWorld!'
   >>> print("Hello\nWorld!")
   Hello
   World!

Strings können mit ``+`` aneinandergehängt und mit ``*`` vervielfacht werden:

   >>> 3 * 'un' + 'ium'
   'unununium'

Durch indizieren mit einer Zahl können einzelne Zeichen aus einem String geholt werden. Das erste Zeichen hat den Index 0. Es gibt keinen separaten Datentyp für Zeichen, ein Zeichen ist einfach ein String der Länge 1. ::

   >>> word = 'Python'
   >>> word[0]
   'P'
   >>> word[5]
   'n'

Negative Indizes zählen vom Ende her::

   >>> word[-1]
   'n'
   >>> word[-6]
   'P'

Indizieren mit zwei Zahlen, sog. *slice indexing*, holt einen Substring mit den angegebenen Anfangs- und Endpositionen heraus::

   >>> word[1:4]
   'yth'
   >>> word[3:-1]
   'ho'

Die Positionen liegen dabei *zwischen* den Zeichen, so dass der Slice ``a:b`` (für nichtnegative ``a``, ``b``) immer ``b-a`` Zeichen lang ist::

      P   y   t   h   o   n  
    └───┴───┴───┴───┴───┴───┘
    0   1   2   3   4   5   6
   -6  -5  -4  -3  -2  -1

Ausgelassene Slice-Indizes bedeuten «ganz am Anfang» resp. «ganz am Ende»::

   >>> word[:2]
   'Py'
   >>> word[2:]
   'thon'

Ein Einzelindex ausserhalb des verfügbaren Bereiches gibt einen Fehler::

   >>> word[42]
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   IndexError: string index out of range

Slices hingegen werden auf den verfügbaren Bereich zurechtgeschnitten::

   >>> word[4:42]
   'on'
   >>> word[42:]
   ''

Die eingebaute Funktion ``len()`` gibt die Länge eines Strings zurück:

   >>> len(word)
   6

Listen
------

Eine Liste ist eine Sammlung von Objekten in einer bestimmten Reihenfolge und wird durch eckige Klammern erzeugt. Es können Elemente unterschiedlicher Typen in derselben Liste vorkommen, auch verschachtelte Listen. Alle Operationen, die oben mit Strings durchgeführt wurden, funktionieren auch mit Listen::

   >>> l = [2, 5, 'a', 3.141]
   >>> l[1]
   5
   >>> l[2:]
   ['a', 3.141]
   >>> len(l)
   4
   >>> l + 2 * [10, 20]
   [2, 5, 'a', 3.141, 10, 20, 10, 20]

Im Gegensatz zu Strings sind Listen aber veränderbar (mutable). Du kannst durch Zuweisung mit einem Index Elemente ersetzen. Zuweisung mit einem Slice ersetzt ganze Bereiche, was die Länge der Liste ändern kann::

   >>> l[1] = 8
   >>> l
   [2, 8, 'a', 3.141]
   >>> l[1:3] = ['x', 'y', 'z']
   >>> l
   [2, 'x', 'y', 'z', 3.141]

Der Operator ``del`` entfernt Elemente oder Bereiche::

   >>> del l[2]
   >>> l
   [2, 'x', 'z', 3.141]
   >>> del l[2:]
   >>> l
   [2, 'x']

Elemente am Ende anhängen geht mit der Methode ``append()``::

   >>> l.append(3)
   >>> l
   [2, 'x', 3]

Wenn ein Objekt einer Variable zugewiesen wird, dann zeigt die Variable aufs Objekt. Dasselbe Objekt weiteren Variablen (oder Listenelementen) zuzuweisen erzeugt keine unabhängigen Kopien, sondern es zeigen dann alle auf dasselbe Objekt. Das gilt für alle Objekte, auch Strings und Zahlen, aber fällt besonders ins Gewicht bei veränderbaren Objekten wie Listen: Wenn sie von einem Ort aus geändert werden, ist die Änderung von überall her sichtbar. ::

  >>> m = l
  >>> m[2] = 'hey'
  >>> l
  [2, 'x', 'hey']

Um das zu vermeiden und stattdessen mehrere unabhängig voneinander veränderbare Listen zu erhalten, muss explizit eine Kopie gemacht werden. Das kann mittels Slice ohne Anfang und Ende erreicht werden, da slicen immmer eine neue Liste erzeugt. Es handelt sich hier um eine *shallow copy*, d.h. die Elemente werden nicht kopiert. ::

  >>> n = l[:]
  >>> n
  [2, 'x', 'hey']
  >>> n[2] = 'ho'
  >>> n
  [2, 'x', 'ho']
  >>> l
  [2, 'x', 'hey']

Tuples
------

Tuples funktionieren ähnlich wie Listen, sind aber unveränderbar (immutable). Sie werden z.B. verwendet, um mehrere Werte aus einer Funktion zurück zu geben. Wir werden später Anwendungen dafür sehen. Tuples werden durch runde statt eckige Klammern erzeugt::

   >>> t = (3, 4, [5, 6])
   >>> t[1]
   4

Dictionaries
------------

Ein Dictionary ist eine ungeordnete Sammlung von Schlüssel-Wert-Paaren. Im Gegensatz zur Liste, wo ein Element durch seine numerische Position identifiziert wird, wird in einem Dictionary ein Element durch seinen Schlüssel identifiziert, welcher ein beliebiges unveränderbares Objekt sein kann, z.B. ein String. Jeder Schlüssel kann höchstens ein Mal im Dictionary vorkommen, eine Zuweisung eines neuen Wertes an einen existierenden Schlüssel überschreibt den bisherigen Wert. Dictionaries werden durch geschweifte Klammern erzeugt::

   >>> d = { 'eins': 1, 'zwei': 2, 3: 'drei' }
   >>> d
   {'eins': 1, 'zwei': 2, 3: 'drei'}
   >>> d['zwei']
   2
   >>> d['vier'] = 4.0
   >>> d
   {'eins': 1, 'zwei': 2, 'vier': 4.0, 3: 'drei'}
   >>> d[3] = 'DREI'
   >>> d
   {'eins': 1, 'zwei': 2, 'vier': 4.0, 3: 'DREI'}

Typkonversion
-------------

Alle bisher besprochenen Datentypen existieren als Funktion, die die Umwandlung aus einem anderen Datentyp übernimmt::

   >>> str(5.00)
   '5.0'
   >>> float('5.00')
   5.0
   >>> int(5.6)
   5
   >>> tuple([7, 8])
   (7, 8)
   >>> list((9, 10))
   [9, 10]
   >>> list('ha!')
   ['h', 'a', '!']
   >>> dict([('a', 'b'), (2, 3)])
   {'a': 'b', 2: 3}

Für mehr Einflussmöglichkeiten bei der Konversion von Zahlen nach Strings gibt es die String-Methode :py:meth:`~str.format()`, siehe Dokumentation (wir brauchen sie hier nicht).

Vergleiche und Boolesche Logik
------------------------------

Es stehen die üblichen Vergleichsoperatoren ``<``, ``<=``, ``==``, ``>=``, ``>``, ``!=`` zur Verfügung. Für Strings vergleichen sie lexikographisch. Sie geben einen Booleschen Wert zurück. Die Booleschen Werte heissen ``True`` und ``False`` (beachte die Grossschreibung). ::

   >>> 5 <= 2
   False
   >>> 'Az' > 'Aa'
   True
   >>> 'a' + 'b' == 'ab'
   True
   >>> 'a' + 'b' != 'abc'
   True

Vergleiche können hintereinandergereiht werden, es müssen dann alle davon erfüllt sein (UND-Verknüpfung)::

   >>> 3 < 2 < 5
   False
   >>> 3 < 4 < 5
   True
   >>> 3 < 8 < 5
   False

Die Operatoren für logische Verknüpfungen heissen ``and``, ``or`` und ``not``::

	>>> True and True
	True
	>>> False or True
	True
	>>> not True
	False

``if``-Entscheidungen
---------------------

Das ``if``-Statement gibt die generelle Form der Kontrollstrukturen in Python vor. Nach ``if`` kommt die Bedingung, dann ein Doppelpunkt, der den folgenden Block einführt, dann eingerückt die kontrollierten Zeilen. Die Einrückung ist nicht wie bei anderen Sprachen nur zur Übersichtlichkeit für menschliche Leser da, sondern sie definiert den Block! Dafür braucht es keine geschweiften Klammern oder andere Beginn-/Ende-Zeichen. Ob mit Tabs oder Spaces eingerückt und mit wievielen davon ist egal, es muss nur im ganzen Block konsistent sein. Auf der REPL wird die Einrückung automatisch vorgeschlagen und mit dem Prompt ``...`` angegeben, dass noch weitere Zeilen erwartet werden. Um die Eingabe abzuschliessen, wird da eine ausgerückte leere Zeile eingegeben. ::

   >>> i = 5
   >>> if i > 2:
   ...     print('yep')
   ...     print(i)
   ...
   yep
   5

Der alternative Zweig wird mit ``else:`` und einem weiteren eingerückten Block angegeben::

   >>> if i > 10:
   ...     print('yep')
   ... else:
   ...     print('nope')
   ...
   nope

Funktionen aufrufen
-------------------

Wir haben schon einige der eingebauten Funktionen von Python verwendet, z.B. ``len()`` und ``print()``. Es gibt eine ganze Menge davon, sie sind im Kapitel *Built-in Functions* der Python-Standard-Library-Referenz aufgelistet: https://docs.python.org/3/library/functions.html. Das ist eine sehr wichtige Dokumentationsseite, und es empfiehlt sich, sie beim Programmieren stets griffbereit zu halten. Eine weiteres, ebenso wichtiges Kapitel ist im Übrigen *Built-in Types*, https://docs.python.org/3/library/stdtypes.html. Es beschreibt alle die oben behandelten Datentypen wie Strings und Listen und die damit möglichen Operationen, das sind viele mehr als hier behandelt. Auch diese Seite gehört zu den essentiellen Werkzeugen des Python-Programmierers.

Die Funktion ``print()`` kann mehrere Argumente nehmen, welche durch Komma getrennt angegeben werden. Die entsprechenden Werte werden dann in dieser Reihenfolge hintereinander ausgegeben. ::

   >>> print('Bring', i, 'Donuts!')
   Bring 5 Donuts!

Neben diesen durch ihre Position in der Argumentliste identifizierten Argumenten können manche Funktionen auch noch Argumente mit Namen nehmen, sog. *keyword arguments*. ``print()`` nimmt gemäss Dokumentation u.a. die optionalen Keywords ``sep`` und ``end``, welche die Trennung zwischen den einzelnen Elementen (standardmässig ein Space) sowie den Abschluss nach dem letzten Element (standardmässig ein Zeilenumbruch) angeben. Damit kann z.B. mit separaten ``print()``-Aufrufen auf dieselbe Zeile geschrieben werden::

   >>> if i < 10:
   ...     print('Bring', end=' ')
   ...     print(i, end='')
   ...     print(' Donuts', '!', sep='')
   ...
   Bring 5 Donuts!


.. _hello-world:

Hello World: LED blinken
========================

.. image:: feather-led.*
   :align: center

Die Funktionalität zum Zugriff auf Hardware ist im Modul ``machine`` zusammengefasst. Es ist dokumentiert auf http://docs.micropython.org/en/latest/library/machine.html. Um ein Modul zu benützen, muss es *importiert* werden. Das ``import``-Statement macht zwei Dinge:

1. Es führt den Python-Code aus, der das Modul definiert. Was solcher Code typischerweise macht, ist Funktionen und Klassen zu definieren.
2. Es macht unter dem importierten Namen das Modul-Objekt im aktuellen Kontext verfügbar. Auf dem Modul-Objekt sind die in Schritt 1 definierten Funktionen und Klassen als Attribute verfügbar.

Schritt 1 wird nur beim erstmaligen Importieren ausgeführt, danach wird das Modul-Objekt im Speicher behalten.

GPIO-Pins (General Purpose Input/Output) werden durch die Klasse ``Pin`` aus dem Modul ``machine`` repräsentiert. ::

   >>> import machine
   >>> led = machine.Pin(16, machine.Pin.OUT)
   >>> led.value(1)
   >>> led.value(0)

Zum Blinken brauchen wir zusätzlich das Modul :py:mod:`time <micropython:utime>`. Es ist in MicroPython eine reduzierte Version des Moduls :py:mod:`python:time` der Python-Standard-Library. ::

   >>> import time
   >>> while True:
   ...     led.value(1)
   ...     time.sleep(0.5)
   ...     led.value(0)
   ...     time.sleep(0.5)
   ...

Mit ctrl-C auf der REPL kann aus der unendlichen Schleife ausgebrochen werden.

Es gibt auf dem Feather-Board noch eine LED, die mit Pin 0 verbunden ist, sowie auf dem ESP-12-Modul eine, die mit Pin 2 verbunden ist. Versuche auch diese anzusteuern.

Welche Module nebst ``machine`` und  ``time`` sonst noch verfügbar sind, lässt sich abfragen mit ::

   >>> help('modules')

Nachdem ein Modul importiert ist, kann ``help()`` auch Auskunft geben über seinen Inhalt::

  >>> help(machine)
