PewPew
======

Es existiert eine ganze Familie von PewPew-Geräten. Allen gemeinsam ist das 8×8-Display mit 4 Farben und die 6 Tasten, und sie werden über dasselbe API angesteuert, so dass Programme unverändert auf allen laufen, soweit sie keine weiteren Hardware-spezifischen Funktionen benützen. Sie unterscheiden sich in der Form, den Displayfarben und der Wahl des Mikrocontrollers und können beim Entwickler Radomir Dopieralski auf https://www.tindie.com/stores/deshipu/ erworben werden.

Installation
------------

Die nötigen Libraries für den Betrieb unseres PewPew Lite 4.2 mit MicroPython finden sich auf https://github.com/pewpew-game/pew-pewpewlite-4.x-micropython. Ausserdem benötigen wir https://github.com/cwalther/game-menu/tree/menumodule. Verwende bei beiden *Clone or download* ▸ *Download ZIP* (oder klone sie mit Git). Wer will, kann von den *game-* Repositories unter https://github.com/pewpew-game noch ein paar Beispiel-Spiele herunterladen.

Beim ersten Start nach der Installation wurde von MicroPython in dem Teil des Flash-Speichers, der nicht von der Firmware belegt ist, ein Filesystem eingerichtet. Dort können Libraries und Scripts abgelegt werden, ohne dass die Firmware neu installiert werden muss. Um Dateien vom Computer zu übertragen, kann WebREPL verwendet werden, entweder mit dem Kasten oben rechts im Web-Interface oder auf der Kommandozeile mit dem im WebREPL-Download enthaltenen Tool *webrepl_cli.py*. Es gibt keinen offiziellen Standard, um Dateien über die serielle (USB-) Verbindung zu übertragen, jedoch tun das einige von der Community entwickelte Tools, indem sie sie quasi automatisiert über die REPL eintippen. Beispiele sind `rshell <https://github.com/dhylands/rshell>`_ und `ampy <https://github.com/pycampers/ampy>`_.

Die Dateien *pew.py*, *random.py* und *time.py* aus *pew-pewpewlite-4.x-micropython* sowie *pew_menu.py* aus *game-menu* müssen in den Ordner ``/lib/`` auf dem Board. Dieser existiert standardmässig noch nicht und muss erst erzeugt werden. Bei *webrepl_cli.py* kann ein absoluter Zielpfad angegeben werden, das Web-Interface platziert Dateien immer im *current working directory*. Dieses kann per REPL geändert werden. Führe also folgendes aus::

   >>> import os
   >>> os.mkdir('/lib')
   >>> os.chdir('/lib')

Optional kann *main.py* aus *game-menu* ins Wurzelverzeichnis ``/`` installiert werden, es präsentiert dann beim Einschalten ein Menu, aus dem alle installierten Spiele gestartet werden können. Jegliche Datei namens ``/main.py`` wird beim Starten automatisch ausgeführt, nach ``/boot.py``, welches automatisch kreiert wurde und einige Grundeinstellungen macht, unter anderem den WebREPL-Server startet. Um mit dem Web-Interface dorthin zu installieren, einfach vorher wieder ``os.chdir('/')`` ausführen.

Der Inhalt des Filesystems kann mit ``os.listdir()`` und anderen Funktionen aus dem Modul :py:mod:`os <uos>` erkundet werden.

API-Dokumentation
-----------------

Die Dokumentation für PewPew ist zu finden unter https://pewpew.readthedocs.io/, und das Kapitel zum API ist hier zur Gänze reproduziert:

PewPew Library Reference
^^^^^^^^^^^^^^^^^^^^^^^^

.. module:: pew

.. function:: init()

    Initialize the module.

    This function switches the display on and performs some basic setup.


.. function:: brightness(level)

    Set the brightness of the display, from 0 (minimum) to 15 (maximum). On
    devices that don't support varying the brightness this does nothing.


