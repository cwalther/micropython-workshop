Vier Gewinnt Teil 1: Offline
============================

Zur späteren Referenz: Der Code für alle folgenden Schritte ist verfügbar auf https://github.com/cwalther/micropython-workshop-fourinarow/commits/master. Während des Kurses brauchen wir das aber nicht.

Schritt 1 war das Programm-Skelett aus dem letzten Kapitel. Wir bauen es nun schrittweise aus.

Schritt 2: Cursor
-----------------

.. image:: cursor-board.*
   :align: center

*Übung:* Zeige einen Cursor an, der sich mit den Links- und Rechts-Tasten auf den ersten 7 Pixeln der obersten Zeile bewegen lässt.

*Tipp:* Benutze den bitweisen Und-Operator ``&`` mit ``k`` und den Konstanten ``pew.K_…``, um festzustellen, ob eine Taste gedrückt ist. Sein Ergebnis ist entweder Null oder ungleich Null und kann somit direkt in einer ``if``-Bedingung verwendet werden.

Schritt 3: ``main()``-Funktion
------------------------------

*Mitschreiben:* Den Code in eine Funktion einpacken. Dies hat folgende Vorteile:

* Die Funktion kann nochmals aufgerufen werden, ohne dass das Modul nochmals importiert werden muss. (Das funktioniert jedoch nur, wenn der Import fehlerfrei abläuft. Das ist bei uns bisher nicht der Fall, da wir ihn, wegen unendlicher Schleife, mit ctrl-C abbrechen. In diesem Fall wird der zweite Teil des Imports, das Verfügbarmachen im aktuellen Kontext unter dem importierten Namen, nicht ausgeführt. Das kann aber mit einem erneuten Import nachgeholt werden, da das unfertige Modulobjekt in ``sys.modules`` vorhanden ist.)
* In MicroPython sind lokale Variablen effizienter als globale.


*Neues:* ``def``

Schritt 4: Wer ist am Zug?
--------------------------

*Mitschreiben:* Nach jedem Einwerfen eines Steins umschalten, wer am Zug ist.

*Neues:* Bitweises Oder: ``|``

*Übung:* Teste das Programm. Was ist noch nicht gut?

Schritt 5: Flanken-Detektion
----------------------------

*Mitschreiben:* Nur auf die steigende Flanke des Tastendrucks reagieren.

*Neues:* Bitweises Nicht: ``~``, Binärzahlen: ``0b…``

Schritt 6: Brett anzeigen und Stein platzieren
----------------------------------------------

*Mitschreiben:* Brett anzeigen.

*Übung:* Platziere bei einem Zug den neuen Stein am richtigen Ort auf dem Brett (noch ohne Animation, direkt an die endgültige Position). Finde dazu mit einer ``while``-Schleife den untersten noch freien Platz in der Spalte unter dem Cursor.

Schritt 7: Gewinn horizontal
----------------------------

.. image:: win-horizontal.*
   :align: center

*Mitschreiben:* Gewinn durch horizontale Reihen detektieren und hervorheben.

*Neues:* ``for … in range()``, ``all()``, *List Comprehension* und *Generator Expression* (auf REPL ausprobieren), Tuple in mehrere Variablen auflösen

Schritt 8: Gewinn vertikal und diagonal
---------------------------------------

.. image:: win-vertical-diagonal.*
   :align: center

*Übung:* Detektiere auch Gewinn durch vertikale und diagonale Reihen.

Schritt 9: Beenden
------------------

*Mitschreiben:* Nach einem Gewinn die normale Tastenbehandlung stoppen und beim nächsten Tastendruck das Programm beenden.

Da das Programm nun sauber beenden kann und nicht mehr mit ctrl-C unterbrochen werden muss, kann es auch aus dem Menu gestartet werden und kehrt nach dem Beenden zum Menu zurück. Das erspart einem das ständige ``del sys.modules['four']; import four``. Drücke ctrl-D, um MicroPython neu zu starten, so dass *main.py* ausgeführt wird und das Menu anzeigt. Der Neustart unterbricht die WebREPL-Verbindung, sie kann aber nachher wieder aufgebaut werden, um trotzdem die Ausgabe des Programms und eventuelle Fehlermeldungen zu sehen.

Schritt 10: Animation
---------------------

*Mitschreiben:* Das Einwerfen eines Steins animieren, auf solche Art, dass mehrere Steine gleichzeitig fallen können.

*Neues:* *Generator Function*, ``range()`` mit negativem Schritt, ``next()``, ``try … except``

*Stand des Programms:* https://github.com/cwalther/micropython-workshop-fourinarow/blob/s10/four.py

Schritt 11: Blinken
-------------------

*Mitschreiben:* Eine gewinnende Reihe durch Blinken statt durch orange Farbe hervorheben.

*Übung:* Teste das Programm. Welche zwei Dinge funktionieren noch nicht gut?

Schritt 12: Warten auf Animationen
----------------------------------

*Übung:* Blinke und beende bei Tastendruck erst, wenn die ``drop``-Animation fertig ist.

*Tipp:*

* Prüfe ``len(animations)``, um zu sehen, wie viele Animationen noch am Laufen sind.
* ``blink()`` darf durchaus schon vorher starten, es kann einfach so lange nichts tun, wie kein sichtbares Blinken erwünscht ist.
