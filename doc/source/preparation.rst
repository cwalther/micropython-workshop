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

Mac
^^^

* **Python** 2.7 ist bei macOS im Lieferumfang enthalten, es ist keine Installation nötig. (Python wird nur für *esptool.py* benötigt, nicht für den Rest des Kurses – da arbeiten wir auf dem Mikrocontroller, und die dortige Installation ist Teil des Kurses.)

* **esptool.py**. Falls du das mit macOS mitgelieferte Python verwendest: Lade das neuste Release von https://github.com/espressif/esptool/releases herunter und installiere es mit ``sudo python setup.py install`` im Terminal gemäss der Anleitung unter `https://github.com/espressif/esptool#installation-\\-dependencies <https://github.com/espressif/esptool#installation--dependencies>`_. Den heruntergeladenen Ordner kannst du danach wieder löschen.

  Falls du eine eigene Python-Installation verwendest, welche ``pip`` enthält, ist auch ``pip install esptool`` möglich.

  Wenn du mit ``esptool.py -h`` den Hilfetext aufrufen kannst, ist die Installation in Ordnung.

* Ein zum Programmieren geeigneter **Text-Editor**. Syntax-Highlighting für Python ist von Vorteil, aber nicht unbedingt nötig. Vorschläge, falls du noch keinen Lieblings-Editor hast:

  * *BBEdit* – http://www.barebones.com/products/bbedit/
  * *Mu* – https://codewith.mu – speziell für (Micro)Python gemacht. Version mindestens 1.1.0, zur Zeit erst als Alpha verfügbar – vorher fehlt die Integration von ESP-Mikrocontrollern.

  Der mitgelieferte Editor *TextEdit* von macOS geht zur Not, ist aber eher unkomfortabel.

  Eine komplette IDE (Integrated Development Environment) ist nicht nötig. Wer trotzdem eine probieren möchte, kann folgende anschauen, es wird jedoch im Kurs nicht auf sie eingegangen:

  * Visual Studio Code – http://code.visualstudio.com
  * PyCharm – https://www.jetbrains.com/pycharm/
  * Thonny – https://thonny.org
  * uPyCraft – https://github.com/DFRobot/uPyCraft

* Eine **Terminal-Emulation für serielle Anschlüsse**. Vorschläge:

  * Es kann das mitgelieferte Kommando ``screen`` verwendet werden, keine Installation nötig.
  * *Mu* (siehe oben) hat ein eingebautes serielles Terminal.

* Der **USB-Seriell-Treiber** von Silicon Labs: https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers

Windows
^^^^^^^

* **Python**, Version entweder 2.7 oder mindestens 3.4. Wenn du noch keine passende Installation hast, lade die neuste Version 3.x von https://www.python.org/downloads/ herunter. (Python wird nur für *esptool.py* benötigt, nicht für den Rest des Kurses – da arbeiten wir auf dem Mikrocontroller, und die dortige Installation ist Teil des Kurses.)

* **esptool.py**. Falls deine Python-Installation ``pip`` enthält (meistens der Fall), installiere mit ``pip install esptool`` in der Eingabeaufforderung.

  Ansonsten lade das neuste Release von https://github.com/espressif/esptool/releases herunter und installiere es mit ``python setup.py install`` gemäss der Anleitung unter `https://github.com/espressif/esptool#installation-\\-dependencies <https://github.com/espressif/esptool#installation--dependencies>`_. Den heruntergeladenen Ordner kannst du danach wieder löschen.

  Wenn du mit ``esptool.py -h`` den Hilfetext aufrufen kannst, ist die Installation in Ordnung.

