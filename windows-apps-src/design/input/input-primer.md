---
Description: Benutzerinteraktionen in der Universellen Windows-Plattform (UWP) stellen eine Kombination von Eingabe- und Ausgabequellen dar (z. B. Maus, Tastatur, Stift, Toucheingabe, Touchpad, Spracherkennung, Cortana, Controller, Gesten, Mimik usw.), neben verschiedenen Modi oder Modifizierern, die erweiterte Funktionen ermöglichen (u. a. Mausrad und -tasten, Radierer- und Zeichenstift-Schaltflächen, Bildschirmtastatur sowie App-Dienste im Hintergrund).
title: Einführung in die Interaktion
ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8
label: Interaction primer
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b9fbe76244d37bda69a1737e04f7172a64b3af44
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684224"
---
# <a name="interaction-primer"></a>Einführung in die Interaktion

![Windows-Eingabetypen](images/input-interactions/icons-inputdevices03.png)

Benutzerinteraktionen in der Universellen Windows-Plattform (UWP) stellen eine Kombination von Eingabe- und Ausgabequellen dar (z. B. Maus, Tastatur, Stift, Toucheingabe, Touchpad, Spracherkennung, **Cortana**, Controller, Gesten, Mimik usw.), neben verschiedenen Modi oder Modifizierern, die erweiterte Funktionen ermöglichen (u. a. Mausrad und -tasten, Radierer- und Zeichenstift-Schaltflächen, Bildschirmtastatur sowie App-Dienste im Hintergrund).

Die UWP verwendet ein intelligentes, kontextbezogenes Interaktionssystem, mit dem in den meisten Fällen die von der App empfangenen eindeutigen Eingabetypen nicht individuell behandelt werden müssen. Dazu zählt das Behandeln von Toucheingabe, Touchpad, Maus und Stifteingabe als generischer Zeigertyp, um statische Gesten wie Tippen oder Gedrückthalten und Manipulationsgesten wie Ziehen zum Verschieben oder zum Rendern von Freihandeingabe zu unterstützen.

Machen Sie sich mit den verschiedenen Arten von Eingabegeräten sowie ihren Verhaltensweisen, Möglichkeiten und Einschränkungen in Verbindung mit bestimmten Formfaktoren vertraut. Dies erleichtert Ihnen die Entscheidung, ob die Steuerelemente und Angebote der Plattform für die App ausreichend sind oder ob Sie angepasste Funktionen für die Benutzerinteraktion bereitstellen müssen.

## <a name="gaze"></a>Anvisieren

Mit dem **Windows 10-Update April 2018** haben wir die Unterstützung für die Blickeingabe mit Eye- und Head-Tracking-Eingabegeräten eingeführt. 

> [!NOTE]
> Die Unterstützung für Eye Tracking-Hardware wurde mit dem **Windows 10 Fall Creators Update** zusammen mit der [Augensteuerung](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control) eingeführt, einem integrierten Feature, mit dem Sie Ihre Augen verwenden können, um den Bildschirmzeiger zu steuern, Text über die Bildschirmtastatur einzugeben und mit Personen über Text-zu-Sprache zu kommunizieren.

### <a name="device-support"></a>Geräteunterstützung

- Tablet
- PCs und Laptops

### <a name="typical-usage"></a>Typische Verwendung

Verfolgen Sie den Blick, die Aufmerksamkeit und die Präsenz eines Benutzers anhand der Position und Bewegung seiner Augen. Diese leistungsstarke neue Methode zur Verwendung und Interaktion mit Ihren UWP-Apps ist besonders hilfreich als unterstützende Technologie für Benutzer mit neuromuskulären Erkrankungen (wie ALS) und anderen Behinderungen mit beeinträchtigten Muskel- oder Nervenfunktionen. Die Blickeingabe bietet zudem überzeugende Möglichkeiten für Spiele (einschließlich Zielerfassung und -verfolgung) und traditionelle Produktivitätsanwendungen, Kioske und andere interaktive Szenarien, in denen herkömmliche Eingabegeräte (Tastatur, Maus, Touch) nicht verfügbar sind oder in denen es nützlich/hilfreich sein kann, wenn die Hände des Benutzers für andere Aufgaben (z. B. das Halten von Einkaufstaschen) frei bleiben.

