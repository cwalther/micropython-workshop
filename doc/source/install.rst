MicroPython installieren
========================

Schliesse das Feather-Board, noch ohne aufgesetztes PewPew, mit dem USB-Kabel an deinen Computer an.

Lade von http://micropython.org/download unter *Firmware for ESP8266 boards* die aktuelle stabile Version herunter, zur Zeit *esp8266-20191220-v1.12.bin*.

Folge dann den Anweisungen im dort verlinkten Tutorial http://docs.micropython.org/en/latest/esp8266/tutorial/intro.html#deploying-the-firmware. In Kürze:

.. code-block:: sh

   esptool.py --port <port> erase_flash
   esptool.py --port <port> --baud 460800 write_flash --flash_size=detect 0 esp8266-20191220-v1.12.bin

wobei für ``<port>`` der Name des verwendeten seriellen Anschlusses eingesetzt wird, so etwas wie ``/dev/cu.SLAB_USBtoUART`` auf macOS, ``/dev/ttyUSB0`` auf Linux oder ``COM4`` auf Windows.

Öffne mit einem Terminal-Emulations-Programm eine Verbindung auf denselben seriellen Anschluss mit 115200 Baud. Falls eine Einstellung für Flow-Control vorhanden ist, muss diese ausgeschaltet sein. Auf macOS und Linux beispielsweise

.. code-block:: sh

   screen <port> 115200

Drücke die Return-Taste. Du solltest folgende Antwort vom ESP8266 erhalten::

   >>>

Das ist der Prompt des interaktiven Python-Interpreters, auch *REPL* genannt, für «Read – Evaluate – Print Loop», weil er einen eingetippten Befehl einliest (read), ausführt (evaluate), das Ergebnis zurück schreibt (print), und wieder von vorne mit Einlesen beginnt (loop). Du kannst hier Python-Ausdrücke eingeben, versuch mal ::

   5+2

oder ::

   print("Hello World!")

Drücke ctrl-D. Damit wird MicroPython neu gestartet, und du siehst einige Meldungen, die du vorher verpasst hast, weil du zum Startzeitpunkt noch nicht verbunden warst, unter anderem die Version der installierten MicroPython-Firmware:

.. code-block:: none

   MicroPython v1.12 on 2019-12-20; ESP module with ESP8266

Drücke den Reset-Knopf (*RST*) auf dem Feather-Board. Im Terminal erscheint eine Menge unverständlicher Zeichen. Das sind die Boot-Meldungen des ESP8266, sie können interessant sein, wenn etwas nicht funktioniert. Weil sie mit einer anderen Baud-Rate gesendet werden, kann das Terminal, das auf 115200 Baud eingestellt ist, sie nicht richtig erkennen. Verbinde stattdessen mit 74880 Baud, um sie zu sehen. Nicht alle Terminal-Programme können das, weil es keine Standard-Rate ist – eines, das es kann, ist *miniterm.py*, welches als Teil von pySerial mit esptool.py mitkommt:

.. code-block:: sh

   miniterm.py --raw <port> 74880

Damit sollten die Boot-Meldungen lesbar herauskommen. Dafür sind jetzt die darauf folgenden Meldungen von MicroPython unverständlich, welche mit 115200 Baud gesendet wurden.

Um ``screen`` zu beenden, drücke ctrl-A, dann K. Um ``miniterm.py`` zu beenden, ctrl-] (US-Tastatur) oder ctrl-¨ (CH-Tastatur).

Netzwerk und WebREPL einrichten
===============================

Verbinde wieder mit 115200 Baud mit der REPL und gib ::

   help()

ein. Es werden einige Hinweise ausgegeben, darunter eine Anleitung, wie die Verbindung mit einem WLAN-Netzerk eingerichtet wird, was wir als nächstes tun wollen.

Die WLAN-Hardware des ESP8266 erscheint in MicroPython als zwei Netzwerk-Interfaces: Eines für den «Station»-Modus, also Verbindung zu einem bestehenden Netzwerk, und eines für den «Access Point»-Modus, also Anbieten eines eigenen Netzwerks. Es können beide unabhängig voneinander aktiv sein. In einer frischen Installation ist das STA-Interface ausgeschaltet und das AP-Interface eingeschaltet (du kannst es von einem anderen WLAN-fähigen Gerät aus sehen, es heisst sowas wie *MicroPython-123456* und das Passwort lautet ``micropythoN``, mehr dazu im `Tutorial <http://docs.micropython.org/en/latest/esp8266/tutorial/intro.html#wifi>`_).

Um das AP-Interface auszuschalten, welches wir hier nicht brauchen, und stattdessen das STA-Interface einzuschalten und mit dem lokalen WLAN zu verbinden, führe folgende Zeilen auf der REPL aus, mit dem richtigen Netzwerknamen und Passwort eingesetzt::

   import network
   ap = network.WLAN(network.AP_IF)
   ap.active(False)
   sta = network.WLAN(network.STA_IF)
   sta.active(True)
   sta.connect('<Netzwerkname>', '<Passwort>')
   sta.isconnected()
   sta.isconnected()
   sta.ifconfig()

``ìsconnected()`` kannst du so lange wiederholen, bis es ``True`` zurück gibt, danach gibt ``ifconfig()`` als ersten Eintrag die IP-Adresse zurück, die das Board vom DHCP-Server erhalten hat, wir werden sie im nächsten Schritt brauchen.

Der ESP8266 speichert die WLAN-Konfiguration im Flash und wird beim nächsten Einschalten wieder versuchen, mit demselben Netzwerk zu verbinden. Das ist bequem, hat aber den Nachteil, dass man die Konfiguration nicht zu häufig unnötig ändern sollte, da dabei die entsprechenden Flash-Speicherzellen abgenützt werden.

Wenn der ESP8266 am Netzwerk ist, kann auf diesem Wege auch auf die REPL zugegriffen werden. So spart man sich das USB-Kabel (ausser für die Stromversorgung). Das nennt sich *WebREPL* und ist nach der frischen Installation standardmässig nicht aktiv, sondern muss erst eingerichtet werden. Dazu führst du auf der REPL ::

   import webrepl_setup

aus und folgst den Anweisungen, um ein Passwort zu setzen und die WebREPL beim Einschalten automatisch zu starten.

Der Client, um auf die WebREPL zuzugreifen, kann von https://github.com/micropython/webrepl heruntergeladen werden (*Clone or download* ▸ *Download ZIP*). Öffne *webrepl.html* in einem Webbrowser. Alternativ kann auch http://micropython.org/webrepl verwendet werden. Setze die oben herausgefundene IP-Adresse anstelle von ``192.168.4.1`` ins Feld oben links auf der Seite ein (nicht ins Adressfeld des Browsers) und klicke *Connect*. Im schwarzen Terminal-Bereich erscheint eine Passwort-Abfrage. Gib dort das oben konfigurierte Passwort ein und bestätige mit Return, worauf der REPL-Prompt ``>>>`` erscheint. Du kannst dort nun arbeiten wie auf der seriellen REPL.
