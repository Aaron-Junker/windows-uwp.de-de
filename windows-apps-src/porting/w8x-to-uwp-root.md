---
description: Wenn Sie eine Universal 8.1-app haben &\#8212; gibt an, ob er Windows 8.1, Windows Phone 8.1 oder beides abzielt &\#8212; und Sie werden feststellen, dass der Quellcode und Fähigkeiten reibungslos auf Windows 10 Portieren werden.
title: Wechsel von Windows-Runtime 8.x zu UWP
ms.assetid: ac163b57-dee0-43fa-bab9-8c37fbee3913
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3985bfef66bfcbcaab90a4f915aeab33b7f7bd33
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372221"
---
# <a name="move-from-windows-runtime-8x-to-uwp"></a>Wechsel von Windows-Runtime 8.x zu UWP


Wenn Sie eine Universal 8.1-app haben – gibt an, ob er Windows 8.1, Windows Phone 8.1 oder beides abzielt, und Sie werden feststellen, dass der Quellcode und Fähigkeiten reibungslos auf Windows 10 Portieren werden. Unter Windows 10 können Sie eine app (Universelle Windows Plattform) erstellen, die eine einzelne app-Paket ist, die Ihre Kunden auf jede Art von Gerät installieren können. Weitere Hintergrundinformationen für Windows 10 UWP-apps und die Konzepte von adaptiven Code und adaptive Benutzeroberfläche, in denen wir in diesem Leitfaden zum Portieren erwähnt werden, finden Sie unter [Anleitung für UWP-apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

Beim Portieren, werden Sie feststellen, dass Windows 10 die meisten APIs, mit der früheren Plattformen sowie XAML-Markup, Benutzeroberflächen-Framework und Tools teilt, und Sie es finden alle reassuringly vertraut. Genau wie vorher, können Sie weiterhin zwischen C++, C# und Visual Basic als Programmiersprache mit dem XAML-Benutzeroberflächenframework wählen. Die ersten Schritte bei der Planung, was genau mit der aktuellen App oder den Apps geschehen soll, hängt von der Art der vorhandenen Apps und Projekte ab. Dies wird in den folgenden Abschnitten erläutert.

## <a name="if-you-have-a-universal-81-app"></a>Bei einer universellen 8.1-App

Eine universelle 8.1-App wird aus einem universellen 8.1-App-Projekt erstellt. Nehmen wir an den Namen des Projekts ist AppName\_81. Es enthält diese untergeordneten Projekte.

-   AppName\_81.Windows. Dies ist das Projekt, das das app-Paket für Windows 8.1-builds.
-   AppName\_81.WindowsPhone. Dies ist das Projekt, das das App-Paket für Windows Phone 8.1 erstellt.
-   AppName\_81.Shared. Dies ist das Projekt, das den Quellcode, die Markupdateien und andere Assets und Ressourcen enthält, die von den beiden anderen Projekten verwendet werden.

Häufig eine 8.1 universelle Windows-app die gleichen Funktionen bietet – und dies mit den gleichen Code und Markup – in der Windows 8.1 und Windows Phone 8.1-Formularen. Eine app, ist ein idealer Kandidat für das Portieren auf eine einzelne Windows 10-app, die auf die Universal-Gerätefamilie (und Sie können auf die größtmögliche Palette an Geräten installieren). Sie portieren im Grunde den Inhalt des freigegebenen Projekts und müssen nur wenige oder gar keine Elemente von den anderen beiden Projekten verwenden, da sie nur wenig oder keinen Inhalt haben.

In anderen Fällen, die Windows 8.1-und/oder den Windows Phone 8.1-Form der app enthalten spezielle Features. Oder sie enthalten den gleichen Featureumfang, implementieren diese Features jedoch mit verschiedenen Techniken oder anderen Technologien. Mit einer solchen App haben Sie die Möglichkeit, sie zu einer einzelnen App zu portieren, die auf die universelle Gerätefamilie ausgerichtet ist (in diesem Fall sollte sich die App selbst an verschiedene Geräte anpassen), oder Sie können sie zu mehr als einer App portieren, z. B. zu einer, die auf die Desktopgerätefamilie ausgerichtet ist, und einer für die Mobilgerätefamilie. Die Art der universellen 8.1-App bestimmt, welche Option für diesen Fall am besten geeignet ist.

1.  Portieren Sie den Inhalt des freigegebenen Projekts zu einer App für die universelle Gerätefamilie. Übernehmen Sie ggf. andere Inhalte aus den Windows- und WindowsPhone-Projekten, und verwenden Sie diese Inhalte bedingungslos in der App oder bedingt auf dem Gerät, auf dem Ihre App zu diesem Zeitpunkt ausgeführt wird (das letztere Verhalten wird auch als *adaptiv* bezeichnet).
2.  Portieren Sie den Inhalt des WindowsPhone-Projekts zu einer App für universelle Geräte. Retten Sie ggf. andere Inhalte aus dem Windows-Projekt, und verwenden Sie sie bedingungslos oder adaptiv.
3.  Portieren Sie den Inhalt des Windows-Projekts zu einer App für universelle Geräte. Retten Sie ggf. andere Inhalte aus dem WindowsPhone-Projekt, und verwenden Sie sie bedingungslos oder adaptiv.
4.  Portieren Sie den Inhalt des Windows-Projekts in eine App für universelle Geräte oder Desktopgeräte, und portieren Sie auch den Inhalt des WindowsPhone-Projekts zu einer App für universelle Geräte oder Mobilgeräte. Sie können eine Projektmappe mit einem freigegebenen Projekt erstellen und weiterhin Quellcode, Markupdateien und andere Assets und Ressourcen zwischen den beiden Projekten gemeinsam nutzen. Oder Sie können verschiedene Projektmappen erstellen und weiterhin dieselben Elemente mithilfe von Links teilen.

## <a name="if-you-have-a-windows-81-app"></a>Bei einer Windows 8.1-App

Portieren Sie das Projekt zu einer App für universelle Geräte oder Desktopgeräte. Wenn Sie die universelle Gerätefamilie wählen und Ihre App die APIs aufruft, die nur in der Desktopgerätefamilie implementiert sind, können Sie diese Aufrufe mit adaptivem Code schützen.

## <a name="if-you-have-a-windows-phone-81-app"></a>Bei einer Windows Phone 8.1-App

Portieren Sie das Projekt zu einer App für universelle Geräte oder Mobilgeräte. Wenn Sie die universelle Gerätefamilie wählen und Ihre App die APIs aufruft, die nur in der Mobilgerätefamilie implementiert sind, können Sie diese Aufrufe mit adaptivem Code schützen.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Anpassen der App an mehrere Formfaktoren

Die in den vorherigen Abschnitten gewählte Option bestimmt das Gerätespektrum, auf dem Ihre App oder Apps ausgeführt werden, und dabei kann es sich um eine sehr breite Palette von Geräten handeln. Selbst wenn Sie Ihre App auf mobile Geräte beschränken, müssen Sie dennoch ein breites Spektrum an Bildschirmgrößen unterstützen. Wenn Ihre App unter verschiedenen Formfaktoren ausgeführt wird, die sie früher nicht unterstützt haben, sollten Sie Ihre Benutzeroberfläche daher für diese Formfaktoren testen und ggf. Änderungen vornehmen, damit die Benutzeroberfläche entsprechend angepasst wird. Stellen Sie sich dies als eine Aufgabe nach der Portierung oder eine erweiterte Zielvorgabe für das Portieren vor. In den Fallstudien [Bookstore2](w8x-to-uwp-case-study-bookstore2.md) und [QuizGame](w8x-to-uwp-case-study-quizgame.md) gibt es einige Praxisbeispiele dafür.

## <a name="approaching-porting-layer-by-layer"></a>Portieren der einzelnen Ebenen

Beim Portieren einer universellen 8.1-App zum Modell für UWP-Apps können Sie auf nahezu alle Ihre Kenntnisse und Erfahrungen sowie einen Großteil Ihres Quellcodes, Markups und Ihrer Softwaremuster zurückgreifen.

-   **Ansicht**. Die Ansicht und das Ansichtsmodell bilden die UI Ihrer App. Im Idealfall besteht die Ansicht aus Markup, das an feststellbare Eigenschaften eines Ansichtsmodells gebunden ist. Ein weiteres gängiges, aber nur auf kurze Sicht zweckmäßiges Muster ist das direkte Ändern von UI-Elementen mit imperativem Code in einer CodeBehind-Datei. In beiden Fällen lassen sich Ihre UI-Markups und -Designs – und sogar imperativer Code zum Ändern von UI-Elementen – einfach portieren.
-   **Ansichtsmodelle und Datenmodelle**. Auch wenn Sie Muster zur Trennung der Zuständigkeiten (z. B. MVVM) nicht ausdrücklich anwenden, gibt es in Ihrer App zwangsläufig Code, der die Funktion des Ansichts- und Datenmodells übernimmt. Code für das Ansichtsmodell nutzt Typen in den Namespaces des Benutzeroberflächenframeworks. Sowohl der Code für das Ansichtsmodell als auch der Code für das Datenmodell nutzen zudem nicht visuelle Betriebssystem- und .NET Framework-APIs (darunter APIs für den Datenzugriff). Und diese APIs sind [auch für UWP-Apps verfügbar](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), sodass der Großteil dieses Codes, wenn nicht gar der gesamte Code, ohne Änderung portiert wird.
-   **Clouddienste**. Höchstwahrscheinlich werden einige (oder sogar die meisten) Teile Ihrer App in Form von Diensten in der Cloud ausgeführt. Der auf dem Clientgerät ausgeführte Teil der App stellt eine Verbindung mit diesen Diensten her. Dies ist der Teil einer verteilten App, bei dem es am wahrscheinlichsten ist, dass er beim Portieren des Clientteils unverändert bleibt. Falls Sie noch keine Clouddienste nutzen, sind [Microsoft Azure Mobile Services](https://azure.microsoft.com/services/mobile-services/) eine gute Wahl für Ihre UWP-App. Sie bieten leistungsstarke Back-End-Komponenten, die Ihre App für verschiedenste Dienste aufrufen kann – angefangen bei einfachen Benachrichtigungen für Live-Kachelaktualisierungen bis hin zur komplexen Skalierbarkeit, die eine Serverfarm bereitstellen kann.

Überlegen Sie vor oder während der Portierung, ob Ihre App dadurch verbessert werden könnte, wenn Sie sie umgestalten und Code mit einem ähnlichen Zweck in Ebenen zusammenfassen, anstatt ihn willkürlich zu verteilen. Die Aufteilung Ihrer App in Ebenen wie die oben beschriebenen erleichtert Ihnen das Ausschließen von Fehlern, Testen und spätere Lesen und Warten Ihrer App. Sie können Funktionalität stärker wiederverwendbar machen, indem Sie dem Model-View-ViewModel ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx))-Muster folgen. Dieses Muster trennt die Daten-, Geschäfts- und UI-Teile der App voneinander. Auch innerhalb der UI kann das Muster Zustand und Verhalten von den visuellen Elementen getrennt halten, sodass diese separat getestet werden können. Das MVVM-Muster bietet Ihnen die Möglichkeit, Ihre Daten und Geschäftslogik einmal zu schreiben und auf allen Geräten, unabhängig von der UI, zu verwenden. Es ist wahrscheinlich, dass Sie auch einen Großteil des Ansichtsmodells und der Ansichtselemente auf verschiedenen Geräten wiederverwenden können.

