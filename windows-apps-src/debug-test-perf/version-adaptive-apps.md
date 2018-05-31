---
author: jwmsft
title: Versionsadaptive Apps
description: Erfahren Sie, wie Sie neue APIs nutzen und gleichzeitig die Kompatibilität mit früheren Versionen gewährleisten.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d43cd9d03977e34b57d78e1f22bd7e8b340ff4ab
ms.sourcegitcommit: cceaf2206ec53a3e9155f97f44e4795a7b6a1d78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
ms.locfileid: "1701036"
---
# <a name="version-adaptive-apps-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>Versionsadaptive Apps: Verwendung neuer APIs bei gleichzeitiger Gewährleistung der Kompatibilität mit früheren Versionen

Mit jeder Version des Windows10SDKs kommen spannende neue Funktionen hinzu, die Sie nicht ungenutzt lassen sollten. Da allerdings nicht alle Kunden ihre Geräte gleichzeitig auf die neueste Version von Windows10 aktualisieren, müssen Sie sicherstellen, dass Ihre App auf einem möglichst breiten Gerätespektrum verwendet werden kann. Hier zeigen wir Ihnen, wie Sie Ihre App so gestalten, dass sie unter früheren Versionen von Windows10 ausgeführt werden kann, aber auch neue Features nutzt, wenn sie auf einem Gerät ausgeführt wird, auf dem das neueste Update installiert ist.

Mit den folgenden 3 Schritten können Sie sicherstellen, dass Ihre App auf einem möglichst breiten Spektrum von Windows10-Geräten verwendet werden kann:

- Erstens: Konfigurieren Sie Ihr VisualStudio-Projekt mit den neuesten APIs. Dies beeinflusst die Kompilierung der App.
- Zweitens: Führen Sie Laufzeitprüfungen durch, um sicherzustellen, dass nur APIs aufgerufen werden, die auf dem Gerät vorhanden sind, auf dem die App ausgeführt wird.
- Drittens: Testen Sie Ihre App mit der Mindestversion und der Zielversion von Windows10.

## <a name="configure-your-visual-studio-project"></a>Konfigurieren des VisualStudio-Projekts

Im ersten Schritt zur Unterstützung mehrerer Versionen von Windows10 müssen im VisualStudio-Projekt die unterstützten *Ziel*- und *Mindestversionen* des Betriebssystems/SDKs angegeben werden.

- *Ziel*: Die SDK-Version, für die Visual Studio den App-Code kompiliert und alle Tools ausführt. Alle APIs und Ressourcen in dieser SDK-Version stehen zur Kompilierzeit im App-Code zur Verfügung.
- *Minimum*: Die SDK-Version, die die niedrigste Betriebssystemversion unterstützt, unter der Ihre App ausgeführt werden kann (und vom Store bereitgestellt wird), und die Version, für die Visual Studio den Markupcode der App kompiliert. 

Zur Laufzeit wird Ihre App für die Betriebssystemversion ausgeführt, für die sie bereitgestellt wurde. Wenn Sie also Ressourcen verwenden oder APIs aufrufen, die in dieser Version nicht zur Verfügung stehen, löst Ihre App Ausnahmen aus. Weiter unten in diesem Artikel erfahren Sie, wie Sie mithilfe von Laufzeitprüfungen die passenden APIs aufrufen.

Die Einstellungen für Ziel- und Mindestversion geben jeweils das Ende eines Bereichs von Betriebssystem-/SDK-Versionen an. Wenn Sie Ihre App allerdings unter der Mindestversion testen, können Sie sicher sein, dass sie unter jeder beliebigen Version zwischen der Mindest- und Zielversion verwendet werden kann.

> [!TIP]
> Visual Studio gibt im Zusammenhang mit der API-Kompatibilität keine Warnungen aus. Daher müssen Sie selbst testen und sicherstellen, dass Ihre App unter allen Betriebssystemversionen zwischen Mindest- und Zielversion (jeweils einschließlich) erwartungsgemäß funktioniert.

