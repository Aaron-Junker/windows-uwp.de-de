---
Description: Die Microsoft Store erzwingt bestimmte Regeln, die im Zusammenhang mit Versionsnummern, die in verschiedenen Betriebssystemversionen etwas anders funktioniert.
title: Paketversionsnummern
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bf71e5f6dd77da025a50866d32caca2870d3525b
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788425"
---
# <a name="package-version-numbering"></a>Paketversionsnummern

Jedes von Ihnen bereitgestellte Paket muss eine Versionsnummer aufweisen (als Wert im **Version** -Attribut des [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)-Elements im App-Manifest). Die Microsoft Store erzwingt bestimmte Regeln, die im Zusammenhang mit Versionsnummern, die in verschiedenen Betriebssystemversionen etwas anders funktioniert.

> [!NOTE]
> Dieses Thema bezieht sich auf "Pakete", sofern nicht angegeben, die gleichen Regeln gelten jedoch für Versionsnummern für.msix/.appx und.msixbundle/.appxbundle-Dateien.


## <a name="version-numbering-for-windows10-packages"></a>Versionsnummern für Windows 10-Pakete

> [!IMPORTANT]
> Bei Paketen für Windows 10 (UWP) der letzten (vierte) Teil der Versionsnummer ist für Store-Verwendung reserviert und muss als 0 bleiben werden, wenn Sie Ihr Paket erstellen (obwohl der Store den Wert in diesem Abschnitt ändern kann). Die anderen Abschnitte müssen festgelegt werden, um eine ganze Zahl zwischen 0 und 65535 (mit Ausnahme der erste Abschnitt, der nicht 0 sein darf).

Bei der Auswahl eines UWP-Pakets über die veröffentlichten Übermittlung verwendet den Microsoft Store immer die höchste mit versionsverwaltung durch das Paket, das auf Windows 10-Gerät für den Kunden anwendbar ist. Dadurch sind Sie flexibler und haben die Kontrolle darüber, welche Pakete Kunden auf bestimmten Gerätetypen bereitgestellt werden. Außerdem können Sie diese Pakete in beliebiger Reihenfolge übermitteln; Sie sind nicht darauf beschränkt, bei nachfolgenden Übermittlungen Pakete mit höheren Versionsnummern bereitzustellen.