### <a name="more-info"></a>Weitere Informationen

[Blick Interaktionen und Eye Tracking](gaze-interactions.md)

## <a name="surface-dial"></a>Surface Dial

Mit dem **Windows 10 Anniversary Update** haben wir eine neue Kategorie von Eingabegeräten eingeführt: Windows Wheel. Surface Dial ist das erste Angebot dieser Art.

### <a name="device-support"></a>Geräteunterstützung

- Tablet
- PCs und Laptops

### <a name="typical-usage"></a>Typische Verwendung

Der Formfaktor von Surface Dial entspricht einer Drehaktion (oder -geste). Surface Dial soll als sekundäres, multimodales Eingabegerät genutzt werden, das Eingaben über ein primäres Gerät ergänzt oder modifiziert. In den meisten Fällen wird das Gerät von einem Benutzer mit der nicht dominanten Hand bedient, während er mit seiner dominanten Hand eine Aufgabe ausführt (z. B. Freihandzeichnen mit einem Stift).

### <a name="more-info"></a>Weitere Informationen

[Entwurfs Richtlinien für Oberflächen Wähl](windows-wheel-interactions.md)

## <a name="cortana"></a>Cortana

In Windows 10 können Sie mit der **Cortana** -Erweiterbarkeit Sprachbefehle von einem Benutzer verarbeiten und Ihre Anwendung starten, um eine einzelne Aktion auszuführen.

### <a name="device-support"></a>Geräteunterstützung

-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox-Taste
-   HoloLens

![Cortana](images/input-interactions/icons-cortana01.png)

### <a name="typical-usage"></a>Typische Verwendung

Ein Sprachbefehl ist eine einzelne, in einer Sprachbefehldefinitions-Datei (VCD-Datei) definierte Äußerung, die über **Cortana** an eine installierte App weitergeleitet wird. Die App kann im Vordergrund oder im Hintergrund gestartet werden, je nach Ebene und Komplexität der Interaktion. Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern, werden beispielsweise am besten im Vordergrund ausgeführt, während grundlegende Befehle im Hintergrund behandelt werden können.