| Thema | Beschreibung |
|-------|-------------|
| [Portieren das Projekt](w8x-to-uwp-porting-to-a-uwp-project.md) | Sie haben zwei Möglichkeiten, wenn Sie mit dem Portierungsprozess beginnen. Eine Möglichkeit ist das Bearbeiten einer Kopie der vorhandenen Projektdateien, einschließlich des App-Paketmanifests (die Vorgehensweise finden Sie in den Informationen zum Aktualisieren der Projektdateien unter [Migrieren von Apps zur universellen Windows-Plattform (UWP)](https://docs.microsoft.com/visualstudio/misc/migrate-apps-to-the-universal-windows-platform-uwp?view=vs-2015)). Die andere Möglichkeit ist das Erstellen eines neuen Windows 10-Projekts in Visual Studio und das Kopieren Ihrer Dateien in dieses Projekt. |
| [Problembehandlung](w8x-to-uwp-troubleshooting.md) | Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird. Zu diesem Zweck können Sie einen temporären Fortschritt erzielen, indem Sie allen nicht unbedingt erforderlichen Code auskommentieren und anschließend zurückkehren, um die Schulden später zu bezahlen. Die Tabelle mit Symptomen und Möglichkeiten zur Problembehandlung in diesem Thema kann in dieser Phase hilfreich für Sie sein, ersetzt jedoch nicht das Lesen der nächsten Themen. Sie können die Tabelle jederzeit zu Rate ziehen, während Sie die weiteren Themen lesen. |
| [Portieren von XAML und Benutzeroberfläche](w8x-to-uwp-porting-xaml-and-ui.md) | Die Vorgehensweise zum Definieren einer Benutzeroberfläche in Form von deklarativem XAML-Markup lässt sich sehr gut von universellen 8.1-Apps auf UWP-Apps für die universelle Windows-Plattform (UWP) übertragen. Sie werden feststellen, dass der Großteil des Markups kompatibel ist. Unter Umständen sind jedoch einige Anpassungen bei Systemressourcenschlüsseln oder benutzerdefinierten Vorlagen erforderlich. |
| [Für e/a-, Geräte- und app-Modell Portieren](w8x-to-uwp-input-and-sensors.md) | Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer. Auch die Datenverarbeitung kann einbezogen werden. Aber dieser Code wird in der Regel nicht als UI-Ebene oder Datenebene betrachtet. Dieser Code enthält die Integration in Vibrationscontroller, Beschleunigungsmesser, Gyroskop, Mikrofon und Lautsprecher (überschneiden sich mit Spracherkennung und Sprachsynthese), (geografischen) Standort und Eingabemodalitäten, z. B. Touch, Maus, Tastatur und Stift. |
| [Fallstudie: Bookstore1](w8x-to-uwp-case-study-bookstore1.md) | Dieses Thema enthält eine Fallstudie für das Portieren einer sehr einfachen universellen 8.1-App zu einer UWP-App für Windows 10. Bei einer universellen 8.1-App wird ein App-Paket für Windows 8.1 und ein anderes App-Paket für Windows Phone 8.1 erstellt. Mit Windows 10 können Sie ein einzelnes App-Paket erstellen, das Ihre Kunden auf einer Vielzahl von Geräten installieren können – und genau das werden wir in dieser Fallstudie tun. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). |
| [Fallstudie: Bookstore2](w8x-to-uwp-case-study-bookstore2.md) | Diese Fallstudie baut auf den Informationen zum [SemanticZoom](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Steuerelement auf. Im Ansichtsmodell stellt jede Instanz der Author-Klasse die Gruppe der vom betreffenden Autor verfassten Titel dar. In SemanticZoom können wir dann entweder die Bücherliste nach Autoren gruppiert anzeigen oder die Liste verkleinern, um eine Sprungliste der Autoren zu erhalten. |
| [Fallstudie: QuizGame](w8x-to-uwp-case-study-quizgame.md) | Dieses Thema enthält eine Fallstudie zum Portieren eines funktionierenden Peer-zu-Peer-Quizspiels (WinRT 8.1-Beispiel-App) zu einer UWP-App für Windows 10. |

## <a name="related-topics"></a>Verwandte Themen

**Dokumentation**
* [Windows-Runtime-Referenz](https://docs.microsoft.com/uwp/api/)
* [Erstellen von universellen Windows-apps für alle Windows-Geräte](https://go.microsoft.com/fwlink/p/?LinkID=397871)
* [Entwickeln der UX für apps](https://docs.microsoft.com/previous-versions/windows/hh767284(v=win.10))
