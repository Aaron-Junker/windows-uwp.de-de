---
Description: Dieses Handbuch hilft Ihnen, Ihre App zur Handhabung der Unternehmensdaten, die von der Windows Information Protection (WIP)-Richtlinie verwaltet werden, sowie von persönlichen Daten zu optimieren.
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Entwicklerhandbuch für Windows Information Protection (WIP)
ms.date: 06/21/2017
ms.topic: article
keywords: Windows 10, UWP, WIP, Windows Information Protection, Unternehmensdaten, Unternehmensdatenschutz, edp, optimierte Apps
ms.assetid: 913ac957-ea49-43b0-91b3-e0f6ca01ef2c
ms.localizationpriority: medium
ms.openlocfilehash: 6d026c00b1aec4fd8e80b10c0b86c8bd8145f925
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258598"
---
# <a name="windows-information-protection-wip-developer-guide"></a>Entwicklerhandbuch für Windows Information Protection (WIP)

Eine *optimierte* App unterscheidet zwischen Firmen- oder persönlichen Daten und weiß, welche sie schützen soll, basierend auf Windows Information Protection (WIP)-Richtlinien, die vom Administrator definiert werden.

In diesem Handbuch zeigen wir Ihnen, wie eine erstellt wird. Wenn Sie fertig sind, werden Richtlinienadministratoren Ihrer App vertrauen, Daten für ihre Organisation zu verwenden. Und Mitarbeiter schätzen es, dass ihre persönlichen Daten auf den Geräten unangetastet bleiben, auch wenn sie die Registrierung des Geräts in der mobilen Geräteverwaltung (Mobile Device Management, MDM) aufheben oder das Unternehmen ganz verlassen.