.. function:: show(pix)

    Show the provided image on the display, starting at the top left corner.
    You will want to call this once for every frame.


.. function:: keys()

    Return a number telling which keys (or buttons) have been pressed since the
    last check.  The number can then be filtered with the ``&`` operator and
    the ``K_X``, ``K_DOWN``, ``K_LEFT``, ``K_RIGHT``, ``K_UP``, and ``K_O``
    constants to see whether any of the keys was pressed.


.. function:: tick(delay)

    Wait until ``delay`` seconds have passed since the last call to this
    function. You can call it every frame to ensure a constant frame rate.


.. class:: Pix(width=8, height=8, buffer=None)

    Pix represents a drawing surface, ``width`` pixels wide and ``height``
    pixels high.

    If no ``buffer`` is specified for storing the data, a suitable one will
    be automatically created.

    .. classmethod:: from_iter(cls, lines)

        Creates a new Pix and initialzes its contents by iterating over
        ``lines`` and then over individual pixels in each line. All the lines
        have to be at least as long as the first one.

    .. classmethod:: from_text(cls, text, color=None, background=0, colors=None)

        Creates a new Pix and renders the specified text on it. It is exactly
        the size needed to fit the specified text. Newlines and other control
        characters are rendered as spaces.

        If ``color`` is not specified, it will use yellow and red for the
        letters by default. Otherwise it will use the specified color, with
        ``background`` color as the background.

        Alternatively, ``colors`` may be specified as a 4-tuple of colors,
        and then the ``color`` and ``background`` arguments are ignored, and
        the four specified colors are used for rendering the text.

    .. method:: pixel(self, x, y, color=None)

        If ``color`` is specified, sets the pixel at location ``x``, ``y`` to
        that color. If not, returns the color of the pixel at that location.

        If the location is out of bounds of the drawing surface, returns 0.

    .. method:: box(self, color, x=0, y=0, width=self.width, height=self.height)

        Draws a filled box with the specified ``color`` with its top left
        corner at the specified location and of the specified size. If no
        location and size are specified, fills the whole drawing surface.

    .. method:: blit(self, source, dx=0, dy=0, x=0, y=0, width=None, height=None, key=None)

        Copied the ``source`` drawing surface onto this surface at location
        specified with ``dx`` and ``dy``.

        If ``x``, ``y``, ``widht`` and ``height`` are specified, only copies
        that fragment of the ``source`` image, otherwise copies it whole.

        If ``key`` color is specified, that color is considered transparent
        on the source image, and is not copied onto this drawing surface.

Ausprobieren
------------

Spiele auf der REPL mit dem PewPew-API: Zeichne etwas aufs Display! Prüfe, welche Tasten gedrückt sind! Welche der Zahlen 0 – 3 stellt welche Farbe dar?

Vorschläge zum Einstieg::

   >>> import pew
   >>> pew.init()
   >>> p = pew.Pix()
   >>> p.pixel(1, 2, 3)
   >>> pew.show(p)
   >>> pew.keys()

Schritt 1: Programm-Skelett
---------------------------

*Mitschreiben:* Grundlegende Spiel-Schleife.

*Neues:* Kommentare

Speichere das Programm als *four.py* und übertrage es aufs Board. Um es von der REPL aus auszuführen, wird das ``import``-Statement verwendet::

   >>> import four

Mit ctrl-C kommst du aus der unendlichen Schleife auf den REPL-Prompt zurück. Wird jetzt nochmals ``import four`` ausgeführt, geschieht nichts. Warum? Wie in :ref:`hello-world` erwähnt, wird der Code nur beim ersten Import ausgeführt und das resultierende Modul-Objekt gecacht. Diesen Cache müssen wir erst löschen, um das Programm nochmals auszuführen. Er ist von Python aus zugänglich als Dictionary ``sys.modules``. Also::

   >>> import sys
   >>> del sys.modules['four']
