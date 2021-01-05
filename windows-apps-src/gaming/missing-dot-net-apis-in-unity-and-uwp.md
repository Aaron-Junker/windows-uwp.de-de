---
title: Fehlende .NET-APIs in Unity und UWP
description: Erfahren Sie mehr über die fehlenden .NET-APIs, wenn Sie UWP-Spiele in Unity entwickeln und Problem Umgehungen für häufige Probleme.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 02/21/2018
ms.topic: article
keywords: Windows 10, UWP, Games, .net, Unity
ms.localizationpriority: medium
ms.openlocfilehash: b687f3ec09a99ae6ccb81e5c205eb454e0af0e04
ms.sourcegitcommit: 4cafc1c55511741dd1e5bfe4496d9950a9b4de1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2021
ms.locfileid: "97860119"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>Fehlende .NET-APIs in Unity und UWP

Wenn Sie ein UWP-Spiel mit .NET entwickeln, werden Sie möglicherweise feststellen, dass einige APIs, die Sie möglicherweise im Unity-Editor oder für ein eigenständiges PC-Spiel verwenden, nicht für UWP vorhanden sind. Dies liegt daran, dass .net für UWP-apps eine Teilmenge der Typen enthält, die im vollständigen .NET Framework für jeden Namespace bereitgestellt werden.

Darüber hinaus verwenden einige Spiel-Engines verschiedene .NET-Versionen, die nicht vollständig mit .net für UWP kompatibel sind, wie z. b. das Mono von Unity. Wenn Sie also Ihr Spiel schreiben, kann alles im Editor einwandfrei funktionieren. Wenn Sie jedoch den Build für UWP verwenden, erhalten Sie möglicherweise Fehler wie die folgende: **der Typ oder Namespace "Formatierer" ist im Namespace "System. Runtime. Serialization" nicht vorhanden (fehlt ein Assemblyverweis?)**

Glücklicherweise stellt Unity einige dieser fehlenden APIs als Erweiterungs Methoden und Ersetzungs Typen bereit, die unter [universelle Windows-Plattform: fehlende .NET-Typen im .NET-Skript](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)-Back-End beschrieben werden. Wenn die von Ihnen benötigte Funktionalität jedoch nicht hier aufgeführt ist, wird in der [Übersicht über .net für Windows 8. x-apps](/previous-versions/windows/apps/br230302(v=vs.140)) erläutert, wie Sie Ihren Code für die Verwendung von WinRT oder .net für Windows-Runtime-APIs konvertieren können. (In diesem Thema wird Windows 8 erläutert, es gilt aber auch für Windows 10-UWP-apps.)

## <a name="net-standard"></a>.NET Standard

