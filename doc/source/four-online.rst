Vier Gewinnt Teil 2: Online
===========================

Protokolldesign
---------------

.. tabularcolumns:: |l|C|L|L|

+-------------------------------------------+--------+------------------------------------------+--------------------------------------------+
| Topic                                     | retain | Inhalt                                   | abonniert als                              |
+===========================================+========+==========================================+============================================+
| ``fourinarow/lobby/``\ *SenderName*       |   ✓    | ``b'1'`` = präsent                       | ``fourinarow/lobby/+``                     |
|                                           |        |                                          |                                            |
|                                           |        | ``b''`` = weg (löscht beim Broker        |                                            |
|                                           |        | gespeicherte Meldung) ← **letzter        |                                            |
|                                           |        | Wille**                                  |                                            |
+-------------------------------------------+--------+------------------------------------------+--------------------------------------------+
| ``fourinarow/join/``\ *EmpfängerName*     |        | Name des Senders (Herausfordernden)      | ``fourinarow/join/``\ *MeinName*           |
+-------------------------------------------+--------+------------------------------------------+--------------------------------------------+
| ``fourinarow/game/``\ *Name*\ ``/cursor`` |   ✓    | Cursor-Position 0-6 als Byte (8-bit-\    | ``fourinarow/game/``\ *GegnerName*\ ``/#`` |
|                                           |        | Ganzzahl)                                |                                            |
+-------------------------------------------+--------+------------------------------------------+                                            +
| ``fourinarow/game/``\ *Name*\ ``/drop``   |        | Kolonne 0-6 als Byte                     |                                            |
+-------------------------------------------+--------+------------------------------------------+--------------------------------------------+

Schritt 13: Präsenz in die Lobby publizieren
--------------------------------------------

*Mitschreiben*

*Neues:* ``import as``, Dateien lesen und schreiben, ``with``, ``try … finally``

Schritt 14: Lobby abonnieren
----------------------------

*Mitschreiben*

*Neues:* ``set`` (wie ``dict`` ohne Werte), Leerstring gilt als *false*

Schritt 15: Lobby als Menu anzeigen
-----------------------------------

*Mitschreiben*

*Neues:* Menu-API, Slice-Zuweisung, List Comprehension mit ``ìf``

Schritt 16: Herausfordern
-------------------------

*Mitschreiben*

*Neues:* ``None``, ``nonlocal``, ``elif``, ``break``, ``for … else``

Schritt 17: Cursor publizieren
------------------------------

*Übung:* Publiziere nach jedem Links-/Rechts-Tastendruck die Cursor-Position des lokalen Spielers auf das entsprechende Topic.

*Tipp:* Eine Möglichkeit, eine Zahl ``n`` von 0 bis 255 als Byte in ein ``bytes`` mit Länge 1 zu konvertieren::

  bytes((n,))

Das bedeutet: zuerst in ein ein-elementiges Tuple packen (innere Klammer; braucht ein abschliessendes Komma, sonst wird die Klammer bloss als arithmetische Gruppierung betrachtet), dann elementweise in ``bytes`` konvertieren (äussere Klammer vom Funktionsaufruf).

Für komplexere Konversionen von und nach Binärdaten als hier nötig gibt es das Modul :py:mod:`struct`, in MicroPython abgespeckt als :py:mod:`ustruct` verfügbar.

Schritt 18: Cursor abonnieren
-----------------------------

*Mitschreiben:* Beide Cursors zeichnen.

*Neues:* ternärer Operator ``a if c else b``

*Übung:* Abonniere die Cursor-Position des Gegners und aktualisiere die Variable ``opcursor``, wenn sie sich ändert.

*Tipp:* Du kannst eine neue Callback-Funktion setzen, um während des Spiels die Meldungen zu verarbeiten. Sie ersetzt die bisherige, die nicht mehr gebraucht wird. Die neue Funktion wird allerdings immer noch auch die Meldungen aus der Lobby empfangen – von denen sollten wir uns eigentlich abmelden, aber leider gibt es in ``umqtt`` keine ``unsubscribe``-Methode, die scheint einfach vergessen gegangen zu sein (MQTT unterstützt die Funktionalität durchaus).

Schritt 19: Zug publizieren
---------------------------

*Übung:* Publiziere die entsprechende Meldung, wenn der lokale Spieler einen Stein einwirft.

Schritt 20: Refactoring
-----------------------

*Mitschreiben:* Den Code fürs Einwerfen in eine Funktion packen, damit er später sowohl für die Züge des lokalen Spielers als auch die des Gegners verwendet werden kann.

Schritt 21: Zug abonnieren
--------------------------

*Mitschreiben:* Einwerfen des Gegners abonnieren. Gemeinsame Präfixe der Topics zusammenfassen.

Schritt 22: Anzeigen, wer am Zug ist
------------------------------------

*Übung:* Zeige dem lokalen Spieler an, wenn er am Zug ist, zum Beispiel mit einem Pixel in seiner Farbe im freien Bereich oben rechts.
