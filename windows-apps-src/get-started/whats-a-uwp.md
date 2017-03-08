---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: Was ist eine App der universellen Windows-Plattform (UWP)?
description: "Hier werden die verschiedenen App-Typen vorgestellt, die wir als universelle Windows-Apps bezeichnen – Windows Store-Apps, Windows Phone Store-Apps und Windows-Runtime-Apps."
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, UWP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0507086a7c7ea67f43f61feef6dc5c8de5afe2e5
ms.lasthandoff: 02/07/2017

---

# <a name="whats-a-universal-windows-platform-uwp-app"></a>Was ist eine universellen Windows-Plattform (UWP)-App?

Wenn Sie noch nicht mit der Windows-Plattform vertraut sind oder aus dem Bereich .NET, Windows Forms oder Silverlight kommen, möchten Sie jetzt vielleicht wissen, was eine UWP-App tatsächlich *ist*. 

Wie in einem bekannten Buch einmal gesagt wurde: „Nur keine Panik“, denn bald wird alles geklärt. 

Eine universellen Windows-Plattform (UWP)-App ist eine Windows-Benutzeroberfläche auf der Basis der universellen Windows-Plattform (UWP), die erstmals unter Windows 8 als Windows-Runtime eingeführt wurde. Die Idee der UWP-Apps besteht im Prinzip darin, dass Benutzer ihre *Erfahrungen* auf ALLEN Geräten nutzen und jeweils das Gerät verwenden möchten, das für die entsprechende Aufgabe am einfachsten oder produktivsten einzusetzen ist.

Mit Windows 10 ist die Entwicklung von Apps für die UWP einfacher. Sie benötigen nur einen API-Satz, ein App-Paket und einen Speicher für alle Windows 10-Geräte – PC, Tablet, Smartphone, Xbox, HoloLens, Surface Hub und mehr. Es ist einfacher, eine Reihe von Bildschirmgrößen und eine Vielzahl von Interaktionsmodellen zu unterstützen, sei es Touch, Maus und Tastatur, ein Gamecontroller oder ein Stift. Wichtig an dieser Stelle: Sie brauchen C# und XAML nicht zu verwenden, wenn Sie das nicht möchten. Entwickeln Sie gerne in Unity oder in MonoGame? Bevorzugen Sie JavaScript? Kein Problem, Sie können beliebige Programmiersprachen verwenden.

Fazit: Sie können mit vertrauten Programmiersprachen, Frameworks und APIs in einem einzigen Projekt arbeiten und denselben Code auf dem großen Spektrum der heute vorhandenen Windows-Hardware ausführen lassen. Nachdem Sie Ihre UWP-App entwickelt haben, können Sie sie im Store veröffentlichen und damit der ganzen Welt präsentieren.

![Windows-Geräte](images/1894834-hig-device-primer-01-500.png)

##<a name="so-what-exactly-is-a-uwp-app"></a>Was also ist eine UWP-App *genau*?


Was ist das Besondere an einer UWP-App? Hier sind einige der Merkmale, die UWP-Apps unter Windows 10 von anderen Apps abheben.

-   Sie entwickeln für Gerätefamilien, nicht für ein Betriebssystem.

    Eine Gerätefamilie identifiziert die APIs, Systemmerkmale und Verhaltensweisen, die Sie auf allen Geräten innerhalb der Gerätefamilie erwarten können. Es bestimmt außerdem die Gerätegruppe, auf der Ihre App aus dem Store installiert werden kann.

-   Apps werden mit dem AppX-Paketformat gepackt und verteilt.

    Alle UWP-Apps werden als AppX-Paket verteilt. Dies stellt einen vertrauenswürdigen Installationsmechanismus bereit und stellt sicher, dass Ihre Apps problemlos bereitgestellt und aktualisiert werden können.

-   Es gibt einen Store für alle Geräte.

    Nach der Registrierung als App-Entwickler können Sie Ihre App an den Store übermitteln und auf allen Gerätefamilien oder nur für eine Auswahl zur Verfügung stellen. Sie übermitteln und verwalten alle Ihre Apps für Windows-Geräte an einem zentralen Ort.

-   Es gibt eine allgemeine API-Oberfläche für Gerätefamilien.

    Die zentralen UWP-APIs (Universelle Windows-Plattform) sind für alle Windows-Gerätefamilien identisch. Wenn Ihre App nur die wichtigsten APIs verwendet, kann sie auf allen Windows 10-Geräten ausgeführt werden.

-   Erweiterungs-SDKs können Ihre App auf speziellen Geräten hervorheben.

    Erweiterungs-SDKs fügen spezielle APIs für jede Gerätefamilie hinzu. Wenn sich Ihre App an eine bestimmte Gerätefamilie richtet, können Sie sie mit diesen APIs hervorheben. Sie können weiterhin über ein App-Paket verfügen, das auf allen Geräten ausgeführt wird, indem Sie vor dem Aufrufen einer Erweiterungs-API überprüfen, auf welcher Gerätefamilie Ihre App ausgeführt wird.

-   Adaptive Steuerelemente und Eingaben

    Benutzeroberflächenelemente verwenden *effektive Pixel* (siehe [Reaktionsfähiges Design – Grundlagen für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/Dn958435)), sodass sie sich selbst basierend auf der Anzahl der auf dem Gerät verfügbaren Pixel automatisch anpassen. Sie funktionieren außerdem mit mehreren Arten von Eingaben, z. B. Tastatur, Maus, Touch, Stift und Xbox One-Controller. Wenn Sie die Benutzeroberfläche auf eine bestimmte Bildschirmgröße oder ein bestimmtes Gerät anpassen müssen, helfen Ihnen neue Layoutbereiche und Tools dabei, die Benutzeroberfläche auf die Geräte anzupassen, auf denen Ihre App möglicherweise ausgeführt wird.