Um zu verstehen, warum einige APIs möglicherweise nicht funktionieren, ist es wichtig, sich mit den verschiedenen .net-Varianten und der Implementierung von .net von UWP vertraut zu machen. Der [.NET Standard](/dotnet/standard/net-standard) ist eine formale Spezifikation von .NET-APIs, die plattformübergreifend sein sollen, und die verschiedenen .net-Varianten vereinheitlichen. Jede Implementierung von .NET unterstützt eine bestimmte Version der .NET Standard. Eine Tabelle mit Standards und Implementierungen finden Sie unter [Unterstützung der .NET-Implementierung](/dotnet/standard/net-standard#net-implementation-support).

Jede Version des UWP SDK entspricht einer anderen .NET Standard Ebene. Beispielsweise unterstützt das 16299 SDK (das Fall Creators Update) .NET Standard 2,0.

Wenn Sie wissen möchten, ob eine bestimmte .NET-API in der UWP-Version unterstützt wird, die Sie als Ziel verwenden, können Sie die [.NET Standard-API-Referenz](/dotnet/api/index?view=netstandard-2.0&preserve-view=true) überprüfen und die Version der .NET Standard auswählen, die von dieser Version von UWP unterstützt wird.

## <a name="scripting-backend-configuration"></a>Skripterstellung für Backend

Wenn Sie Probleme beim Erstellen von UWP haben, müssen Sie zunächst die **Player Einstellungen** überprüfen (**Datei > Buildeinstellungen**, wählen Sie **universelle Windows-Plattform** und dann **Spieler Einstellungen**). Unter **anderen Einstellungen > Konfiguration** sind die ersten drei Dropdown Listen (**Skript-Lauf Zeit Version**, **Skript**-Back-End und **API-Kompatibilitäts Grad**) alle wichtigen Einstellungen zu beachten.

Die **Skripting-Laufzeitversion** verwendet das Unity-Skript-Back-End, mit dem Sie die (ungefähr) gleichwertige Version .NET Framework von Ihnen ausgewählten Unterstützung erhalten können. Beachten Sie jedoch, dass nicht alle APIs in dieser Version der .NET Framework unterstützt werden, sondern nur die APIs in der Version von .NET Standard, für die ihre UWP als Ziel verwendet wird.

Häufig werden bei neuen .NET-Releases weitere APIs zu .NET Standard hinzugefügt, die es Ihnen ermöglichen, den gleichen Code für eigenständige und UWP zu verwenden. Beispielsweise wurde die [System.Runtime.Serialization.Js](/dotnet/api/system.runtime.serialization.json) für den Namespace in .NET Standard 2,0 eingeführt. Wenn Sie die CLR- **Laufzeitversion** auf **.NET 3,5-Äquivalent** festlegen (was auf eine frühere Version der .NET Standard abzielt), erhalten Sie eine Fehlermeldung, wenn Sie versuchen, die API zu verwenden. Wechseln Sie zur **.NET 4,6-Entsprechung** (die .NET Standard 2,0 unterstützt), und die API funktioniert.

Das **Skript** für die Skripterstellung kann **.net** oder **IL2CPP** sein. In diesem Thema wird davon ausgegangen, dass Sie **.net** ausgewählt haben, da hier die hier beschriebenen Probleme auftreten. Weitere Informationen finden Sie unter [Skripting-Back-Ends](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html) .

Schließlich sollten Sie den API- **Kompatibilitäts Grad** auf die Version von .net festlegen, auf der das Spiel ausgeführt werden soll. Dies sollte der **Skript Lauf Zeit Version** entsprechen.

Im Allgemeinen sollten Sie für die **Skript Laufzeit-Version** und den **API-Kompatibilitäts Grad** die neueste verfügbare Version auswählen, um eine höhere Kompatibilität mit dem .NET Framework zu ermöglichen und so die Verwendung von weiteren .NET-APIs zu ermöglichen.

![Konfiguration: Skripterstellung für Lauf Zeit Version; Skripterstellung für Backend API-Kompatibilitäts Grad](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>Platt Form abhängige Kompilierung

Wenn Sie Ihr Unity-Spiel für mehrere Plattformen erstellen, einschließlich UWP, sollten Sie eine Platt Form abhängige Kompilierung verwenden, um sicherzustellen, dass der für UWP vorgesehene Code nur ausgeführt wird, wenn das Spiel als UWP erstellt wird. Auf diese Weise können Sie die vollständige .NET Framework für eigenständige Desktops und andere Plattformen sowie WinRT-APIs für UWP verwenden, ohne Buildfehler zu erhalten.

Verwenden Sie die folgenden Direktiven, um beim Ausführen als UWP-app nur Code zu kompilieren:

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` dient nur dazu, zu überprüfen, ob Sie c#-Code für das .NET-Skript-Back-End kompilieren. Wenn Sie ein anderes Skript für die Skripterstellung verwenden, z. b. IL2CPP, verwenden Sie [`ENABLE_WINMD_SUPPORT`](https://docs.unity3d.com/Manual/windowsstore-code-snippets.html) stattdessen.

Eine vollständige Liste der Platt Form abhängigen Kompilierungs Direktiven finden Sie unter [Platt Form abhängige Kompilierung](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html).

## <a name="common-issues-and-workarounds"></a>Häufig auftretende Probleme und Problemumgehungen

In den folgenden Szenarien werden häufige Probleme beschrieben, die auftreten können, wenn .NET-APIs in der UWP-Teilmenge fehlen, und Möglichkeiten, Sie zu umgehen.

### <a name="data-serialization-using-binaryformatter"></a>Datenserialisierung mit BinaryFormatter

Spiele werden häufig zum Serialisieren von Daten gespeichert, sodass Sie von den Playern nicht einfach bearbeitet werden können. Allerdings ist [BinaryFormatter](/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter), der ein Objekt in Binärdateien serialisiert, nicht in früheren Versionen des .NET Standard (vor 2,0) verfügbar. Verwenden Sie stattdessen [XmlSerializer](/dotnet/api/system.xml.serialization.xmlserializer) oder [DataContractJsonSerializer](/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer) .

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>E/A-Vorgänge

Einige Typen im [System.IO](/dotnet/api/system.io) -Namespace, z. b. [FileStream](/dotnet/api/system.io.filestream), sind in früheren Versionen der .NET Standard nicht verfügbar. Unity stellt jedoch die [Verzeichnis](/dotnet/api/system.io.directory)-, [Datei](/dotnet/api/system.io.file)-und **FileStream** -Typen bereit, sodass Sie Sie in Ihrem Spiel verwenden können.

Alternativ können Sie die [Windows. Storage](/uwp/api/Windows.Storage) -APIs verwenden, die nur für UWP-apps verfügbar sind. Diese APIs beschränken jedoch die APP auf das Schreiben in ihren spezifischen Speicher und gewähren keinen kostenlosen Zugriff auf das gesamte Dateisystem. Weitere Informationen finden Sie [unter Dateien, Ordner und Bibliotheken](../files/index.md) .

Ein wichtiger Hinweis ist, dass die [Close](/dotnet/api/system.io.stream.close) -Methode nur in .NET Standard 2,0 und höher verfügbar ist (obwohl Unity eine Erweiterungsmethode bereitstellt). Verwenden [Sie](/dotnet/api/system.io.stream.dispose) stattdessen "verwerfen".

### <a name="threading"></a>Threading

Einige Typen in den [System. Threading](/dotnet/api/system.threading) -Namespaces, wie z. b. [Thread Pool](/dotnet/api/system.threading.threadpool), sind in früheren Versionen der .NET Standard nicht verfügbar. In diesen Fällen können Sie das [Windows.System verwenden. ](/uwp/api/windows.system.threading) Stattdessen wird ein Threading-Namespace erstellt.

Im folgenden wird erläutert, wie Sie das Threading in einem Unity-Spiel handhaben können, indem Sie die Platt Form abhängige Kompilierung verwenden, um sowohl für UWP-als auch für nicht-UWP

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>Sicherheit

Ein Teil von " **System. Security".** _-Namespaces, wie z. b [. System. Security. Cryptography. X509Certificates](/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0&preserve-view=true), sind nicht verfügbar, wenn Sie ein Unity-Spiel für UWP erstellen. In diesen Fällen verwenden Sie die _*Windows. Security.* *_ APIs, die einen Großteil der gleichen Funktionalität abdecken.

Im folgenden Beispiel werden einfach die Zertifikate aus einem Zertifikat Speicher mit dem angegebenen Namen abgerufen:

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

Weitere Informationen zur Verwendung der WinRT-Sicherheits-APIs finden Sie unter [Sicherheit](../security/index.md) .

### <a name="networking"></a>Netzwerk

Einige der _*System- &period; net.* *_ Namespaces, wie z. b [. System .net. Mail](/dotnet/api/system.net.mail?view=netstandard-2.0&preserve-view=true), sind auch bei der Erstellung eines Unity-Spiels für UWP nicht verfügbar. Verwenden Sie für die meisten dieser APIs die entsprechende _*Windows. Networking.* *_ und _*Windows. Web.* *_ WinRT-APIs, um eine ähnliche Funktionalität zu erhalten. Weitere Informationen finden Sie unter [Netzwerk-und Webdienste](../networking/index.md) .

Verwenden Sie im Fall von _ * System .net. Mail * * den [Windows. applicationmodel. Email-](/uwp/api/windows.applicationmodel.email) Namespace. Weitere Informationen finden [Sie unter e-Mail senden](../contacts-and-calendar/sending-email.md) .

## <a name="see-also"></a>Weitere Informationen

* [Universelle Windows-Plattform: fehlende .NET-Typen für .NET-Skript-Back-End](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [Übersicht über .net für UWP-apps](/previous-versions/windows/apps/br230302(v=vs.140))
* [Leitfaden zum Portieren von Unity UWP](https://unity3d.com/partners/microsoft/porting-guides)
