---
layout: post
title: "E-Mails verschlüsseln mit S/MIME"
description: ""
category: Security
tags: [Encryption, S/MIME]
---

Unverschlüsselte E-Mails sind wie Postkarten. Jeder könnte sie lesen. Auch wenn keine wichtigen Informationen in ihnen stehen, so sollte man seine Privatsphäre nicht einfach ohne Weiteres aufgeben. Viel zu viel steht auf dem Spiel, denn jeder hat schon mal den paraphrasiert Satz von Benjamin Franklin gehört: „Wer die Freiheit aufgibt um Sicherheit zu gewinnen, der wird am Ende beides verlieren.“ Dabei ist es garnicht schwer oder viel Aufwand E-Mails zu verschlüsseln, wie die nächsten sechs Schritte zeigen.

<!--more-->

1.  Zuerst bei [Comodo](http://comodo.com) ein kostenloses Zertifikat (Klasse 1) für die zu verschlüsselnde E-Mailadresse bestellen. 

2.  Den Link in der E-Mail von Comodo öffnen. Dadurch wird das Zertifikat im jeweiligen Standardbrowser installieren.

3.  Als nächstes das Zertifikat exportieren und an einer sicheren Stelle aufbewahren, da ohne dieses Zertifikat die damit verschlüsselten E-Mails nicht mehr gelesen werden können. Beim Exportieren sollte ein sehr starkes Passwort verwendet werden, um auszuschließen, dass jemand Anderes dieses verwenden kann sollte es doch einmal verloren gehen:
    
	### Firefox
	
	* `Extras` -> `Einstellungen` -> `Erweitert` -> `Zertifikate anzeigen`
	* Eigenes Zertifikat unter `Ihre Zertifikate` auswählen und als PKCS12 Datei (*.p12) `Sichern...`. Dabei ein starkes Passwort verwenden, da dieses der [_Private Schlüssel_](#PKV) ist. 
	* Zusätzlich unter `Zertifizierungsstellen` das _COMODO Client Athentication and Secure Email CA Zertifikat_ unter _The USERTRUST Network_ `Exportieren...` und als X.509-Zertifikat (.crt) speichern. 

	###  Internet Explorer
	
	* `Internetoptionen` -> `Inhalte` -> `Zertifikate`
	* Unter `Eigene Zertifikate` das eigenes Zertifikat `Exportieren` -> `Weiter` -> _Ja, privaten Schlüssel exportieren_ auswählen und `Weiter` -> _Privater Informationsaustausch -PKCS #12 (.PFX)_ auswählen, _Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen_ aktivieren und `Weiter` -> Starkes Kennwort festlegen, da der [_Private Schlüssel_](#PKV) mit exportiert wird und `Weiter` -> Dateiname wählen und `Weiter` -> `Fertig stellen`
	* Zusätzlich unter `Zwischenzertifizierungsstellen` das _COMODO Client Athentication and Secure Email CA Zertifikat_ `Exportieren...` und als X.509-Zertifikat (.crt) speichern.

	Neben der Sicherung des Zertifikates wird das exportierte Zertifikat benötigt, um auf anderen Geräten das selbe Zertifikat verwenden zu können. Das Comodo Zertifikat wird nur benötigt wenn die Comodo Zertifizierungsauthorität noch nicht auf einem Gerät vorhanden ist, wie z.B. dem iPhone.
	
4.  Exportierte Zertifikate an eigene E-Mailadresse senden.

5.  E-Mail auf dem Smartphone/Tablet öffnen und Zertifikat durch anklicken installieren. Das eigene Zertifikat erscheint als vertrauenswürdig sobald Comodo-Zertifikat installiert ist. [Weitere Informationen](#CA) 

6.  In den iOS `Einstellungen` des E-Mailkontos unter `Erweiterte Einstellungen`, ganz unten, `S/MIME` aktivieren (signieren und verschlüsseln)

Das wars. Wenn man jetzt eine Email versendet, dann wird sie signiert. Der Empfänger sieht an einem Icon (Outlook/iOS), dass die E-Mail nicht manipuliert wurde, vorausgesetzt ein Angreifer ist nicht an das private Zertifikat gelangt.

Besitzt ihr das öffentliche Zertifikat einer E-Mailadresse, so wird die Nachricht der E-Mail automatisch verschlüsselt und nur der Empfänger kann sie entschlüsseln , da er ja den privaten Schlüssel besitzt.

Dar öffentliche Zertifikat kann immer an die E-Mail gehangen werden. Die Vertrauenswürdigkeit wird ja von Comodo bestätigt und so kann jeder mit eurem öffentlichen Zertifikat verschlüsselte Nachrichten an euch senden.
Ein Zertifikat E-Mail kann man bei iOS durch klick auf den Namen mit dem Signaturicon vertrauen. Bei Outlook ist darauf achten, dass der Kontakt auch als Kontakt gespeichert ist. Andernfalls wird das Zertifikat desjenigen nicht gespeichert und man kann keine Nachrichten an denjenigen verschlüsselt versenden. 

## Weitere Infos

### Warum sind Datenschutz und Privatsphäre wichtig?
Ein wirklich ansenlich gemachtest Video zeigt, [warum jeder lieber etwas zu verbergen hat](http://youtu.be/iHlzsURb0WI).
[![Überwachung](http://img.youtube.com/vi/iHlzsURb0WI/0.jpg)](http://youtu.be/iHlzsURb0WI)

### S/MIME
S/MIME arbeitet nach dem Public-Key-Verfahren. Es wird damit jedoch nur der Inhalt der E-Mail, jedoch nicht der Header und die Metadaten, verschlüsselt. Da im Header die Betreffzeile steht, sollten dort keine wichtigen Daten stehen. Natürlich hilft alles nichts wenn z.B. durch einen Trojaner oder Keylogger die Eingaben angefangen werden.

### <a name="PKV"></a>Public-Key Verfahren
Beim Public-Key-Verfahren werden zwei Schlüssel verwendet. Ein privater Schlüssel und ein öffentliche Schlüssel bilden ein sogenanntes Schlüsselpaar. Der öffentliche Schlüssel kann ohne Bedenken an jeden verteilt werden. Er wird dazuverwendet um Nachrichten an den Besitzer des zugehörigen privaten Schlüssels zu verschlüsseln. Nur der Besitzer des privaten Schlüssels kann die verschlüsselte Nachricht wieder entschlüsseln. Damit man auch den richtigen öffentlichen Schlüssel von jemanden nutzt, und nicht den eines Angreifers, vertraut man nur Schlüsseln, die durch Andersrum kann mit dem privaten Schlüssel eine Nachricht signiert werden. Mit dem öffentlichen Schlüssel kann nun überprüft werden ob die Nachricht wirklich vom Absender stammt und nicht manipuliert wurde.

### <a name="CA"></a>Comodo, Certification Authority (CA) und das Web of trust
Comodo dient sozusagen als Bürge und bestätigt mit dem Klasse 1 Zertifikat, dass es diese E-Mailadresse gibt, denn Comodo hat die E-Mail für das herunterladen des eigenen Zertigikates an diese E-Mailadresse gesendet. Das Zertifikat welches man bekommt ist mit dem Privaten Schlüssel, den nur Comodo besitzt, signiert. Als CA ist das Comodo Zertifikat in der Regel schon auf dem Computer installiert und es wird diesem vertraut. Andernfalls muss dieses Zertifikat installiert und ihm vertraut werden. Zertifikate, die von Comodo signiert wurden, wird  darauf automatisch vertraut. Prüft man einen manuell den Fingerprint eines Zertifikates, so kann man einem Zertifikat auch manuell vertrauen und ggf. einstellen, dass Zertifikate, die mit diesem Zertifikat signiert wurden ebenfalls vertraut werden kann. So kann man ein Netzwerk des Vertrauens aufbauen.

### Schlüsselrückruf
Sollte einmal ein Schlüssel verloren gehen, so kann man ihn unter Angabe des Rückrufpasswortes [zurückrufen](https://secure.comodo.com/products/!SecureEmailCertificate_Revoke). Diesem Zertifikat gilt dann nicht mehr als vertrauenswürdig.

## Haftungsausschluss
Diese Dokumentation ist nach bestem Wissen und Gewissen erstellt worden, stellt die Verschlüssellung jedoch vereinfacht dar, um möglichst jedem zuermöglichen eine E-Mail Verschlüsselung einzurichten. 100% Sicherheit gibt es nicht. Insbesondere durch Fehlverhalten wie der Verlust des Privatenschlüssels etc. kann dieser Schutz ausgehebelt werde. Die Anwendung der Anleitung geschieht auf, eigene Gefahr. Der Autor kann nicht auf Grund von Fehlern in der Anleitung für etwaige Schäden haftbar gemacht werden.
