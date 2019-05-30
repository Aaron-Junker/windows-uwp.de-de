---
title: Fehlende .NET-APIs in Unity und UWP
description: Informieren Sie sich über die fehlenden .NET-APIs beim Erstellen von UWP-Spielen in Unity sowie über Lösungen für häufig auftretende Probleme.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 02/21/2018
ms.topic: article
keywords: Windows 10, UWP, Spiele, .NET, Unity
ms.localizationpriority: medium
ms.openlocfilehash: 878a598c8a0b71e4ee394f7f98c215e5462b44e7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368433"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>Fehlende .NET-APIs in Unity und UWP

Beim Erstellen eines UWP-Spiels mithilfe von .NET werden Sie möglicherweise feststellen, dass einige APIs, die Sie im Unity-Editor oder für ein eigenständiges PC-Spiel verwenden möchten, für UWP nicht vorhanden sind. Der Grund ist, dass .NET für UWP-Apps nur eine Teilmenge der Typen enthält, die im vollständigen .NET-Framework für jeden Namespace bereitgestellt werden.

Zudem verwenden einige Spiele-Engines verschiedene Arten von .NET, die mit .NET für UWP nicht vollständig kompatibel sind, z. B. Mono von Unity. Also, wenn Sie Ihr Spiel schreiben, alles funktioniert einwandfrei im Editor, aber wenn Sie navigieren, um für die UWP zu erstellen, erhalten Sie möglicherweise Fehler wie folgt: **Der Typ- oder Namespacename 'Formatierungsprogramme"im Namespace"System.Runtime.Serialization"nicht vorhanden (fehlt einen Assemblyverweis?)**

Glücklicherweise stellt einige dieser fehlenden APIs Unity als Erweiterungsmethoden und Ersatz-Typen, die in beschriebenen [universelle Windows-Plattform: Fehlende .NET Typen in .NET Back-End-Scripting](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html). Hilfe für den Fall, dass die benötigte Funktionalität nicht vorhanden ist, finden Sie unter [Überblick über Apps für .NET für Windows 8.x](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)). Dort wird erläutert, wie Sie Ihren Code konvertieren können, um WinRT- oder „.NET für UWP”-APIs zu verwenden. (Es wird Windows 8 behandelt, aber die Erläuterung gilt auch für UWP-Apps unter Windows 10.)

## <a name="net-standard"></a>.NET-Standard

