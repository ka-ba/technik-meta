.. _workflows:

Workflows
=========

.. _fastd_keys:

fastd-keys
----------

Damit die Nodes eine VPN-Verbindung aufbauen können, muss der *öffentliche* **fastd** *Schlüssel* auf die Gateways gelangen.

:see:
    - :ref:`fastd`

Wir verwalten diese Keys in Git-Repositories:

* Mainz: peers-ffmz.git_
* Wiesbaden: peers-ffwi.git_

.. _peers-ffmz.git: https://github.com/freifunk-mwu/peers-ffmz
.. _peers-ffwi.git: https://github.com/freifunk-mwu/peers-ffwi

Bei der Installation der Gluon Node wird ein **fastd** *Schlüsselpaar* erzeugt.

Im Beschreibungstext steht, man solle den erzeugten *öffentlichen Schlüssel* per Mail an unsere Listen schicken. Alternativ lässt sich auch ein Link klicken, der das Email-Programm öffnet und eine neue Mail ausfüllt.

* Mainz: ``keys (at) freifunk-mainz.de``
* Wiesbaden: ``keys (at) freifunk-wiesbaden.de``

Die Empfänger dieser Liste sind am Besten auch Mitglieder des `fastd-keys GitHub Teams`_ - Mitglieder dieser Gruppe haben Schreibrechte auf die *peers* Repositories.

.. _fastd-keys GitHub Teams: https://github.com/orgs/freifunk-mwu/teams/fastd-keys

Die neuen Keys von der Liste werden wie unten gezeigt lokal eingetragen, die Änderungen wie gewohnt gepusht.

.. note:: Damit der Nodebesitzer und die anderen Empfänger der Keys-Listen bescheid wissen muss nach dem Eintragen der Keys die Mail beantwortet werden. Den *Nodebesitzer* ins **To:**-Feld, ``keys@freifunk-...`` ins **CC:**.

Mehrmals täglich kommt FFctl_ vorbei und synchronisiert die neuen Keys von GitHub auf die entsprechenden Gateways.

.. _FFctl: http://ffctl.readthedocs.org/

.. _fastd_key_format:

fastd-keys Format
-----------------

Ein **fastd**-key ist ein 64-Zeichen langer HEX-String::

    0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef

Damit der **fastd**-daemon den Schlüssel lesen kann, muss noch etwas Syntax drum rum::

    key "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef";

Bei Start oder Neuladen des **fastd**-daemons wird das Config-File neu eingelesen.
In diesem steht, **fastd** möge zusätzlich noch alle Dateien aus dem ``peers``-Ordner mit einlesen (== *peers-... .git* Repository).

Es ist unerheblich wie viele Nodes pro Datei im ``peers``-Ordner hinterlegt sind.

Wir gruppieren die Nodes nach Nutzern (weitere Gruppen sind ``backbone``, ``event`` und ``whatever``).

.. TODO: Sinnvolle Node-Gruppen ausdenken

Kommentare beginnen mit dem **#**-Zeichen.

1. (Nick-)Name des Freifunkers
2. Kontaktmöglichkeit (Achtung: Spamschutz, Datenschutz)
3. kurze Beschreibung zum Freifunker
4. Leerzeile
5. Danach pro Node:
    * ``key " ... "; # Node-Name``
    * (Optional: Weitere Kommentarzeilen über die Node)
    * Leerzeile

.. TODO: Sinnvolle Kontaktmöglichkeit ausdenken

.. _fastd_key_beispiel:

Beispiel - Roy Kabel
^^^^^^^^^^^^^^^^^^^^

Der bekannte, ehemalige Hobbyfunker *Roy Kabel* - jetzt Freifunker hat zum Beispiel drei Nodes::

    # Roy Kabel
    # roy (at) kabel.com
    # bekannter, ehemaliger Hobbyfunker

    key "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef"; # MyNode
    # TL-WR1337xD

    key "abcdef0123456789abcdef0123456789abcdef0123456789abcdef0123456789"; # TheNode
    # TL-WDR2300

    key "abcdef01234567890123456789abcdefabcdef01234567890123456789abcdef"; # Mobile Node
    # TL-MR2342


.. _exitvpn_accounts:

exitVPN Accounts
----------------

Anfragen aus dem Freifunknetz in das weltweite Internet tunneln wir durch das sog. **exitVPN**, um die Störerhaftung zu umgehen.

Dies hat den Vorteil, dass Anfragen in das Internet anonymisiert werden, Anbieter sehen nur dass die Anfrage aus dem Freifunk-Netz kommt.

:see:
    - :ref:`openvpn`

Hierbei handelt es sich um **OpenVPN** Angebote, meist in Schweden oder Niederlande.

Diese werden im Vorraus gezahlt, und müssen von Hand aufgeladen werden.

**Problem** - Dies wird all zu gerne verpeilt!

Im `gateway-configs.git`_ findet sich eine ``exitVPN.md``

.. _gateway-configs.git: https://github.com/freifunk-mwu/gateway-configs/

Dort wird pro Gateway hinterlegt, welcher VPN-Account hinterlegt ist, und bis zu welchem Datum dieser bezahlt ist.

Einmal tägtlich kommt ein Script vorbei, und schreibt bei nähern des Datums Mails auf die ``admin@``-Listen.

1. ## Gateway-Name
2. ### Account-Nr/Login - VPN-Anbieter (mullvad/ipredator)
3. * Datum bis zu dem gezahlt wurde: DD.MM.YYYY
4. Leerzeile
5. Leerzeile

.. note:: Dieses Script sowie die ``exitVPN.md`` ist noch in Arbeit. Bitte etwas geduld.

Beispiel
^^^^^^^^

In etwa so::

    ## Hartwurstsuppe
    ### abcdef0123 - ipredator
    * 23.05.2023


    ## Popcorn
    ### 0123456789 - mullvad
    * 23.05.2042


    # ...

.. TODO Script schreiben.