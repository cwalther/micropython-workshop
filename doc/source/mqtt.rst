MQTT
====

Übersicht
---------

MQTT ist ein einfaches Publish/Subscribe-Protokoll, das häufig im Internet-of-Things-Bereich verwendet wird, um etwa Sensorwerte zu übertragen. Meist wird es über TCP/IP verwendet, funktioniert also übers Internet, aber es sind auch andere Datenverbindungen möglich. Der Name MQTT ist keine Abkürzung, er wird manchmal als «Message Queue Telemetry Transport» erklärt, aber die tatsächliche Geschichte dahinter ist komplizierter als das. Wer alle Details wissen will, liest am besten die Spezifikation, welche von http://mqtt.org verlinkt ist (neuste Version ist 5.0, am weitesten verbreitet und auch hier verwendet aber noch 3.1.1), hier behandeln wir nur die nötigsten Grundlagen.

Publish/Subscribe bedeutet, dass Clients Meldungen austauschen, indem ein Client seine Meldung an einen Server schickt, auch *Broker* genannt, und dieser sie dann an alle interessierten anderen Clients weiterleitet. Dadurch muss der Sender nicht wissen, welches die Empfänger sind, was den Client-Code einfach und die benötigte Datenmenge auf dem Netzwerk klein macht. Eine Meldung besteht aus einem Thema (*Topic*), z.B. «Temperatur im Wohnzimmer», und einem Inhalt, z.B. «20.8 °C». Der Client in der Messstation wird solche Meldungen publizieren (*publish*). Der Client in der Anzeigestation wird dem Broker mitteilen, dass er an Meldungen zu diesem Thema interessiert ist (abonnieren, *subscribe*), und wird sie dann vom Broker weitergeleitet erhalten, bis er sich wieder abmeldet. Beide haben dafür eine ständige Verbindung zum Broker offen, wobei nicht zwischen Sendern und Empfängern unterschieden wird, es können alle Clients beides über dieselbe Verbindung tun.

Topics sind in Textform und haben eine hierarchische Struktur, getrennt durch ``/``, z.B.

* ``wohnzimmer/temperatur``
* ``wohnzimmer/licht/leselampe``

Beim Abonnieren wird mittels eines *Topic Filters* angegeben, an welchen Topics man interessiert ist. Das kann entweder ein einzelnes Topic sein, z.B.

* ``wohnzimmer/temperatur``

oder mittels der Platzhalter ``#`` und ``+`` eine ganze Menge davon.

``#`` kann als einzelne Komponente am Schluss des Pfades stehen und passt auf null bis mehrere beliebige Pfadkomponenten, bedeutet also «dies und den gesamten darin enthaltenen Unterbaum».

* ``wohnzimmer/#`` heisst, gib mir Meldungen zum Wohnzimmer und allem, was darin passiert.
* ``#`` allein heisst, gib mir alle Meldungen überhaupt.

``+`` kann als einzelne Komponente irgendwo im Pfad stehen und passt auf genau eine beliebige Pfadkomponente.

* ``+/temperatur`` bedeutet, gib mir die Temperatur in allen Zimmern.

Der Inhalt einer Meldung ist eine beliebige Folge von Bytes, die vom Protokoll nicht weiter spezifiziert wird. Ihr eine Bedeutung zuzuweisen, ist Sache der Anwendung. Zahlenwerte werden häufig als ASCII-Text kodiert, das muss aber nicht so sein.

Meldungen können zwei verschiedene Charaktere haben: Sie können einen Zustand beschreiben, z.B. «Temperatur im Wohnzimmer», oder ein Ereignis, z.B. «Türklingel». Wenn ich mich als neuer Client beim Broker anmelde und interessiert bin an der Temperatur im Wohnzimmer, dann möchte ich sofort den zuletzt gemessenen Wert erhalten können und nicht warten müssen, bis der Sensor zum nächsten Mal einen Messwert publiziert und mir der Broker diesen weiterleitet. Dafür hat der Broker die Möglichkeit, die zuletzt publizierte Meldung zu einem gewissen Topic zu speichern, und wird sie dann jedem Client, der ab dann dieses Topic neu abonniert, sofort weiterleiten. Bei der Türklingel ist dies hingegen nicht notwendig: Wenn ich mich neu auf dieses Topic abonniere, dann interessiert mich nicht, ob in der Vergangenheit schon einmal an der Tür geklingelt wurde, sondern ich möchte einfach benachrichtigt werden, wenn das in Zukunft geschient. Um diese beiden Fälle zu unterscheiden, wird bei jedem Publizieren einer Meldung das Boolesche Flag *retain* mitgegeben. Wenn es gesetzt ist, wird der Broker diese Meldung weiterleiten und speichern, wenn nicht, wird er sie nur weiterleiten und wieder vergessen. Für Meldungen, die Zustände betreffen, wird man also *retain = True* angeben, für Meldungen über Ereignisse *retain = False*. Um eine auf dem Broker gespeicherte Meldung zu löschen, wird mit gesetztem *retain*-Flag eine leere Meldung auf das Topic publiziert.

