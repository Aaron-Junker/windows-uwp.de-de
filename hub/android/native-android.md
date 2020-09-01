---
title: Native Android-Entwicklung unter Windows
description: Beginnen Sie mit der Entwicklung von Android Native apps unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Android Studio, Visual Studio, c++ Android Game, Windows Defender, Emulator, Virtual Device, install, Java, Kotlin
ms.date: 04/28/2020
ms.openlocfilehash: c9c718d2cccc6a38ac75d3220a79c7b2ec757f54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166844"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Beginnen Sie mit der nativen Android-Entwicklung unter Windows

In dieser Anleitung erfahren Sie, wie Sie mithilfe von Windows Native Android-Anwendungen erstellen. Wenn Sie eine plattformübergreifende Lösung bevorzugen, finden Sie unter Übersicht über die [Android-Entwicklung unter Windows](./overview.md) eine kurze Zusammenfassung einiger Optionen.

Die direkteste Methode zum Erstellen einer nativen Android-App ist die Verwendung von Android Studio mit [Java oder Kotlin](#java-or-kotlin), obwohl es auch möglich ist, [C oder C++ für die Android-Entwicklung zu verwenden](#use-c-or-c-for-android-game-development) , wenn Sie einen bestimmten Zweck haben. Mit den Android Studio SDK-Tools werden Ihre Code-, Daten-und Ressourcen Dateien in ein Archiv-Android-Paket, eine APK-Datei, kompiliert. Eine APK-Datei enthält den gesamten Inhalt einer Android-App und ist die Datei, die von Android-Geräten zum Installieren der APP verwendet wird.

## <a name="install-android-studio"></a>Installieren von Android Studio

Android Studio ist die offizielle integrierte Entwicklungsumgebung für das Android-Betriebssystem von Google. [Laden Sie die neueste Version von Android Studio für Windows herunter](https://developer.android.com/studio).

- Wenn Sie eine exe-Datei heruntergeladen haben (empfohlen), doppelklicken Sie darauf, um Sie zu starten.
- Wenn Sie eine ZIP-Datei heruntergeladen haben, entpacken Sie die ZIP-Datei, kopieren Sie den Ordner "Android-Studio" in den Ordner "Programme", öffnen Sie den Ordner "Android-Studio > bin", und starten Sie studio64.exe (für 64-Bit-Computer) oder studio.exe (für 32-Bit-Computer).

Führen Sie den Setup-Assistenten in Android Studio aus, und installieren Sie alle von ihm empfohlenen SDK-Pakete. Wenn neue Tools und andere APIs verfügbar werden, werden Sie von Android Studio über ein Popup benachrichtigt, oder Sie suchen nach Updates, indem Sie **Hilfe**  >  **Prüfung für Update**auswählen.

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

Wählen Sie **Datei**  >  **neu**neues  >  **Projekt**aus.

Im Fenster " **Projekt auswählen** " können Sie zwischen den folgenden Vorlagen wählen:

- **Grundlegende Aktivität**: erstellt eine einfache APP mit einer APP-Leiste, einer Gleit Komma Aktions Schaltfläche und zwei Layoutdateien: eine für die Aktivität und eine zum Trennen von Text Inhalt.

- **Leere Aktivität**: erstellt eine leere Aktivität und eine einzelne Layoutdatei mit Beispiel Text Inhalt.

- **Bottom Navigation Activity**: erstellt eine standardmäßige untere Navigationsleiste für eine Aktivität. Weitere Informationen finden Sie unter der [untersten Navigations Komponente](https://material.io/guidelines/components/bottom-navigation.html) in den Material Design Guidelines von Google.

- Weitere Informationen zum [Auswählen einer Aktivitäts Vorlage](https://developer.android.com/studio/projects/templates#SelectTemplate) finden Sie in der Android Studio-Dokumentation.

Vorlagen werden häufig zum Hinzufügen von Aktivitäten zu neuen und vorhandenen APP-Modulen verwendet. Um z. b. einen Anmeldebildschirm für die Benutzer Ihrer APP zu erstellen, fügen Sie eine Aktivität mit der [Anmelde Aktivitäts Vorlage](https://developer.android.com/studio/projects/templates#LoginActivity)hinzu.

> [!NOTE]
> Das Android-Betriebssystem basiert auf der Idee der **Komponenten** und verwendet die Begriffe " **Aktivität** " und " **Absicht** ", um Interaktionen zu definieren. Eine **Aktivität** stellt eine einzelne, fokussierte Aufgabe dar, die ein Benutzer ausführen kann. Eine **Aktivität** stellt ein Fenster zum Entwickeln der Benutzeroberfläche mithilfe von Klassen bereit, die auf der **Ansichts** Klasse basieren. Es gibt einen Lebenszyklus für **Aktivitäten** im Android-Betriebssystem, die durch eine Reihe von sechs Rückrufen definiert werden: `onCreate()` , `onStart()` , `onResume()` , `onPause()` , `onStop()` und `onDestroy()` . Die Aktivitäts Komponenten interagieren mithilfe von **Intent** -Objekten miteinander. Beabsichtigt definiert entweder die Aktivität, die gestartet werden soll, oder beschreibt den Typ der auszuführenden Aktion (und das System wählt die entsprechende Aktivität aus, die sogar von einer anderen Anwendung abgeleitet werden kann). Erfahren Sie mehr über [Aktivitäten](https://developer.android.com/reference/android/app/Activity), den [Aktivitäts Lebenszyklus](https://developer.android.com/guide/components/activities/activity-lifecycle)und [Intents](https://developer.android.com/reference/android/content/Intent.html) in der Android-Dokumentation.

### <a name="java-or-kotlin"></a>Java oder Kotlin

**Java** wurde zu einer Sprache in 1991, entwickelt von Was ist Sun Microsystems, aber jetzt im Besitz von Oracle. Es wurde zu einer der beliebtesten und leistungsfähigsten Programmiersprachen mit einer der größten Support Communitys weltweit. Java ist klassenbasiert und objektorientiert und so konzipiert, dass so wenig Implementierungs Abhängigkeiten wie möglich vorhanden sind. Die Syntax ähnelt C und C++, aber Sie verfügt über weniger Low-Level-Funktionen als beide.

" **Kotlin** " wurde zunächst als neue Open Source-Sprache von JetBrains in 2011 angekündigt und ist seit 2017 als Alternative zu Android Studio Java enthalten. Im Mai 2019 hat Google die Unterstützung für Android-App-Entwickler als bevorzugte Sprache angekündigt. obwohl es sich um eine neuere Sprache handelt, verfügt es auch über eine starke Support Community und wurde als eine der am schnellsten wachsenden Programmiersprachen identifiziert. Kotlin ist plattformübergreifend, statisch typisiert und für die Interoperabilität mit Java konzipiert.

Java wird für eine größere Anzahl von Anwendungen häufiger verwendet und bietet einige Features, die sich nicht auf die Funktion von Kotlin bezieht, z. b. überprüfte Ausnahmen, primitive Typen, die keine Klassen, statische Member, nicht private Felder, Platzhalter Typen und Ternäre Operatoren sind. Der Wert von "Kotlin" wurde speziell für Android entwickelt und empfohlen. Außerdem werden einige Features bereitstellt, die Java nicht bietet, wie z. b. NULL-Verweise, die vom Typsystem gesteuert werden, keine Rohdaten Typen, invariante Arrays, ordnungsgemäße Funktionstypen (im Gegensatz zur Sam-Konvertierungen von Java), die Varianz der Verwendung von Websites ohne Platzhalter, intelligente Umwandlungen usw. Die "Kotlin" [-Dokumentation bietet eine ausführlichere Betrachtung des Vergleichs zu Java](https://kotlinlang.org/docs/reference/comparison-to-java.html).

### <a name="minimum-api-level"></a>Minimale API-Ebene

Sie müssen die minimale API-Ebene für die Anwendung festlegen. Dadurch wird festgelegt, welche Android-Version von Ihrer Anwendung unterstützt wird. Niedrigere API-Ebenen sind älter und unterstützen in der Regel mehr Geräte, aber höhere API-Ebenen sind neuer und stellen weitere Funktionen bereit.

![Mindestens Android Studio API-Auswahlbildschirm](../images/android-minimum-api-selection.png)

Wählen Sie den Link **Help Me Choose** aus, um ein Vergleichs Diagramm zu öffnen, das die Geräte Unterstützungs Verteilung und die wichtigsten Features der Platt Form Versions Freigabe anzeigt.

![Android Studio minimaler API-Vergleichs Bildschirm](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>Sofortige app-Unterstützung und androidx-Artefakte

Möglicherweise bemerken Sie ein Kontrollkästchen, um **sofort-apps zu unterstützen** , und einen weiteren, um **androidx-Artefakte** in den Projekt Erstellungs Optionen Die *sofortige app-Unterstützung* wird nicht geprüft, und der *androidx* -Wert wird als empfohlener Standardwert geprüft.

Google Play **sofort-apps** bieten Personen die Möglichkeit, eine APP oder ein Spiel auszuprobieren, ohne Sie zuerst zu installieren. Diese sofort ausgegebene Apps können über die Play Store, Google Search, soziale Netzwerke und überall dort, wo Sie einen Link freigeben, zugänglich gemacht werden. Indem Sie das Kontrollkästchen **Unterstützung für Instant apps** aktivieren, bitten Sie Android Studio, das Google Play Instant Development SDK in Ihr Projekt einzubinden. Weitere Informationen zu [Google Play Instant apps](https://developer.android.com/topic/google-play-instant) und zum [Erstellen eines sofort aktivierten App Bundle](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle)finden Sie in der Android-Dokumentation.

**Androidx-Artefakte** stellen die neue Version der Android-Unterstützungs Bibliothek dar und bieten eine Abwärtskompatibilität über Android-Versionen hinweg. Androidx stellt einen konsistenten Namespace bereit, beginnend mit der Zeichenfolge androidx für alle verfügbaren Pakete.

> [!NOTE]
> "Androidx" ist jetzt die Standardbibliothek. Um dieses Kontrollkästchen zu deaktivieren und die vorherige Unterstützungs Bibliothek zu verwenden, muss das letzte Android Q SDK entfernt werden. Anweisungen dazu finden Sie unter [uncheck use androidx Artefakts](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) on StackOverflow (Verwenden von androidx-Artefakten in StackOverflow). Beachten Sie jedoch zunächst, dass die früheren Unterstützungs Bibliothekspakete den entsprechenden androidx Eine vollständige Zuordnung aller alten Klassen und Build-Artefakte zu den neuen Klassen finden Sie unter [Migrieren zu androidx](https://developer.android.com/jetpack/androidx/migrate).

## <a name="project-files"></a>Projektdateien

Das Fenster Android Studio **Projekt** enthält die folgenden Dateien (stellen Sie sicher, dass die Android-Ansicht im Dropdown Menü ausgewählt ist):

**App > Java > com. example. MyFirstApp > mainactivity**

Die Hauptaktivität und der Einstiegspunkt für Ihre APP. Wenn Sie die APP erstellen und ausführen, wird vom System eine Instanz dieser Aktivität gestartet und das Layout geladen.

**App > res > Layout > activity_main.xml**

Die XML-Datei, die das Layout für die Benutzeroberfläche der Aktivität definiert. Sie enthält ein TextView-Element mit dem Text "Hallo Welt".

**App > Manifeste > AndroidManifest.xml**

Die Manifest-Datei, die die grundlegenden Merkmale der APP und ihrer Komponenten beschreibt.

**Gradle-Skripts > Build. gradle**

Es gibt zwei Dateien mit dem Namen "Project: My First App", für das gesamte Projekt und "Module: App" für jedes app-Modul. Ein neues Projekt weist anfänglich nur ein Modul auf. Verwenden Sie die Datei Build. File des Moduls, um zu steuern, wie das gradle-Plug-in Ihre APP erstellt Weitere Informationen finden Sie in der Android-Dokumentation unter [Konfigurieren](https://developer.android.com/studio/build/index) des buildartikels.

## <a name="use-c-or-c-for-android-game-development"></a>Verwenden von C oder C++ für die Entwicklung von Android-spielen

Das Android-Betriebssystem ist darauf ausgelegt, in Java oder Kotlin geschriebene Anwendungen zu unterstützen, die von in der Architektur des Systems eingebetteten Tools profitieren. Viele Systemfunktionen, wie die Android-Benutzeroberfläche und die beabsichtigte Verarbeitung, werden nur über Java-Schnittstellen verfügbar gemacht. Es gibt einige Fälle, in denen Sie ggf. **C-oder C++-Code über das Android Native Development Kit (NDK) verwenden** möchten, trotz einiger der damit verbundenen Herausforderungen. Die Spieleentwicklung ist ein Beispiel, da Spiele in der Regel benutzerdefinierte Renderinglogik verwenden, die in OpenGL oder Vulkan geschrieben ist, und von einer Vielzahl von C-Bibliotheken profitieren, die sich Die Verwendung von C oder C++ *kann* auch helfen, eine zusätzliche Leistung von einem Gerät zu erreichen, um geringe Latenzzeiten zu erzielen oder rechenintensive Anwendungen auszuführen, wie z. b. Physik-Simulationen. Das NDK **ist jedoch für die meisten Anfänger von Android-Programmierern nicht geeignet** . Wenn Sie nicht über einen bestimmten Zweck für die Verwendung des NDK verfügen, empfiehlt es sich, mit Java, Kotlin oder einem der Platt [Form übergreifenden Frameworks](./overview.md)zu halten.

So erstellen Sie ein neues Projekt mit C/C++-Unterstützung:

- Wählen Sie im Android Studio Assistenten im Abschnitt **Projekt auswählen** den Typ *native C++** Project aus. Wählen Sie **weiter**, füllen Sie die restlichen Felder aus, und klicken Sie dann erneut auf **weiter** .

- Im Abschnitt **Anpassen der C++-Unterstützung** des Assistenten können Sie das Projekt mit dem **C++-Standard** Feld anpassen. Wählen Sie in der Dropdown Liste aus, welche Standardisierung von C++ Sie verwenden möchten. Bei Auswahl von **Toolchain default** wird die cmake-Standardeinstellung verwendet. Wählen Sie **Fertig stellen**aus.

- Nachdem Android Studio das neue Projekt erstellt hat, finden Sie einen **cpp** -Ordner im **Projekt** Bereich, der die systemeigenen Quelldateien, Header, Buildskripts für cmake oder NDK-Build und vorgefertigte Bibliotheken enthält, die Teil des Projekts sind. Sie können auch eine Beispiel-C++-Quelldatei, `native-lib.cpp` , im `src/main/cpp/` Ordner finden, der eine einfache Funktion bereitstellt, `stringFromJNI()` die die Zeichenfolge "Hello from C++" zurückgibt. Außerdem finden Sie ein cmake [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html) -Buildskript () im Stammverzeichnis Ihres Moduls, das zum Erstellen der nativen Bibliothek erforderlich ist.

Weitere Informationen finden Sie im Android docs Topic: [Hinzufügen von C-und C++-Code zu Ihrem Projekt](https://developer.android.com/studio/projects/add-native-code). Beispiele finden Sie in den [Android-NDK-Beispielen mit Android Studio C++-integrationsrepository](https://github.com/android/ndk-samples) auf GitHub. Um ein C++-Spiel unter Android zu kompilieren und auszuführen, verwenden Sie die [Google Play Game Services-API](https://developers.google.com/games/services/cpp/gettingStartedAndroid).

## <a name="design-guidelines"></a>Entwurfsrichtlinien

Gerätebenutzer erwarten, dass Anwendungen auf eine bestimmte Weise Aussehen und sich Verhalten... unabhängig davon, ob Sie auf sprach Steuerelemente schwenken oder tippen oder diese verwenden, halten Benutzer bestimmte Erwartungen an die Art und Weise, wie Ihre Anwendung aussehen sollte und wie Sie verwendet wird. Diese Erwartungen sollten konsistent bleiben, um Verwirrung und Frustration zu vermeiden. Android bietet eine Anleitung zu diesen Plattformen und Geräte Erwartungen, in der die Google Material Design Foundation für visuelle und Navigationsmuster zusammen mit Qualitätsrichtlinien für Kompatibilität, Leistung und Sicherheit kombiniert wird.

Weitere Informationen finden Sie in der [Android-Design Dokumentation](https://developer.android.com/design).

### <a name="fluent-design-system-for-android"></a>Flüssiges Entwurfs System für Android

Außerdem bietet Microsoft Entwurfs Leit Fäden mit dem Ziel, eine nahtlose Darstellung des gesamten Portfolios der mobilen Microsoft-apps bereitzustellen.

[Flüssiges Entwurfs System für Android](https://www.microsoft.com/design/fluent/#/android) Design und Erstellung benutzerdefinierter apps, bei denen es sich um System eigene Android-Apps handelt, die weiterhin eindeutig sind

- [Sketch Toolkit](https://aka.ms/fluenttoolkits/android/sketch)
- [Figma-Toolkit](https://aka.ms/fluenttoolkits/android/figma)
- [Android-Schriftart](https://fonts.google.com/specimen/Roboto)
- [Richtlinien für Android-Benutzeroberflächen](https://developer.android.com/design/)
- [Richtlinien für Android-App-Symbole](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Android-Anwendungs Grundlagen](https://developer.android.com/guide/components/fundamentals)

- [Entwickeln Sie Dual-Screen-Apps für Android, und erhalten Sie das Surface Duo-Geräte-SDK](/dual-screen/android/)

- [Windows Defender-Ausschlüsse zum Verbessern der Leistung hinzufügen](defender-settings.md)

- [Virtualisierungsunterstützung zur Verbesserung der emulatorleistung aktivieren](emulator.md#enable-virtualization-support)