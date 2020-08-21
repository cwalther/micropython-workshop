Vorbereitung
============

Mitbringen
----------

Du brauchst einen Laptop (Mac, Windows, Linux) mit

* WLAN
* USB-A-Anschluss (oder falls nur USB C, ein eigenes C-zu-Micro-B-Kabel)
* Software gemäss den folgenden Abschnitten installiert. Bitte vor dem Kurs installieren; wenn es dabei Schwierigkeiten gibt, beim Kursleiter melden.

Für den Online-Kurs wird zusätzlich benötigt: eine Webcam, damit andere auf dein Gerät sehen können, und ein Webbrowser, der mit dem Videokonferenzdienst https://whereby.com funktioniert. Probiere es vor dem Kurs aus (Zugang kann beim Kursleiter erfragt werden), insbesondere auch Screen-Sharing, damit du bei Bedarf dein Programm dem Kursleiter zeigen kannst.

.. _einfache-installation:

Einfache Installation
---------------------

Wer unsicher ist, ist mit dieser Software gut bedient. Wer sich weniger reinreden lassen will, findet im nächsten Abschnitt weitere Optionen.

* **Python**. Lade die neuste Version 3.x von https://www.python.org/downloads/ herunter oder (Linux) installiere sie vom Paketmanager deines Systems (inklusive ``pip``, welches möglicherweise ein separates Paket im Paketmanager ist).
  Windows: Aktiviere auf der ersten Seite des Installers die Option «Add Python 3.x to PATH», sonst funktioniert der nächste Schritt nicht.

* **esptool.py** wird installiert mit ``pip install esptool`` im Kommandozeilen-Terminal deines Systems. Wenn du mit ``esptool.py -h`` den Hilfetext aufrufen kannst, ist die Installation in Ordnung.

* **Mu**, welches speziell für (Micro)Python gemacht ist, als **Text-Editor** und **serielles Terminal**. Lade von https://codewith.mu die Version 1.1.0 oder neuer herunter, zur Zeit erst als Alpha verfügbar – vorher fehlt die Integration von ESP-Mikrocontrollern.

* Der **USB-Seriell-Treiber** von Silicon Labs: https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers

  * *Mac*: Manuelle Installation notwendig.
  * *Windows*: Auf aktuellen Systemen wird er automatisch per Windows-Update heruntergeladen und installiert, sobald das Feather-Board per USB verbunden wird. Ansonsten manuell installieren.
  * *Linux*: Normalerweise keine Installation notwendig, da der Treiber beim Kernel dabei ist; daran zu erkennen, dass beim Einstecken des Feather-Boards ``/dev/ttyUSB0`` erscheint. Um Zugriff auf serielle Anschlüsse zu erhalten, muss dein Benutzer möglicherweise in einer speziellen Gruppe sein. Beachte dazu die Dokumentation deiner Distribution. Auf Debian-basierten Systemen beispielsweise ``sudo adduser `whoami` dialout``, anschliessend neu einloggen.

Fortgeschrittene Installation
-----------------------------

Wenn du schon deine Lieblings-Werkzeuge hast oder gern selber entscheidest, was auf deinem System installiert wird, sind abweichend von der einfachen Installation folgende Varianten möglich:

* **Python**: Eine bestehende Installation von Version entweder 2.7 oder mindestens 3.4 genügt. Darunter fällt insbesondere das bei macOS im Lieferumfang enthaltene Python 2.7, so dass dort keine Installation nötig ist.

  Wenn, wie beim mit macOS mitgelieferten 2.7, deine Python-Installation kein ``pip`` enthält, muss **esptool.py** manuell installiert werden: Lade das neuste Release von https://github.com/espressif/esptool/releases herunter und installiere es mit ``sudo python setup.py install`` im Terminal gemäss der Anleitung unter `https://github.com/espressif/esptool#installation-\\-dependencies <https://github.com/espressif/esptool#installation--dependencies>`_. Den heruntergeladenen Ordner kannst du danach wieder löschen.

  (Python wird nur für *esptool.py* benötigt, nicht für den Rest des Kurses – da arbeiten wir auf dem Mikrocontroller, und die dortige Installation ist Teil des Kurses.)

* Ein beliebiger zum Programmieren geeigneter **Text-Editor**. Syntax-Highlighting für Python ist von Vorteil, aber nicht unbedingt nötig. Beispiele:

  * für macOS: *BBEdit* – http://www.barebones.com/products/bbedit/
  * für Windows: *Notepad++* – https://notepad-plus-plus.org

  Die mitgelieferten Editoren *TextEdit* von macOS und *Notepad* von Windows gehen zur Not, sind aber eher unkomfortabel. Die mitgelieferten Editoren diverser Linux-Distributionen, z.B. *gedit*, sind im Allgemeinen geeignet.

  Eine komplette IDE (Integrated Development Environment) ist nicht nötig. Wer trotzdem eine probieren möchte, kann folgende anschauen, es wird jedoch im Kurs nicht auf sie eingegangen:

  * Visual Studio Code – http://code.visualstudio.com
  * PyCharm – https://www.jetbrains.com/pycharm/
  * Thonny – https://thonny.org
  * uPyCraft – https://github.com/DFRobot/uPyCraft

* Eine beliebige **Terminal-Emulation für serielle Anschlüsse**. Statt des eingebauten seriellen Terminals von *Mu* (siehe :ref:`einfache-installation`) können beispielsweise verwendet werden:

  * auf macOS und Linux: das mitgelieferte oder im Paketmanager verfügbare Kommando ``screen``
  * auf Windows: *PuTTY* – https://www.putty.org