__Hinweis:__ Dieses Handbuch hilft Ihnen beim Optimieren einer UWP-App. Wenn Sie eine C++-Windows-Desktop-App optimieren möchten, lesen Sie die Informationen unter [Entwicklerhandbuch für Windows Information Protection (WIP) (C++)](https://docs.microsoft.com/previous-versions/windows/desktop/EDP/wip-developer-guide?redirectedfrom=MSDN).

Weitere Informationen zu WIP und optimierten Apps hier: [Windows Information Protection (WIP)](wip-hub.md).

Ein vollständiges Beispiel finden Sie [hier](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection).

Wenn Sie bereit sind, die einzelnen Aufgaben durchzugehen, lassen Sie uns starten.

## <a name="first-gather-what-you-need"></a>Sammeln Sie zuerst die gewünschten Informationen.

Sie benötigen Folgendes:

* Einen virtuellen Testcomputer mit Windows 10, Version 1607, oder höher. Sie debuggen Ihre App unter Verwendung dieses virtuellen Testcomputers.

* Einen Entwicklungscomputer mit Windows 10, Version 1607, oder höher. Dies kann Ihr virtueller Testcomputer sein, wenn Visual Studio darauf installiert ist.

## <a name="setup-your-development-environment"></a>Richten Sie Ihre Entwicklungsumgebung ein.

Sie führen folgende Schritte aus:

* [Installieren des WIP-Setup-Assistenten für Entwickler auf dem virtuellen Testcomputer](#install-assistant)

* [Erstellen einer Schutzrichtlinie mithilfe des WIP-Setup-Assistenten für Entwickler](#create-protection-policy)

* [Einrichten eines Visual Studio-Projekts](#setup-vs-project)

* [Remote Debuggen einrichten](#setup-remote-debugging)

* [Hinzufügen von Namespaces zu Ihren Code Dateien](#add-namespaces)

<a id="install-assistant" />

### <a name="install-the-wip-setup-developer-assistant-onto-your-test-vm"></a>Installieren Sie den WIP Setup Developer Assistant auf dem virtuellen Testcomputer.

 Verwenden Sie dieses Tool, um eine Windows Information Protection-Richtlinie auf Ihrem virtuellen Testcomputer einzurichten.

 Laden Sie das Tool hier herunter: [WIP Setup Developer Assistant](https://www.microsoft.com/store/p/wip-setup-developer-assistant/9nblggh526jf).

<a id="create-protection-policy" />

### <a name="create-a-protection-policy"></a>Erstellen Sie eine Schutzrichtlinie.

Definieren Sie die Richtlinie, indem Sie in jedem Abschnitt des WIP Setup Developer Assistant Informationen eingeben. Um mehr über die Verwendung der einzelnen Einstellungen zu erfahren, wählen Sie das daneben angezeigte Hilfesymbol aus.

Allgemeine Informationen zur Verwendung dieses Tools finden Sie im Abschnitt „Versionshinweise“ auf der Downloadseite der App.

<a id="setup-vs-project" />

### <a name="setup-a-visual-studio-project"></a>Richten Sie ein Visual Studio-Projekt ein.

1. Öffnen Sie das Projekt auf Ihrem Entwicklungscomputer.

2. Fügen Sie einen Verweis auf die Desktop- und Mobilerweiterungen für die Universelle Windows-Plattform (UWP) hinzu.

    ![UWP-Erweiterungen hinzufügen](images/extensions.png)

3. Fügen Sie diese Funktion der Paketmanifestdatei hinzu:

    ```xml
       <rescap:Capability Name="enterpriseDataPolicy"/>
    ```
   >*Optionale Info*: Das Präfix „Rescap“ bedeutet *eingeschränkte Funktion*. Siehe [Spezielle und eingeschränkte Funktionen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

4. Fügen Sie diesen Namespace der Paketmanifestdatei hinzu:

    ```xml
      xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    ```
5. Fügen Sie das Namespacepräfix dem ``<ignorableNamespaces>``-Element der Paketmanifestdatei hinzu.

    ```xml
        <IgnorableNamespaces="uap mp rescap">
    ```

    Auf diese Weise ignoriert Windows die ``enterpriseDataPolicy``-Funktion, wenn Ihre App auf einer Version des Windows-Betriebssystems ausgeführt wird, die eingeschränkte Funktionen nicht unterstützt.

<a id="setup-remote-debugging" />

### <a name="setup-remote-debugging"></a>Richten Sie das Remotedebuggen ein.

Installieren Sie Visual Studio-Remotetools auf Ihrem virtuellen Testcomputer nur dann, wenn Sie Ihre App auf einem anderem Computer als einem virtuellen Computer entwickeln. Starten Sie den Remotedebugger auf dem Entwicklungscomputer, und überprüfen Sie, ob Ihre App auf dem virtuellen Testcomputer ausgeführt wird.

Siehe [Anweisungen zu Remote-PCs](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

<a id="add-namespaces" />

### <a name="add-these-namespaces-to-your-code-files"></a>Fügen Sie diese Namespaces zu Ihren Codedateien hinzu.

Fügen Sie diese using-Anweisungen am Anfang Ihrer Codedateien ein (Verwenden Sie Codeausschnitte aus diesem Handbuch):

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Windows.Security.EnterpriseData;
using Windows.Web.Http;
using Windows.Storage.Streams;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml;
using Windows.ApplicationModel.Activation;
using Windows.Web.Http.Filters;
using Windows.Storage;
using Windows.Data.Xml.Dom;
using Windows.Foundation.Metadata;
using Windows.Web.Http.Headers;
```

## <a name="determine-whether-to-use-wip-apis-in-your-app"></a>Bestimmen Sie, ob WIP-APIs in Ihrer App verwendet werden sollen.

Stellen Sie sicher, dass das Betriebssystem, das Ihre App ausführt, WIP unterstützt und WIP auf dem Gerät aktiviert ist.

```csharp
bool use_WIP_APIs = false;

if ((ApiInformation.IsApiContractPresent
    ("Windows.Security.EnterpriseData.EnterpriseDataContract", 3)
    && ProtectionPolicyManager.IsProtectionEnabled))
{
    use_WIP_APIs = true;
}
else
{
    use_WIP_APIs = false;
}
```
Rufen Sie keine WIP-APIs auf, wenn das Betriebssystem WIP nicht unterstützt oder WIP nicht auf dem Gerät aktiviert ist.

## <a name="read-enterprise-data"></a>Lesen von Unternehmensdaten

Ihre App muss Zugriff anfordern, um geschützte Dateien, Netzwerkendpunkte, Daten aus der Zwischenablage und Daten zu lesen, die Sie von einem Freigabe-Vertrag akzeptieren.

Windows Information Protection erteilt Ihrer App die Berechtigung, wenn Ihre App in der Liste der zugelassenen Apps der Schutzrichtlinie enthalten ist.

**In diesem Abschnitt:**

* [Lesen von Daten aus einer Datei](#read-file)
* [Lesen von Daten aus einem Netzwerk Endpunkt](#read-network)
* [Lesen von Daten aus der Zwischenablage](#read-clipboard)
* [Lesen von Daten aus einem Freigabe Vertrag](#read-share)

<a id="read-file" />

### <a name="read-data-from-a-file"></a>Lesen von Daten aus einer Datei

**Schritt 1: Datei Handle erhalten**

```csharp
    Windows.Storage.StorageFolder storageFolder =
        Windows.Storage.ApplicationData.Current.LocalFolder;

    Windows.Storage.StorageFile file =
        await storageFolder.GetFileAsync(fileName);
```

**Schritt 2: bestimmen, ob die APP die Datei öffnen kann**

Rufen Sie [FileProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync) auf, um festzustellen, ob Ihre App die Datei öffnen kann.

```csharp
FileProtectionInfo protectionInfo = await FileProtectionManager.GetProtectionInfoAsync(file);

if ((protectionInfo.Status != FileProtectionStatus.Protected &&
    protectionInfo.Status != FileProtectionStatus.Unprotected))
{
    return false;
}
else if (protectionInfo.Status == FileProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}
```

Ein [FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)-Wert von **Geschützt** bedeutet, dass die Datei geschützt ist und Ihre App sie öffnen kann, da Ihre App in der Liste der zugelassenen Apps der Richtlinie enthalten ist.

Ein [FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)-Wert von **Ungeschützt** bedeutet, dass die Datei nicht geschützt ist, und Ihre App die Datei sogar öffnen kann, wenn Sie nicht in der Liste der zugelassenen Apps der Richtlinie enthalten ist.

> **APIs** <br>
[Fileschutzmanager. getschutzinfoasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[Fileschutzinfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[Fileschutzstatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[Schutzpolicymanager. isidentitymanaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**Schritt 3: Lesen der Datei in einen Stream oder Puffer**

*Datei in einen Stream lesen*

```csharp
var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

*Datei in einen Puffer lesen*

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(file);
```
<a id="read-network" />

### <a name="read-data-from-a-network-endpoint"></a>Lesen Sie Daten von einem Netzwerkendpunkt aus

Erstellen Sie einen geschützten Threadkontext zum Lesen aus einem Unternehmens-Endpunkt.

**Schritt 1: erhalten der Identität des Netzwerk Endpunkts**

```csharp
Uri resourceURI = new Uri("http://contoso.com/stockData.xml");

Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

Wenn der Endpunkt nicht mithilfe der Gruppenrichtlinie verwaltet wird, erhalten Sie einen leeren String zurück.

> **APIs** <br>
[Schutzpolicymanager. getprimarymanagedidentityfornetworkendpointasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)


**Schritt 2: Erstellen eines geschützten Thread Kontexts**

Wenn der Endpunkt durch die Richtlinie verwaltet wird, erstellen Sie einen geschützten Threadkontext. Dadurch werden alle Netzwerkverbindungen markiert, die mit dieser Identität auf demselben Thread hergestellt werden.

Sie erhalten auch Zugriff auf Unternehmensnetzwerkressourcen, die von dieser Richtlinie verwaltet werden.

```csharp
if (!string.IsNullOrEmpty(identity))
{
    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
    }
}
else
{
    return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
}
```
Dieses Beispiel umschließt Socket-Aufrufe in einem ``using``-Block. Wenn Sie dies nicht tun, stellen Sie sicher, dass Sie den Threadkontext schließen, nachdem Sie die Ressource abgerufen haben. Siehe [ThreadNetworkContext.Close](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.threadnetworkcontext.close).

Erstellen Sie keine persönlichen Dateien auf geschützten Threads, da diese Dateien automatisch verschlüsselt werden.

Die [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)-Methode gibt ein [**ThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.threadnetworkcontext)-Objekt zurück, unabhängig davon, ob der Endpunkt durch die Richtlinie verwaltet wird. Wenn Ihre App persönliche und Unternehmensressourcen behandelt, rufen Sie [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext) für alle Identitäten auf.  Nachdem Sie die Ressource erhalten haben, geben Sie den ThreadNetworkContext frei, um jedes Identitätstag des aktuellen Threads zu deaktivieren.

> **APIs** <br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[Schutzpolicymanager. kreatecurrentthreadnetworkcontext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

**Schritt 3: Lesen der Ressource in einen Puffer**

```csharp
private static async Task<IBuffer> GetDataFromNetworkHelperMethod(Uri resourceURI)
{
    HttpClient client;

    client = new HttpClient();

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**Optionale Verwenden Sie ein Header Token, anstatt einen geschützten Thread Kontext zu erstellen.**

```csharp
public static async Task<IBuffer> GetDataFromNetworkbyUsingHeader(Uri resourceURI)
{
    HttpClient client;

    Windows.Networking.HostName hostName =
        new Windows.Networking.HostName(resourceURI.Host);

    string identity = await ProtectionPolicyManager.
        GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);

    if (!string.IsNullOrEmpty(identity))
    {
        client = new HttpClient();

        HttpRequestHeaderCollection headerCollection = client.DefaultRequestHeaders;

        headerCollection.Add("X-MS-Windows-HttpClient-EnterpriseId", identity);

        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }
    else
    {
        client = new HttpClient();
        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }

}

private static async Task<IBuffer> GetDataFromNetworkbyUsingHeaderHelperMethod(HttpClient client, Uri resourceURI)
{

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**Seiten Umleitungen verarbeiten**

In einigen Fällen leitet ein Webserver Datenverkehr auf eine aktuellere Version einer Ressource weiter.

Um damit umzugehen, stellen Sie Anfragen, bis der Antwortstatus Ihrer Anfrage den Wert **OK** hat.

Verwenden Sie dann den URI der Antwort, um die Identität des Endpunkts abzurufen. Hier ist eine Möglichkeit, dies zu tun:

```csharp
private static async Task<IBuffer> GetDataFromNetworkRedirectHelperMethod(Uri resourceURI)
{
    HttpClient client = null;

    HttpBaseProtocolFilter filter = new HttpBaseProtocolFilter();
    filter.AllowAutoRedirect = false;

    client = new HttpClient(filter);

    HttpResponseMessage response = null;

        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Get, resourceURI);
        response = await client.SendRequestAsync(message);

    if (response.StatusCode == HttpStatusCode.MultipleChoices ||
        response.StatusCode == HttpStatusCode.MovedPermanently ||
        response.StatusCode == HttpStatusCode.Found ||
        response.StatusCode == HttpStatusCode.SeeOther ||
        response.StatusCode == HttpStatusCode.NotModified ||
        response.StatusCode == HttpStatusCode.UseProxy ||
        response.StatusCode == HttpStatusCode.TemporaryRedirect ||
        response.StatusCode == HttpStatusCode.PermanentRedirect)
    {
        message = new HttpRequestMessage(HttpMethod.Get, message.RequestUri);
        response = await client.SendRequestAsync(message);

        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
    else
    {
        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
}

```

> **APIs** <br>
[Schutzpolicymanager. getprimarymanagedidentityfornetworkendpointasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)<br>
[Schutzpolicymanager. kreatecurrentthreadnetworkcontext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)<br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="read-clipboard" />

### <a name="read-data-from-the-clipboard"></a>Daten aus der Zwischenablage lesen

**Berechtigung zum Verwenden von Daten aus der Zwischenablage**

Zum Abrufen von Daten aus der Zwischenablage bitten Sie Windows um Erlaubnis. Verwenden Sie dazu [**DataPackageView.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync) .

```csharp
public static async Task PasteText(TextBox textBox)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

        if (result == ProtectionPolicyEvaluationResult..Allowed)
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            textBox.Text = contentsOfClipboard;
        }
    }
}
```

> **APIs** <br>
[Datapackageview. requestaccessasync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)

**Ausblenden oder Deaktivieren von Funktionen, die Zwischenablage Daten verwenden**

Ermitteln Sie, ob die aktuelle Ansicht über die Berechtigung zum Abrufen von Daten, die sich in der Zwischenablage befinden, verfügt.

Wenn dies nicht der Fall ist, können Sie Steuerelemente, mit denen Benutzer Informationen aus der Zwischenablage einsetzen oder den Inhalt in der Vorschau anzeigen, deaktivieren oder ausblenden.

```csharp
private bool IsClipboardAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;

    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))

        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed |
        protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.ConsentRequired);
}
```

> **APIs** <br>
[Schutzpolicyevaluationresult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**Verhindern, dass Benutzer über ein Zustimmungs Dialogfeld aufgefordert werden**

Ein neues Dokument ist nicht *persönlich* oder *Unternehmen*. Es ist einfach neu. Wenn ein Benutzer Unternehmensdaten in die Datei einsetzt, setzt Windows Richtlinien durch und der Benutzer wird aufgefordert, einem Dialogfeld zuzustimmen. Dieser Code verhindert, dass dies geschieht. Es geht bei dieser Aufgabe nicht darum, Daten zu schützen. Es geht mehr darum, Benutzer davon abzuhalten, Dialogfelder zur Zustimmung zu sehen – in Fällen, in denen Ihre App ein völlig neues Element erstellt hat.

```csharp
private async void PasteText(bool isNewEmptyDocument)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
            {
                ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                // add this string to the new item or document here.          

            }
            else
            {
                ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

                if (result == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                    // add this string to the new item or document here.
                }
            }
        }
    }
}
```

> **APIs** <br>
[Datapackageview. requestaccessasync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)<br>
[Schutzpolicyevaluationresult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[Schutzpolicymanager. tryapplyprocessuipolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="read-share" />

### <a name="read-data-from-a-share-contract"></a>Lesen von Daten aus einem Freigabe-Vertrag

Wenn Mitarbeiter zur Freigabe ihrer Informationen Ihre App auswählen, öffnet Ihre App ein neues Element, das diese Inhalte enthält.

Wie bereits erwähnt, wird ein neues Element nicht *persönlich* oder *Unternehmen*. Es ist einfach neu. Wenn Ihr Code Unternehmensinhalt zu dem Element hinzufügt, setzt Windows Richtlinien durch und der Benutzer wird mit einem Zustimmungsdialogfeld aufgefordert. Dieser Code verhindert, dass dies geschieht.

```csharp
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isNewEmptyDocument = true;
    string identity = "corp.microsoft.com";

    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog
                ProtectionPolicyManager.TryApplyProcessUIPolicy(shareOperation.Data.Properties.EnterpriseId);
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.

                ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();

                if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string text = await shareOperation.Data.GetTextAsync();

                    // Do something with that text.
                }
            }
        }
        else
        {
            // If the data has no enterprise identity, then we already have access.
            string text = await shareOperation.Data.GetTextAsync();

            // Do something with that text.
        }

    }

}
```

> **APIs** <br>
[Schutzpolicymanager. requestaccessasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.requestaccessasync)<br>
[Schutzpolicyevaluationresult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[Schutzpolicymanager. tryapplyprocessuipolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

## <a name="protect-enterprise-data"></a>Unternehmensdaten schützen

Schützen Sie Unternehmensdaten, die Ihre App verlassen. Daten verlassen Ihre App, wenn Sie diese auf einer Seite zeigen. Sichern Sie sie in einer Datei oder einem Netzwerkendpunkt oder über einen Freigabe-Vertrag.

**In diesem Abschnitt:**

* [Schützen von Daten, die in Seiten angezeigt werden](#protect-pages)
* [Schützen von Daten in einer Datei als Hintergrundprozess](#protect-background)
* [Schützen eines Teils einer Datei](#protect-part-file)
* [Lesen des geschützten Teils einer Datei](#read-protected)
* [Schützen von Daten in einem Ordner](#protect-folder)
* [Schützen von Daten auf einem Netzwerk Endpunkt](#protect-network)
* [Schützen von Daten, die Ihre APP über einen Freigabe Vertrag freigibt](#protect-share)
* [Schützen von Dateien, die Sie an einen anderen Speicherort kopieren](#protect-other-location)
* [Unternehmensdaten schützen, wenn der Bildschirm des Geräts gesperrt ist](#protect-locked)

<a id="protect-pages" />

### <a name="protect-data-that-appears-in-pages"></a>Schützen Sie Daten, die auf Seiten angezeigt werden.

Wenn Sie Daten auf einer Seite anzeigen, können Sie Windows mitteilen, um welche Art von Daten es sich handelt (persönlich oder Unternehmen). Zu diesem Zweck *markieren Sie* die aktuelle App-Ansicht, oder markieren Sie den gesamten App-Prozess.

Wenn Sie die Ansicht oder den Prozess markieren, setzt Windows die Richtlinie durch. Dadurch werden Datenlecks verhindert, die aus Aktionen, die Ihre App nicht steuert, entstehen. Beispielsweise auf einem Computer könnten ein Benutzer mit STRG+V Unternehmensinformationen aus einer Ansicht kopieren und diese Informationen in einer anderen App einsetzen. Windows schützt davor. Windows hilft auch, die Freigabe-Verträge durchzusetzen.

**Markieren der aktuellen App-Ansicht**

Vorgehensweise, wenn Ihre App mehrere Ansichten hat, in denen einige Ansichten Unternehmensdaten und einige persönliche Daten nutzen.

```csharp

// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
ProtectionPolicyManager.GetForCurrentView().Identity = identity;

// tag as personal data.
ProtectionPolicyManager.GetForCurrentView().Identity = String.Empty;
```

> **APIs** <br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**Tagprozess**

Vorgehensweise, wenn alle Ansichten in der App mit nur einem Typ von Daten (persönlich oder Unternehmen) verwendet werden.

Dadurch müssen Sie nicht unabhängig voneinander markierte Ansichten verwalten.

```csharp


// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(identity);

// tag as personal data.
ProtectionPolicyManager.ClearProcessUIPolicy();
```

> **APIs** <br>
[Schutzpolicymanager. tryapplyprocessuipolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="protect-file" />

### <a name="protect-data-to-a-file"></a>Daten in einer Datei schützen

Erstellen Sie eine geschützte Datei, und schreiben Sie dann hinein.

**Schritt 1: ermitteln, ob Ihre APP eine Unternehmens Datei erstellen kann**

Ihre App kann eine Unternehmensdatei erstellen, wenn der Identitätsstring durch die Richtlinie verwaltet wird und Ihre App sich auf der Liste der zulässigen Apps von dieser Richtlinie befindet.

```csharp
  if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **APIs** <br>
[Schutzpolicymanager. isidentitymanaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)


**Schritt 2: Erstellen Sie die Datei, und schützen Sie Sie in der Identität.**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile storageFile = await storageFolder.CreateFileAsync("sample.txt",
    CreationCollisionOption.ReplaceExisting);

FileProtectionInfo fileProtectionInfo =
    await FileProtectionManager.ProtectAsync(storageFile, identity);
```

> **APIs** <br>
[Fileschutzmanager. protectasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)

**Schritt 3: Schreiben Sie diesen Stream oder Puffer in die Datei.**

*Schreiben eines Streams*

```csharp
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

        using (var outputStream = stream.GetOutputStreamAt(0))
        {
            using (var dataWriter = new DataWriter(outputStream))
            {
                dataWriter.WriteString(enterpriseData);
            }
        }

    }
```

*Schreiben eines Puffers*

```csharp
     if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
     {
         var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
             enterpriseData, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

         await FileIO.WriteBufferAsync(storageFile, buffer);

      }
```

> **APIs** <br>
[Fileschutzinfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[Fileschutzstatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

<a id="protect-background" />

### <a name="protect-data-to-a-file-as-a-background-process"></a>Schützen von Daten in einer Datei als Hintergrundprozess

Während der Bildschirm des Geräts gesperrt ist, kann dieser Code ausgeführt werden. Wenn der Administrator eine sichere "Schutz von Daten bei Sperre" (Data Protection Under Lock, DPL) Richtlinie konfiguriert, entfernt Windows die Verschlüsselungsschlüssel, die erforderlich sind, um Zugriff auf geschützte Ressourcen aus dem Arbeitsspeicher des Geräts zu erhalten. Dies beugt Datenverlusten vor, wenn das Gerät verloren geht. Das gleiche Feature entfernt auch Schlüssel, die mit geschützten Dateien heruntergeladen wurden, wenn deren Handles geschlossen werden.

Sie müssen einen Ansatz verwenden, der das Datei-Handle geöffnet hält, wenn Sie eine Datei erstellen.  

**Schritt 1: ermitteln, ob Sie eine Unternehmens Datei erstellen können**

Sie können eine Unternehmensdatei erstellen, wenn die Identität, die Sie verwenden, durch die Richtlinie verwaltet wird und Ihre App sich auf der Liste der zulässigen Apps dieser Richtlinie befindet.

```csharp
if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **APIs** <br>
[Schutzpolicymanager. isidentitymanaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**Schritt 2: Erstellen Sie eine Datei, und schützen Sie Sie in der Identität.**

Der [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync) erstellt eine geschützte Datei und hält das Datei-Handle geöffnet, während Sie einen Schreibvorgang ausführen.

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

ProtectedFileCreateResult protectedFileCreateResult =
    await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
        "sample.txt", identity, CreationCollisionOption.ReplaceExisting);
```

> **APIs** <br>
[Fileschutzmanager. kreateprotectedandopenasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync)

**Schritt 3: Schreiben eines Streams oder Puffers in die Datei**

Dieses Beispiel schreibt einen Datenstrom in eine Datei.

```csharp
if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
{
    IOutputStream outputStream =
        protectedFileCreateResult.Stream.GetOutputStreamAt(0);

    using (DataWriter writer = new DataWriter(outputStream))
    {
        writer.WriteString(enterpriseData);
        await writer.StoreAsync();
        await writer.FlushAsync();
    }

    outputStream.Dispose();
}
else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
{
    // Perform any special processing for the access suspended case.
}

```

> **APIs** <br>
[Protectedfilekreateresult. schutzinfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.protectioninfo)<br>
[Fileschutzstatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[Protectedfilekreateresult. Stream](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.stream)<br>

<a id="protect-part-file" />

### <a name="protect-part-of-a-file"></a>Einen Teil einer Datei schützen

In den meisten Fällen ist es sauberer, Unternehmens- und persönliche Daten separat zu speichern, jedoch können Sie sie in derselben Datei speichern, wenn Sie möchten. Beispielsweise kann Microsoft Outlook Unternehmens-E-Mails zusammen mit den persönlichen E-Mails in einer einzigen Archivdatei speichern.

Verschlüsseln Sie die Unternehmensdaten, jedoch nicht die gesamte Datei. Auf diese Weise können Benutzer weiterhin mit der Datei arbeiten, selbst wenn sie von MDM aufgehoben werden oder ihre Unternehmensdatenzugriffsrechte widerrufen werden. Darüber hinaus sollte Ihre App den Überblick behalten, welche Daten sie verschlüsselt, damit bekannt ist, welche Daten zu schützen sind, wenn sie die Datei zurück in den Arbeitsspeicher liest.

**Schritt 1: Hinzufügen von Unternehmensdaten zu einem verschlüsselten Stream oder Puffer**

```csharp
string enterpriseDataString = "<employees><employee><name>Bill</name><social>xxx-xxx-xxxx</social></employee></employees>";

var enterpriseData= Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        enterpriseDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

BufferProtectUnprotectResult result =
   await DataProtectionManager.ProtectAsync(enterpriseData, identity);

enterpriseData= result.Buffer;
```

> **APIs** <br>
[Dataschutzmanager. protectasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[Bufferprotectunprotectresult. Buffer](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)


**Schritt 2: Hinzufügen personenbezogener Daten zu einem unverschlüsselten Stream oder Puffer**

```csharp
string personalDataString = "<recipies><recipe><name>BillsCupCakes</name><cooktime>30</cooktime></recipe></recipies>";

var personalData = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    personalDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

**Schritt 3: Schreiben von Datenströmen oder Puffern in eine Datei**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

StorageFile storageFile = await storageFolder.CreateFileAsync("data.xml",
    CreationCollisionOption.ReplaceExisting);

 // Write both buffers to the file and save the file.

var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

using (var outputStream = stream.GetOutputStreamAt(0))
{
    using (var dataWriter = new DataWriter(outputStream))
    {
        dataWriter.WriteBuffer(enterpriseData);
        dataWriter.WriteBuffer(personalData);

        await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    }
}
```

**Schritt 4: verfolgen Sie den Speicherort der Unternehmensdaten in der Datei.**

Ihre App ist dafür verantwortlich, die zum Unternehmen gehörigen Daten in der Datei im Blick zu behalten.

Sie können diese Informationen in der Datei-Eigenschaft, einer Datenbank oder in einem Kopfzeilentext in der Datei speichern.

Dieses Beispiel sichert die Informationen in eine separate XML-Datei.

```csharp
StorageFile metaDataFile = await storageFolder.CreateFileAsync("metadata.xml",
   CreationCollisionOption.ReplaceExisting);

await Windows.Storage.FileIO.WriteTextAsync
    (metaDataFile, "<EnterpriseDataMarker start='0' end='" + enterpriseData.Length.ToString() +
    "'></EnterpriseDataMarker>");
```
<a id="read-protected" />

### <a name="read-the-protected-part-of-a-file"></a>Lesen Sie den geschützten Teil einer Datei

Hier wird beschrieben, wie die Unternehmensdaten aus der Datei gelesen werden würde.

**Schritt 1: erhalten Sie die Position der Unternehmensdaten in der Datei.**

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;

 Windows.Storage.StorageFile metaDataFile =
   await storageFolder.GetFileAsync("metadata.xml");

string metaData = await Windows.Storage.FileIO.ReadTextAsync(metaDataFile);

XmlDocument doc = new XmlDocument();

doc.LoadXml(metaData);

uint startPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("start")).InnerText);

uint endPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("end")).InnerText);
```

**Schritt 2: Öffnen Sie die Datendatei, und stellen Sie sicher, dass Sie nicht geschützt ist.**

```csharp
Windows.Storage.StorageFile dataFile =
    await storageFolder.GetFileAsync("data.xml");

FileProtectionInfo protectionInfo =
    await FileProtectionManager.GetProtectionInfoAsync(dataFile);

if (protectionInfo.Status == FileProtectionStatus.Protected)
    return false;
```

> **APIs** <br>
[Fileschutzmanager. getschutzinfoasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[Fileschutzinfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[Fileschutzstatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

**Schritt 3: Lesen der Unternehmensdaten aus der Datei**

```csharp
var stream = await dataFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

stream.Seek(startPosition);

Windows.Storage.Streams.Buffer tempBuffer = new Windows.Storage.Streams.Buffer(50000);

IBuffer enterpriseData = await stream.ReadAsync(tempBuffer, endPosition, InputStreamOptions.None);
```

**Schritt 4: Entschlüsseln des Puffers, der Unternehmensdaten enthält**

```csharp
DataProtectionInfo dataProtectionInfo =
   await DataProtectionManager.GetProtectionInfoAsync(enterpriseData);

if (dataProtectionInfo.Status == DataProtectionStatus.Protected)
{
    BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync(enterpriseData);
    enterpriseData = result.Buffer;
}
else if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}

```

> **APIs** <br>
[Dataschutzinfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectioninfo)<br>
[Dataschutzmanager. getschutzinfoasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.getstreamprotectioninfoasync)<br>

<a id="protect-folder" />

### <a name="protect-data-to-a-folder"></a>Schützen Sie Daten in einem Ordner

Sie können einen Ordner erstellen und diesen schützen. Auf diese Weise sind alle Elemente, die Sie zu diesem Ordner hinzufügen, automatisch geschützt.

```csharp
private async Task<bool> CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
        return false;
    }
    return true;
}
```

Stellen Sie sicher, dass der Ordner leer ist, bevor Sie ihn schützen. Es ist nicht möglich, einen Ordner zu schützen, der bereits Elemente enthält.

> **APIs** <br>
[Schutzpolicymanager. isidentitymanaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)<br>
[Fileschutzmanager. protectasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)<br>
[Fileschutzinfo. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo.identity)<br>
[Fileschutzinfo. Status](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo.status)

<a id="protect-network" />

### <a name="protect-data-to-a-network-end-point"></a>Schützen Sie Daten in einem Netzwerkendpunkt

Erstellen Sie einen geschützten Threadkontext zum Senden von Daten an einen Unternehmensendpunkt.  

**Schritt 1: erhalten der Identität des Netzwerk Endpunkts**

```csharp
Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

> **APIs** <br>
[Schutzpolicymanager. getprimarymanagedidentityfornetworkendpointasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)

**Schritt 2: Erstellen eines geschützten Thread Kontexts und Senden von Daten an den Netzwerk Endpunkt**

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(m_EnterpriseId))
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;

    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Put, resourceURI);
        message.Content = new HttpStreamContent(dataToWrite);

        HttpResponseMessage response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.Ok)
            return true;
        else
            return false;
    }
}
else
{
    return false;
}
```

> **APIs** <br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[Schutzpolicymanager. kreatecurrentthreadnetworkcontext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

<a id="protect-share" />

### <a name="protect-data-that-your-app-shares-through-a-share-contract"></a>Schützen Sie Daten, die Ihre App über einen Freigabe-Vertrag teilt

Wenn Benutzer Inhalt aus Ihrer App freigeben sollen, müssen Sie einen Freigabe-Vertrag implementieren und das Ereignis [**DataTransferManager.DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) behandeln.

Legen Sie in Ihrem Ereignishandler den Unternehmensidentitätskontext im Datenpaket fest.

```csharp
private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

> **APIs** <br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="protect-other-location" />

### <a name="protect-files-that-you-copy-to-another-location"></a>Schützen von Dateien, die Sie an einen anderen Speicherort kopieren

```csharp
private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

> **APIs** <br>
[Fileschützmanager. copyschutzasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.copyprotectionasync)<br>

<a id="protect-locked" />

### <a name="protect-enterprise-data-when-the-screen-of-the-device-is-locked"></a>Unternehmensdaten schützen, wenn der Bildschirm des Geräts gesperrt ist

Entfernen Sie alle sensible Daten im Arbeitsspeicher, wenn das Gerät gesperrt wird. Wenn der Benutzer das Gerät entsperrt, kann Ihre App diese Daten wieder sicher hinzufügen.

Behandeln des [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending) Ereignis, damit Ihre App weiß, wann der Bildschirm gesperrt ist. Dieses Ereignis wird nur ausgelöst, wenn der Administrator einen sicheren Datenschutz bei Sperre in den Richtlinien konfiguriert. Windows entfernt vorübergehend die Datenschutzschlüssel, welche auf dem Gerät bereitgestellt werden. Windows entfernt diese Schlüssel, um sicherzustellen, dass es keine nicht autorisierten Zugriffe auf verschlüsselte Daten gibt, wenn das Gerät gesperrt und möglicherweise nicht im Besitz des Besitzers ist.  

Behandeln des [**ProtectionPolicyManager.ProtectedAccessResumed**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) Ereignis, damit Ihre App weiß, wann der Bildschirm entsperrt ist. Dieses Ereignis wird ausgelöst, unabhängig davon, ob der Administrator einen sicheren Datenschutz bei Sperre in den Richtlinien konfiguriert.

#### <a name="remove-sensitive-data-in-memory-when-the-screen-is-locked"></a>Entfernen Sie sensible Daten im Arbeitsspeicher, wenn der Bildschirm gesperrt wird

Schützen Sie sensible Daten, und schließen Sie alle Datenströme, die Ihre App auf geschützten Dateien geöffnet hat, um sicherzustellen, dass das System keine sensiblen Daten im Speicher zwischenspeichert.

Dieses Beispiel sichert Inhalte aus einem TextBlock-Steuerelement in einem verschlüsselten Puffer und entfernt die Inhalte von diesem TextBlock-Steuerelement.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessSuspending(object sender, ProtectedAccessSuspendingEventArgs e)
{
    Deferral deferral = e.GetDeferral();

    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        IBuffer documentBodyBuffer = CryptographicBuffer.ConvertStringToBinary
           (documentTextBlock.Text, BinaryStringEncoding.Utf8);

        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (documentBodyBuffer, ProtectionPolicyManager.GetForCurrentView().Identity);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            this.protectedDocumentBuffer = result.Buffer;
            documentTextBlock.Text = null;
        }
    }

    // Close any open streams that you are actively working with
    // to make sure that we have no unprotected content in memory.

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    deferral.Complete();
}
```

> **APIs** <br>
[Schutzpolicymanager. protectedaccesssuspendieren](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending)<br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[Dataschutzmanager. protectasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[Bufferprotectunprotectresult. Buffer](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)<br>
[Protectedaccesssuspendingeventargs. getdeferral](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedaccesssuspendingeventargs.getdeferral)<br>
[Deferral. Complete](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete)<br>

#### <a name="add-back-sensitive-data-when-the-device-is-unlocked"></a>Fügen Sie wieder sensible Daten ein, wenn das Gerät entsperrt wurde

[**Schutzpolicymanager. protectedaccessfort**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) setzen wird ausgelöst, wenn das Gerät entsperrt wird und die Schlüssel auf dem Gerät wieder verfügbar sind.

[**Protectedaccessresumedebug. Identities**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedaccessresumedeventargs.identities) ist eine leere Sammlung, wenn der Administrator keinen sicheren Datenschutz unter Sperr Richtlinie konfiguriert hat.

In diesem Beispiel erfolgt das Gegenteil der Aktionen des vorherigen Beispiels. Es entschlüsselt den Puffer, fügt Informationen dieses Puffers zum Textfeld hinzu und gibt anschließend den Puffer frei.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessResumed(object sender, ProtectedAccessResumedEventArgs e)
{
    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.protectedDocumentBuffer);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            documentTextBlock.Text = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.protectedDocumentBuffer = null;
        }
    }

}
```

> **APIs** <br>
[Schutzpolicymanager. protectedaccessfort gesetzt](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed)<br>
[Schutzpolicymanager. getforcurrentview](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[Schutzpolicymanager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[Dataschutzmanager. unprotectasync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.unprotectasync)<br>
[Bufferprotectunprotectresult. Status](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult)<br>

## <a name="handle-enterprise-data-when-protected-content-is-revoked"></a>Behandeln Sie Unternehmensdaten, wenn der geschützte Inhalt verweigert wird

Wenn Ihre App benachrichtigt werden soll, wenn ein Gerät von MDM aufgehoben ist oder wenn der Richtlinien-Administrator explizit den Zugriff auf Unternehmensdaten widerruft, behandeln Sie das [**ProtectionPolicyManager_ProtectedContentRevoked**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked)-Ereignis.

In diesem Beispiel wird bestimmt, ob die Daten in einem Unternehmenspostfach für eine E-Mail-App gesperrt wurden.

```csharp
private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

> **APIs** <br>
[ProtectionPolicyManager_ProtectedContentRevoked](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked)<br>

## <a name="related-topics"></a>Verwandte Themen

[Windows Information Protection-Beispiel (WIP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)