Eine ausführlichere Betrachtung der UWP finden Sie unter [Anleitung für Universal Windows Platform-Apps](universal-application-platform-guide.md).

## <a name="use-a-language-you-already-know"></a>Verwenden einer vertrauten Sprache


Sie können mit den Programmiersprachen, die Sie am besten kennen, z. B. C# oder Visual Basic mit XAML, JavaScript mit HTML oder C++ mit DirectX und/oder Extensible Application Markup Language (XAML) UWP-Apps erstellen. Sie können sogar Komponenten in einer Programmiersprache schreiben und in einer App verwenden, die in einer anderen Sprache geschrieben ist.

UWP-Apps verwenden die Windows-Runtime. Hierbei handelt es sich um eine systemeigene, in das Betriebssystem integrierte API. Diese API ist in C++ implementiert und wird in JavaScript, C#, Visual Basic und C++ unterstützt, als wäre sie Teil der jeweiligen Sprache.

Microsoft Visual Studio 2015 stellt eine Vorlage für UWP-Apps für jede Sprache bereit. Damit können Sie ein einzelnes Projekt für alle Geräte erstellen. Nachdem Sie Ihre Arbeit abgeschlossen haben, können Sie ein App-Paket erstellen und aus Visual Studio heraus an den Windows Store senden, um die App für Kunden mit beliebigen Windows 10-Geräten bereitzustellen.

## <a name="uwp-apps-come-to-life-on-windows"></a>UWP-Apps entfalten unter Windows ihre Vielseitigkeit


Unter Windows kann Ihre App relevante Infos in Echtzeit für Benutzer bereitstellen, damit sie immer wieder auf die App zugreifen. In der modernen App-Welt mit vielen Konkurrenzprodukten muss Ihre App so attraktiv sein, dass sie von Benutzern gern und häufig genutzt wird. Windows bietet Ihnen viele Ressourcen, mit deren Hilfe Sie erreichen können, dass Benutzer Ihre App immer wieder nutzen:

-   Auf Live-Kacheln und dem Sperrbildschirm werden kontextbezogene, relevante und aktuelle Informationen auf einen Blick sichtbar angezeigt.
-   Pushbenachrichtigungen informieren Benutzer in Echtzeit über wichtige Ereignisse, sobald diese verfügbar sind.

-   Benachrichtigungen und Inhalte, auf die Benutzer reagieren müssen, können Sie im neuen Info-Center organisieren und anzeigen.

-   Hintergrundausführung und Auslöser erwecken Ihre App zum Leben, wenn der Benutzer sie benötigt.

-   Per Sprache und mit Bluetooth LE-Geräten kann Ihre App Benutzern die Interaktion mit der Welt um sie herum ermöglichen.

Schließlich können Sie auch Roamingdaten und das Windows-Schließfach für Anmeldeinformationen nutzen, um auf allen Windows-Bildschirmen, auf denen Benutzer Ihre App ausführen, für eine einheitliche Roamingumgebung zu sorgen. Roamingdaten sind eine einfache Möglichkeit zum Speichern der Präferenzen und Einstellungen von Benutzern in der Cloud, ohne Ihre eigene Infrastruktur für die Synchronisierung erstellen zu müssen. Und Sie können die Anmeldeinformationen von Benutzern im Schließfach für Anmeldeinformationen speichern, wo Sicherheit und Zuverlässigkeit oberste Priorität haben.

##  <a name="monetize-your-app-your-way"></a>Machen Sie Ihre App nach Ihren eigenen Vorstellungen zu Geld


Unter Windows können Sie Ihre App nach Ihren eigenen Vorstellungen zu Geld machen – für Smartphones, Tablets, PCs und andere Geräte. Wir bieten Ihnen verschiedene Möglichkeiten, wie Sie Ihre App und die zugehörigen Dienste zu Geld machen können. Sie müssen nur die für Sie am besten geeignete Methode auswählen:

-   Ein bezahlter Download ist die einfachste Möglichkeit. Geben Sie einfach nur Ihren Preis an.
-   Testversionen sind eine großartige Möglichkeit, um Benutzern vor dem Kauf das Ausprobieren Ihrer App zu ermöglichen. Gegenüber den herkömmlicheren „Freemium“-Optionen verbessert dies die Auffindbarkeit und steigert die Kaufabschlüsse.
-   In-App-Käufe bieten in Bezug auf die Monetarisierung von Apps die größte Flexibilität.

## <a name="lets-get-started"></a>Los geht's


Eine ausführlichere Betrachtung der UWP finden Sie unter [Anleitung für Universal Windows Platform-Apps](universal-application-platform-guide.md). Sehen Sie sich dann [Vorbereiten](get-set-up.md) an, um die Tools herunterladen, Sie zum Erstellen von Apps benötigen.

## <a name="related-topics"></a>Verwandte Themen


* [Anleitung für UWP-Apps (Universelle Windows-Plattform)](universal-application-platform-guide.md)
* [Einrichten](get-set-up.md)

## <a name="more-advanced-topics"></a>Fortgeschrittenere Themen

* [.NET Native - Bedeutung für UWP-Entwickler (Universelle Windows-Plattform)](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [Universelle Windows-Apps in .NET](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [.NET für UWP-Apps](https://msdn.microsoft.com/en-us/library/mt185501.aspx)

