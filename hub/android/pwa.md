---
title: PWA-Ansatz für die Android-Entwicklung unter Windows
description: Beginnen Sie mit der Entwicklung von Android-Apps mithilfe des PWA-Ansatzes unter Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android unter Windows, PWA, Android, Cordova, Ionic, PhoneGap, Hybrid-Web-App
ms.date: 04/28/2020
ms.openlocfilehash: 4559e795b4a9737bf68129790029f6f9136b4f81
ms.sourcegitcommit: 99f5544d9642c87a16e3bd21f76c2fcbc97c20d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743593"
---
# <a name="get-started-developing-a-pwa-or-hybrid-web-app-for-android"></a>Einstieg in die Entwicklung einer PWA-oder Hybrid-Web-App für Android

Dieser Leitfaden unterstützt Sie bei den ersten Schritten beim Erstellen einer hybriden Web-App oder einer progressiven Web-App (PWA) unter Windows mithilfe einer einzelnen HTML-/CSS/JavaScript-Codebasis, die im Web und auf Geräte Plattformen (Android, Ios, Windows) verwendet werden kann.

Durch die Verwendung der richtigen Frameworks und Komponenten können webbasierte Anwendungen auf einem Android-Gerät auf eine Weise funktionieren, die Benutzern sehr ähnlich wie eine native App entspricht.

## <a name="features-of-a-pwa-or-hybrid-web-app"></a>Funktionen einer PWA-oder Hybrid-Web-App

Es gibt zwei Haupttypen von Web-Apps, die auf Android-Geräten installiert werden können. Der Hauptunterschied besteht darin, ob Ihr Anwendungscode in ein App-Paket (Hybrid) eingebettet oder auf einem Webserver (PWA) gehostet wird.

- **Hybride Web-Apps**: Code (HTML, JS, CSS) ist in einem APK verpackt und kann über die Google Play Store verteilt werden. Die Anzeige-Engine ist vom Internetbrowser des Benutzers getrennt, ohne Sitzung oder Cache Freigabe.

- **Progressive Web-Apps (PWAs)**: Code (HTML, JS, CSS) befindet sich im Web und muss nicht als APK verpackt werden. Ressourcen werden mithilfe eines Service Workers heruntergeladen und bei Bedarf aktualisiert. Der Chrome-Browser wird Ihre APP Rendering und anzeigen, aber Sie wird in der systemeigenen Ansicht angezeigt und enthält nicht die normale Browser Adressleiste usw. Sie können Speicher, Cache und Sitzungen mit dem Browser freigeben. Dabei handelt es sich im Wesentlichen um das Installieren einer Verknüpfung mit dem Chrome-Browser in einem speziellen Modus. PWAs kann auch im Google Play Store mit vertrauenswürdiger Webaktivität aufgeführt werden.

PWAs-und Hybrid-Web-Apps sind einer nativen Android-App sehr ähnlich:

- Kann über den App Store installiert werden (Google Play Store und/oder Microsoft Store)
- Zugriff auf native Gerätefunktionen wie Kamera, GPS, Bluetooth, Benachrichtigungen und Kontaktliste
- Offline arbeiten (keine Internetverbindung)

PWAs verfügt auch über einige einzigartige Features:

