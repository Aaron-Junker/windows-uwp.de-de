---
title: Android-Entwicklung unter Windows
description: Eine Anleitung für den Einstieg in die Entwicklung für Android unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android unter Windows, xamarin. Android, reagieren auf native, Cordova, Ionic, PhoneGap, c++ Android Game, Windows Defender, Emulator
ms.date: 04/28/2020
ms.openlocfilehash: 315c32eb6219deafe2817ba8df36b4f9d1cb1dae
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254287"
---
# <a name="overview-of-android-development-on-windows"></a>Übersicht über die Android-Entwicklung unter Windows

Es gibt mehrere Pfade zum Entwickeln einer Android-Geräte-App mithilfe des Windows-Betriebssystems. Diese Pfade werden in drei Haupttypen unterteilt: **[native Android-Entwicklung](#native-android)**, **[plattformübergreifende Entwicklung](#cross-platform)** und **[Android-Spieleentwicklung](#game-development)**. Mithilfe dieser Übersicht können Sie entscheiden, welcher Entwicklungspfad für die Entwicklung einer Android-App befolgt werden soll, und dann die [nächsten Schritte](#next-steps) zur Unterstützung der Verwendung von Windows für die Entwicklung mit ausführen:

- [Natives Android](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md)
- [Cordova, Ionic oder PhoneGap](pwa.md)
- [C/C++ für die Spieleentwicklung](native-android.md#use-c-or-c-for-android-game-development)

Außerdem erhalten Sie in diesem Handbuch Tipps zur Verwendung von Windows für folgende Aktionen:

- [Testen auf einem Android-Gerät oder-Emulator](emulator.md)
- [Aktualisieren der Windows Defender-Einstellungen zur Verbesserung der Leistung](defender-settings.md)
- [Entwickeln Sie Dual-Screen-Apps für Android, und erhalten Sie das Surface Duo-Geräte-SDK](/dual-screen/android/)

## <a name="native-android"></a>Natives Android

Die [native Android-Entwicklung unter Windows](./native-android.md) bedeutet, dass Ihre APP nur auf Android (nicht auf IOS-oder Windows-Geräte) abzielt. Sie können [Android Studio](https://developer.android.com/studio/install#windows) oder [Visual Studio](https://visualstudio.microsoft.com/vs/android/) verwenden, um innerhalb des Ökosystems zu entwickeln, das speziell für das Android-Betriebssystem entwickelt wurde. Die Leistung wird für Android-Geräte optimiert. das Erscheinungsbild der Benutzeroberfläche ist mit anderen systemeigenen apps auf dem Gerät konsistent, und alle Features oder Funktionen des Benutzer Geräts sind direkt für den Zugriff und die Nutzung von nutzen. Wenn Sie Ihre APP in einem systemeigenen Format entwickeln, hilft es Ihnen, das richtige zu finden, da Sie alle Interaktionsmuster und die Standards der Benutzererfahrung befolgt, die speziell für Android-Geräte eingerichtet wurden.

## <a name="cross-platform"></a>Plattformübergreifend

Plattformübergreifende Frameworks bieten eine einzelne Codebasis, die (größtenteils) von Android-, IOS-und Windows-Geräten gemeinsam genutzt werden kann. Die Verwendung eines plattformübergreifenden Frameworks kann Ihrer APP helfen, das gleiche Aussehen, Verhalten und die gleiche Erfahrung auf Geräte Plattformen aufrechtzuerhalten, und das automatische Rollout von Updates und Korrekturen zu nutzen. Anstatt eine Vielzahl von gerätespezifischen Code Sprachen kennen zu müssen, wird die app in einer freigegebenen Codebasis entwickelt, in der Regel in einer einzigen Sprache.

Plattformübergreifende Frameworks Zielen zwar so nah wie möglich bei nativen apps ab, Sie sind jedoch nie so nahtlos integriert wie eine nativ entwickelte APP und können die Leistung beeinträchtigen und die Leistung beeinträchtigt werden. Außerdem verfügen die Tools, die für die Erstellung plattformübergreifender Apps verwendet werden, möglicherweise nicht über alle Features, die von den einzelnen Geräte Plattformen angeboten werden, was möglicherweise Problem Umgehungen erfordert.

Eine Codebasis besteht in der Regel aus **UI-Code** zum Erstellen der Benutzeroberfläche, wie z. b. Seiten, Schaltflächen Steuerelemente, Bezeichnungen, Listen usw., und **Logik Code**, zum Aufrufen von Webdiensten, zum Zugreifen auf eine Datenbank, zum Aufrufen von Hardwarefunktionen und zum Verwalten des Zustands. Im Durchschnitt können 90% dieser wieder verwendet werden, obwohl es in der Regel notwendig ist, Code für jede Geräteplattform anzupassen. Diese Generalisierung hängt größtenteils von der Art der App ab, die Sie erstellen, bietet jedoch einen etwas Kontext, der hoffentlich bei ihrer Entscheidungsfindung hilfreich ist.  

## <a name="choosing-a-cross-platform-framework"></a>Auswählen eines plattformübergreifenden Frameworks

[Xamarin Native (xamarin. Android)](xamarin-android.md)

- UI-Code: XML mit Android Designer und Material Design
- Logik Code: c# oder F #
- Sie können auch auf einige Native Android-Elemente tippen, aber gut für die Wiederverwendung der Codebasis für andere Plattformen (Ios, Windows).
- Nur der Logik Code ist plattformübergreifend freigegeben, nicht UI-Code.
- Hervorragend für komplexere apps mit einer gerätespezifischen Benutzeroberfläche.

[Xamarin-Formulare (xamarin. Forms)](xamarin-forms.md)

- UI-Code: XAML und .net (mit Visual Studio)
- Logik Code: C #
- Gibt ungefähr 60 – 90% der Logik und den UI-Code für Android-, IOS-und Windows-Geräte-apps frei. 
- Verwendet gängige Benutzer Steuerelemente wie Schaltfläche, Bezeichnung, Eintrag, ListView, Stacklayout, Calendar, tabbedpage usw. Erstellen Sie eine Schaltfläche, und xamarin Forms gibt an, wie die systemeigene Schaltfläche für jede Plattform mithilfe der Bindungs Bibliothek aufgerufen wird, um Java-oder SWIFT-Code aus c# aufzurufen.
- Hervorragend geeignet für einfache apps, wie z. b. interne oder branchenspezifische apps, Prototypen oder MVPs. Jede APP, die mit einer einfachen Benutzeroberfläche etwas Standard oder generisch aussehen kann.

[React Native](react-native.md)

- UI-Code: JavaScript
- Logik Code: JavaScript
- Das Ziel von "systemeigene Reaktion" ist nicht das einmalige Schreiben des Codes und das Ausführen des Codes auf einer beliebigen Plattform, sondern das einmalige lernen (die Reaktionsweise) und das Schreiben überall.
- Die Community hat Tools wie z. b. "Expo" und "Create" systemeigene app hinzugefügt, um apps zu unterstützen, die ohne Verwendung von Xcode oder Android Studio
- Ähnlich wie xamarin (c#) können Sie systemeigene Benutzeroberflächen Elemente (JavaScript) aufrufen (ohne Java/Kotlin oder SWIFT schreiben zu müssen).

[Progressive Web Apps (PWAs)](pwa.md)

- UI-Code: HTML, CSS, JavaScript
- Logik Code: JavaScript
- PWAs sind Web-Apps, die mit Standard Mustern erstellt wurden, damit Sie sowohl Web-als auch native App-Features nutzen können. Sie können ohne ein Framework erstellt werden, aber einige gängige Frameworks sind [Ionic](https://ionicframework.com/docs/intro) und [PhoneGap](https://phonegap.com/about/).
- PWAs kann auf einem Gerät (Android, IOS oder Windows) installiert werden und kann aufgrund der Einbindung eines Service Workers Offline funktionieren.
- PWAs kann ohne App Store mithilfe einer Web-URL verteilt und installiert werden. Die Microsoft Store und Google Play Store zulassen, dass PWAs aufgeführt wird. der Apple Store ist derzeit nicht möglich, aber Sie können weiterhin auf einem IOS-Gerät installiert werden, auf dem 12,2 oder höher ausgeführt wird.
- Weitere Informationen finden Sie in der [Einführung zu PWAs](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction) in MDN.

## <a name="game-development"></a>Spieleentwicklung

Die Spieleentwicklung für Android ist häufig von der Entwicklung einer Android-Standard-App eindeutig, da Spiele normalerweise benutzerdefinierte Renderinglogik verwenden, die häufig in OpenGL oder Vulkan geschrieben ist Aus diesem Grund und aufgrund der vielen verfügbaren C-Bibliotheken, die die Spieleentwicklung unterstützen, ist es für Entwickler üblich, [C/C++ mit Visual Studio](/cpp/cross-platform/?view=vs-2019)zusammen mit dem Android [native Development Kit (NDK)](/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)zu verwenden, um Spiele für Android zu erstellen. [Beginnen Sie mit C/C++ für die Spieleentwicklung](native-android.md#use-c-or-c-for-android-game-development).

Ein weiterer gängiger Pfad zum Entwickeln von Spielen für Android ist die Verwendung einer Spiel-Engine. Es sind viele kostenfreie und Open Source-Engines verfügbar, z. b. [Unity mit Visual Studio](/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019), [Unreal Engine](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html), [monogame mit xamarin](/xamarin/graphics-games/monogame/introduction/), [urhusharp mit xamarin](/xamarin/graphics-games/urhosharp/introduction), [skiasharp mit xamarin. Forms](/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) cocoonjs, App Game Kit, Fusion, Corona SDK, Cocos 2D und mehr.

## <a name="next-steps"></a>Nächste Schritte

- [Beginnen Sie mit der nativen Android-Entwicklung unter Windows](native-android.md)
- [Einstieg in die Entwicklung für Android mit xamarin. Android](xamarin-android.md)
- [Einstieg in die Entwicklung für Android mit xamarin. Forms](xamarin-forms.md)
- [Einstieg in die Entwicklung für Android mithilfe von "auf systemeigenen](react-native.md)
- [Einstieg in die Entwicklung eines PWA für Android](pwa.md)
- [Entwickeln Sie Dual-Screen-Apps für Android, und erhalten Sie das Surface Duo-Geräte-SDK](/dual-screen/android/)
- [Windows Defender-Ausschlüsse zum Verbessern der Leistung hinzufügen](defender-settings.md)
- [Virtualisierungsunterstützung zur Verbesserung der emulatorleistung aktivieren](emulator.md#enable-virtualization-support)