Wenn Sie ein neues Projekt in Visual Studio2015 (Update2 oder höher) erstellen, werden Sie aufgefordert, die von Ihrer App unterstützte Ziel- und Mindestversion festzulegen. Die Zielversion ist standardmäßig die höchste installierte SDK-Version, die Mindestversion die niedrigste installierte SDK-Version. Als Ziel- und Mindestversion stehen nur SDK-Versionen zur Auswahl, die auf Ihrem Computer installiert sind. 

![Festlegen des Ziel-SDKs in Visual Studio](images/vs-target-sdk-1.png)

In der Regel empfiehlt es sich, die Standardeinstellungen beizubehalten. Falls Sie jedoch eine Vorschauversion des SDKs installiert haben und Produktionscode schreiben, empfiehlt es sich, die Zielversion zu ändern und auf die neueste offizielle SDK-Version festzulegen. 

Navigieren Sie zum Ändern der Mindest- und Zielversion für ein bereits in Visual Studio erstelltes Projekt zu „Projekt“> „Eigenschaften“> Registerkarte „Anwendung“> „Ziel“.

![Ändern des Ziel-SDKs in Visual Studio](images/vs-target-sdk-2.png) 

Im Anschluss finden Sie die Buildnummern für die einzelnen SDKs. Weitere Informationen zu Windows10-Updates finden Sie unter [Windows10-Versionsinformationen](https://technet.microsoft.com/windows/release-info) auf TechNet.

Anzeigename | Version | BS/SDK-Build | Notizen
---- | ---- | ---- | ----
RTM | 1507 | 10240 | Lesen Sie die wichtigen [Support](https://support.microsoft.com/help/4015562/windows-10-version-1507-will-no-longer-receive-security-updates)-Informationen.
November-Update | 1511 | 10586 | Lesen Sie die wichtigen [Support](https://support.microsoft.com/help/4035050/windows-10-version-1511-will-no-longer-receive-security-updates)-Informationen.
Anniversary Update | 1607 | 14393 |
Creators Update | 1703 | 15063 |
Fall Creators Update | 1709 | 16299 |

Sie können jede beliebige veröffentlichte Version des SDKs aus dem [WindowsSDK- und Emulator-Archiv](https://developer.microsoft.com/downloads/sdk-archive) herunterladen. Das neueste Windows Insider Preview SDK können Sie im Entwicklerabschnitt der [Windows-Insider-Website](https://insider.windows.com/Home/BuildWithWindows) herunterladen.

## <a name="perform-api-checks"></a>API-Überprüfung

Der Schlüssel für versionsadaptive Apps ist die Kombination von API-Verträgen und [ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation)-Klasse. Mit dieser Klasse können Sie erkennen, ob ein angegebener API-Vertrag, Typ oder Member vorhanden ist, damit Sie API-Aufrufe problemlos über eine Vielzahl von Geräten und Betriebssystemversionen vornehmen können.

### <a name="api-contracts"></a>API-Verträge

Der Satz von APIs in einer Gerätefamilie wird in weitere Untereinheiten aufgeschlüsselt, die als „API-Verträge“ bezeichnet werden. Sie können die **ApiInformation.IsApiContractPresent**-Methode verwenden, um zu testen, ob ein API-Vertrag vorhanden ist. Dies ist hilfreich, wenn Sie das Vorhandensein vieler APIs testen möchten, die alle in der gleichen Version eines API-Vertrags vorhanden sind.

```csharp
    bool isScannerDeviceContract_1_Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1);
```

Was ist ein API-Vertrag? Im Wesentlichen ist ein API-Vertrag ein Feature – eine Gruppe verwandter APIs, die zusammen eine bestimmte Funktionalität bereitstellen. Ein hypothetischer API-Vertrag kann einen Satz von APIs darstellen, mit insgesamt zwei Klassen, fünf Schnittstellen, einer Struktur, zwei Enumerationen usw.

In einem API-Vertrag sind logisch zusammenhängende Typen zusammengefasst. Ab Windows10 ist jede Windows-Runtime-API Mitglied eines API-Vertrags. Mit API-Verträgen überprüfen Sie die Verfügbarkeit eines bestimmten Features oder einer API auf dem Gerät. Diese Überprüfung der Funktionalität eines Gerät ist effektiver als die Suche nach einem bestimmten Gerät oder Betriebssystem. Eine Plattform, die eine API eines API-Vertrags implementiert, muss jede API in diesem API-Vertrag implementieren. Sie können somit testen, ob das ausgeführte Betriebssystem einen bestimmten API-Vertrag unterstützt und, falls das der Fall ist, eine beliebige API aus dem API-Vertrag aufrufen, ohne jede einzeln zu überprüfen.

Die größte und am häufigsten verwendete API-Vertrag ist der **Windows.Foundation.UniversalApiContract**. Es enthält die meisten APIs für die Universelle Windows-Plattform. Die Dokumentation [Erweiterungs-SDKs für Gerätefamilien und API-Verträge](https://docs.microsoft.com/uwp/extension-sdks/) beschreibt die Vielzahl der verfügbaren API-Verträge. In den meisten von ihnen sind APIs mit ähnlicher Funktionalität zusammengefasst.

> [!NOTE]
> Für eine installierte Vorabversion des Windows Software Development Kit (SDK), die noch nicht dokumentiert ist, finden Sie Informationen über die Unterstützung von API-Verträgen in der Datei „Platform.xml” im SDK-Installationsordner unter "\(Programmdateien (x86))\Windows Kits\10\Platforms\<Plattform>\<SDK-Version>\Platform.xml’.

### <a name="version-adaptive-code-and-conditional-xaml"></a>Versionsadaptiver Code und bedingte XAML

In allen Versionen von Windows 10 können Sie die ApiInformation-Klasse in einer Bedingung im Code verwenden, um zu testen, ob die aufzurufende API vorhanden ist. In Ihrem adaptiven Code können Sie verschiedene Methoden der Klasse, z.B. IsTypePresent, IsEventPresent, IsMethodPresent und IsPropertyPresent verwenden, um mit der jeweils erforderliche Granularität nach APIs zu suchen.

Beispiele und weitere Informationen finden Sie unter **[Versionsadaptiver Code](version-adaptive-code.md)**.

Wenn die Mindestversion für Ihre Apps Build 15063 (Creators Update) oder höher ist, können Sie *bedingte XAML* verwenden, um Eigenschaften festzulegen und Objekte zu instanziieren, ohne Code-Behind verwenden zu müssen. Bedingte XAML bietet eine Möglichkeit, die Methode ApiInformation.IsApiContractPresent im Markup zu verwenden.

Weitere Informationen und Beispiele finden Sie unter **[Bedingte XAML](conditional-xaml.md)**.

## <a name="test-your-version-adaptive-app"></a>Testen Ihrer versionsadaptiven App

Wenn Sie versionsadaptiven Code oder bedingten XAML-Code verwenden, um eine versionsadaptive App zu schreiben, müssen Sie diese auf einem Gerät testen, auf dem die Mindestversion ausgeführt wird, und auf einem Gerät unter der Zielversion von Windows10.

Sie können nicht alle bedingten Codepfade auf nur einem Gerät testen. Um sicherzustellen, dass alle Codepfade getestet werden, müssen Sie Ihre App auf einem Remotegerät (oder einem virtuellen Computer) bereitstellen und testen, auf dem die Mindestversion des Betriebssystems ausgeführt wird.
Weitere Informationen über Remotedebugging finden Sie unter [Bereitstellen und Debuggen von UWP-Apps](deploying-and-debugging-uwp-apps.md).

## <a name="related-articles"></a>Verwandte Artikel

- [Was ist eine UWP-App?](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Dynamically detecting features with API contracts (Dynamisches Erkennen von Features mithilfe von API-Verträgen)](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API-Verträge](https://channel9.msdn.com/Events/Build/2015/3-733) (Video für Build 2015)