Um zu verstehen, warum einige APIs möglicherweise nicht funktionieren, ist es wichtig, die verschiedenen .NET-Varianten zu kennen und zu verstehen, wie .NET von UWP implementiert wird. Der [.NET-Standard](https://docs.microsoft.com/dotnet/standard/net-standard) ist eine formale Spezifikation von .NET-APIs, die plattformübergreifend sein und die verschiedenen .NET-Varianten vereinheitlichen soll. Jede Implementierung von .NET unterstützt eine bestimmte Version des .NET-Standards. Eine Tabelle mit Standards und Implementierungen finden Sie unter [Unterstützung für die .NET-Implementierung](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support).

Jede Version des UWP SDK entspricht einer anderen Ebene des .NET-Standards. Beispielsweise unterstützt das 16299-SDK (Fall Creators Update) den .NET-Standard 2.0.

Wenn Sie wissen möchten, ob eine bestimmte .NET-API in der von Ihnen ausgewählten UWP-Version unterstützt wird, können Sie in der [API-Referenz für den .NET-Standard](https://docs.microsoft.com/dotnet/api/index?view=netstandard-2.0) die Version des .NET-Standards suchen, die von dieser UWP-Version unterstützt wird.

## <a name="scripting-backend-configuration"></a>Scripting-Backend-Konfiguration

Wenn Sie Probleme mit Builds für UWP haben, sollten Sie zuerst die **Player-Einstellungen** überprüfen (**Datei > Build-Einstellungen**, wählen Sie **Universelle Windows-Plattform**, und dann **Player-Einstellungen**). Unter **Andere Einstellungen > Konfiguration** sind die ersten drei Dropdowns (**Scripting-Laufzeitversion**, **Scripting-Backend** und **Api-Kompatibilitätsgrad**) wichtige Einstellungen, die zu berücksichtigen sind.

Anhand der vom Unity-Scripting-Backend verwendeten **Scripting-Laufzeitversion** können Sie die (ungefähre) äquivalente Version der von Ihnen gewählten .NET Framework-Unterstützung ermitteln. Beachten Sie jedoch, dass nicht alle APIs in dieser Version des .NET-Frameworks unterstützt werden, sondern nur die in der Version des .NET-Standards, auf die Ihr UWP abzielt.

Mit neuen .NET-Versionen werden häufig zusätzliche APIs zu .NET-Standard hinzugefügt, die es Ihnen ermöglichen, für eigenständige und UWP-Anwendungen den gleichen Code zu verwenden. Beispielsweise wurde der Namespace [System.Runtime.Serialization.Json](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json) mit .NET-Standard 2.0 eingeführt. Wenn Sie die **Scripting-Laufzeitversion** auf **.NET 3.5 Equivalent** festlegen (wodurch auf eine frühere Version von .NET-Standard abgezielt wird), tritt ein Fehler auf bei dem Versuch, die API zu verwenden. Wenn Sie **.NET 4.6 Equivalent** (unterstützt .NET-Standard 2.0) wählen, wird die API funktionieren.

Das **Scripting-Backend-** kann **.NET** oder **IL2CPP** sein. In diesem Thema wird angenommen, dass Sie **.NET** gewählt haben, da in diesem Fall die hier beschriebenen Probleme auftreten. Weitere Informationen finden Sie unter [Scripting-Backends](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html).

Außerdem sollten Sie den **Api-Kompatibilitätsgrad** auf die Version von .NET festlegen, mit der Ihr Spiel ausgeführt werden sollen. Dieser muss der **Scripting-Laufzeitversion** entsprechen.

Im Allgemeinen sollten Sie für **Scripting-Laufzeitversion** und **Api-Kompatibilitätsgrad** die neueste verfügbare Version wählen, um die bestmögliche Kompatibilität mit dem .NET Framework sicherzustellen und möglichst viele .NET-APIs bereitzustellen.

![Konfiguration: Scripting Runtime Version; Skripterstellung Back-Ends. API-Kompatibilitätsebene verwendet](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>Plattformabhängige Kompilierung

Wenn Sie Ihr Unity-Spiel für mehrere Plattformen, einschließlich UWP, erstellen, sollten Sie plattformabhängige Kompilierung verwenden, um sicherzustellen, dass für UWP vorgesehener Code nur ausgeführt wird, wenn das Spiel als eine UWP-Anwendung erstellt wird. Auf diese Weise können Sie das vollständige .NET-Framework für eigenständige Desktops sowie andere Plattformen und WinRT-APIs für UWP verwenden, ohne dass Buildfehler auftreten.

Verwenden Sie die folgenden Direktiven, um Code nur zu kompilieren, wenn er als UWP-App ausgeführt wird:

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` Dient zum Überprüfen, ob Sie die Kompilierung erfolgt nur C# Code für die Skripterstellung .NET-Back-End. Verwenden Sie `UNITY_WSA_10_0`, wenn Sie ein anderes Scripting-Backend verwenden, beispielsweise IL2CPP.

Die vollständige Liste der plattformabhängigen Kompilierungsdirektiven finden Sie unter [Plattformabhängige Kompilierung](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html).

## <a name="common-issues-and-workarounds"></a>Häufige Probleme und deren Umgehung

In den folgenden Szenarien werden häufig auftretende Probleme beschrieben, die auftreten können, wenn .NET-APIs in der UWP-Teilmenge fehlen, und es werden Möglichkeiten aufgezeigt, diese Probleme zu umgehen.

### <a name="data-serialization-using-binaryformatter"></a>Datenserialisierung mit BinaryFormatter

Es ist üblich, dass Spiele Daten speichern, damit sie von Spielern nicht einfach manipuliert werden können. Allerdings ist [BinaryFormatter](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter), womit ein Objekt in die Binärform serialisiert wird, in früheren Versionen des .NET-Standard (vor 2.0) nicht verfügbar. Verwenden Sie stattdessen [XmlSerializer](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) oder [DataContractJsonSerializer](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer).

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

### <a name="io-operations"></a>E/A-Operationen

Einige Typen im Namespace [System.IO](https://docs.microsoft.com/dotnet/api/system.io) sind in früheren Versionen von .NET-Standard nicht verfügbar, z. B. [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream). Unity bietet jedoch die Typen [Directory](https://docs.microsoft.com/dotnet/api/system.io.directory), [File](https://docs.microsoft.com/dotnet/api/system.io.file) und **FileStream**, die Sie stattdessen in Ihrem Spiel verwenden können.

Sie können auch die [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage)-APIs verwenden, die nur für UWP-Apps verfügbar sind. Diese APIs beschränken die App jedoch auf das Schreiben in ihren spezifischen Speicher und gewähren ihr keinen freien Zugriff auf das gesamte Dateisystem. Weitere Informationen finden Sie unter [Dateien, Ordner und Bibliotheken](https://docs.microsoft.com/windows/uwp/files/).

Wichtiger Hinweis: Die Methode [Close](https://docs.microsoft.com/dotnet/api/system.io.stream.close) ist nur im .NET-Standard 2.0 und höheren Versionen verfügbar (obwohl Unity eine Erweiterungsmethode enthält). Verwenden Sie stattdessen [Dispose](https://docs.microsoft.com/dotnet/api/system.io.stream.dispose).

### <a name="threading"></a>Threading

Einige Typen im Namespace [System.Threading](https://docs.microsoft.com/dotnet/api/system.threading) sind in früheren Versionen des .NET-Standards nicht verfügbar, z. B. [ThreadPool](https://docs.microsoft.com/dotnet/api/system.threading.threadpool). In diesen Fällen können Sie stattdessen den Namespace [Windows.System.Threading](https://docs.microsoft.com/uwp/api/windows.system.threading) verwenden.

So können Sie das Threading in einem Unity-Spiel anwenden und mithilfe der plattformabhängigen Kompilierung sowohl auf UWP- als auch auf Nicht-UWP-Plattformen abzielen:

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

Einige der **System.Security.** *-Namespaces, z. B. [System.Security.Cryptography.X509Certificates](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0), sind nicht verfügbar, wenn Sie ein Unity-Spiel für UWP erstellen. Verwenden Sie in diesen Fällen die **Windows.Security.** *-APIs, die die gleichen Funktionen behandelt.

Das folgende Beispiel erhält die Zertifikate aus einem Zertifikatspeicher mit dem angegebenen Namen:

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

Weitere Informationen zur Verwendung von WinRT Sicherheits-APIs finden Sie unter [Sicherheit](https://docs.microsoft.com/windows/uwp/security/).

### <a name="networking"></a>Netzwerk

Einige der **System&period;Net.** *-Namespaces, z. B. [System.Net. Mail](https://docs.microsoft.com/dotnet/api/system.net.mail?view=netstandard-2.0), sind ebenfalls nicht verfügbar, um ein Unity-Spiel für UWP zu erstellen. Für die meisten dieser APIs, verwenden Sie die entsprechenden **Windows.Networking.** * und **Windows.Web.** *-WinRT-APIs, um ähnliche Funktionen zu erhalten. Weitere Informationen finden Sie unter [Netzwerk und Webdienste](https://docs.microsoft.com/windows/uwp/networking/).

Im Fall von **System.Net. Mail**, verwenden Sie den [Windows.ApplicationModel.Email](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email)-Namespace. Weitere Informationen finden Sie unter [E-Mail senden](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email).

## <a name="see-also"></a>Siehe auch

* [Universelle Windows-Plattform: Fehlender .NET Typen in .NET Back-End-Skripts](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [.NET für UWP-apps – Übersicht](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [Unity UWP Portieren Führungslinien](https://unity3d.com/partners/microsoft/porting-guides)