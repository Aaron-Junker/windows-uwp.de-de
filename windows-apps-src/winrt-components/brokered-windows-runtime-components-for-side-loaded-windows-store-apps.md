---
title: Broker Windows-Runtime Komponenten für eine neben geladene UWP-App
description: In diesem Artikel wird ein von Windows 10 unterstütztes Unternehmensziel vorgestellt, das es Touchscreen ermöglicht, den vorhandenen Code zu verwenden, der für wichtige geschäftskritische Vorgänge verantwortlich ist.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: b28df646bb505889626ced8591c5ef9e6ece3f44
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690346"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>Broker Windows-Runtime Komponenten für eine neben geladene UWP-App

In diesem Artikel wird ein von Windows 10 unterstütztes Unternehmensziel vorgestellt, mit dem von Touchscreen eingebundenen .net-apps der vorhandene Code verwendet werden kann, der für wichtige geschäftskritische Vorgänge verantwortlich ist.

## <a name="introduction"></a>Einführung

>**Beachten Sie**  der Beispielcode, der diesem Whitepaper folgt, für [Visual Studio 2015 & 2017](https://aka.ms/brokeredsample)heruntergeladen werden kann. Die Microsoft Visual Studio Vorlage zum Erstellen von Broker Windows-Runtime Komponenten kann hier heruntergeladen werden: [Visual Studio 2015-Vorlage für universelle Windows-Apps für Windows 10](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

Windows umfasst ein neues Feature, das als *Broker Windows-Runtime Komponenten für neben geladene Anwendungen*bezeichnet wird. Wir verwenden den Begriff IPC (prozessübergreifende Kommunikation), um die Möglichkeit zu beschreiben, vorhandene Desktop Software Ressourcen in einem Prozess (Desktop Komponente) auszuführen, während Sie mit diesem Code in einer UWP-App interagieren. Dies ist ein bekanntes Modell für Unternehmensentwickler, da Datenbankanwendungen und-Anwendungen, die NT-Dienste in Windows nutzen, eine ähnliche Architektur mit mehreren Prozessen nutzen.

Das Sideloading der APP ist eine wichtige Komponente dieses Features.
Unternehmensspezifische Anwendungen haben keinen Platz in der allgemeinen consumerMicrosoft Store und Unternehmen haben sehr spezielle Anforderungen in Bezug auf Sicherheit, Datenschutz, Verteilung, Einrichtung und Wartung. Daher ist das Sideloading-Modell sowohl eine Anforderung von denjenigen, die dieses Feature verwenden, als auch ein kritisches Implementierungsdetail.

Datenzentrierte Anwendungen sind ein wichtiges Ziel für diese Anwendungsarchitektur. Es ist bereits darauf bezogen, dass vorhandene Geschäftsregeln verwertet werden, z. b. in SQL Server, ein allgemeiner Teil der Desktop Komponente. Dies ist sicherlich nicht der einzige Funktionstyp, der von der Desktop Komponente angeboten werden kann, aber ein großer Teil der Anforderung für dieses Feature bezieht sich auf vorhandene Daten und Geschäftslogik.

Schließlich wurde dieses Feature aufgrund der überwältigenden Durchdringung der .NET-Laufzeit und der C-\# Sprache in der Unternehmensentwicklung mit einem Schwerpunkt auf die Verwendung von .net für die UWP-APP und die Desktop Komponenten Seite entwickelt. Wenngleich andere Sprachen und Laufzeiten für die UWP-APP möglich sind, veranschaulicht das dazugehörige Beispiel nur C-\#und ist ausschließlich auf die .NET-Laufzeit beschränkt.

## <a name="application-components"></a>Anwendungskomponenten

>**Beachten Sie**  diese Funktion ausschließlich für die Verwendung von .NET verwendet wird. Sowohl die Client-App als auch die Desktop Komponente müssen mithilfe von .NET erstellt werden.

**Anwendungsmodell**

Diese Funktion baut auf der allgemeinen Anwendungsarchitektur auf, die MVVM (Model View View-Model) genannt wird. Daher wird davon ausgegangen, dass das "Modell" vollständig in der Desktop Komponente abgelegt ist. Daher sollte sofort offensichtlich sein, dass es sich bei der Desktop Komponente um einen "headless"-Wert handelt (d. h. ohne Benutzeroberfläche). Die Ansicht ist vollständig in der neben geladenen Unternehmens Anwendung enthalten. Obwohl es nicht erforderlich ist, dass diese Anwendung mit dem Konstrukt "View-Model" erstellt wird, wird davon abgerechnet, dass die Verwendung dieses Musters üblich ist.

**Desktop Komponente**

Die Desktop Komponente in diesem Feature ist ein neuer Anwendungstyp, der als Teil dieses Features eingeführt wird. Diese Desktop Komponente kann nur in C-\# geschrieben werden und muss für Windows 10 auf .NET 4,6 oder höher ausgerichtet sein. Der Projekttyp ist ein Hybrid zwischen der CLR für UWP, da das prozessübergreifende Kommunikationsformat UWP-Typen und-Klassen umfasst, während die Desktop Komponente alle Teile der .net-Lauf Zeit Klassenbibliothek abrufen kann. Die Auswirkungen auf das Visual Studio-Projekt werden später ausführlich beschrieben. Diese Hybrid Konfiguration ermöglicht das Marshalling von UWP-Typen zwischen der Anwendung, die auf den Desktop Komponenten basiert, und ermöglicht gleichzeitig das Aufrufen von Desktop-CLR-Code innerhalb der Implementierung der Desktop Komponente.

**Bedingungen**

Der Vertrag zwischen der sideloloaded-Anwendung und der Desktop Komponente wird in Bezug auf das UWP-Typsystem beschrieben. Dies umfasst das Deklarieren einer oder mehrerer C-\# Klassen, die eine UWP darstellen können. Weitere Informationen finden Sie [im MSDN-Thema Erstellen von Windows-Runtime Komponenten in c\# und Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) , um Windows-Runtime Klasse mithilfe von c\#zu erstellen.

>**Beachten Sie** , dass  enums zurzeit nicht im Windows-Runtime Components-Vertrag zwischen der Desktop Komponente und der sideloloaded-Anwendung unterstützt werden.

**Sideloloaded-Anwendung**

Bei der neben geladenen Anwendung handelt es sich um eine normale UWP-app in jeder Hinsicht mit Ausnahme von einem: Sie wird neben dem Microsoft Store nicht über die installiert. Die meisten Installations Mechanismen sind identisch: das Manifest und die Anwendungs Verpackung sind ähnlich (eine Ergänzung zum Manifest wird später ausführlich beschrieben). Wenn das Sideloading aktiviert ist, kann ein einfaches PowerShell-Skript die erforderlichen Zertifikate und die Anwendung selbst installieren. Die übliche bewährte Vorgehensweise besteht darin, dass die Anwendung, die die Seite geladen hat, den Wack-Zertifizierungstest übergibt, der im Menü "Projekt/Speicher" in Visual Studio enthalten ist.

>**Hinweis** Das Sideloading kann in "Settings-&gt; Update & Security-&gt; für Entwickler aktiviert werden.

Ein wichtiger Punkt ist, dass der APP-Broker-Mechanismus, der als Teil von Windows 10 ausgeliefert wird, nur 32 Bit ist. Die Desktop Komponente muss 32 Bit sein.
Sideloloaded-Anwendungen können 64-Bit-Version sein (vorausgesetzt, es sind sowohl ein 64-Bit-als auch ein 32-Bit-Proxys registriert), dies ist jedoch nicht typisch. Wenn Sie die Anwendung mit Sideloading in C\# mit der normalen "neutralen" Konfiguration und der Standardeinstellung "bevorzugtes 32-Bit" erstellen, werden auf natürliche Weise 32-Bit-Anwendungen erstellt.

**Server Instanz und AppDomains**

Jede seitig geladene Anwendung erhält eine eigene Instanz eines App-Broker Servers (so genannte "multiinstancing"). Der Servercode wird innerhalb einer einzelnen AppDomain ausgeführt. Dies ermöglicht, dass mehrere Versionen von Bibliotheken in separaten Instanzen ausgeführt werden. Beispielsweise benötigt Anwendung A v 1.1 einer Komponente, und Anwendung B benötigt v2. Diese werden durch die Komponenten v 1.1 und v2 in separaten Server Verzeichnissen getrennt getrennt, und die Anwendung wird auf den Server verwiesen, der die richtige gewünschte Version unterstützt.

Die Implementierung des Server Codes kann von mehreren App Broker-Server Instanzen gemeinsam genutzt werden, indem mehrere Anwendungen auf dasselbe Serververzeichnis verwiesen werden. Es werden immer noch mehrere Instanzen des App Broker-Servers vorhanden sein, aber es wird identischer Code ausgeführt. Alle Implementierungs Komponenten, die in einer einzelnen Anwendung verwendet werden, sollten im gleichen Pfad vorhanden sein.

## <a name="defining-the-contract"></a>Definieren des Vertrags

Der erste Schritt beim Erstellen einer Anwendung mithilfe dieses Features besteht darin, den Vertrag zwischen der Seite mit den Seiten geladenen Anwendungen und der Desktop Komponente zu erstellen. Dies muss ausschließlich mithilfe von Windows-Runtime Typen erfolgen.
Glücklicherweise sind diese mithilfe von C-\# Klassen leicht zu deklarieren. Es gibt jedoch wichtige Überlegungen zur Leistung, wenn Sie diese Konversationen definieren, die in einem späteren Abschnitt behandelt werden.

Die Sequenz zum Definieren des Vertrags wird wie folgt eingeführt:

**Schritt 1:** Erstellen Sie eine neue Klassenbibliothek in Visual Studio. Stellen Sie sicher, dass Sie das Projekt mit der **Klassen Bibliotheks** Vorlage und nicht mit der Vorlage für die **Windows-Runtime-Komponente** erstellen.

Eine-Implementierung folgt offensichtlich, aber in diesem Abschnitt wird nur die Definition des prozessübergreifenden Vertrags behandelt. Das zugehörige Beispiel enthält die folgende Klasse (EnterpriseServer.cs), deren Anfangsform aussieht:

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

Dadurch wird die Klasse "EnterpriseServer" definiert, die von der neben geladenen Anwendung instanziiert werden kann. Diese Klasse stellt die Funktionalität bereit, die in der runtimeclass versprochen wurde. Die runtimeclass kann verwendet werden, um den Verweis-winmd zu generieren, der in der neben geladenen Anwendung enthalten ist.

**Schritt 2:** Bearbeiten Sie die Projektdatei manuell, um den Ausgabetyp des Projekts in **Windows-Runtime Komponente**zu ändern.

Klicken Sie dazu mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie "Projekt entladen" aus. Klicken Sie dann erneut mit der rechten Maustaste, und wählen Sie "EnterpriseServer. csproj bearbeiten" aus, um die Projektdatei (eine XML-Datei) zum Bearbeiten zu öffnen.

Suchen Sie in der geöffneten Datei nach dem \<OutputType\> Tag, und ändern Sie dessen Wert in "winmdobj".

**Schritt 3:** Erstellen Sie eine Buildregel, die eine "Verweis"-Windows-Metadatendatei (winmd-Datei) erstellt. bedeutet, dass über keine Implementierung verfügt.

**Schritt 4:** Erstellen Sie eine Buildregel, die eine Windows-Metadatendatei "Implementation" erstellt, d.h. die gleichen Metadateninformationen enthält, aber auch die-Implementierung enthält.

Dies erfolgt mithilfe der folgenden Skripts. Fügen Sie die Skripts der Befehlszeile für das Postbuildereignis in den Projekt **Eigenschaften** > **Buildereignisse**hinzu.

> **Beachten Sie** , dass sich das Skript auf der Grundlage der Windows-Version (Windows 10) und der verwendeten Version von Visual Studio unterscheidet.

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

Sobald der Verweis- **winmd** (im Ordner "Reference" unter dem Zielordner des Projekts) erstellt wurde, wird er für jedes verwendete neben geordnete Anwendungsprojekt Hand geladen (kopiert), und es wird darauf verwiesen. Dies wird weiter unten im nächsten Abschnitt beschrieben. Die Projektstruktur, die in den obigen Buildregeln enthalten ist, stellt sicher, dass sich die Implementierung und der Verweis- **winmd** in eindeutig getrennten Verzeichnissen in der buildhierarchie befinden, um Verwirrung zu vermeiden.

## <a name="side-loaded-applications-in-detail"></a>Paralleles Laden von Anwendungen im Detail
Wie bereits erwähnt, wird die Seite mit Seiten geladenen Anwendungen wie jede andere UWP-App erstellt, aber es gibt noch ein zusätzliches Detail: Deklarieren der Verfügbarkeit der runtimeclass (es) im Manifest der neben geladenen Anwendung. Dadurch kann die Anwendung einfach neu schreiben, um auf die Funktionalität in der Desktop Komponente zuzugreifen. Ein neuer Manifest-Eintrag im Abschnitt "<Extension>" beschreibt die in der Desktop Komponente implementierte runtimeclass und Informationen dazu, wo Sie sich befinden. Diese Deklarations Inhalte im Anwendungs Manifest sind für apps, die auf Windows 10 ausgerichtet sind, identisch. Beispiel:

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

Die Kategorie ist "inprocessserver", weil in der Kategorie "oudefprocessserver" mehrere Einträge vorhanden sind, die für diese Anwendungskonfiguration nicht zutreffend sind. Beachten Sie, dass die <Path> Komponente immer "clrhost. dll" enthalten muss (Dies wird jedoch **nicht** erzwungen, und die Angabe eines anderen Werts schlägt auf nicht definierte Weise fehl).

Der Abschnitt "<ActivatableClass>" ist mit der "true" in-Process "runtimeclass" identisch, die von einer Windows-Runtime Komponente im Paket der APP bevorzugt wird. <ActivatableClassAttribute> ist ein neues Element, und die Attribute Name = "desktopapplicationpath" und Type = "String" sind obligatorisch und invariant. Das value-Attribut verweist auf den Speicherort, an dem sich die winmd-Implementierung der Desktop Komponente befindet (Ausführlichere Informationen hierzu finden Sie im nächsten Abschnitt). Jede von der Desktop Komponente bevorzugte runtimeclass sollte über eine eigene <ActivatableClass>-Elementstruktur verfügen. Activatableclassid muss mit dem vollständig Namespace qualifizierten Namen der runtimeclass identisch sein.

Wie im Abschnitt "definieren des Vertrags" erwähnt, muss ein Projekt Verweis auf den Verweis-winmd der Desktop Komponente vorgenommen werden. Das Visual Studio-Projekt System erstellt normalerweise eine Verzeichnisstruktur mit zwei Ebenen mit demselben Namen. Im Beispiel ist es enterpriseipcapplication\\enterpriseipcapplication. Der **winmd** -Verweis wird manuell in dieses Verzeichnis der zweiten Ebene kopiert, und dann wird das Dialogfeld "Projekt Verweise" verwendet (Klicken Sie auf " **Durchsuchen".** Schaltfläche), um diese **winmd**zu suchen und darauf zu verweisen. Danach sollte der Namespace auf oberster Ebene der Desktop Komponente (z. b. fabrikam) als Knoten der obersten Ebene im Verweis Teil des Projekts angezeigt werden.

>**Hinweis** Es ist sehr wichtig, den **Verweis-winmd** in der neben geladenen Anwendung zu verwenden. Wenn Sie die **Implementierung von winmd** versehentlich in das neben geladene App-Verzeichnis übertragen und darauf verweisen, erhalten Sie wahrscheinlich eine Fehlermeldung im Zusammenhang mit dem Hinweis, dass istringable nicht gefunden werden kann. Dabei handelt es sich um ein sicheres Vorzeichen, auf das der falsche **winmd** verwiesen wurde. Die postbuildregeln in der IPC Server-app (im nächsten Abschnitt ausführlich erläutert) trennen diese beiden **winmd** sorgfältig in separate Verzeichnisse.

Umgebungsvariablen (insbesondere% Program Files%) kann in <ActivatableClassAttribute Value="path"> verwendet werden. Wie bereits erwähnt, unterstützt der APP-Broker nur 32-Bit, sodass% Program Files% in C:\\-Programmdateien (x86) aufgelöst werden, wenn die Anwendung auf einem 64-Bit-Betriebssystem ausgeführt wird.

## <a name="desktop-ipc-server-detail"></a>Desktop IPC-Server Details

In den vorherigen beiden Abschnitten wird die Deklaration der-Klasse und die Mechanismen zum Transportieren des **verweiswinmd** zum neben geladenen Anwendungsprojekt beschrieben. Der Großteil der verbleibenden Arbeit in der Desktop Komponente umfasst die Implementierung. Da der gesamte Punkt der Desktop Komponente in der Lage sein soll, den Desktop Code aufzurufen (in der Regel, um vorhandene codeassets wiederzuverwenden), muss das Projekt auf besondere Weise konfiguriert werden.
Normalerweise verwendet ein Visual Studio-Projekt, in dem .NET verwendet wird, eines von zwei "Profiles".
Eine ist für Desktop (". NETFramework ") und eines für den UWP-App-Teil der CLR (". Netcore "). Eine Desktop Komponente in dieser Funktion ist eine Hybrid Verbindung zwischen diesen beiden. Daher wird der Verweis Abschnitt sehr sorgfältig erstellt, um diese beiden Profile zu kombinieren.

Ein normales UWP-App-Projekt enthält keine expliziten Projekt Verweise, da die gesamte Windows-Runtime API-Oberfläche implizit eingeschlossen ist.
Normalerweise werden nur andere projektübergreifende Verweise erstellt. Allerdings verfügt ein Desktop Komponenten Projekt über einen sehr speziellen Satz von verweisen. Die Lebensdauer wird als "klassisches Desktop\\-Klassenbibliothek"-Projekt gestartet und ist daher ein Desktop Projekt. Daher müssen explizite Verweise auf die Windows-Runtime-API (über Verweise auf **winmd** -Dateien) erstellt werden. Fügen Sie ordnungsgemäße Verweise hinzu, wie unten gezeigt.

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

Die obigen Verweise stellen eine sorgfältige Mischung aus eferences dar, die für den ordnungsgemäßen Betrieb dieses Hybrid Servers von entscheidender Bedeutung sind. Das Protokoll dient zum Öffnen der CSPROJ-Datei (wie unter Bearbeiten des Projekts OutputType beschrieben) und zum Hinzufügen dieser Verweise nach Bedarf.

Sobald die Verweise ordnungsgemäß konfiguriert sind, besteht die nächste Aufgabe darin, die Funktionalität des Servers zu implementieren. Weitere Informationen finden [Sie im Thema bewährte Methoden für die Interoperabilität mit Windows-Runtime-Komponenten (UWPC++ -apps mit C\#/VB/und XAML)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10)).
Die Aufgabe besteht darin, eine Windows-Runtime Komponenten-DLL zu erstellen, die im Rahmen der Implementierung von Desktop Code aufgerufen werden kann. Das zugehörige Beispiel enthält die wichtigsten Muster, die in Windows-Runtime verwendet werden:

-   Methodenaufrufe

-   Ereignisse von der Desktop Komponente Windows-Runtime Ereignissen

-   Asynchrone Vorgänge Windows-Runtime

-   Zurückgeben von Arrays von Basis Typen

**Installieren**

Um die APP zu installieren, kopieren Sie die **winmd** -Implementierung in das richtige Verzeichnis, das im zugehörigen Manifest der zugehörigen neben geladenen Anwendung angegeben ist: <ActivatableClassAttribute>es Value = "Path". Kopieren Sie außerdem alle zugeordneten Unterstützungs Dateien und die Proxy-/Stub-DLL (die nachfolgenden Details finden Sie unten). Wenn die Implementierung von **winmd** nicht in den Speicherort des Server Verzeichnisses kopiert wird, wird der Fehler "Class Not Registered" ausgelöst, wenn alle Aufrufe von "New" der Seite "New" in der runtimeclass aufgerufen werden. Wenn Sie den Proxy/Stub nicht installieren (oder die Registrierung nicht durchführen), werden alle Aufrufe fehlschlagen, ohne dass Rückgabewerte angezeigt werden. Der letztere Fehler ist häufig **nicht** mit sichtbaren Ausnahmen verknüpft.
Wenn Ausnahmen aufgrund dieses Konfigurations Fehlers festgestellt werden, können Sie auf "ungültige Umwandlung" verweisen.

**Überlegungen zur Server Implementierung**

Der Desktop Windows-Runtime Server kann als "Worker" oder "Task"-basiert betrachtet werden. Jeder Server Server wird in einem nicht-UI-Thread betrieben, und der gesamte Code muss Multithread-fähig und sicher sein. Der Teil der neben geladenen Anwendung, der die Funktionalität des Servers aufruft, ist ebenfalls wichtig. Es ist wichtig, dass Sie immer vermeiden, dass Code mit langer Ausführungszeit von einem beliebigen UI-Thread in der neben geladenen Anwendung aufgerufen wird. Hierfür gibt es zwei Möglichkeiten:

1.  Wenn Sie die Server Funktionalität von einem UI-Thread aufrufen, verwenden Sie immer ein Async-Muster in der öffentlichen Oberfläche und Implementierung des Servers.

2.  Die Funktionalität des Servers wird von einem Hintergrund Thread in der neben geladenen Anwendung aufgerufen.

**Windows-Runtime Async auf dem Server**

Aufgrund der prozessübergreifenden Art des Anwendungs Modells haben Aufrufe an den Server mehr Aufwand als Code, der exklusiv Prozess übergreifend ausgeführt wird. Normalerweise ist es sicher, eine einfache Eigenschaft aufzurufen, die einen Speicher internen Wert zurückgibt, da Sie schnell genug ausgeführt wird, sodass das Blockieren des UI-Threads nicht relevant ist. Allerdings kann jeder Aufruf, der e/a-Vorgänge (einschließlich aller Datei Behandlung und Daten Bank Abruf Vorgänge) einschließt, den aufrufenden UI-Thread blockieren und bewirkt, dass die Anwendung aufgrund einer nicht Reaktionsfähigkeit beendet wird. Außerdem wird aus Leistungsgründen von der Verwendung von Eigenschafts aufrufen für Objekte in dieser Anwendungsarchitektur abgeraten.
Dies wird im folgenden Abschnitt ausführlicher behandelt.

Ein ordnungsgemäß implementierter Server implementiert normalerweise Aufrufe, die direkt aus Benutzeroberflächenthreads über das Windows-Runtime Async-Muster vorgenommen werden. Dies kann mithilfe des folgenden Musters implementiert werden. Zuerst die Deklaration (wiederum aus dem zugehörigen Beispiel):

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

Dadurch wird ein Windows-Runtime asynchroner Vorgang deklariert, der eine ganze Zahl zurückgibt.
Die Implementierung des Async-Vorgangs hat normalerweise das folgende Format:

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**Hinweis** Es kommt häufig vor, dass beim Schreiben der Implementierung einige andere möglicherweise lange ausgelaufende Vorgänge erwartet werden. Wenn dies der Fall ist, muss der **Task. Run** -Code deklariert werden:

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

Clients dieser Async-Methode können diesen Vorgang wie jede andere Windows-Runtime aysnc-Operation erwarten.

**Server Funktionalität aus einem Anwendungs Hintergrund Thread abrufen**

Da sowohl der Client als auch der Server von der gleichen Organisation geschrieben werden, kann eine Programmier Übung verwendet werden, die alle Aufrufe an den Server durch einen Hintergrund Thread in der neben geladenen Anwendung durchführt. Ein direkter-Befehl, der einen oder mehrere Daten Batches vom Server sammelt, kann aus einem Hintergrund Thread erstellt werden. Wenn die Ergebnisse vollständig abgerufen werden, kann der Daten Batch, der im Arbeitsspeicher des Anwendungsprozesses vorhanden ist, normalerweise direkt aus dem UI-Thread abgerufen werden. C @ no__t_0_-Objekte sind natürlich zwischen Hintergrundthreads und Benutzeroberflächenthreads flexibel und somit besonders nützlich für diese Art von Aufruf Mustern.\#

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Erstellen und Bereitstellen des Windows-Runtime Proxys

Da der IPC-Ansatz das Marshalling von Windows-Runtime Schnittstellen zwischen zwei Prozessen umfasst, muss ein Global registrierter Windows-Runtime Proxy und Stub verwendet werden.

**Erstellen des Proxys in Visual Studio**

Der Prozess zum Erstellen und Registrieren von Proxys und stubys für die Verwendung in einem regulären UWP-App-Paket wird im Thema zum Erstellen von [Ereignissen in Windows-Runtime Komponenten](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))beschrieben.
Die in diesem Artikel beschriebenen Schritte sind komplizierter als der unten beschriebene Prozess, da Sie das Registrieren des Proxys/Stubs innerhalb des Anwendungspakets (im Gegensatz zur globalen Registrierung) beinhalten.

**Schritt 1:** Erstellen Sie in Visual Studio mithilfe der Projekt Mappe für das Desktop Komponenten Projekt ein Proxy-/Stubprojekt:

**Projekt Mappe > > Projekt hinzufügen C++ > Visual > Win32-Konsole wählen Sie dll-Option aus.**

Bei den folgenden Schritten wird davon ausgegangen, dass die Serverkomponente **mywinrtcomponent**heißt.

**Schritt 3:** Löschen Sie alle cpp/H-Dateien aus dem Projekt.

**Schritt 4:** Der vorherige Abschnitt "definieren des Vertrags" enthält einen Postbuildbefehl, der " **winmdidl. exe**", " **Mittel l. exe**", " **mdmerge. exe**" und so weiter ausführt. Eine der Ausgaben aus dem Mittel l-Schritt dieses Postbuild-Befehls generiert vier wichtige Ausgaben:

a) dlldata. c

b) eine Header Datei (z. b. mywinrtcomponent. h)

c) A \*\_i. c-Datei (z. b. mywinrtcomponent\_i. c)

d) A \*\_p. c-Datei (z. b. mywinrtcomponent\_p. c)

**Schritt 5:** Fügen Sie diese vier generierten Dateien dem Projekt "mywinrtproxy" hinzu.

**Schritt 6:** Fügen Sie dem Projekt "mywinrtproxy" eine DEF-Datei hinzu **(Projekt > Neues Element > Code > Modul Definitionsdatei hinzufügen**), und aktualisieren Sie den Inhalt wie folgt:

Bibliothek mywinrtcomponent. Proxies. dll

Ö

DllCanUnloadNow privat

DllGetClassObject privat

DllRegisterServer privat

DllUnregisterServer privat

**Schritt 7:** Öffnen Sie die Eigenschaften für das Projekt "mywinrtproxy":

**Comfiguration-Eigenschaften > allgemeiner > Zielname:**

Mywinrtcomponent. Proxies

**Präprozessordefinitionen für C/C++ > > Hinzufügen**

Win32\_Windows;\_Proxy\_dll registrieren "

**Vorkompilierter C/>-C++ Header: Wählen Sie "nicht verwendeten vorkompilierten Header" aus.**

**Linker > Allgemein > Import Bibliothek ignorieren: Klicken Sie auf "Ja".**

**Linker > Eingabe > zusätzliche Abhängigkeiten: Add rpcrt4. lib; runtimeobject. lib**

**Linker > Windows-Metadaten > Generieren von Windows-Metadaten: Wählen Sie "Nein" aus.**

**Schritt 8:** Erstellen Sie das Projekt "mywinrtproxy".

**Proxy Bereitstellung**

Der Proxy muss Global registriert werden. Die einfachste Möglichkeit hierfür ist, dass der Installationsprozess DllRegisterServer auf der Proxy-dll aufruft. Beachten Sie Folgendes: da die Funktion nur Server unterstützt, die für x86 erstellt wurden (d. h. keine 64-Bit-Unterstützung), ist die einfachste Konfiguration die Verwendung eines 32-Bit-Servers, eines 32-Bit-Proxys und einer 32-Bit-Anwendung. Der Proxy befindet sich normalerweise neben der **winmd** -Implementierung für die Desktop Komponente.

Ein zusätzlicher Konfigurationsschritt muss ausgeführt werden. Damit der sideloloaded-Prozess den Proxy laden und ausführen kann, muss das Verzeichnis für ALL_APPLICATION_PACKAGES als "Lesen/Ausführen" gekennzeichnet sein. Dies erfolgt über das Befehlszeilen Tool " **icacls. exe** ". Dieser Befehl sollte in dem Verzeichnis ausgeführt werden, in dem sich die **winmd** -Implementierung und die Proxy-/Stub-DLL befinden:

*icacls. /T/Grant \*S-1-15-2-1: RX*

## <a name="patterns-and-performance"></a>Muster und Leistung

Es ist sehr wichtig, dass die Leistung des prozessübergreifenden Transports sorgfältig überwacht wird. Ein Prozess übergreifender-Rückruf ist mindestens doppelt so teuer wie ein in-Process-Aufrufe. Die prozessübergreifende Erstellung von Konversationen und die Durchführung wiederholter Übertragungen von großen Objekten wie bitmapimages kann zu unerwarteter und unerwünschter Anwendungsleistung führen.

Im folgenden finden Sie eine nicht vollständige Liste der zu berücksichtigenden Dinge:

-   Synchrone Methodenaufrufe aus dem UI-Thread der Anwendung an den Server sollten immer vermieden werden. Aufrufen der-Methode aus einem Hintergrund Thread in der Anwendung und anschließendes Verwenden von corewindowdispatcher, um die Ergebnisse bei Bedarf in den UI-Thread zu übernehmen.

-   Das Aufrufen von asynchronen Vorgängen über einen Anwendungs Benutzeroberflächen-Thread ist sicher, aber beachten Sie die unten beschriebenen Leistungsprobleme.

-   Durch die Massenübertragung der Ergebnisse wird die prozessübergreifende Zeichenfolge reduziert. Dies erfolgt normalerweise mithilfe des Windows-Runtime Array-Konstrukts.

-   Das Zurückgeben von *Listen<T>* , wobei *t* ein Objekt aus einem asynchronen Vorgang oder einem Abruf der Eigenschaft ist, führt zu einer großen prozessübergreifenden unter Bearbeitung. Nehmen wir beispielsweise an, dass Sie eine*Liste&lt;People&gt;* -Objekten zurückgeben. Jeder Iterations Durchlauf ist ein Prozess übergreifender Rückruf. Jedes zurückgegebene *People* -Objekt wird durch einen Proxy dargestellt, und jeder Rückruf einer Methode oder Eigenschaft für dieses einzelne Objekt führt zu einem prozessübergreifenden Aufrufvorgang. Eine "unschuldige" *Liste&lt;People&gt;* Objekt, bei dem die *Anzahl* groß ist, führt zu einer großen Anzahl von langsamen aufrufen. Eine bessere Leistung ergibt sich aus der Massenübertragung von Strukturen des Inhalts in einem Array. Beispiel:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

Geben Sie dann * personstruct\[\]* anstelle von *List&lt;personobject&gt;* zurück.
Hierdurch werden alle Daten in einem prozessübergreifenden "Hop" abgerufen.

Wie bei allen Leistungs Überlegungen sind Messungen und Tests wichtig. Im Idealfall sollten Telemetriedaten in die verschiedenen Vorgänge eingefügt werden, um zu bestimmen, wie lange Sie dauern. Es ist wichtig, sich über einen Bereich zu messen, z. b. wie lange es dauert, bis alle *People* -Objekte für eine bestimmte Abfrage in der neben geladenen Anwendung verarbeitet werden?

Eine andere Methode sind Variablen Auslastungs Tests. Dies können Sie erreichen, indem Sie leistungstesthooks in der Anwendung platzieren, die eine Variable verzögertes Laden in die Server Verarbeitung einbringen. Dadurch können verschiedene Arten von Auslastung und die Reaktion der Anwendung auf die unterschiedliche Serverleistung simuliert werden.
Das Beispiel veranschaulicht, wie Sie Zeitverzögerungen in den Code einfügen, indem Sie ordnungsgemäße Async-Techniken verwenden. Die genaue Menge an Verzögerung, die eingefügt werden soll, und der Bereich der Randomisierung, der in diese künstliche Auslastung eingefügt werden soll, variieren je nach Anwendungs Entwurf und der erwarteten Umgebung, in der die Anwendung ausgeführt wird.

## <a name="development-process"></a>Entwicklungsprozess

Wenn Sie Änderungen am Server vornehmen, müssen Sie sicherstellen, dass alle Instanzen, die bereits ausgeführt werden, nicht mehr ausgeführt werden. COM führt den Prozess letztendlich aus, aber der rundowntimer dauert länger, als für die iterative Entwicklung effizient ist. Daher ist das Abbrechen einer zuvor laufenden Instanz ein normaler Schritt während der Entwicklung. Dies erfordert, dass der Entwickler nachverfolgen kann, welche dllhost-Instanz den Server hostet.

Der Server Prozess kann mit dem Task-Manager oder anderen Drittanbieter-apps gefunden und abgebrochen werden. Das Befehlszeilen Tool * * tasklist. exe * * ist ebenfalls enthalten und verfügt über eine flexible Syntax, z. b.:

  
 | **Befehl** | **Aktion** |
 | ------------| ---------- |
 | tasklist | Listet alle laufenden Prozesse in der ungefähren Reihenfolge der Erstellungszeit mit den zuletzt erstellten Prozessen im unteren Bereich auf. |
 | tasklist/Fi "IMAGENAME eq dllhost. exe"/M | Listet Informationen zu allen dllhost. exe-Instanzen auf. Der Schalter/M listet die Module auf, die Sie geladen haben. |
 | tasklist/Fi "PID EQ 12564"/M | Sie können diese Option verwenden, um die Datei "Dllhost. exe" abzufragen, wenn Sie Ihre PID kennen. |

Die Modulliste für einen Broker Server sollte " *clrhost. dll* " in der Liste der geladenen Module auflisten.

## <a name="resources"></a>Ressourcen

-   [Projektvorlagen für Broker-WinRT-Komponenten für Windows 10 und vs 2015](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [Northwindrt-Broker WinRT-Komponenten Beispiel](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [Bereitstellung zuverlässiger und vertrauenswürdiger Microsoft Store-Apps](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [App-Verträge und-Erweiterungen (Windows Store-Apps)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [Querladen von apps unter Windows 10](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [Bereitstellen von UWP-Apps für Unternehmen](https://go.microsoft.com/fwlink/p/?LinkID=264770)

