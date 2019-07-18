---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Verpacken von UWP-Apps
description: Um Ihre UWP-App (Universelle Windows-Plattform) zu vertreiben und zu verkaufen, müssen Sie ein App-Paket erstellen.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, UWP
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 1038913732c8b25a5baa3daf2c04176ff747a85f
ms.sourcegitcommit: 2062d06567ef087ad73507a03ecc726a7d848361
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303595"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Verpacken einer UWP-App mit Visual Studio

Um Ihre Universelle Windows Plattform (UWP)-App zu verkaufen oder an andere Benutzer zu verteilen, müssen Sie es verpacken. Wenn Sie Ihre App nicht über den Microsoft Store verteilen möchten, können Sie das App-Paket direkt auf einem Gerät querladen oder über [Web Install](installing-UWP-apps-web.md) verteilen. In diesem Artikel wird das Konfigurieren, Erstellen und Testen von UWP-App-Paketen mit Visual Studio beschrieben. Weitere Informationen zum Verwalten und Bereitstellen von branchenspezifischen Apps (Line-of-Business, LOB) finden Sie unter [Enterprise-App-Verwaltung](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

In Windows 10 können Sie ein App-Paket, App Bundle oder eine komplette Uploaddatei für das App-Paket an [Partner Center](https://partner.microsoft.com/dashboard)übermitteln. Diese Optionen bieten das Senden einer APP-Paket-Uploaddatei.

## <a name="types-of-app-packages"></a>App-Pakettypen

- **App-Paket (. AppX oder. msix)**  
    Eine Datei, die Ihre App in einem Format enthält, das auf einem Gerät quergeladen werden kann. Jede einzelne APP-Paketdatei, die von Visual Studio erstellt wurde, soll **nicht** an Partner Center übermittelt werden und sollte nur für Sideload und Testzwecke verwendet werden. Wenn Sie Ihre APP an Partner Center übermitteln möchten, verwenden Sie die Uploaddatei des App-Pakets.  

- **App-Bundle (. appxbundle oder. msixbundle)**  
    Ein App-Bündel ist ein Pakettyp, der mehrere App-Pakete enthalten kann, von denen jedes so erstellt wurde, dass es eine bestimmte Gerätearchitektur unterstützt. Beispielsweise kann ein App-Bündel drei separate App-Pakete für die Konfigurationen x86, x64 und ARM enthalten. App-Bündel sollten nach Möglichkeit generiert werden, da sie ermöglichen, dass Ihre App auf den verschiedensten Geräten verfügbar ist.  

- **App-Paket-Uploaddatei (. appxupload oder. msixupload)**  
    Eine einzelne Datei, die mehrere App-Pakete oder ein App-Bündel zur Unterstützung verschiedener Prozessorarchitekturen enthalten kann. Die Uploaddatei des App-Pakets enthält auch eine Symbol Datei zum [Analysieren der APP-Leistung](https://docs.microsoft.com/windows/uwp/publish/analytics) , nachdem Ihre APP in der Microsoft Store veröffentlicht wurde. Diese Datei wird automatisch erstellt, wenn Sie Ihre APP mit Visual Studio verpacken, um Sie zur Veröffentlichung an Partner Center zu senden.

Hier sehen Sie eine Übersicht über die Schritte zum Vorbereiten und Erstellen eines App-Pakets:

1.  [Vor dem Verpacken der App](#before-packaging-your-app). Führen Sie diese Schritte aus, um sicherzustellen, dass Ihre APP für die Partner Center-Übermittlung bereit ist.
2.  [Konfigurieren eines App-Pakets](#configure-an-app-package). Verwenden Sie den Visual Studio Manifest-Designer, um das Paket zu konfigurieren. Fügen Sie beispielsweise Kachelbilder hinzu, und wählen Sie die von Ihrer App unterstützten Ausrichtungen aus.
3.  [Erstellen einer App-Paketuploaddatei](#create-an-app-package-upload-file). Verwenden Sie den Visual Studio App-Verpackungsassistenten, um ein App-Paket zu erstellen. Zertifizieren Sie dann Ihr Paket mit dem Zertifizierungskit für Windows-Apps.
4.  [Querladen des App-Pakets](#sideload-your-app-package). Nach dem Querladen Ihrer App auf ein Gerät können Sie testen, ob sie erwartungsgemäß funktioniert.

Nachdem Sie die vorangehenden Schritte abgeschlossen haben, können Sie Ihre App verteilen. Wenn Sie über eine Branchen-App verfügen, die Sie nicht verkaufen möchten, da Sie nur für interne Benutzer vorgesehen ist, können Sie diese APP querladen, um Sie auf jedem Windows 10-Gerät zu installieren.

## <a name="before-packaging-your-app"></a>Vor dem Verpacken der App

1.  **Testen Sie Ihre APP.** Bevor Sie Ihre APP für die Partner Center-Übermittlung Verpacken, müssen Sie sicherstellen, dass Sie auf allen Gerätefamilien, die Sie unterstützen möchten, erwartungsgemäß funktioniert. Diese Gerätefamilien umfassen Desktop-, Mobile-, Surface Hub-, Xbox-, IoT- und andere Geräte. Weitere Informationen zum Bereitstellen und Testen Ihrer App mithilfe von Visual Studio finden Sie unter Bereitstellen [und Debuggen von UWP-apps](../debug-test-perf/deploying-and-debugging-uwp-apps.md).
2.  **Optimieren Sie Ihre APP.** Sie können die Profilerstellungs- und Debugtools von Visual Studio verwenden, um die Leistung Ihrer UWP-App zu optimieren. Zu diesen Tools gehören das Zeitachsentool für „Reaktionsfähigkeit der Benutzeroberfläche“, das Speichernutzungstool, das CPU-Auslastungstool und viele mehr. Weitere Informationen zur Verwendung dieser Tools finden Sie im Thema [Profilerstellungsfeature-Tour](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour).
3.  **Überprüfen Sie die .net Native Kompatibilität ( C# für VB und Apps).** Mit der Universelle Windows-Plattform wurde ein neuer systemeigener Compiler eingeführt, der die Laufzeitleistung Ihrer App verbessert. Diese Änderung macht es erforderlich, dass Sie Ihre App in dieser Kompilierungsumgebung testen. Standardmäßig aktiviert die **Release**-Buildkonfiguration die .NET Native-Toolkette. Daher ist es wichtig, die App und das erwartete Verhalten mit dieser **Release**-Konfiguration zu testen. Einige häufige Debugprobleme, die bei .NET Native auftreten können, werden [Native .NET Windows Universal Apps debuggen](https://devblogs.microsoft.com/devops/debugging-net-native-windows-universal-apps/) ausführlich erläutert.

## <a name="configure-an-app-package"></a>Konfigurieren eines App-Pakets

Die App-Manifestdatei (Package.appxmanifest.xml) ist eine XML-Datei, die über die Eigenschaften und Einstellungen verfügt, die für die Erstellung des App-Pakets erforderlich sind. Die Eigenschaften in der App-Manifestdatei beschreiben z. B. das Bild, das als App-Kachel verwendet wird, und die Ausrichtungen, die von der App beim Drehen des Geräts unterstützt werden.

Visual Studio verfügt über einen Manifest-Designer, mit dem Sie die Manifestdatei ohne Bearbeitung der XML-Rohdaten der Datei aktualisieren können.

### <a name="configure-a-package-with-the-manifest-designer"></a>Konfigurieren eines Pakets mit dem Manifest-Designer

1.  Erweitern Sie im **Projektmappen-Explorer** den Projektknoten Ihrer UWP-App.
2.  Doppelklicken Sie auf die Datei **Package.appxmanifest**. Wenn die Manifestdatei bereits in der XML-Codeansicht geöffnet ist, werden Sie von Visual Studio zum Schließen der Datei aufgefordert.
3.  Jetzt können Sie entscheiden, wie Sie Ihre App konfigurieren möchten. Jede Registerkarte enthält Informationen, die Sie für Ihre App konfigurieren können, sowie Links zu weiteren Informationen, wenn notwendig.  
    ![Manifest-Designer in Visual Studio](images/packaging-screen1.jpg)

    Überprüfen Sie auf der Registerkarte **Visuelle Anlagen**, ob Sie über alle Bilder verfügen, die für eine UWP-App erforderlich sind.

    Auf der Registerkarte **Verpacken** können Sie Veröffentlichungsdaten eingeben. An dieser Stelle können Sie auswählen, welches Zertifikat zur Signierung der App verwendet werden soll. Alle UWP-Apps müssen anhand eines Zertifikats signiert werden.

    > [!NOTE]
    > Ab Visual Studio 2019 wird ein temporäres Zertifikat nicht mehr in UWP-Projekten generiert. Um Zertifikate zu erstellen oder zu exportieren, verwenden Sie die in [diesem Artikel](create-certificate-package-signing.md)beschriebenen PowerShell-Cmdlets.

    > [!IMPORTANT]
    > Wenn Sie Ihre App im Microsoft Store veröffentlichen, wird Ihre App mit einem vertrauenswürdigen Zertifikat für Sie signiert. Dadurch kann der Benutzer Ihre App installieren und ausführen, ohne das zugehörige App-Signaturzertifikat zu installieren.

    Wenn Sie Ihre App nicht veröffentlichen und einfach ein App-Paket querladen möchten, müssen Sie zunächst dem Paket vertrauen. Um dem Paket zu vertrauen, muss das Zertifikat auf dem Gerät des Benutzers installiert sein. Weitere Informationen zum Querladen finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

4.  Speichern Sie die Datei **Package.appxmanifest**, nachdem Sie die erforderlichen Bearbeitungsschritte für die App vorgenommen haben.

Wenn Sie Ihre APP über das Microsoft Store verteilen, kann Visual Studio das Paket mit dem Store verknüpfen. Klicken Sie hierzu in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Store**->-**App mit Store verknüpfen**aus. Dies ist auch im Assistenten zum **Erstellen von App-Paketen** möglich, der im folgenden Abschnitt beschrieben wird. Wenn Sie Ihre App zuordnen, werden einige Felder der Registerkarte „Verpacken“ im Manifest-Designer automatisch aktualisiert.

## <a name="create-an-app-package-upload-file"></a>Erstellen einer App-Paketuploaddatei

Um eine APP über Microsoft Store zu verteilen, müssen Sie ein App-Paket (. AppX oder. msix), App Bundle (. appxbundle oder. msixbundle) oder eine Uploaddatei für das App-Paket (. appxupload oder. msixupload) erstellen und [die APP für Partner Center übermitteln](https://docs.microsoft.com/windows/uwp/publish/app-submissions). Obwohl es möglich ist, ein App-Paket oder App Bundle nur an Partner Center zu senden, empfiehlt es sich, eine Uploaddatei für das App-Paket zu übermitteln. Sie können mit dem Assistenten zum **Erstellen von App-Paketen** in Visual Studio eine Uploaddatei für das App-Paket erstellen, oder Sie können Sie manuell aus vorhandenen APP-Paketen oder App-bündeln erstellen.

>[!NOTE]
> Wenn Sie ein App-Paket (. AppX oder. msix) oder App Bundle (. appxbundle oder. msixbundle) manuell erstellen möchten, finden Sie weitere Informationen unter [Erstellen eines App-Pakets mit dem Tool "makeappx. exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)".

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Erstellen der Uploaddatei des App-Pakets mithilfe von Visual Studio

1.  Öffnen Sie im **Projektmappen-Explorer** die Projektmappe für Ihr UWP-App-Projekt.
2.  Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Store**->**App-Pakete erstellen** aus. Wenn diese Option deaktiviert ist oder nicht angezeigt wird, überprüfen Sie, ob es sich beim Projekt um ein Universal Windows-Projekt handelt.  
    ![Kontextmenü mit Navigation zum Erstellen von App-Paketen](images/packaging-screen2.jpg)

    Der Assistent **App-Pakete erstellen** wird angezeigt.

3.  Wählen Sie **Ich möchte Pakete zum Hochladen in die Microsoft Store erstellen mit einem neuen APP-Namen** im ersten Dialogfeld aus, und klicken Sie dann auf **weiter**.  
    ![Dialogfenster "Pakete erstellen" angezeigt](images/packaging-screen3.jpg)

    Wenn Sie Ihr Projekt bereits mit einer APP im Store verknüpft haben, haben Sie auch die Möglichkeit, Pakete für die zugehörige Store-App zu erstellen. Wenn Sie sich **für die Erstellung von Paketen für das Sideload**entscheiden, generiert Visual Studio die APP-Paket Upload-Datei (. msixupload oder. appxupload) nicht für Partner Center-Übermittlungen. Sie können diese Option auswählen, wenn Sie die App lediglich für die Ausführung auf internen Geräten querladen oder für Testzwecke verwenden möchten. Weitere Informationen zum Querladen finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
4.  Melden Sie sich auf der nächsten Seite mit Ihrem Entwicklerkonto bei Partner Center an. Wenn Sie noch kein Entwicklerkonto besitzen, hilft Ihnen der Assistent bei der Erstellung.
    ![Fenster "App-Pakete erstellen" mit der angezeigten App-namens Auswahl](images/packaging-screen4.jpg)
5.  Wählen Sie den APP-Namen für das Paket aus der Liste der Apps aus, die zurzeit für Ihr Konto registriert sind, oder reservieren Sie ein neues, wenn Sie es noch nicht in Partner Center reserviert haben.  
6.  Stellen Sie sicher, dass Sie im Dialogfeld **Auswählen und Konfigurieren von Paketen** alle drei Architekturkonfigurationen (x86, x64 und ARM) auswählen, um zu gewährleisten, dass Ihre App auf einer breiten Palette von Geräten bereitgestellt werden kann. Wählen Sie im Listenfeld **App-Bundle erstellen** die Option **Immer**. Eine APP Bundle (. appxbundle oder. msixbundle) wird für eine einzelne APP-Paketdatei bevorzugt, da Sie eine Sammlung von App-Paketen enthält, die für jeden Typ von Prozessorarchitektur konfiguriert sind. Wenn Sie die APP Bundle generieren, werden die APP Bundle in die endgültige App-Paket Upload-Datei (. appxupload oder. msixupload) zusammen mit den Informationen zum Debuggen und Abstürzen von Abstürzen eingeschlossen. Wenn Sie nicht sicher sind, welche Architektur(en) Sie auswählen sollen, oder wenn Sie mehr darüber erfahren möchten, welche Architekturen von verschiedenen Geräten verwendet werden, finden Sie weitere Informationen unter [App-Paketarchitekturen](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).  
    ![Fenster "App-Pakete erstellen" mit der angezeigten Paketkonfiguration](images/packaging-screen5.jpg)
7.  Schließen Sie vollständige PDB-Symbol Dateien ein, um die [App-Leistung](https://docs.microsoft.com/windows/uwp/publish/analytics) aus Partner Center zu analysieren, nachdem Ihre App veröffentlicht wurde. Konfigurieren Sie zusätzliche Details wie die Versionsnummer oder den Ausgabespeicherort des Pakets.
8.  Klicken Sie zum Erstellen des App-Pakets auf **Erstellen**. Wenn Sie eines der Pakete erstellen möchten, die in die Microsoft Store Optionen in Schritt 3 **hochgeladen** werden sollen, und ein Paket für die Partner Center-Übermittlung erstellen, wird vom Assistenten eine Paket Upload-Datei (. appxupload oder. msixupload) erstellt. Wenn Sie in Schritt 3 Pakete für das querladen **erstellen möchten** , erstellt der Assistent basierend auf Ihrer Auswahl in Schritt 6 entweder ein einzelnes App-Paket oder eine APP Bundle.
9. Wenn Ihre APP erfolgreich verpackt wurde, wird dieses Dialogfeld angezeigt, und Sie können die Uploaddatei des App-Pakets aus dem angegebenen Ausgabe Speicherort abrufen. An diesem Punkt können Sie das [App-Paket auf dem lokalen Computer oder einem Remote Computer über](#validate-your-app-package) prüfen und Store-Übermittlungen [automatisieren](#automate-store-submissions).
    ![Fenster "Paket Erstellung abgeschlossen" mit angezeigten Validierungs Optionen](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>Manuelles Erstellen der Uploaddatei des App-Pakets

1. Platzieren Sie die folgenden Dateien in einem Ordner:
    - Mindestens ein App-Paket (. msix oder. AppX) oder ein App Bundle (. msixbundle oder. appxbundle).
    - Eine appxsym-Datei. Dabei handelt es sich um eine komprimierte PDB-Datei mit öffentlichen Symbolen Ihrer APP, die im Partner Center für [Absturz Analysen](../publish/health-report.md) verwendet wird. Sie können diese Datei weglassen, aber in diesem Fall sind keine Informationen zur Absturz Analyse oder zum Debuggen für Ihre app verfügbar.
2. Zippen Sie den Ordner.
3. Ändern Sie den Namen der ZIP-Datei für die Ordner Erweiterung von. zip in. msixupload oder. appxupload.

## <a name="validate-your-app-package"></a>Überprüfen des App-Pakets

Überprüfen Sie Ihre APP, bevor Sie Sie zur Zertifizierung auf einem lokalen oder Remote Computer an Partner Center übermitteln. Versionsbuilds können nur für Ihr App-Paket, nicht aber für Debugbuilds überprüft werden. Weitere Informationen zum Übermitteln Ihrer APP an Partner Center finden Sie unter [App](https://docs.microsoft.com/windows/uwp/publish/app-submissions)-Übermittlung.

### <a name="validate-your-app-package-locally"></a>Lokales Überprüfen des App-Pakets

1. Lassen Sie auf der Seite abschließende **Paket Erstellung abgeschlossen** des Assistenten **App-Pakete erstellen** die Option **lokaler Computer** ausgewählt, und klicken Sie auf zertifizierungskit für **Windows-apps starten**. Weitere Informationen zum Testen der App mit dem Zertifizierungskit für Windows-Apps finden Sie unter [Zertifizierungskit für Windows-Apps](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

    Das Windows-zertifizierungskit für Apps (Wack) führt verschiedene Tests aus und gibt die Ergebnisse zurück. Weitere Informationen finden Sie unter [Tests des Zertifizierungskits für Windows-Apps](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests).

    Wenn Sie über ein Windows 10-Remote Gerät verfügen, das Sie für den Test verwenden möchten, müssen Sie das zertifizierungskit für Windows-apps manuell auf diesem Gerät installieren. Im nächsten Abschnitt werden die erforderlichen Schritte beschrieben. Nachdem Sie damit fertig sind, wählen Sie **Remotecomputer** und klicken auf **Zertifizierungskit für Windows-Apps starten**, um eine Verbindung zum Remotegerät herzustellen und die Überprüfungen ausführen.

2. Nachdem der Wack-Vorgang abgeschlossen ist und Ihre APP die Zertifizierung übermittelt hat, können Sie Ihre APP an Partner Center übermitteln. Stellen Sie sicher, dass Sie die richtige Datei hochladen. Der Standard Speicherort der Datei befindet sich im Stamm Ordner der `\[AppName]\AppPackages` Projekt Mappe und endet mit der Dateierweiterung ". appxupload" oder ". msixupload". Der Name weist das `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` Format auf, oder `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` Sie haben sich für eine APP Bundle entschieden, bei der die gesamte Paket Architektur ausgewählt ist.

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>Überprüfen des App-Pakets auf einem Windows 10-Remote Gerät

1. Aktivieren Sie das Windows 10-Gerät für die Entwicklung, indem Sie die Anweisungen unter [Aktivieren Ihres Geräts für die Entwicklung](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) befolgen.
    >[!IMPORTANT]
    > Sie können Ihr App-Paket nicht auf einem Arm-Remote Gerät für Windows 10 überprüfen.
2. Laden Sie die Remotetools für Visual Studio herunter, und installieren Sie sie. Diese Tools werden verwendet, um das Zertifizierungskit für Windows-Apps remote auszuführen. Weitere Informationen zu diesen Tools einschließlich der Downloadseite finden Sie unter [Ausführen von UWP-Apps auf einem Remotecomputer](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015).
3. Laden Sie das erforderliche [zertifizierungskit für Windows-apps](https://go.microsoft.com/fwlink/p/?LinkID=309666) herunter, und installieren Sie es auf Ihrem Windows 10-Gerät
4. Aktivieren Sie auf der Seite **Paketerstellung abgeschlossen** des Assistenten das Optionsfeld **Remotecomputer**. Klicken Sie anschließend neben der Schaltfläche **Testverbindung** auf die Schaltfläche mit den Auslassungszeichen.
    >[!NOTE]
    > Die Options Schaltfläche **Remote Computer** ist nur verfügbar, wenn Sie mindestens eine Projektmappenkonfiguration ausgewählt haben, die die Validierung unterstützt. Weitere Informationen zum Testen der App mit dem WACK finden Sie unter [Zertifizierungskit für Windows-Apps](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).
5. Geben Sie ein Gerät vom Subnetz aus an, oder geben Sie den DNS-Namen (Domain Name Server) oder die IP-Adresse eines Geräts an, das sich außerhalb des Subnetzes befindet.
6. Wählen Sie in der Liste **Authentifizierungsmodus** die Option **Keiner** aus, wenn Ihr Gerät keine Anmeldung mittels Windows-Anmeldeinformationen erfordert.
7. Klicken Sie auf die Schaltfläche **Auswählen** und anschließend auf die Schaltfläche **Zertifizierungskit für Windows-Apps starten**. Wenn die Remotetools auf diesem Gerät ausgeführt werden, stellt Visual Studio eine Verbindung mit dem Gerät her und führt die Überprüfungstests aus. Weitere Informationen finden Sie unter [Tests im Zertifizierungskit für Windows-Apps](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests).

## <a name="automate-store-submissions"></a>Automatisieren von Store-Einreichungen

Ab Visual Studio 2019 können Sie die generierte. appxupload-Datei direkt aus der IDE an die Microsoft Store senden, indem Sie am Ende der die Überprüfungs Option **nach Windows-App-zertifizierungskit automatisch an die Microsoft Store senden** auswählen. [Assistent zum Erstellen von App-Paketen](#create-your-app-package-upload-file-using-visual-studio). Diese Funktion nutzt Azure Active Directory für den Zugriff auf die Partner Center-Kontoinformationen, die zum Veröffentlichen Ihrer APP erforderlich sind. Um dieses Feature verwenden zu können, müssen Sie Azure Active Directory Ihrem Partner Center-Konto zuordnen und mehrere für Einsendungen erforderliche Anmelde Informationen abrufen.

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Zuordnen von Azure Active Directory zu Ihrem Partner Center-Konto

Bevor Sie die Anmelde Informationen abrufen können, die für automatische Store-Übermittlungen erforderlich sind, müssen Sie diese Schritte zunächst im [Partner Center-Dashboard](https://partner.microsoft.com/dashboard) ausführen, falls Sie dies noch nicht getan haben.

1. Ordnen [Sie Ihr Partner Center-Konto dem Azure Active Directory Ihrer Organisation zu](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center). Wenn in Ihrer Organisation bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie bereits über Azure AD. Andernfalls können Sie ohne zusätzliche Kosten einen neuen Azure AD Mandanten innerhalb von Partner Center erstellen.

2. [Fügen Sie Ihrem Partner Center-Konto eine Azure AD Anwendung hinzu](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account). Diese Azure AD Anwendung stellt die APP oder den Dienst dar, die Sie für den Zugriff auf Einsendungen für Ihr dev Center-Konto verwenden. Sie müssen diese Anwendung der **Manager** -Rolle zuweisen. Wenn diese Anwendung bereits in Ihrem Azure AD-Verzeichnis vorhanden ist, können Sie sie auf der Seite **Azure AD-Apps hinzufügen** auswählen, um sie Ihrem Dev Center-Konto hinzuzufügen. Andernfalls können Sie eine neue Azure AD-Anwendung auf der Seite **Azure AD-Apps hinzufügen** erstellen.

### <a name="retrieve-the-credentials-required-for-submissions"></a>Abrufen der für Einsendungen erforderlichen Anmelde Informationen

Als nächstes können Sie die für die Übermittlung erforderlichen Partner Center-Anmelde Informationen abrufen: die Azure- **Mandanten** - **ID**, die Client-ID und den **Client Schlüssel**.

1. Wechseln Sie zum [Partner Center-Dashboard](https://partner.microsoft.com/dashboard) , und melden Sie sich mit ihren Azure AD Anmelde Informationen an.

2. Klicken Sie im Partner Center-Dashboard auf das Zahnrad Symbol (in der Nähe der oberen rechten Ecke des Dashboards), und wählen Sie dann **Entwicklereinstellungen**aus.

3. Klicken Sie im Menü " **Einstellungen** " im linken Bereich auf **Benutzer**.

4. Klicken Sie auf den Namen Ihrer Azure AD Anwendung, um zu den Einstellungen der Anwendung zu wechseln. Kopieren Sie auf dieser Seite die Werte für Mandanten- **ID** und **Client-ID** .

5. Klicken Sie im Abschnitt **Schlüssel** auf **neuen Schlüssel hinzufügen**. Kopieren Sie auf dem nächsten Bildschirm den Schlüsselwert, der dem geheimen Client **Schlüssel** entspricht. Sie sind nicht mehr in der Lage, auf diese Informationen zuzugreifen, nachdem Sie diese Seite verlassen haben. Stellen Sie also sicher, dass Sie nicht verloren gehen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-App](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application).

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Konfigurieren von automatischen Store-Übermittlungen in Visual Studio

Nachdem Sie die vorherigen Schritte ausgeführt haben, können Sie die automatischen Store-Übermittlungen in Visual Studio 2019 konfigurieren.

1. Wählen Sie am Ende des [Assistenten App-Pakete erstellen](#create-your-app-package-upload-file-using-visual-studio)die Option **nach der Validierung des Windows-App-zertifizierungskit automatisch an die Microsoft Store senden** aus, **und klicken Sie**auf

2. Geben Sie im Dialogfeld Einstellungen für die **Microsoft Store Übermittlung konfigurieren** die Azure-Mandanten-ID, Client-ID und den Client Schlüssel ein.

    ![Konfigurieren von Einstellungen für die Microsoft Store Übermittlung](images/packaging-screen8.jpg)

    > [!Important]
    > Ihre Anmelde Informationen können in Ihrem Profil gespeichert werden, sodass Sie in zukünftigen Übermittlungen verwendet werden.

3. Klicken Sie auf **OK**.

Die Übermittlung wird gestartet, nachdem der Wack-Test abgeschlossen wurde. Sie können den Übermittlungs Fortschritt im Fenster **überprüfen und veröffentlichen** verfolgen.

![Status überprüfen und veröffentlichen](images/packaging-screen9.jpg)

## <a name="sideload-your-app-package"></a>Querladen des App-Pakets

Mit UWP-App-Paketen werden apps nicht auf einem Gerät installiert, wie es bei Desktop-Apps der Fall ist. In der Regel laden Sie UWP-Apps aus dem Microsoft Store herunter, wodurch die App auch auf Ihrem Gerät installiert wird. Apps können installiert werden, ohne im Store veröffentlicht zu werden (Querladen). Auf diese Weise können Sie Apps mithilfe der APP-Paketdatei, die Sie erstellt haben, installieren und testen. Wenn Sie über eine App verfügen, die Sie nicht im Store anbieten möchten (z. B. eine branchenspezifische App), können Sie diese App querladen, um sie für Kollegen im Unternehmen bereitzustellen.

Bevor Sie Ihre APP auf einem Zielgerät querladen können, müssen Sie [Ihr Gerät für die Entwicklung aktivieren](../get-started/enable-your-device-for-development.md).

Verwenden Sie zum querladen Ihrer APP auf ein Windows 10 Mobile-Gerät das Tool [winappdeploycmd. exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) . Befolgen Sie die nachstehenden Anweisungen für Desktops, Laptops und Tablets.

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>Querladen des App-Pakets auf Windows 10 Anniversary Update oder höher

App-Pakete, die in Windows 10 Anniversary Update (Windows 10, Version 1607) eingeführt wurden, können einfach durch Doppelklicken auf die APP-Paketdatei installiert werden. Um dies zu verwenden, navigieren Sie zu Ihrem App-Paket oder App Bundle Datei, und doppelklicken Sie darauf. Der [APP-Installer](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) wird gestartet und stellt die grundlegenden app-Informationen sowie eine Installations Schaltfläche, eine Installationsstatus Anzeige und alle relevanten Fehlermeldungen bereit.

![App-Installer zeigt eine Beispiel-App namens Contoso für die Installation an](images/appinstaller-screen.png)

> [!NOTE]
> Das App-Installationsprogramm geht davon aus, dass der APP das Gerät vertraut. Wenn Sie eine Entwickler- oder Unternehmens-App querladen, müssen Sie das Signaturzertifikat im Speicher für vertrauenswürdige Personen oder vertrauenswürdige Herausgeber auf dem Gerät installieren. Wenn Sie nicht sicher sind, wie Sie hierzu vorgehen, finden Sie unter [Testzertifikate installieren](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates) weitere Infos.

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>Querladen des App-Pakets auf früheren Versionen von Windows

1.  Kopieren Sie die Ordner der zu installierenden App-Version auf das Zielgerät.

    Wenn Sie ein App-Bündel erstellt haben, verfügen Sie über einen Ordnernamen bestehend aus Versionsnummer und dem Zusatz `*_Test`. Zum Beispiel die beiden folgenden Ordner (wobei die Version 1.0.2.0 installiert wird):

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    Wenn Sie über kein App-Bündel verfügen, können Sie den Ordner kopieren, um die richtige Architektur und den entsprechenden `*_Test`-Ordner zu übernehmen. Die beiden Ordner sind ein Beispiel für ein App-Paket mit der x64 Architektur und seinen `*_Test`-Ordner:

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  Öffnen Sie den `*_Test`-Ordner auf dem Zielgerät.
3.  Mit der rechten Maustaste auf die **Add-AppDevPackage.ps1**-Datei. Wählen Sie **Mit PowerShell ausführen** und befolgen die Anweisungen.  
    ![Der Datei-Explorer navigiert zum PowerShell-Skript.](images/packaging-screen7.jpg)

    Wenn das App-Paket installiert wurde, zeigt das PowerShell-Fenster die folgende Meldung an: **Ihre APP wurde erfolgreich installiert.**
    >[!TIP]
    > Um das Kontextmenü auf einem Tablet zu öffnen, berühren Sie den Bildschirm, auf den Sie mit der rechten Maustaste klicken können, halten Sie den Finger gedrückt, bis ein vollständiger Kreis angezeigt wird. Das Kontextmenü wird geöffnet, sobald Sie loslassen.

4.  Klicken Sie auf die Schaltfläche „Start“, und geben Sie den Namen der App ein, um sie zu suchen und zu starten.