Internetverbindungen sind nicht immer zuverlässig, besonders wenn es sich um Funkverbindungen zu entfernten Sensoren handelt. In manchen Anwendungen ist es nützlich, wenn Clients, die auf Meldungen eines gewissen Senders abonniert sind, benachrichtigt werden, wenn die Verbindung zu diesem Sender abbricht. Um das zu erreichen, hat jeder Client die Möglichkeit, beim Aufnehmen der Verbindung beim Broker eine Meldung zu hinterlegen, genannt «letzter Wille», welche im Falle seines unerwarteten Verschwindens an darauf abonnierte Clients geschickt werden soll. Der letzte Wille wird *nicht* versendet, wenn sich ein Client auf ordentliche Weise vom Broker abmeldet – wenn er sich trotzdem von seinen Empfängern verabschieden will, kann er das ja vor dem Abmelden durch eine gewöhnliche Nachricht tun.

``umqtt.simple`` API
--------------------

Ein MQTT-Client ist in MicroPython im Modul ``umqtt`` implementiert, welches beim ESP8266 schon im Lieferumfang der offiziellen Firmware enthalten ist. Es existieren zwei Varianten davon, ``umqtt.simple`` und ``umqtt.robust``. Ersteres ist die einfachstmögliche Implementation, letzteres hat zusätzlich die Fähigkeit, nach einem Verbindungsunterbruch automatisch neu zu verbinden. Da für unser Spiel die Verbindungszuverlässigkeit nicht so wichtig ist, verwenden wir der Einfachheit halber ``umqtt.simple``. Für eine abgelegene IoT-Station wäre eine robustere Implementation nötig, wobei das ein komplexes Thema ist und es nur mit der Verwendung von ``umqtt.robust`` nicht getan ist. [#]_

Das API von ``umqtt.simple`` ist summarisch dokumentiert auf https://github.com/micropython/micropython-lib/tree/master/umqtt.simple. Um im Detail zu sehen, was für Argumente die Methoden nehmen, müssen wir im Quellcode nachschauen: https://github.com/micropython/micropython-lib/blob/master/umqtt.simple/umqtt/simple.py. Ein paar Hinweise zum Lesen, da wir uns bisher nicht mit Klassen und objektorientierter Programmierung beschäftigt haben:

* Argumente, die in der Definition mit ``=`` einen Default-Wert zugewiesen bekommen, sind beim Aufruf optional.

* Das erste Argument ``self`` einer Methode kommt im Aufruf nicht vor, sondern nur in der Definition. Es übergibt die Instanz, auf der die Methode aufgerufen wurde (entsprechend ``this`` in einigen anderen Sprachen). Also eine Methode definiert als ::

     def connect(self, clean_session):

  wird aufgerufen als ::

     myclient.connect(True)

  und erhält dann die Argumente ``self = myclient`` und ``clean_session = True``.

* Um eine Instanz der Klasse zu erzeugen, wird die Klasse wie eine Funktion aufgerufen, worauf intern die Methode ``__init__`` ausgeführt wird. Also die Definition ::

     class MQTTClient:
         def __init__(self, client_id, server, port=0, user=None, password=None):

  sagt, dass ein Aufruf lauten kann ::
  
     myclient = MQTTClient('my-client-id', 'mqtt.example.com', 1883)

Zu beachten ist, dass gemäss dem Abschnitt *API design* der Dokumentation Topic-Namen und andere Strings nicht als Strings, sondern als Byte-Folgen ein- und ausgegeben werden, um Konversionsaufwand zu vermeiden, da übers Netzwerk schliesslich Bytes gehen. Byte-Folgen werden in Python durch den Typ ``bytes`` repräsentiert und verhalten sich ziemlich ähnlich wie Strings, nur dass ihre Elemente eben Bytes und nicht Zeichen sind. ``bytes``-Objekte werden erzeugt durch Anführungszeichen mit einem vorangestellten ``b``, wobei die zwischen den Anführungszeichen stehenden Zeichen ASCII-kodiert werden und durch ASCII nicht abgedeckte Byte-Werte durch Escape-Sequenzen wie ``\xC4`` angegeben werden können.

``bytes``-Objekte und Strings können wie üblich ineinander umgewandelt werden durch Aufruf der Typen ``bytes`` und ``str`` selber, wobei als zweites Argument der Name des Encodings angegeben werden muss. (In der Python-Dokumentation findet man als Alternative auch die Methoden :py:meth:`str.encode()` und :py:meth:`bytes.decode()`, sie sind jedoch in MicroPython nicht in allen Ports verfügbar.) ::

   >>> print(str(b'Hello W\xC3\xB6rld!', 'UTF-8'))
   Hello Wörld!
   >>> bytes('Hello W\u00F6rld!', 'UTF-8')
   b'Hello W\xc3\xb6rld!'

Häufig verwendete Encodings, und auch die einzigen, die von MicroPython unterstützt weden, sind ASCII, welches den Byte-Werten 0–127 Zeichen zuweist, und UTF-8, welches durch Sequenzen unterschiedlicher Länge alle Unicode-Zeichen abdeckt, wobei die Sequenzen der Länge 1 genau denen von ASCII entsprechen.

Ausprobieren
------------

*Übung:* Benützt die REPL, um euch mit dem MQTT-Server ``test.mosquitto.org`` zu verbinden und euch gegenseitig Meldungen zu senden!

*Tipp:* Um Meldungen zu empfangen, muss eine Callback-Funktion angegeben werden. Da in der Dokumentation nicht klar beschrieben ist, was für Argumente diese Funktion erhält, kann zum Ausprobieren einfach mal die eingebaute Funktion ``print`` übegeben werden – sie akzeptiert eine beliebige Zahl von Argumenten, und ihrem Output sieht man dann an, welche es waren.

.. [#] Eine gute detaillierte Abhandlung dazu gibt es bei Peter Hinch: https://github.com/peterhinch/micropython-samples/tree/master/resilient.