* Ein zum Programmieren geeigneter **Text-Editor**. Syntax-Highlighting für Python ist von Vorteil, aber nicht unbedingt nötig. Vorschläge, falls du noch keinen Lieblings-Editor hast:

  * *Notepad++* – https://notepad-plus-plus.org
  * *Mu* – https://codewith.mu – speziell für (Micro)Python gemacht. Version mindestens 1.1.0, zur Zeit erst als Alpha verfügbar – vorher fehlt die Integration von ESP-Mikrocontrollern.

  Der mitgelieferte Editor *Notepad* von Windows geht zur Not, ist aber eher unkomfortabel.

  Eine komplette IDE (Integrated Development Environment) ist nicht nötig. Wer trotzdem eine probieren möchte, kann folgende anschauen, es wird jedoch im Kurs nicht auf sie eingegangen:

  * Visual Studio Code – http://code.visualstudio.com
  * PyCharm – https://www.jetbrains.com/pycharm/
  * Thonny – https://thonny.org
  * uPyCraft – https://github.com/DFRobot/uPyCraft

* Eine **Terminal-Emulation für serielle Anschlüsse**. Vorschläge:

  * *PuTTY* – https://www.putty.org.
  * *Mu* (siehe oben) hat ein eingebautes serielles Terminal.

* Der **USB-Seriell-Treiber** von Silicon Labs. Auf aktuellen Systemen wird er automatisch per Windows-Update heruntergeladen und installiert, sobald das Feather-Board per USB verbunden wird. Ansonsten lade ihn herunter von https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers.

Linux
^^^^^

* **Python**, Version entweder 2.7 oder mindestens 3.4. Die meisten Linux-Distributionen dürften das schon vorinstalliert oder zumindest im Paketmanager verfügbar haben. (Python wird nur für *esptool.py* benötigt, nicht für den Rest des Kurses – da arbeiten wir auf dem Mikrocontroller, und die dortige Installation ist Teil des Kurses.)

* **esptool.py**. Falls deine Python-Installation ``pip`` enthält (meistens der Fall, eventuell als separates Paket im Paketmanager), geht die Installation mit ``pip install esptool`` im Terminal.

  Ansonsten lade das neuste Release von https://github.com/espressif/esptool/releases herunter und installiere es mit ``python setup.py install`` (falls es sich über Zugriffsrechte beschwert, ``sudo python setup.py install``) gemäss der Anleitung unter `https://github.com/espressif/esptool#installation-\\-dependencies <https://github.com/espressif/esptool#installation--dependencies>`_. Den heruntergeladenen Ordner kannst du danach wieder löschen.

  Wenn du mit ``esptool.py -h`` den Hilfetext aufrufen kannst, ist die Installation in Ordnung.

* Ein zum Programmieren geeigneter **Text-Editor**. Syntax-Highlighting für Python ist von Vorteil, aber nicht unbedingt nötig. Vorschläge, falls du noch keinen Lieblings-Editor hast:

  * Die mitgelieferten Editoren diverser Linux-Distributionen, z.B. *gedit*, sind im Allgemeinen geeignet.
  * *Mu* – https://codewith.mu – speziell für (Micro)Python gemacht. Version mindestens 1.1.0, zur Zeit erst als Alpha verfügbar – vorher fehlt die Integration von ESP-Mikrocontrollern.

  Eine komplette IDE (Integrated Development Environment) ist nicht nötig. Wer trotzdem eine probieren möchte, kann folgende anschauen, es wird jedoch im Kurs nicht auf sie eingegangen:

  * Visual Studio Code – http://code.visualstudio.com
  * PyCharm – https://www.jetbrains.com/pycharm/
  * Thonny – https://thonny.org
  * uPyCraft – https://github.com/DFRobot/uPyCraft

* Eine **Terminal-Emulation für serielle Anschlüsse**. Vorschläge:

  * Es kann das vorinstallierte oder im Paketmanager verfügbare Kommando ``screen`` verwendet werden.
  * *Mu* (siehe oben) hat ein eingebautes serielles Terminal.
  
  Um Zugriff auf serielle Anschlüsse zu erhalten, muss dein Benutzer möglicherweise in einer speziellen Gruppe sein. Beachte dazu die Dokumentation deiner Distribution. Auf Debian-basierten Systemen beispielsweise ``sudo adduser `whoami` dialout``, anschliessend neu einloggen.

* Der **USB-Seriell-Treiber** von Silicon Labs sollte normalerweise vorinstalliert sein; daran zu erkennen, dass beim Einstecken des Feather-Boards ``/dev/ttyUSB0`` erscheint. Ansonsten ist er erhältlich von https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers.
