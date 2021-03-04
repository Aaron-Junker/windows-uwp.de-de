---
title: Systemeigene Reaktion auf die Android-Entwicklung unter Windows
description: Eine Schritt-für-Schritt-Anleitung für die ersten Schritte bei der Verwendung von "in Windows reagieren" zum Erstellen einer plattformübergreifenden APP, die auf Android-Geräten funktioniert.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, System eigenes System, Emulator, Expo, Metro Bundler, Terminal
ms.date: 04/28/2020
ms.openlocfilehash: 82bf078d6c29c8968ce3e0cc19ce4d6f803e6d71
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823224"
---
# <a name="get-started-developing-for-android-using-react-native"></a>Einstieg in die Entwicklung für Android mithilfe von "auf systemeigenen

Dieser Leitfaden unterstützt Sie bei den ersten Schritten mit der Verwendung von "systemeigene übersetzen" unter Windows zum Erstellen einer plattformübergreifenden APP, die auf Android-Geräten funktioniert.

## <a name="overview"></a>Übersicht

Die systemeigene Reaktion ist ein [Open Source-](https://github.com/facebook/react-native) Framework für mobile Anwendungen, das von Facebook erstellt wurde. Sie wird verwendet, um Anwendungen für Android, Ios, Web und UWP (Windows) zu entwickeln, die native UI-Steuerelemente und vollständigen Zugriff auf die Native Plattform bereitstellen. Das Arbeiten mit "systemeigenen reagieren" erfordert Kenntnisse der JavaScript-Grundlagen.

## <a name="get-started-with-react-native-by-installing-required-tools"></a>Beginnen Sie mit der systemeigenen Reaktion, indem Sie erforderliche Tools installieren.

1. [Installieren Sie Visual Studio Code](https://code.visualstudio.com) (oder den Code-Editor Ihrer Wahl).

2. [Installieren Sie Android Studio für Windows](https://developer.android.com/studio). Android Studio installiert standardmäßig die neueste Android SDK. "Systemeigene Reaktion" erfordert Android 6,0 (Marshmallow) SDK oder höher. Es wird empfohlen, das neueste SDK zu verwenden.

3. Erstellen Sie Umgebungsvariablen Pfade für das Java SDK und Android SDK:
    - Geben Sie im Fenster "Windows-Suche" Folgendes ein: "Edit the System Environment Variables". Daraufhin wird das Fenster **Systemeigenschaften** geöffnet.
    - Wählen Sie **Umgebungsvariablen** aus..., und wählen Sie dann unter **Benutzervariablen** die Option **neu...** aus.
    - Geben Sie den Variablennamen und den Wert (Pfad) ein. Die Standard Pfade für die Java-und Android-sdche lauten wie folgt. Wenn Sie einen bestimmten Speicherort für die Installation der Java-und Android-sdche ausgewählt haben, achten Sie darauf, die Variablen Pfade entsprechend zu aktualisieren.
        - JAVA_HOME: c:\programme\android\android studio\jre\jre
        - ANDROID_HOME: c:\users\username\appdata\local\android\sdk

    ![Screenshot: Hinzufügen eines Umgebungsvariablen Pfads](../images/add-environmental-variable-path.png)

4. [Installieren von nodejs für Windows](https://nodejs.org/en/) Möglicherweise möchten Sie die Verwendung von [Node-Version-Manager (NVM) für Windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) in Erwägung gezogen, wenn Sie mit mehreren Projekten und der Version von nodejs arbeiten werden. Es wird empfohlen, die neueste LTS-Version für neue Projekte zu installieren.

> [!NOTE]
> Sie sollten auch die Installation und Verwendung des Windows- [Terminals](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) zum Arbeiten mit Ihrer bevorzugten Befehlszeilenschnittstelle (Command-Line Interface, CLI) und [git für die Versionskontrolle](https://git-scm.com/downloads)in Erwägung haben. Das [Java JDK](https://www.oracle.com/java/technologies/javase-downloads.html) ist mit Android Studio v 2.2 und höher verpackt, aber wenn Sie Ihr JDK separat von Android Studio aktualisieren müssen, verwenden Sie den [Windows x64-Installer](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html).

## <a name="create-a-new-project-with-react-native"></a>Erstellen eines neuen Projekts mit "systemeigene Reaktion"

1. Verwenden Sie NPM, um [das Befehlszeilen](https://docs.expo.io/versions/latest/) -Hilfsprogramm der Zertifizierungsstelle von der Windows-Eingabeaufforderung, PowerShell, dem [Windows-Terminal](https://www.microsoft.com/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab)oder dem integrierten Terminal in vs Code (> integriertes Terminal anzeigen) zu installieren.

    ```powershell
    npm install -g expo-cli
    ```

2. Verwenden Sie die Expo zum Erstellen einer systemeigenen APP, die unter IOS, Android und Web ausgeführt wird. Sie müssen dann zwischen den Projektvorlagen wählen, die **leer**, **leer (typescript)**, Tabstopps (z. b. Bildschirme mit Reaktions Navigation **),** **Minimal** oder **minimal (typescript)** enthalten.

    ```powershell
    expo init my-new-app
    ```

    > [!NOTE]
    > Wenn Sie verwenden `npx create-react-native-app` , funktioniert dies weiterhin, aber die "Expo-CLI"-init bietet [einige zusätzliche Vorteile](https://github.com/react-native-community/discussions-and-proposals/issues/23).

3. Öffnen Sie das neue Verzeichnis "My-New-app":

    ```powershell
    cd my-new-app
    ```

4. Um das Projekt auszuführen, geben Sie den folgenden Befehl ein. Dadurch wird ein localhost-Fenster in Ihrem Standard Internetbrowser geöffnet, in dem Node Metro Bundler angezeigt wird. Außerdem wird ein QR-Code sowohl in der Befehlszeile als auch im Fenster "Metro Bundler Browser" angezeigt. * Sie können auch den Befehl verwenden: `npm start` oder `npm run android` .

     ```powershell
    expo start
    ```

    ![Screenshot von Metro Bundler im Browser](../images/metro-bundler.png)

5. Zum Anzeigen des Projekts, das auf einem Android-Gerät ausgeführt wird, müssen Sie zunächst [die Client-App für die Expo mit dem Google Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent&hl=en_US) auf Ihrem Android-Gerät installieren. Nachdem die Expo-Client-App installiert ist, öffnen Sie Sie auf Ihrem Gerät, und wählen Sie **QR-Code Scannen** aus. Sobald der QR-Code registriert ist, können Sie sehen, dass das Paket sowohl auf Ihrem Gerät als auch im Fenster "Metro Bundler", das auf "localhost" in Ihrem Browser ausgeführt wird, erstellt wird.

6. Zum Anzeigen des Projekts, das auf einem Android-Emulator ausgeführt wird, müssen Sie zunächst Android Studio öffnen und dann ein virtuelles Gerät erstellen und starten. **Tools**  >  **AVD-Manager**  >  **[+ Virtuelles Gerät erstellen...](https://developer.android.com/studio/run/managing-avds#createavd)** Nachdem das virtuelle Gerät erstellt wurde, wählen Sie die Schaltfläche "Start" ▷ in der Spalte " **Aktionen** " des virtuellen Android-Device Manager aus, um mit der Emulation des Geräts zu beginnen. Sobald das virtuelle Gerät geöffnet ist, kehren Sie zum Fenster "Metro Bundler" zurück, das in Ihrem Internetbrowser Fenster ausgeführt wird, und wählen Sie "auf Android-Gerät/Emulator ausführen" in der linken Spalte aus. Es sollte ein Popup Fenster angezeigt werden, in dem Sie wissen, dass Metro Bundler versucht, einen Simulator zu öffnen... und dann sehen Sie, dass die Expo-Client-app in Ihrem emulierten Android-Gerät geöffnet ist. Sobald der Download des JavaScript-Pakets abgeschlossen ist, wird die Anzeige der systemeigenen app "reagieren" angezeigt. (Wenn Probleme auftreten, [Überprüfen Sie die Dokumentation zum Android-Emulator von Expo](https://docs.expo.io/workflow/android-studio-emulator/).)

7. Öffnen Sie das Native Projekt Ihrer Anwendung, um mit der Arbeit an Ihrer APP zu beginnen. Ihre Änderungen sollten automatisch in der App angezeigt werden, die über den Expo-Client auf Ihrem Gerät oder in Ihrem Android-Emulator ausgeführt wird.

8. Versuchen Sie, den Ansichts Text der Landing Page in "Hallo Welt!" zu ändern. Dies können Sie in der IDE Ihrer Wahl tun. (Wir empfehlen vs Code oder Android Studio.) Die Datei für die Landing Page unterscheidet sich abhängig von der ausgewählten Vorlage. Der Wert kann `App.js` , `App.tsx` oder sein `HomeScreen.js` .

    ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
        </View>
      );
    }
    ```

9. Versuchen Sie, ein Bild hinzuzufügen. Zunächst müssen Sie einen Ordner auf der gleichen Ebene wie die Ordner "Android" und "IOS" in Ihrer APP erstellen. wir nennen ihn "Images". Platzieren Sie ein Bild in diesem Ordner, `your-image.png` z. b.. Verwenden Sie das folgende Format, um auf das Bild zu verweisen und es mit einer Höhe und einer Breite zu formatieren.

     ```typescript
    export default function App() {
      return (
        <View style={styles.container}>
          <Text>Hello World!</Text>
          <Image source={require('./images/your-image.png')} style = {{height: 200, width: 250, }} />
        </View>
      );
    }
    ```

> [!TIP]
> Wenn Sie die Unterstützung für Ihre systemeigene systemeigenen APP so hinzufügen möchten, dass Sie als Windows 10-app ausgeführt wird, lesen Sie die Dokumentation zu den ersten Schritten [mit](https://microsoft.github.io/react-native-windows/docs/getting-started) der systemeigenen Windows-Dokumentation.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Entwickeln Sie Dual-Screen-Apps für Android, und erhalten Sie das Surface Duo-Geräte-SDK](/dual-screen/android/)

- [Windows Defender-Ausschlüsse zum Verbessern der Leistung hinzufügen](defender-settings.md)

- [Virtualisierungsunterstützung zur Verbesserung der emulatorleistung aktivieren](emulator.md#enable-virtualization-support)