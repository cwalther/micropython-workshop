Was ist MicroPython? Was ist der ESP8266?
=========================================

MicroPython
-----------

*MicroPython* ist eine Implementation der Programmiersprache Python, die speziell auf die beschränkten Ressourcen von Mikrocontrollern ausgelegt ist. Es stammt nicht von der auf Desktop-Systemen normalerweise verwendeten Referenz-Implementation *CPython* ab, implementiert aber (nahezu) dieselbe Sprache, Python 3, sowie Teile der zugehörigen Standard-Bibliotheken.

Daraus folgt: Wichtigster Anlaufpunkt für Fragen zur Sprache und zu den Standard-Bibliotheken ist die Python-Dokumentation, https://docs.python.org/3/. Eigenheiten von MicroPython, Unterschiede zu CPython und Spezifisches zur Hardware-Programmierung auf dem Mikrocontroller sind dokumentiert auf http://docs.micropython.org/. Diskussion innerhalb der Anwender-Community findet statt auf http://forum.micropython.org/.

MicroPython wird entwickelt vom australischen Physiker und Robotik-Ingenieur Damien George, inzwischen zusammen mit einem Team von Freiwilligen. Begonnen 2013, wurde es 2014 nach einer Kickstarter-Kampagne erstmals veröffentlicht, zusammen mit dem eigens dafür entwickelten Pyboard, einer Entwicklerplatine basierend auf einem STM32-Mikrocontroller. 2016 kam im Rahmen einer zweiten Kickstarter-Kampagne der Port für den ESP8266 hinzu, und heute werden auch ESP32, nRF51/52 und, zum Teil durch Drittanbieter, diverse weitere Mikrocontroller unterstützt.

Die Open-Source-Lizenz von MicroPython (MIT) erlaubt es diversen anderen Personen und Organisationen, ihre eigenen Varianten (Forks) von MicroPython anzubieten, welche zusätzliche Hardware unterstützen oder andere Änderungen mitbringen. Darunter zu nennen ist insbesondere *CircuitPython* von Adafruit, welches dafür optimiert ist, Anfängern in Elektronik und Programmierung den Einstieg zu erleichtern. Seine APIs für den Umgang mit Mikrocontroller-Hardware und seine Untermenge der Standard-Bibliotheken sind leicht anders, und im Gegensatz zu MicroPython unterstützt es die Mikrocontroller SAMD21 und SAMD51, welche in vielen Adafruit-Produkten zum Einsatz kommen. CircuitPython ist auch das Standard-Betriebssystem für die PewPew-Geräte wie den in diesem Kurs verwendeten PewPew Lite. Es unterstützt jedoch in der neusten Version den ESP8266 nicht mehr, weshalb hier MicroPython verwendet wird.

ESP8266
-------

2014 erschienen, tauchte der ESP8266 vom chinesischen Hersteller Espressif Systems zunächst in billigen WLAN-Adaptern auf. Auch unter Hobby-Bastlern erlangte er dank des tiefen Preises einige Beliebtheit, um als Ko-Prozessor andere Mikrocontroller-Systeme wie Arduinos ans Drahtlosnetzwerk anzuschliessen. Bald schon entdeckten findige Maker aber, dass sich darin neben der integrierten WLAN-Funktionalität auch ein recht leistungsfähiger Allzweck-Mikrocontroller verbirgt. Sie begannen ihre eigene Software dafür zu schreiben, zunächst wegen rarer und nur auf Chinesisch verfügbarer Dokumentation auf eigene Faust, später auch mit offizieller Unterstützung vom Hersteller. Heute kann der ESP8266 so nebst dem Entwickler-Kit des Herstellers auch mit der Arduino-IDE programmiert werden.

Mit 80 MHz Taktfrequenz, 80 KiB RAM und ≥ 512 KiB Flash ist der ESP8266 leistungsfähig genug, um High-Level-Sprachen wie Lua, JavaScript oder Python zu unterstützen. Im Gegensatz zu den meisten anderen Mikrocontrollern hat er kein internes Flash, sondern benützt einen externen Flash-Chip, der über die SPI-Schnittstelle angeschlossen ist. Dies erlaubt, verschiedene Flash-Chips von bis zu 16 MiB zu verwenden (MicroPython unterstützt 1 MiB bis 4 MiB, auf 512 KiB ist es eingeschränkt nutzbar) und defektes/abgenütztes Flash zu ersetzen. Es verunmöglicht auch, den Mikrocontroller durch Flashen von defekter Software unbrauchbar zu machen, da das Flash auch von aussen programmiert werden kann.

Der ESP8266 hat kein USB, d.h. er kann nicht als USB-Laufwerk, Kommunikationsgerät oder Eingabegerät am Entwickler-Computer erscheinen. Das Programmieren und andere Kommunikation geschieht stattdessen per UART (serielle Schnittstelle). Die meisten Entwicklerboards haben dafür einen USB-zu-Seriell-Konverter an Bord.

Wer kein Hardcore-Elektroingenieur ist, verwendet normalerweise nicht den nackten IC, sondern ein Modul, auf dem ESP8266, Flash und Antenne integriert sind. Dies hat auch den Vorteil, dass keine RF-Zertifizierung nötig ist, wenn man sein Produkt an Endkunden vertreiben will, wenn das Modul schon zertifiziert ist. Das heute gebräuchlichste Modul ist das ESP-12 in verschiedenen Varianten.

.. image:: boards.*
   :align: center

Zum Entwickeln und Experimentieren noch bequemer ist es, eine Entwicklerplatine einzusetzen, welche nebst dem ESP8266(-Modul) einen USB-zu-Seriell-Konverter, einen 3.3V-Spannungsregler für die Stromversorgung ab USB, manchmal einen Lithium-Akku-Lader oder zusätzliche Sensoren und Aktoren enthält. Die verbreitetsten ESP8266-Entwicklerboards sind `NodeMCU <https://frightanic.com/iot/comparison-of-esp8266-nodemcu-development-boards/>`_ von diversen Herstellern, `Wemos D1 mini <https://wiki.wemos.cc/products:d1:d1_mini>`_ und `Adafruit Feather HUZZAH <https://www.adafruit.com/product/2821>`_. In diesem Kurs wird das Board von Adafruit verwendet. Es ist, da in den USA entwickelt und hergestellt, teurer als die chinesischen Konkurrenten, dafür von hoher Qualität, bietet guten Support und Dokumentation, und ist kompatibel mit einer grossen Zahl von Zubehörprodukten sowohl von Adafruit als auch von Drittherstellern und Makern, den «Feather Wings» (ähnlich den «Shields» bei Arduino), darunter dem hier verwendeten PewPew Lite.

ESP32
-----

2016 brachte Espressif Systems den ESP32 auf den Markt, mit höherer Taktfrequenz, Dual-Core-CPU, mehr IOs und Bluetooth der «grosse Bruder» des ESP8266. Mittlerweile kostet er fast so wenig wie der ESP8266 und wird inzwischen auch in vielen Maker-Projekten eingesetzt, selbst wenn diese Leistung gar nicht immer nötig wäre. In diesem Kurs bleiben wir vorerst beim ESP8266, aus zwei Gründen:

* Lange Zeit war MicroPython auf dem ESP32 noch etwas weniger ausgereift als auf dem ESP8266. Inzwischen ist die Unterstützung jedoch gut.
* Der ESP32 braucht mehr Strom als der ESP8266.

2020 kam der ESP32-S2 hinzu, mit nur einem Core und ohne Bluetooth, dafür mit USB. Er wird auch von CircuitPython unterstützt.