**Cortana** integriert die grundlegenden Funktionen Ihrer App, bietet einen zentralen Einstiegspunkt, über den der Benutzer die meisten Aufgaben ohne Öffnen der App ausführen kann, und wird somit zum Bindeglied zwischen Ihrer App und dem Benutzer. In vielen Fällen spart der Benutzer dadurch viel Zeit und Mühe. Weitere Informationen finden Sie unter [Cortana-Entwurfsrichtlinien](https://docs.microsoft.com/cortana/skills/cortana-design-guidelines).

### <a name="more-info"></a>Weitere Informationen

[Cortana-Entwurfsrichtlinien](https://docs.microsoft.com/cortana/skills/cortana-design-guidelines)
 

## <a name="speech"></a>Spracherkennung

Die Spracherkennung ist eine effektive und natürliche Möglichkeit, mit Apps zu kommunizieren. Sie bietet eine unkomplizierte und präzise Möglichkeit der Kommunikation mit Apps; damit können Benutzer in einer Vielzahl von Situationen produktiv arbeiten und auf dem Laufenden bleiben.

Die Spracherkennung kann den Haupteingabetyp je nach Gerät in vielen Fällen ergänzen oder sogar an dessen Stelle treten. Beispielsweise unterstützen Geräte wie HoloLens und Xbox keine herkömmlichen Eingabetypen (abgesehen von einer Softwaretastatur in bestimmten Szenarien). Stattdessen verwenden sie für die meisten Benutzerinteraktionen Spracheingabe und -ausgabe (häufig zusammen mit anderen modernen Eingabetypen wie Gestik und Mimik).

Mit Text-zu-Sprache (auch als TTS oder Sprachsynthese bezeichnet) werden Informationen oder Anweisungen an den Benutzer ausgegeben.

### <a name="device-support"></a>Geräteunterstützung

-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox-Taste
-   HoloLens

![Spracherkennung](images/input-interactions/icons-speech01.png)

### <a name="typical-usage"></a>Typische Verwendung

Es gibt drei Modi der Sprachinteraktion:

**Natürliche Sprache**

Mit natürlicher Sprache kommunizieren wir laufend mündlich mit anderen Personen. Unsere Sprache variiert in Abhängigkeit von der jeweiligen Person oder Situation, sie wird jedoch generell verstanden. Wenn dies nicht der Fall ist, verwenden wir häufig andere Wörter oder eine abweichende Wortreihenfolge, um die gleichen Gedanken zu vermitteln.

Die Kommunikation über natürliche Sprache mit einer App verläuft ähnlich: Wir sprechen über unser Gerät mit der App wie mit einer Person und erwarten, dass sie uns versteht und entsprechend reagiert.

Die natürliche Sprache ist der fortgeschrittenste Modus der Sprachinteraktion, der über **Cortana** implementiert und verfügbar gemacht wird.

**Befehl und Steuerung**

Unter Befehl und Steuerung wird die Verwendung von Sprachbefehlen zum Aktivieren von Steuerelementen und Funktionen verstanden, z. B. zum Klicken auf eine Schaltfläche oder zum Auswählen eines Menüelements.

Da Befehl und Steuerung entscheidend für eine erfolgreiche Benutzeroberfläche ist, wird ein einzelner Eingabetyp im Allgemeinen nicht empfohlen. Die Spracherkennung ist in der Regel eine von mehreren Eingabeoptionen für einen Benutzer, die von seinen Vorlieben und Hardwarefunktionen abhängen.

**Diktat**

Die einfachste Methode der Spracheingabe. Jede Äußerung wird in Text umgewandelt.

Das Diktieren wird in der Regel verwendet, wenn eine App die Bedeutung oder die Absicht nicht verstehen muss.

### <a name="more-info"></a>Weitere Informationen

[Richtlinien für den Sprachentwurf](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)
 

## <a name="pen"></a>Stift

Ein (Eingabe-)Stift kann ähnlich wie eine Maus als pixelgenaues Zeigegerät verwendet werden, und er eignet sich optimal für die Freihandeingabe.

**Beachten Sie**  zwei Arten von Stift Geräten vorhanden sind: aktiv und passiv.
  -   Passive Stifte enthalten keine Elektronik, und sie emulieren effektiv die Toucheingabe über einen Finger. Sie benötigen eine Basisgerätanzeige, welche die Eingabe basierend auf dem Berührungsdruck erkennt. Da Benutzer beim Schreiben auf der Eingabeoberfläche häufig die Hand ablegen, können Eingabedaten wegen des nicht erfolgreichen Ablehnens der Handfläche verzerrt werden.
  -   Aktive Stifte enthalten Elektronik und können mit komplexen Gerätedisplays zusammenwirken und viel umfassendere Eingabedaten (u. a. Daten bei Daraufzeigen oder Näherung) für das System und die App liefern. Die Handflächenablehnung ist sehr viel robuster.

In diesem Text beziehen wir uns auf aktive Stifte, die umfangreiche Eingabedaten liefern und in erster Linie für präzise Freihandeingaben und für zeigebasierte Interaktionen verwendet werden.

### <a name="device-support"></a>Geräteunterstützung

-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT

![Stift](images/input-interactions/icons-pen01.png)

### <a name="typical-usage"></a>Typische Verwendung

In Kombination mit einem Stift ermöglicht die Windows-Freihandplattform die natürliche Erstellung von handschriftlichen Notizen, Zeichnungen und Anmerkungen. Die Plattform unterstützt das Erfassen von Freihanddaten aus einem Eingabedigitalisierungsgerät, das Generieren von Freihanddaten, das Ausgeben von Daten und das Rendern von Freihandstrichen auf dem Ausgabegerät sowie das Verwalten von Daten und das Ausführen einer Schrifterkennung. Ihre App kann nicht nur die räumlichen Bewegungen des Stifts beim Schreiben oder Zeichnen erfassen. Sie kann auch Informationen zu Druck, Form, Farbe und Deckkraft sammeln und ermöglicht so eine Arbeitsweise, die der Verwendung eines Stifts, Bleistifts oder Pinsels auf Papier schon sehr nahe kommt.

Die Stift- und die Toucheingabe unterscheiden sich dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (wie Wischen, Ziehen, Drehen usw.) emuliert werden kann.

Sie müssen stiftspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

### <a name="more-info"></a>Weitere Informationen

[Designrichtlinien für die Zeichenstifteingabe](https://docs.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions)
 

## <a name="touch"></a>Toucheingabe

Bei der Toucheingabe können mit einem oder mehreren Fingern ausgeführte Gesten verwendet werden, um die direkte Manipulation von UI-Elementen (wie etwa Verschieben, Drehen, Ändern der Größe oder Bewegen) zu emulieren. Außerdem können die Gesten auch als alternative Eingabemethode (ähnlich einer Maus- oder Stifteingabe) oder als ergänzende Eingabemethode (zum Modifizieren anderer Eingaben; also beispielsweise zum Verwischen eines mit einem Stift gezeichneten Freihandstrichs) verwendet werden. Diese berührungsbasierte Bedienung wird von Benutzern unter Umständen als natürlicher wahrgenommen als die symbolbasierte Interaktion auf einem Bildschirm.

### <a name="device-support"></a>Geräteunterstützung

-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT

![Toucheingabe](images/input-interactions/icons-touch01.png)

### <a name="typical-usage"></a>Typische Verwendung

Die Unterstützung für die Toucheingabe kann je nach Gerät stark variieren.

Einige Geräte unterstützen keinerlei Toucheingabe, einige unterstützen einen einzelnen Touchkontakt, während wiederum andere Multitouchinteraktionen (zwei oder mehr Kontakte) unterstützen.

Die meisten Geräte mit Unterstützung von Multitouchinteraktionen erkennen zehn eindeutige gleichzeitige Kontakte.

Surface Hub-Geräte erkennen 100 eindeutige, gleichzeitige Berührungskontakte.

Im Allgemeinen weist die Toucheingabe folgende Merkmale auf:

-   Einzelner Benutzer, es sei denn, sie kommt mit einem Microsoft-Team-Gerät wie Surface-Hub zur Anwendung, bei dem der Schwerpunkt auf der Zusammenarbeit liegt.
-   Keine Beschränkung hinsichtlich der Geräteausrichtung.
-   Wird für alle Interaktionen, u. a. Texteingabe (Bildschirmtastatur) und Freihandeingabe (für die App konfiguriert) verwendet.

### <a name="more-info"></a>Weitere Informationen

[Richtlinien für die Toucheingabe](https://docs.microsoft.com/windows/uwp/input-and-devices/guidelines-for-user-interaction)
 

## <a name="touchpad"></a>Touchpad

Ein Touchpad vereint die indirekte Multitoucheingabe mit der Präzisionseingabe eines Zeigergeräts (beispielsweise eine Maus). Durch diese Kombination ist das Touchpad sowohl für eine toucheingabeoptimierte Benutzeroberfläche als auch für die kleineren Ziele von Produktivitäts-Apps geeignet.

### <a name="device-support"></a>Geräteunterstützung

-   PCs und Laptops
-   IoT

![Touchpad](images/input-interactions/icons-touchpad01.png)

### <a name="typical-usage"></a>Typische Verwendung

Touchpads unterstützen in der Regel eine Reihe von Touchgesten, die für die direkte Manipulation von Objekten und Benutzeroberflächenelementen eine ähnliche Unterstützung bieten wie die Toucheingabe.

Aufgrund dieser Konvergenz bei der von Touchpads unterstützten Interaktion empfiehlt es sich, sich nicht allein auf die Unterstützung der Toucheingabe zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für die Mauseingabe bereitzustellen. Stellen Sie Touchpad-spezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereit.

Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

### <a name="more-info"></a>Weitere Informationen

[Touchpad-Designrichtlinien](https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions)
 

## <a name="keyboard"></a>Tastatur

Eine Tastatur ist das Haupteingabegerät für Text und häufig unentbehrlich für Personen mit bestimmten körperlichen Beeinträchtigungen oder für Benutzer, die die Tastatur als schnellere und effizientere Interaktionsmethode betrachten.

Mit [Continuum for Phone](https://docs.microsoft.com/windows-hardware/design/device-experiences/continuum-phone?redirectedfrom=MSDN), einem neuen Benutzeroberflächen für kompatible Windows 10 Mobile-Geräte, können Benutzer ihre Telefone mit einer Maus und Tastatur verbinden, damit ihre Telefone wie ein Laptop funktionieren.

### <a name="device-support"></a>Geräteunterstützung

-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox-Taste
-   HoloLens

![Tastatur](images/input-interactions/icons-keyboard01.png)

### <a name="typical-usage"></a>Typische Verwendung

Benutzer können mit Universellen Windows-Apps über eine Hardwaretastatur und zwei Softwaretastaturen (Bildschirmtastatur oder Touchtastatur) interagieren.

Bei der Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur, die Sie anstelle der physischen Tastatur zum Eingeben von Daten per Touch, Maus, (Eingabe-)Stift oder anderen Zeigegeräten verwenden können. Ein Touchscreen ist nicht erforderlich. Die Bildschirmtastatur ist für Systeme ohne physische Tastatur oder für Benutzer vorgesehen, deren Mobilitätseinschränkungen die Verwendung herkömmlicher physischer Eingabegeräte verhindern. Die Bildschirmtastatur emuliert nahezu alle Funktionen der Hardwaretastatur.

Bei der Touch-Bildschirmtastatur handelt es sich um eine visuelle Softwaretastatur für die Texteingabe per Touchscreen. Die Touchtastatur ist kein Ersatz für die Bildschirmtastatur. Sie wird nur für die Texteingabe (ohne Emulierung der Hardwaretastatur) verwendet und nur angezeigt, wenn der Fokus auf einem Textfeld oder einem anderen bearbeitbaren Textsteuerelement liegt. Die Bildschirmtastatur unterstützt keine App- oder Systembefehle.

**Beachten Sie** ,  der OSK Vorrang vor der Touchscreen-Tastatur hat, die nicht angezeigt wird, wenn der OSK vorhanden ist.

Im Allgemeinen weist eine Tastatur folgende Merkmale auf:

-   Einzelner Benutzer.
-   Keine Beschränkung hinsichtlich der Geräteausrichtung.
-   Wird für Texteingabe, Navigation, Spiele und Eingabehilfen verwendet.
-   Immer verfügbar, entweder proaktiv oder reaktiv.

### <a name="more-info"></a>Weitere Informationen

[Tastaturentwurfsrichtlinien](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
 

## <a name="mouse"></a>Maus

Eine Maus eignet sich am besten für Produktivitäts-Apps und Benutzeroberflächen mit hoher Dichte, bei denen Benutzerinteraktionen beim Zielen und bei der Befehlseingabe Pixelgenauigkeit erfordern.

### <a name="device-support"></a>Geräteunterstützung

-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT

![Maus](images/input-interactions/icons-mouse01.png)

### <a name="typical-usage"></a>Typische Verwendung

Die Mauseingabe kann über verschiedene Tasten der Tastatur (STRG, UMSCHALTTASTE, ALT usw.) geändert werden. Diese Tasten können mit der linken Maustaste, der rechten Maustaste, der Radtaste und den X-Tasten zu einem erweiterten, mausoptimierten Befehlssatz kombiniert werden. (Einige Microsoft-Mausgeräte verfügen über zwei weitere Schaltflächen, die als X-Tasten bezeichnet werden. Diese dienen gewöhnlich dazu, in Webbrowsern zurück und vorwärts zu navigieren.

Ähnlich wie bei der Stifteingabe unterscheiden sich Maus- und Toucheingabe dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (z. B. Wischen, Ziehen, Drehen usw.) emuliert werden kann.

Sie müssen mausspezifische Benutzeroberflächenbefehle (oder Angebote) zur Unterstützung dieser Interaktionen bereitstellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.

### <a name="more-info"></a>Weitere Informationen

[Richtlinien für die Mausinteraktion](https://docs.microsoft.com/windows/uwp/input-and-devices/mouse-interactions)
 

## <a name="gesture"></a>Geste

Eine Geste ist jede Art von Benutzerbewegung, die als Steuerungs- oder Interaktionseingabe für eine Anwendung erkannt wird. Es gibt verschiedene Arten von Gesten – von der einfachen Geste, die dazu dient, mit der Hand etwas auf dem Bildschirm zu verwenden, über spezifische, erlernte Bewegungsmuster bis hin zu langen Bewegungsabläufen des gesamten Körpers. Beachten Sie beim Entwerfen benutzerdefinierter Gesten, dass diese in anderen Regionen/Kulturkreisen unter Umständen eine andere Bedeutung haben.

### <a name="device-support"></a>Geräteunterstützung

-   PCs und Laptops
-   IoT
-   Xbox-Taste
-   HoloLens

![Geste](images/input-interactions/icons-gesture01.png)

### <a name="typical-usage"></a>Typische Verwendung

Statische Gestenereignisse werden ausgelöst, wenn eine Interaktion abgeschlossen ist.

- Zu den statischen Gestenereignissen zählen Tapped, DoubleTapped, RightTapped und Holding.

Manipulationsgestenereignisse weisen auf eine andauernde Interaktion hin. Sie werden ausgelöst, wenn der Benutzer ein Element berührt, und bleiben so lange aktiv, bis der Benutzer den bzw. die Finger vom Element hebt oder die Manipulation abgebrochen wird.

- Manipulationsereignisse umfassen Multi-Touch-Interaktionen wie Zoomen, Schwenken und Drehen sowie Interaktionen, die Trägheits- und Geschwindigkeitsdaten nutzen (z. B. Ziehen). (Die von den Manipulationsereignissen bereitgestellten Informationen identifizieren nicht die Interaktion, sondern stellen Daten wie Position, Übersetzungsdelta und Geschwindigkeit bereit.)

- Zeigerereignisse wie PointerPressed und PointerMoved bieten Details auf unterer Ebene für alle Touchkontakte einschließlich Zeigerbewegungen und die Möglichkeit, zwischen Drück- und Freigabeereignissen zu unterscheiden.

Aufgrund der Konvergenz von durch Windows unterstützten Interaktionsumgebungen wird empfohlen, sich nicht nur auf die Unterstützung für Toucheingaben zu verlassen, sondern auch Benutzeroberflächenbefehle oder Angebote für Mauseingaben bereitzustellen. Verwenden Sie beispielsweise Schaltflächen für „Zurück“ und „Weiter“ (oder „+“ und „-“), um dem Benutzer das Blättern durch Seiten bzw. das Drehen, Vergrößern/Verkleinern und Zoomen von Objekten zu ermöglichen.


## <a name="gamepadcontroller"></a>Gamepad/Controller

Ein Gamepad/Controller ist ein sehr spezielles Gerät und kommt in der Regel bei Spielen zum Einsatz. Es wird jedoch auch zum Emulieren einfacher Tastatureingaben sowie für eine tastaturähnliche UI-Navigation verwendet.

### <a name="device-support"></a>Geräteunterstützung

-   PCs und Laptops
-   IoT
-   Xbox-Taste

![Controller](images/input-interactions/icons-controller01.png)

### <a name="typical-usage"></a>Typische Verwendung

Spielen und Interaktionen mit einer speziellen Konsole.


## <a name="multiple-inputs"></a>Mehrfacheingaben

Durch die Berücksichtigung so vieler Benutzer und Geräte wie möglich und den Entwurf Ihrer App für die Kompatibilität mit möglichst vielen Eingabearten (Gesten, Spracherkennung, Toucheingabe, Touchpad, Maus und Tastatur) maximieren Sie die Flexibilität, Benutzerfreundlichkeit und Barrierefreiheit Ihrer Apps.

### <a name="device-support"></a>Geräteunterstützung

-   Smartphones und Phablets
-   Tablet
-   PCs und Laptops
-   Surface Hub
-   IoT
-   Xbox-Taste
-   HoloLens

![Mehrfacheingaben](images/input-interactions/icons-inputdevices03-vertical.png)

### <a name="typical-usage"></a>Typische Verwendung

Ebenso wie Menschen untereinander mit einer Mischung aus Sprache und Gesten kommunizieren, kann sich auch bei der Interaktion mit einer App die Verwendung mehrerer Eingabearten und -modi als hilfreich erweisen. Diese kombinierten Interaktionen müssen jedoch möglichst intuitiv und natürlich gestaltet sein, da sie andernfalls zu Verwirrung führen können.





 

 
