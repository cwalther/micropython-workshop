Vorbereitung
============

Mitbringen
----------

Du brauchst einen Laptop (Mac, Windows, Linux) mit

* WLAN
* USB-A-Anschluss (oder falls nur USB C, ein eigenes C-zu-Micro-B-Kabel)
* folgender Software installiert. Bitte vor dem Kurs installieren; wenn es dabei Schwierigkeiten gibt, beim Kursleiter melden.

Software zu installieren
------------------------

* **Python**, Version entweder 2.7 oder mindestens 3.4. Das bei macOS im Lieferumfang enthaltene Python 2.7 genügt. Wenn du noch keine passende Installation hast, lade die neuste Version 3.x von https://www.python.org/downloads/ herunter. (Python wird nur für *esptool.py* benötigt, nicht für den Rest des Kurses – da arbeiten wir auf dem Mikrocontroller, und die dortige Installation ist Teil des Kurses.)

* **esptool.py**. Falls deine Python-Installation ``pip`` enthält (nicht der Fall beim mit macOS mitgelieferten, sonst meistens schon), geht die Installation mit ``pip install esptool`` im Kommandozeilen-Terminal deines Systems. Ansonsten folge der Anleitung unter `https://github.com/espressif/esptool#installation-\\-dependencies <https://github.com/espressif/esptool#installation--dependencies>`_. Wenn du mit ``esptool.py -h`` den Hilfetext aufrufen kannst, ist die Installation in Ordnung.

* Ein zum Programmieren geeigneter **Text-Editor**. Syntax-Highlighting für Python ist von Vorteil, aber nicht unbedingt nötig. Vorschläge, falls du noch keinen Lieblings-Editor hast:

  * für macOS: *BBEdit* – http://www.barebones.com/products/bbedit/
  * für Windows: *Notepad++* – https://notepad-plus-plus.org
  * *Mu* – https://codewith.mu – speziell für (Micro)Python gemacht. Version mindestens 1.1.0, zur Zeit erst als Alpha verfügbar – vorher fehlt die Integration von ESP-Mikrocontrollern.

  Die mitgelieferten Editoren *TextEdit* von macOS und *Notepad* von Windows gehen zur Not, sind aber eher unkomfortabel. Die mitgelieferten Editoren diverser Linux-Distributionen, z.B. *gedit*, sind im Allgemeinen geeignet.

  Eine komplette IDE (Integrated Development Environment) ist nicht nötig. Wer trotzdem eine probieren möchte, kann folgende anschauen, es wird jedoch im Kurs nicht auf sie eingegangen:

  * Visual Studio Code – http://code.visualstudio.com
  * PyCharm – https://www.jetbrains.com/pycharm/
  * Thonny – https://thonny.org
  * uPyCraft – https://github.com/DFRobot/uPyCraft

* Eine **Terminal-Emulation für serielle Anschlüsse**.

  * Auf macOS und Linux kann das mitgelieferte Kommando ``screen`` verwendet werden.
  * Vorschlag für Windows: *PuTTY* – https://www.putty.org.
  * *Mu* (siehe oben) hat ein eingebautes serielles Terminal.

* Der **USB-Seriell-Treiber** von Silicon Labs: https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers
