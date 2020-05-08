---
title: Ausführen eines Android-Geräts oder-Emulators unter Windows
description: Testen Sie Ihre APP auf einem Android-Gerät oder-Emulator von Windows aus, und aktivieren Sie die Virtualisierung mit Hyper-v und Windows Hypervisor Platform (whpx).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Emulator, Virtual Device, Device Setup, Enable Device, Developer, Configuration, Virtualization, Visual Studio, Hyper-v, Intel, haxm, AMD, Windows Hypervisor Platform, whpx
ms.date: 04/28/2020
ms.openlocfilehash: c651661d573695902368ffa595ce5d3014791a9a
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255154"
---
# <a name="test-on-an-android-device-or-emulator"></a>Testen auf einem Android-Gerät oder-Emulator

Es gibt mehrere Möglichkeiten zum Testen und Debuggen der Android-Anwendung mit einem echten Gerät oder Emulator auf Ihrem Windows-Computer. Wir haben einige Empfehlungen in dieser Anleitung erläutert.

## <a name="run-on-a-real-android-device"></a>Auf einem echten Android-Gerät ausführen

Um Ihre APP auf einem echten Android-Gerät auszuführen, müssen Sie zunächst Ihr Android-Gerät für die Entwicklung aktivieren. Entwickler Optionen unter Android wurden standardmäßig ausgeblendet, da Version 4,2 und deren Aktivierung je nach Android-Version unterschiedlich sein kann.

### <a name="enable-your-device-for-development"></a>Aktivieren Ihres Geräts für die Entwicklung

Für ein Gerät, auf dem eine aktuelle Version von Android 9.0 und höher ausgeführt wird:

1. Verbinden Sie Ihr Gerät mit Ihrem Windows-Entwicklungs Computer mit einem USB-Kabel. Möglicherweise erhalten Sie eine Benachrichtigung, dass ein USB-Treiber installiert wird.
2. Öffnen Sie den Bildschirm **Einstellungen** auf Ihrem Android-Gerät.
3. Wählen **Sie**Info aus.
4. Scrollen Sie nach unten, und tippen Sie sieben Mal auf **Buildnummer** , bis **Sie nun Entwickler sind!** ist sichtbar.
5. Kehren Sie zum vorherigen Bildschirm zurück, und wählen Sie **System**aus.
6. Wählen Sie **erweitert**aus, Scrollen Sie nach unten, und tippen Sie auf **Entwickler Optionen**.
7. Scrollen Sie im Fenster **Entwickler Optionen** nach unten, um das **USB-Debugging**zu suchen und zu aktivieren.

Ein Gerät, auf dem eine ältere Version von Android ausgeführt wird, finden [Sie unter Einrichten des Geräts für die Entwicklung](https://docs.microsoft.com/xamarin/android/get-started/installation/set-up-device-for-development).

### <a name="run-your-app-on-the-device"></a>Ausführen der APP auf dem Gerät

1. Wählen Sie auf der Android Studio Symbolleiste im Dropdown Menü **Lauf Konfigurationseinstellungen** Ihre APP aus.

    ![Menü "Android Studio ausführen"](../images/android-run-config-menu.png)

2. Wählen Sie im Dropdown Menü **Zielgerät** das Gerät aus, auf dem Sie Ihre APP ausführen möchten.

    ![Menü "Android Studio Zielgerät"](../images/android-target-device-menu.png)

3. Wählen Sie ▷ ausführen aus. Dadurch wird die APP auf dem verbundenen Gerät gestartet.

## <a name="run-your-app-on-a-virtual-android-device-using-an-emulator"></a>Ausführen der APP auf einem virtuellen Android-Gerät mithilfe eines Emulators

Der erste Aspekt beim Ausführen eines Android-Emulators auf Ihrem Windows-Computer besteht darin, dass unabhängig von Ihrer IDE (Android Studio, Visual Studio usw.) die Leistung des Emulators durch Aktivieren der Virtualisierungsunterstützung erheblich verbessert wird.

### <a name="enable-virtualization-support"></a>Virtualisierungsunterstützung aktivieren

Bevor Sie ein virtuelles Gerät mit dem Android-Emulator erstellen, empfiehlt es sich, die Virtualisierung durch Aktivieren der Hyper-V-und Windows Hypervisor Platform (whpx)-Funktionen zu aktivieren. Dadurch kann der Prozessor des Computers die Ausführungsgeschwindigkeit des Emulators erheblich verbessern.

> Zum Ausführen der Hyper-V-und Windows-hypervisorplattform muss Ihr Computer Folgendes ausführen:
>
> * 4 GB Arbeitsspeicher verfügbar
> * Einen 64-Bit-Intel-Prozessor oder eine AMD-ryzen-CPU mit Second Level Address Translation (slat)
> * Ausführen von Windows 10 Build 1803 + ([Überprüfen Sie den Build #](ms-settings:about))
> * Über aktualisierte Grafiktreiber verfügen (Geräte-Manager > Adapter > Update-Treiber anzeigen)
>
> Wenn Ihr Computer diese Kriterien nicht erfüllt, können Sie möglicherweise [Intel haxm](https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows) oder [AMD Hypervisor](https://github.com/google/android-emulator-hypervisor-driver-for-amd-processors)ausführen. Weitere Informationen finden Sie im folgenden Artikel: [Hardware Beschleunigung für die emulatorleistung](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/hardware-acceleration) oder die [Android Studio Emulator-Dokumentation](https://developer.android.com/studio/run/emulator).

1. Vergewissern Sie sich, dass die Computer Hardware und Software mit Hyper-V kompatibel ist, indem Sie eine Eingabeaufforderung öffnen und den folgenden Befehl eingeben:`systeminfo`

    ![Hyper-V-Anforderungen von System Info an der Eingabeaufforderung](../images/systeminfo.png)

2. Geben Sie im Windows-Suchfeld (unten links) "Windows-Funktionen" ein. Wählen Sie in den Suchergebnissen die Option Windows-Funktionen ein- **oder ausschalten** aus.

3. Nachdem die Liste mit den **Windows-Features** angezeigt wird, Scrollen **Sie zu** **Hyper-V** suchen (enthält sowohl Verwaltungs Tools und Plattform) als auch **Windows Hypervisor Platform**. Stellen Sie sicher, dass das Kontrollkästchen aktiviert ist, um beides zu aktivieren.

4. Starten Sie den Computer neu, wenn Sie dazu aufgefordert werden.

### <a name="emulator-for-native-development-with-android-studio"></a>Emulator für die native Entwicklung mit Android Studio

Beim entwickeln und Testen einer nativen Android-App empfiehlt es sich, [Android Studio zu verwenden](./native-android.md). Sobald Ihre APP zum Testen bereit ist, können Sie Ihre APP erstellen und ausführen, indem Sie Folgendes ausführen:

1. Wählen Sie auf der Android Studio Symbolleiste im Dropdown Menü **Lauf Konfigurationseinstellungen** Ihre APP aus.

    ![Menü "Android Studio ausführen"](../images/android-run-config-menu.png)

2. Wählen Sie im Dropdown Menü **Zielgerät** das Gerät aus, auf dem Sie Ihre APP ausführen möchten.

    ![Menü "Android Studio Zielgerät"](../images/android-target-device-menu.png)

3. Wählen Sie ▷ ausführen aus. Dadurch wird der [Android-Emulator](https://developer.android.com/studio/run/emulator)gestartet.

> [!TIP]
> Nachdem die APP auf dem Emulatorgerät installiert wurde, können Sie [Änderungen](https://developer.android.com/studio/run#apply-changes) übernehmen verwenden, um bestimmte Code-und Ressourcen Änderungen bereitzustellen, ohne ein neues APK zu entwickeln.

### <a name="emulator-for-cross-platform-development-with-visual-studio"></a>Emulator für plattformübergreifende Entwicklung mit Visual Studio

Für Windows-PCs sind viele [Android-Emulatoroptionen](https://www.androidauthority.com/best-android-emulators-for-pc-655308/) verfügbar. Es wird empfohlen, den [Android-Emulator](https://developer.android.com/studio/run/emulator)von Google zu verwenden, da er Zugriff auf die neuesten Android-Betriebssystem Images und-Google Play Dienste bietet.

### <a name="install-android-emulator-with-visual-studio"></a>Android-Emulator mit Visual Studio installieren

1. Wenn Sie es noch nicht installiert haben, laden Sie [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)herunter. Verwenden Sie die Visual Studio-Installer, um [Ihre Workloads zu ändern](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019#modify-workloads) und sicherzustellen, dass Sie über die **Arbeitsauslastung Mobile-Entwicklung mit .net**verfügen.

2. Erstellen Sie ein neues Projekt. Nachdem Sie [die Android-Emulator eingerichtet](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/)haben, können Sie die [Android Device Manager](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#requirements) verwenden, um eine Vielzahl von virtuellen Android-Geräten zu erstellen, zu duplizieren, anzupassen und zu starten. Starten Sie die Android Device Manager im Menü Extras mit: **Tools** > **Android** > **Android Device Manager**.

3. Wenn das Android Device Manager geöffnet ist, wählen Sie **+ neu** aus, um ein neues Gerät zu erstellen.

4. Sie müssen dem Gerät einen Namen geben, den Basistyp des Geräts in einem Dropdown Menü auswählen, einen Prozessor und eine Betriebssystemversion sowie mehrere andere Variablen für das virtuelle Gerät auswählen. Weitere Informationen [Android Device Manager Hauptbildschirm](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager?tabs=windows&pivots=windows#main-screen).

5. Wählen Sie in der Visual Studio-Symbolleiste zwischen **Debuggen** (wird an den Anwendungsprozess angefügt, der im Emulator ausgeführt wird, nachdem die APP gestartet wurde) oder im **Releasemodus** (deaktiviert den Debugger). Wählen Sie dann im Dropdown Menü Gerät ein virtuelles Gerät aus, und wählen Sie die **Wiedergabe** Schaltfläche ▷, um die Anwendung im Emulator auszuführen.

    ![Visual Studio-Start Android-Emulator](../images/vs-target-device-menu.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Entwickeln Sie Dual-Screen-Apps für Android, und erhalten Sie das Surface Duo-Geräte-SDK](https://docs.microsoft.com/dual-screen/android/)

- [Windows Defender-Ausschlüsse zum Verbessern der Leistung hinzufügen](defender-settings.md)
