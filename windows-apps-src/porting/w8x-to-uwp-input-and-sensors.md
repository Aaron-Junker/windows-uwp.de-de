---
author: stevewhims
description: Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer.
title: Portieren von Windows-Runtime 8.x zu UWP für E/A, Gerät und App-Modell
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8e15014e39ed6d980cbe80daa0a129ff83a021b9
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2018
ms.locfileid: "7574433"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>Portieren von Windows-Runtime 8.x zu UWP für E/A, Gerät und App-Modell




Im vorherigen Thema ging es um das [Portieren von XAML und UI](w8x-to-uwp-porting-xaml-and-ui.md).

Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer. Auch die Datenverarbeitung kann einbezogen werden. Aber dieser Code wird in der Regel nicht als UI-Ebene *oder* Datenebene betrachtet. Dieser Code enthält die Integration in Vibrationscontroller, Beschleunigungsmesser, Gyroskop, Mikrofon und Lautsprecher (Überschneidungen mit Spracherkennung und Sprachsynthese), (geografischen) Standort und Eingabemodalitäten, z.B. Toucheingabe, Maus, Tastatur und Stift.

## <a name="application-lifecycle-process-lifetime-management"></a>App-Lebenszyklus (Prozesslebensdauer-Verwaltung)


Bei universellen8.1-Apps ist zwischen der Deaktivierung der App und dem Auslösen des Anhalteereignisses durch das System eine „Entprellfenster“-Zeit von zwei Sekunden vorhanden. Die Verwendung dieses Entprellfensters als zusätzliche Zeit zum Anhaltezustand ist unsicher, und für eine UWP (Universelle Windows-Plattform)-App ist überhaupt kein Entprellfenster vorhanden; das Anhalteereignis wird ausgelöst, sobald eine App inaktiv wird.

Weitere Informationen finden Sie unter [App-Lebenszyklus](https://msdn.microsoft.com/library/windows/apps/mt243287).

## <a name="background-audio"></a>Hintergrundaudio


Für die Eigenschaft [**MediaElement.AudioCategory**](https://msdn.microsoft.com/library/windows/apps/br227352) sind **ForegroundOnlyMedia** und **BackgroundCapableMedia** für Windows 10-apps als veraltet. Verwenden Sie stattdessen das Windows Phone Store-App-Modell. Weitere Informationen finden Sie unter [Hintergrundaudio](https://msdn.microsoft.com/library/windows/apps/mt282140).

## <a name="detecting-the-platform-your-app-is-running-on"></a>Erkennen der Plattform, auf der Ihre App ausgeführt wird


Die Möglichkeit für die Herangehensweise Ausrichtung von Apps ändert sich mit Windows 10. Das neue konzeptionelle Modell besteht darin, dass eine App auf die Universelle Windows-Plattform (UWP) ausgerichtet ist und auf allen Windows-Geräten ausgeführt wird. Dann besteht die Möglichkeit, Funktionen hervorzuheben, die exklusiv für bestimmte Gerätefamilien angeboten werden. Bei Bedarf besteht auch die Möglichkeit, die App auf eine oder mehrere bestimmte Gerätefamilien zu beschränken. Weitere Informationen zu Gerätefamilien – und wie Sie entscheiden, auf welche Sie eine App ausrichten sollten – finden Sie unter [Anleitung für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631).

Wenn in Ihrer universellen8.1-App Code vorhanden ist, der erkennt, unter welchem Betriebssystem sie ausgeführt wird, müssen Sie diesen je nach dem Grund für die Logik möglicherweise ändern. Wenn die App den Wert weitergibt und nicht verwendet, sollten Sie weiterhin die Betriebssysteminformationen sammeln.

**Hinweis:**  empfohlen, dass Sie nicht Betriebssystem oder die Gerätefamilie zum Ermitteln des Vorhandenseins von Features zu. Das Identifizieren des aktuellen Betriebssystems oder der Gerätefamilie ist in der Regel nicht die beste Möglichkeit, um festzustellen, ob ein bestimmtes Feature für das Betriebssystem oder die Gerätefamilie vorhanden ist. Anstatt das Betriebssystem oder die Gerätefamilie (und Versionsnummer) zu ermitteln, sollten Sie das Vorhandensein des Features selbst überprüfen (siehe [Bedingte Kompilierung und adaptiver Code](w8x-to-uwp-porting-to-a-uwp-project.md)). Wenn ein bestimmtes Betriebssystem oder eine bestimmte Gerätefamilie erforderlich ist, sollten Sie darauf achten, es bzw. sie als unterstützte Mindestversion zu verwenden, anstatt den Test nur für diese Version zu entwerfen.

 

Zum Anpassen der Benutzeroberfläche Ihrer App für verschiedene Geräte gibt es mehrere empfohlene Möglichkeiten. Verwenden Sie weiterhin Elemente mit automatischer Größenanpassung und dynamische Layoutbereiche. Verwenden Sie in Ihrem XAML-Markup weiterhin Größen in effektiven Pixeln (früher „Anzeigepixel“), damit sich die Benutzeroberfläche an verschiedene Auflösungen und Skalierungsfaktoren anpasst (siehe [Effektive Pixel, Abstand zum Bildschirm und Skalierungsfaktoren](w8x-to-uwp-porting-xaml-and-ui.md)). Verwenden Sie außerdem die adaptiven Auslöser und Setter des Visual State-Managers zum Anpassen der Benutzeroberfläche an die Fenstergröße (siehe [Anleitung für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631)).

Bei einem Szenario, in dem das Erkennen der Gerätefamilie unvermeidbar ist, können Sie so vorgehen. In diesem Beispiel verwenden wir die [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165)-Klasse, um zu einer für die jeweilige Mobilgerätefamilie angepassten Seite zu navigieren – falls diese vorhanden ist – und wir stellen sicher, dass andernfalls eine Umleitung auf eine Standardseite erfolgt.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

Ihre App kann auch anhand der aktiven Ressourcenauswahlfaktoren die Gerätefamilie ermitteln, auf der sie ausgeführt wird. Im folgenden Beispiel wird gezeigt, wie dies imperativ durchgeführt wird, und im Thema [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) wird der gängigere Anwendungsfall für die Klasse beim Laden der gerätefamilienspezifischen Ressourcen basierend auf dem Gerätefamilienfaktor beschrieben.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Siehe auch [Bedingte Kompilierung und adaptiver Code](w8x-to-uwp-porting-to-a-uwp-project.md).

## <a name="location"></a>Position


Wenn eine app, die die Position in der app-Paketmanifest deklariert unter Windows 10 ausgeführt wird, fordert das System die Zustimmung des Endbenutzers. Dies gilt unabhängig davon, ob die app eine Windows Phone Store-app oder eine Windows 10-app ist. Falls in Ihrer App eine eigene benutzerdefinierte Aufforderung zur Zustimmung oder eine Schaltfläche zum Aktivieren/Deaktivieren angezeigt wird, sollten Sie sie entfernen, damit Endbenutzer nur eine Aufforderung erhalten.

 

 