Sie können mehrere UWP-Paketen mit der gleichen Versionsnummer angeben. Pakete mit der gleichen Versionsnummer können jedoch nicht dieselbe Architektur aufweisen, da die vollständige Identität eindeutig sein muss, die der Store für die einzelnen Pakete verwendet. Weitere Informationen finden Sie unter [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Wenn Sie mehrere UWP-Pakete, die die gleiche Versionsnummer verwenden bereitstellen, wird die Architektur (in der Reihenfolge X64, x 86, ARM, Neutral) verwendet werden, zu entscheiden, welche der höheren Rang ist (wenn der Store, welches Paket eine das Gerät eines Kunden anbieten bestimmt). Beim Bewerten von App-Bündeln mit gleicher Versionsnummer gilt der höchste Architekturrang im Bündel: ein App-Bündel, das ein x64-Paket enthält, besitzt einen höheren Rang als ein App-Bündel, das lediglich ein x86-Paket enthält.

Dies bietet Ihnen ein hohes Maß an Flexibilität, um Ihre App im Laufe der Zeit weiterzuentwickeln. Sie können das Hochladen und übermitteln Sie neue Pakete, die vom niedrigere Version Zahlen zu verwenden, zum Hinzufügen von Unterstützung für Windows 10-Geräte, die Sie zuvor nicht unterstützte können Sie höhere mit versionsverwaltung durch das Pakete mit strengeren Abhängigkeiten Hardware oder die Funktionen des Betriebssystems, oder Sie nutzen hinzufügen können höher systemversionsverwaltung Pakete hinzufügen, die, die als Updates für einige oder alle Ihre vorhandenen Kunden Basis dienen.

Das folgende Beispiel veranschaulicht, wie Versionsnummern verwaltet werden können, um Ihren Kunden über mehrere Übermittlungen die beabsichtigten Pakete bereitzustellen.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Beispiel: Übermittlung mehrerer Übersetzungen verschieben zu einem einzelnen Paket

Windows 10 ermöglicht Ihnen das Schreiben von eine einzelnen Codebasis, die überall ausgeführt wird. Dadurch wird das Starten eines neuen plattformübergreifenden Projekts sehr viel einfacher. Wegen einer Reihe von Gründen möchten Sie jedoch vielleicht die vorhandene Codebasis nicht zusammenführen, um direkt ein einzelnes Projekt zu erstellen.

Sie können die Paket-Versionsregeln verwenden, verschieben nach und nach Ihren Kunden zu einem einzelnen Paket für die universelle Gerätefamilie, beim Protokollversand einer Reihe von vorläufigen Updates für bestimmte gerätefamilien (einschließlich jener, die Windows 10-APIs nutzen). Das folgende Beispiel veranschaulicht, wie die gleichen Regeln für eine Reihe von Übermittlungen für dieselbe app konsistent angewendet werden.

| Übermittlung | Inhalt                                                  | Benutzerfreundlichkeit                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -Paket Version: 1.1.10.0 <br> -Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Paket Version: 1.1.0.0 <br> -Gerätefamilie: Windows.Mobile, minVersion 10.0.10240.0     | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.0.0 <br> -   Für andere Gerätefamilien kann die App nicht erworben und installiert werden. |
| 2          | -Paket Version: 1.1.10.0 <br> -Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Paket Version: 1.1.0.0 <br> -Gerätefamilie: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -Paket Version: 1.0.0.0 <br> -Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0    | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.0.0 <br> -   Andere (nicht-Desktop, nicht mobile) Gerätefamilien, erhalten nach Einführung Versionsnummer 1.0.0.0 <br> -   Bei Desktop- und mobilen Geräten, auf denen die App bereits installiert ist, wird kein Updates angezeigt (da diese bereits die beste verfügbare Version besitzen – 1.1.10.0 und 1.1.0.0 sind beide höher als 1.0.0.0) |
| 3          | -Paket Version: 1.1.10.0 <br> -Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Paket Version: 1.1.5.0 <br> -Gerätefamilie: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -Paket Version: 1.0.0.0 <br> -Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0    | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10250.0 und höher erhalten Versionsnummer 1.1.5.0 <br> -   Geräte unter Windows 10 Mobile Build >= 10.0.10240.0 und < 10.010250.0 erhalten Versionsnummer 1.1.0.0 
| 4          | -Paket Version: 2.0.0.0 <br> -Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0   | -   Alle Kunden mit allen Gerätefamilien unter Windows 10 Build v10.0.10240.0 und höher erhalten Paket 2.0.0.0 | 

> [!NOTE]
>  In allen Fällen erhalten Kundengeräte das Paket mit der höchsten möglichen Versionsnummer, der sie qualifiziert sind. In der dritten Übermittlung oben erhalten beispielsweise alle Desktopgeräte v1.1.10.0, auch wenn sie die Betriebssystemversion 10.0.10250.0 oder höher haben und somit auch mit v1.1.5.0 kompatibel wären. Da 1.1.10.0 für sie die höchsten Versionsnummer ist, erhalten sie dieses Paket.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Verwenden der Versionsnummern zum Durchführen eines Rollbacks auf ein vorheriges Paket für Neuanschaffungen

Wenn Sie Kopien von Paketen halten, müssen Sie die Option zum Zurücksetzen Ihrer app-Paket in den Store zu einem früheren Windows 10-Paket, wenn Sie Probleme mit einer Version erkennen soll. Dies ist eine temporäre Möglichkeit, die die Unterbrechungen für Ihre Kunden zu begrenzen, während Sie dauert das Problem zu beheben.

Zu diesem Zweck erstellen Sie ein neues [Übermittlung](app-submissions.md). Entfernen Sie das problematische Paket und laden Sie das alte Paket hoch, das Sie im Store bereitstellen möchten. Kunden, die bereits das Paket erhalten haben, für das Sie einen Rollback durchführen, weisen immer noch das problematische Paket auf (da das ältere Paket eine frühere Versionsnummer besitzt). Dadurch wird verhindert, dass jemand das problematische Paket erhält, und die App ist im Store weiterhin verfügbar.

Zum Beheben des Problems für Kunden, die bereits das problematische Paket erhalten haben, können Sie ein neues Windows 10-Paket übermitteln, die eine höhere Versionsnummer als das fehlerhafte Paket bald wie möglich. Danach durchläuft die Übermittlung den Zertifizierungsprozess, und alle Kunden werden auf das neue Paket aktualisiert, da es eine höhere Versionsnummer aufweist.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Versionsnummern für Windows 8.1 (und früher) und Windows Phone 8.1-Paketen

> [!IMPORTANT]
> Ab 31. Oktober 2018 keine Produkte neu erstellten Pakete mit dem Ziel Windows 8.x/Windows enthalten Phone 8.x oder früher. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Bei APPX-Paketen für Windows Phone 8.1-Pakete muss die Versionsnummer des Pakets in einer neuen Übermittlung immer höher sein als die des in der letzten Übermittlung (oder vorherigen Übermittlung) enthaltenen Pakets.

Für AppX-Pakete, die Windows 8 und Windows 8.1 als Ziel, das gleiche gilt pro Architektur: die Versionsnummer des Pakets in eine neue Übermittlung muss immer größer als der das Paket zuletzt auf den Store für die gleiche Architektur veröffentlicht sein.

Darüber hinaus muss die Versionsnummer des Windows 8.1-Paketen immer größer als die Versionsnummern aller Ihrer Windows 8-Pakete für die gleiche app sein. Das heißt, muss alle Windows 8-Pakete, die Sie senden die Versionsnummer niedriger als die Versionsnummer der ein Paket für Windows 8.1, die Sie für dieselbe app übermittelt haben.

> [!NOTE]
> Wenn Ihre app ebenfalls Windows 10-Pakete enthält, muss die Versionsnummer der Windows 10-Pakete höher als die für Ihre Windows 8, Windows 8.1 und/oder Windows Phone 8.1-Pakete. Weitere Informationen finden Sie unter [Hinzufügen von Paketen für Windows 10 für eine app zuvor veröffentlichte](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Hier sind einige Beispiele für Vorgänge in anderen Version Anzahl Updateszenarien für Pakete, die für Windows 8 und Windows 8.1.

| Version der App im Store  | Hochgeladene Version | Nachdem die neue Version in den Store hochgeladen wurde, wird sie als Neukauf installiert. | Nachdem die neue Version in den Store hochgeladen wurde, wird sie aktualisiert, wenn der Kunde die App bereits besitzt. |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nichts                                     | x86, v1.0.0.0               | x86, v1.0.0.0 auf x86- und x64-PCs                                                | Keine. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 für die Architektur des Kunden                                                   | Keine. Die Versionsnummern sind identisch. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 für Kunden mit einem x86-PC <br> v1.0.0.1 für Kunden mit einem x64-PC                 | Nichts für Kunden, die die App auf einem x86-PC ausführen. <br> v1.0.0.0 wird für Kunden, die die App auf einem x64-PC ausführen, auf v1.0.0.1 aktualisiert. <br> **Beachten Sie**  Wenn die Version der app, auf x X64 ausgeführt wird X86 Computer, die app wird nicht aktualisiert, um die X64 Version, wenn der Kunde deinstalliert und neu installiert werden. |
| Nichts                                     | Neutral, v1.0.0.1           | Neutral, v1.0.0.1 auf allen PCs                                                         | Keine. |
| Neutral, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 für die Architektur des PC des Kunden.          | Keine. Kunden mit der neutralen Version v1.0.0.1 verwenden weiter diese Version der App. |
| Neutral, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 für die Architektur des PC des Kunden. | Nichts für Kunden mit der neutralen Version v1.0.0.1. <br> v1.0.0.0 wird für Kunden, die die Version v1.0.0.0 der App für die spezifische Architektur ihres PC verwenden, auf v1.0.0.1 aktualisiert. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 für die Architektur des PC des Kunden.  | v1.0.0.1 wird für Kunden, die die Version v1.0.0.1 der App für die spezifische Architektur ihres PC verwenden, auf v1.0.0.2 aktualisiert. |

> [!NOTE]
> Im Gegensatz zu APPX-Paketen werden die Versionsnummern in allen XAP-Paketen beim Ermitteln der für einen gegebenen Kunden bereitzustellenden Pakete ignoriert. Um ein Kunde von einem XAP-Paket auf ein neueres zu aktualisieren, stellen Sie sicher, dass die ältere XAP-Datei in der neuen Übermittlung entfernt wird.
