---
Description: 'In diesem Abschnitt erfahren Sie, wie Sie Zeichenfolgen-, Bild- und Dateiressourcen Ihrer App erstellen, verpacken und verwenden.'
title: App-Ressourcen und das Ressourcenverwaltungssystem
label: Intro
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: "Windows\_10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer"
ms.localizationpriority: medium
---
# <a name="app-resources-and-the-resource-management-system"></a>App-Ressourcen und das Ressourcenverwaltungssystem


In diesem Abschnitt erfahren Sie, wie Sie Zeichenfolgen-, Bild- und Dateiressourcen Ihrer App erstellen, verpacken und verwenden. Zum Beispiel könnten Sie eine Datei zusammen mit Ihrem Lieblingsspiel verpacken, die eine Definition der Spielebenen enthält, und die Datei zur Laufzeit laden. Außerdem zeigen wir Ihnen, wie Sie Ihre Ressourcen unabhängig von der Logik der App verwalten können. Dadurch wird es einfacher, die App für verschiedene Gebietsschemas, Geräteanzeigen, Barrierefreiheiteinstellungen und andere Benutzer- und Computersituationen zu lokalisieren und anzupassen. Ressourcen wie Zeichenfolgen und Bilder müssen in der Regel in mehreren Sprachen, Skalierungen und Kontrastvarianten vorhanden sein. Bei der Handhabung solcher Ressourcen unterstützt Sie das [Ressourcenverwaltungssystem](resource-management-system.md).

Es gibt zwei Arten von App-Ressourcen.
- Eine Dateiressource ist eine Ressource, die als Datei auf einem Datenträger gespeichert ist. Eine Dateiressource kann Bitmap-Bilder sowie XAML-, XML-, HTML-Dateien oder beliebige andere Daten enthalten.
- Eine eingebettete Ressource ist eine Ressource, die in eine Ressourcendatei mit Inhalt eingebettet ist. Das häufigste Beispiel ist eine Zeichenfolgenressource, die in eine Ressourcendatei (.resw oder .resjson) eingebettet ist.

Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md).

| Artikel | Beschreibung |
|---------|-------------|
| [Ressourcenverwaltungssystem](resource-management-system.md) | Zum Buildzeitpunkt erstellt das Ressourcenverwaltungssystem einen Index aller verschiedenen Varianten der Ressourcen, die mit Ihrer App gepackt sind. Zur Laufzeit erkennt das System die momentan geltenden Benutzer- und Computereinstellungen und lädt die Ressourcen, die für diese Einstellungen am besten geeignet sind. |
| [Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt](how-rms-matches-and-chooses-resources.md) | Wenn eine Ressource angefordert wird, kann es mehrere Kandidaten geben, für die sich in einem gewissen Maße eine Übereinstimmung mit dem aktuellen Ressourcenkontext ergibt. Das Ressourcenverwaltungssystem analysiert alle Kandidaten und ermittelt den besten Kandidaten für die Rückgabe. In diesem Thema wird dieser Prozess ausführlich und anhand von Beispielen beschrieben. |
| [Wie das Ressourcenverwaltungssystem Sprachtags zuordnet](how-rms-matches-lang-tags.md) | Im vorherigen Thema ([Wie das Ressourcenverwaltungssystem Ressourcen zuordnet und auswählt](how-rms-matches-and-chooses-resources.md)) wird die Zuordnung von Qualifizierern im Allgemeinen behandelt. Dieses Thema konzentriert sich ausführlicher auf den Vergleich von Sprachtags. |
| [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md) | In diesem Thema wird das allgemeine Konzept der Ressourcenqualifizierer erläutert, wie sie verwendet werden und wozu die einzelnen Qualifizierernamen dienen. |
| [Lokalisieren von Zeichenfolgen auf der Benutzeroberfläche und im App-Paketmanifest](localize-strings-ui-manifest.md) | Wenn Sie möchten, dass Ihre App verschiedene Anzeigesprachen unterstützt und Ihr Code oder XAML-Markup- oder App-Paketmanifest Zeichenfolgenliterale enthält, verschieben Sie diese Zeichenfolgen in eine Ressourcendatei (.resw). Sie können dann eine übersetzte Kopie dieser Ressourcendatei für jede Sprache erstellen, die Ihre App unterstützt. |
| [Laden von Bildern und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast usw.](images-tailored-for-scale-theme-contrast.md) | Ihre App kann Bildressourcendateien mit Bildern laden, die speziell auf den Skalierungsfaktor für die Anzeige, das Design, den hohen Kontrast und andere Laufzeitkontexte angepasst wurden. |
| [URI-Schemas](uri-schemes.md) | Es gibt mehrere URI (Uniform Resource Identifier)-Schemen, die Sie verwenden können, um auf Dateien aus Ihrem App Paket, dem App-Ordner oder der Cloud zu verweisen. Sie können auch ein URI-Schema verwenden, um auf Zeichenfolgen zu verweisen, die von den App-Ressourcendateien (.resw) geladen wurden. |
| [Angeben der von der App verwendeten Standardressourcen](specify-default-resources-installed.md) | Wenn Ihre App nicht über Ressourcen verfügt, die den speziellen Einstellungen eines Kundengeräts entsprechen, werden die Standardressourcen der App verwendet. In diesem Thema wird erläutert, wie Sie diese Standardressourcen festlegen. |
| [Integrieren von Ressourcen im App-Paket und nicht in einem Ressourcenpaket](build-resources-into-app-package.md) | Einige Arten von Apps (mehrsprachige Wörterbücher, Übersetzungstools usw.) müssen das Standardverhalten von einem App-Paket überschreiben und Ressourcen im App-Paket und nicht in separaten Ressourcenpaketen integrieren. In diesem Thema wird erläutert, wie das geht. |
| [APIs für die Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme](pri-apis-custom-build-systems.md) | Mit den [APIs für die Paketressourcenindizierung (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690) können Sie ein benutzerdefiniertes Buildsystem für die Ressourcen Ihrer UWP-App entwickeln. Das Buildsystem kann Indexdateien der Paketressource (PRI) erstellen, versionieren und per Dump sichern (als XML), und zwar für jedes Maß an Komplexität, das Ihre UWP-App benötigt. |
| [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md) | „MakePri.exe“ ist ein Befehlszeilentool, mit dem Sie PRI-Dateien erstellen und sichern können. Es ist über MSBuild in Microsoft Visual Studio integriert, kann aber auch für Entwickler von Nutzen sein, die Pakete manuell oder mithilfe benutzerdefinierter Buildsysteme erstellen. |
| [Verwenden des Ressourcenverwaltungssystems für Windows 10 in älteren Apps oder Spielen](using-mrt-for-converted-desktop-apps-and-games.md) | Indem Sie Ihre .NET- oder Win32-App oder Ihr Spiel als AppX-Paket verpacken, können Sie das Ressourcenverwaltungssystem nutzen, um App-Ressourcen zu laden, die auf den Laufzeitkontext zugeschnitten sind. In diesem Thema werden die Techniken detailliert beschrieben. |

Siehe auch [Unterstützte Kachel- und Popupbenachrichtigungen für Sprache, Skalierungsfaktor und hohen Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).