- Kann direkt aus dem Internet (ohne App Store) auf dem Android-Startbildschirm installiert werden.
- Kann zusätzlich über die Google Play Store [mithilfe einer vertrauenswürdigen Webaktivität](https://css-tricks.com/how-to-get-a-progressive-web-app-into-the-google-play-store/) installiert werden.
- Kann über die Websuche ermittelt oder über einen URL-Link freigegeben werden.
- Verlassen Sie sich auf einen [Dienstmitarbeiter](https://developers.google.com/web/fundamentals/primers/service-workers) , um zu vermeiden, dass nativer Code paketieren muss

Sie benötigen kein Framework, um eine Hybrid-APP oder PWA zu erstellen, aber es gibt einige beliebte Frameworks, die in diesem Handbuch behandelt werden, einschließlich PhoneGap (mit Cordova) und Ionic (mit Cordova oder einem Kondensator, der Angular oder eine Reaktion verwendet).

## <a name="apache-cordova"></a>Apache Cordova

[Apache Cordova](https://cordova.apache.org/) ist ein Open-Source-Framework, das die Kommunikation zwischen Ihrem JavaScript-Code, der in einer nativen [WebView](https://developer.android.com/reference/android/webkit/WebView) und der systemeigenen Android-Plattform lebt [, mithilfe von](https://cordova.apache.org/plugins/?platforms=cordova-android)Plug-ins vereinfacht. Diese Plug-ins machen JavaScript-Endpunkte verfügbar, die von Ihrem Code aufgerufen und zum Aufrufen von systemeigenen Android-Geräte-APIs verwendet werden. Beispiele für Cordova-Plug-ins sind z. b. der Zugriff auf Geräte Dienste wie z. b. Akku Status, Dateizugriff, Vibrations-/Ring Diese Features sind in der Regel nicht für Web-Apps oder Browser verfügbar.

Es gibt zwei beliebte Verteilungen von Cordova:

- [PhoneGap](https://blog.phonegap.com/update-for-customers-using-phonegap-and-phonegap-build-cc701c77502c): Unterstützung wurde von Adobe eingestellt.

- [Ionic](https://ionicframework.com/)

## <a name="adobe-phonegap"></a>Adobe PhoneGap

Die Unterstützung wurde vor kurzem eingestellt. Weitere Informationen finden Sie in diesem [Blogbeitrag von Adobe](https://blog.phonegap.com/update-for-customers-using-phonegap-and-phonegap-build-cc701c77502c).

### <a name="install-phonegap"></a>Installieren von PhoneGap

Zum Einstieg in die Entwicklung einer PWA-oder Hybrid-Web-App mit PhoneGap sollten Sie zunächst die folgenden Tools installieren:

- Node.js für die Interaktion mit dem Ionic-Ökosystem. [Laden Sie nodejs für Windows herunter](https://nodejs.org/en/) , oder befolgen Sie das [nodejs-Installationshandbuch](../nodejs/setup-on-wsl2.md) unter Verwendung des Windows-Subsystems für Linux (WSL). Möglicherweise möchten Sie die Verwendung von [Node-Version-Manager (NVM)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) in Erwägung gezogen, wenn Sie mit mehreren Projekten und der Version von nodejs arbeiten.

Installieren Sie PhoneGap, indem Sie Folgendes in der Befehlszeile eingeben:

```bash
npm install -g phonegap
```

Um ein neues PhoneGap-Projekt zu erstellen, befolgen Sie die Schritte für den [Einstieg](https://phonegap.com/getstarted/). Besuchen Sie den Abschnitt [PWA-Funktionen](http://stage.docs.phonegap.com/tutorials/stockpile/911-pwa-features/) der PhoneGap-Dokumentation, um zu erfahren, wie Sie Ihre APP von einer Hybriden zu einer PWA migrieren.  

## <a name="ionic"></a>Ionic

[Ionic](https://ionicframework.com/) ist ein Framework, das die Benutzeroberfläche (UI) Ihrer APP so anpasst, dass Sie mit der Entwurfs Sprache der einzelnen Plattformen (Android, Ios, Windows) identisch ist. Ionic ermöglicht Ihnen die Verwendung von [Angular](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro) oder der [Reaktion](https://ionicframework.com/react).

> [!NOTE]
> Es gibt eine neue Version von Ionic, die eine Alternative zu Cordova namens " [Kondensator](https://capacitor.ionicframework.com/)" verwendet. Diese Alternative verwendet Container, um Ihre APP [webfreundlicher](https://ionicframework.com/blog/announcing-capacitor-1-0/)zu gestalten.

### <a name="get-started-with-ionic-by-installing-required-tools"></a>Beginnen Sie mit Ionic, indem Sie erforderliche Tools installieren.

Zum Einstieg in die Entwicklung einer PWA-oder Hybrid-Web-App mit Ionic sollten Sie zunächst die folgenden Tools installieren:

- Node.js für die Interaktion mit dem Ionic-Ökosystem. [Laden Sie nodejs für Windows herunter](https://nodejs.org/en/) , oder befolgen Sie das [nodejs-Installationshandbuch](../nodejs/setup-on-wsl2.md) unter Verwendung des Windows-Subsystems für Linux (WSL). Möglicherweise möchten Sie die Verwendung von [Node-Version-Manager (NVM)](../nodejs/setup-on-wsl2.md#install-nvm-nodejs-and-npm) in Erwägung gezogen, wenn Sie mit mehreren Projekten und der Version von nodejs arbeiten.

- VS Code zum Schreiben von Code. [Laden Sie vs Code für Windows herunter](https://code.visualstudio.com/). Möglicherweise möchten Sie auch die [WSL-Remote Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) installieren, wenn Sie Ihre APP mit einer Linux-Befehlszeile erstellen möchten.

- Windows-Terminal zum Arbeiten mit Ihrer bevorzugten Befehlszeilenschnittstelle (Command-Line Interface, CLI). [Installieren Sie das Windows-Terminal von Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

- Git für Versionskontrolle. [Git herunterladen](https://git-scm.com/downloads).

## <a name="create-a-new-project-with-ionic-cordova-and-angular"></a>Erstellen eines neuen Projekts mit Ionic Cordova und Angular

Installieren Sie Ionic und Cordova, indem Sie Folgendes in der Befehlszeile eingeben:

```bash
npm install -g @ionic/cli cordova
```

Erstellen Sie mithilfe der APP-Vorlage "Tabs" eine Ionic-Angular-APP, indem Sie den folgenden Befehl eingeben:

```bash
ionic start photo-gallery tabs
```

Wechseln Sie in den app-Ordner:

```bash
cd photo-gallery
```

Führen Sie die app in Ihrem Webbrowser aus:

```bash
ionic serve
```

Weitere Informationen finden Sie in der [Ionic Cordova Angular](https://ionicframework.com/docs/developer-resources/guides/first-app-v4/intro)-Dokumentation. Besuchen Sie den Abschnitt [Erstellen der Angular-APP zu PWA](https://ionicframework.com/docs/angular/pwa) der Ionic-Dokumente, um zu erfahren, wie Sie Ihre APP von einer Hybriden zu einer PWA migrieren.

## <a name="create-a-new-project-with-ionic-capacitor-and-angular"></a>Erstellen eines neuen Projekts mit Ionic-Kondensator und Angular

Installieren Sie die Ionic-und Cordova-Res, indem Sie Folgendes in der Befehlszeile eingeben:

```bash
npm install -g @ionic/cli native-run cordova-res
```

Erstellen Sie mithilfe der APP-Vorlage "Registerkarten" eine Ionic Angular-APP, und fügen Sie durch Eingabe des Befehls den folgenden Befehl hinzu:

```bash
ionic start photo-gallery tabs --type=angular --capacitor
```

Wechseln Sie in den app-Ordner:

```bash
cd photo-gallery
```

Fügen Sie Komponenten hinzu, um die APP zu einer PWA zu machen:

```bash
npm install @ionic/pwa-elements
```

Importieren @ionic/pwa-elements Sie, indem Sie der Datei Folgendes hinzufügen `src/main.ts` :

```typescript
import { defineCustomElements } from '@ionic/pwa-elements/loader';

// Call the element loader after the platform has been bootstrapped
defineCustomElements(window);
```

Führen Sie die app in Ihrem Webbrowser aus:

```bash
ionic serve
```

Weitere Informationen finden Sie in der Angular-Dokumentation für das [Ionic-Kondensator](https://ionicframework.com/docs/angular/your-first-app). Besuchen Sie den Abschnitt [Erstellen der Angular-APP zu PWA](https://ionicframework.com/docs/angular/pwa) der Ionic-Dokumente, um zu erfahren, wie Sie Ihre APP von einer Hybriden zu einer PWA migrieren.  

## <a name="create-a-new-project-with-ionic-and-react"></a>Erstellen Sie ein neues Projekt mit Ionic, und reagieren Sie

Installieren Sie die Ionic-CLI, indem Sie Folgendes in der Befehlszeile eingeben:

```bash
npm install -g @ionic/cli
```

Erstellen Sie ein neues Projekt mit reagieren, indem Sie den folgenden Befehl eingeben:

```bash
ionic start myApp blank --type=react
```

Wechseln Sie in den app-Ordner:

```bash
cd myApp
```

Führen Sie die app in Ihrem Webbrowser aus:

```bash
ionic serve
```

Weitere Informationen finden Sie unter [Ionic](https://ionicframework.com/docs/react/quickstart)-Dokumentation. Weitere Informationen zum Verschieben Ihrer APP aus einer Hybriden zu einer PWA finden Sie im Abschnitt " [Erstellen ihrer Reaktion-app in einer PWA](https://ionicframework.com/docs/react/pwa) " der Ionic-Dokumente.

## <a name="test-your-ionic-app-on-a-device-or-emulator"></a>Testen Ihrer Ionic-App auf einem Gerät oder Emulator

Um Ihre Ionic-App auf einem Android-Gerät zu testen, müssen Sie Ihr Gerät einbinden ([Stellen Sie sicher, dass es zuerst für die Entwicklung aktiviert ist](emulator.md#enable-your-device-for-development)), und geben Sie dann in der Befehlszeile Folgendes ein:

```bash
ionic cordova run android
```

Um Ihre Ionic-App auf einem Android-Geräteemulator zu testen, müssen Sie folgende Schritte ausführen:

1. [Installieren Sie die erforderlichen Komponenten-Java Development Kit (JDK), gradle und die Android SDK](https://cordova.apache.org/docs/en/latest/guide/platforms/android/#installing-the-requirements).

2. [Erstellen Sie ein virtuelles Android-Gerät (AVD)](https://developer.android.com/studio/run/managing-avds.html).

3. Geben Sie den Befehl für Ionic ein, um die APP zu erstellen und auf dem Emulator bereitzustellen: `ionic cordova emulate [<platform>] [options]` . In diesem Fall sollte der Befehl wie folgt lauten:

```bash
ionic cordova emulate android --list
```

Weitere Informationen finden Sie im [Cordova-Emulator](https://ionicframework.com/docs/cli/commands/cordova-emulate) in der Ionic-Dokumentation.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Entwickeln Sie Dual-Screen-Apps für Android, und erhalten Sie das Surface Duo-Geräte-SDK](/dual-screen/android/)

- [Windows Defender-Ausschlüsse zum Verbessern der Leistung hinzufügen](defender-settings.md)

- [Virtualisierungsunterstützung zur Verbesserung der emulatorleistung aktivieren](emulator.md#enable-virtualization-support)
