---
Description: Der Microsoft Store erzwingt bestimmte Regeln im Zusammenhang mit Versionsnummern, die in unterschiedlichen Betriebssystemversionen etwas unterschiedlich funktionieren.
title: Paketversionsnummern
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b09c5688fd8a043d1a4ca1783af046398b504050
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219733"
---
# <a name="package-version-numbering"></a>Paketversionsnummern

Jedes von Ihnen bereitgestellte Paket muss eine Versionsnummer aufweisen (als Wert im **Version** -Attribut des [Package/Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)-Elements im App-Manifest). Der Microsoft Store erzwingt bestimmte Regeln im Zusammenhang mit Versionsnummern, die in unterschiedlichen Betriebssystemversionen etwas unterschiedlich funktionieren.

> [!NOTE]
> Dieses Thema bezieht sich auf "Pakete", aber sofern nicht anders angegeben, gelten die gleichen Regeln auch für Versionsnummern sowohl für msix-, AppX-und msixbundle-/appxbundle-Dateien.


## <a name="version-numbering-for-windows10-packages"></a>Versionsnummern für Windows 10-Pakete

> [!IMPORTANT]
> Bei Windows 10-Paketen (UWP) ist der letzte (vierte) Abschnitt der Versionsnummer für die Verwendung durch den Speicher reserviert und muss beim Erstellen des Pakets als 0 belassen werden (obwohl der Speicher den Wert in diesem Abschnitt ändern kann). Die anderen Abschnitte müssen auf eine ganze Zahl zwischen 0 und 65535 festgelegt werden (mit Ausnahme des ersten Abschnitts, der nicht 0 sein kann).

Wenn Sie ein UWP-Paket aus ihrer veröffentlichten Übermittlung auswählen, wird für das Microsoft Store immer das Paket mit der höchsten Versions Angabe verwendet, das für das Windows 10-Gerät des Kunden anwendbar ist. Dadurch sind Sie flexibler und haben die Kontrolle darüber, welche Pakete Kunden auf bestimmten Gerätetypen bereitgestellt werden. Außerdem können Sie diese Pakete in beliebiger Reihenfolge übermitteln; Sie sind nicht darauf beschränkt, bei nachfolgenden Übermittlungen Pakete mit höheren Versionsnummern bereitzustellen.

Sie können mehrere UWP-Pakete mit der gleichen Versionsnummer bereitstellen. Pakete mit der gleichen Versionsnummer können jedoch nicht dieselbe Architektur aufweisen, da die vollständige Identität eindeutig sein muss, die der Store für die einzelnen Pakete verwendet. Weitere Informationen finden Sie unter [**Identity**](/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Wenn Sie mehrere UWP-Pakete bereitstellen, die dieselbe Versionsnummer verwenden, wird die-Architektur (in der Reihenfolge x64, x86, arm, neutral) verwendet, um zu entscheiden, welcher Wert höher ist (wenn der Speicher bestimmt, welches Paket für das Gerät eines Kunden bereitgestellt werden soll). Beim Bewerten von App-Bündeln mit gleicher Versionsnummer gilt der höchste Architekturrang im Bündel: ein App-Bündel, das ein x64-Paket enthält, besitzt einen höheren Rang als ein App-Bündel, das lediglich ein x86-Paket enthält.

Dies bietet Ihnen ein hohes Maß an Flexibilität, um Ihre App im Laufe der Zeit weiterzuentwickeln. Sie können neue Pakete, die niedrigere Versionsnummern verwenden, hochladen und übermitteln, um Unterstützung für Windows 10-Geräte hinzuzufügen, die Sie zuvor nicht unterstützt haben. Sie können Pakete mit höherer Versions Angabe hinzufügen, die strengere Abhängigkeiten aufweisen, um die Vorteile von Hardware-oder Betriebssystemfunktionen zu nutzen, oder Sie können Pakete mit höherer Versionsverwaltung hinzufügen

Das folgende Beispiel veranschaulicht, wie Versionsnummern verwaltet werden können, um Ihren Kunden über mehrere Übermittlungen die beabsichtigten Pakete bereitzustellen.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Beispiel: Wechsel zu einem einzelnen Paket über mehrere Übermittlungen

Mit Windows 10 können Sie eine einzelne Codebasis schreiben, die überall ausgeführt werden kann. Dadurch wird das Starten eines neuen plattformübergreifenden Projekts sehr viel einfacher. Wegen einer Reihe von Gründen möchten Sie jedoch vielleicht die vorhandene Codebasis nicht zusammenführen, um direkt ein einzelnes Projekt zu erstellen.

Sie können die Paket-Versionsnummernregeln verwenden, um den Wechsel Ihrer Kunden nach und nach auf ein einzelnes Paket für die universellen Gerätefamilie vorzunehmen. Dabei werden eine Reihe von vorläufigen Aktualisierungen für bestimmte Gerätefamilien (einschließlich derjenigen, die die Windows 10-APIs nutzen) bereitgestellt. Im folgenden Beispiel wird veranschaulicht, wie dieselben Regeln konsistent auf eine Reihe von Übermittlungen für dieselbe App angewendet werden.

| Übermittlung | Inhalte                                                  | Benutzerfreundlichkeit                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   Paketversion: 1.1.10.0 <br> -   Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.1.0.0 <br> -   Gerätefamilie: Windows.Mobile, minVersion 10.0.10240.0     | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.0.0 <br> -   Für andere Gerätefamilien kann die App nicht erworben und installiert werden. |
| 2          | -   Paketversion: 1.1.10.0 <br> -   Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.1.0.0 <br> -   Gerätefamilie: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.0.0.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0    | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.0.0 <br> -   Andere (nicht-Desktop, nicht mobile) Gerätefamilien, erhalten nach Einführung Versionsnummer 1.0.0.0 <br> -   Bei Desktop- und mobilen Geräten, auf denen die App bereits installiert ist, wird kein Updates angezeigt (da diese bereits die beste verfügbare Version besitzen – 1.1.10.0 und 1.1.0.0 sind beide höher als 1.0.0.0) |
| 3          | -   Paketversion: 1.1.10.0 <br> -   Gerätefamilie: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Paketversion: 1.1.5.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   Paketversion: 1.0.0.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0    | -   Geräte unter Windows 10 Desktop Build 10.0.10240.0 und höher erhalten Versionsnummer 1.1.10.0 <br> -   Geräte unter Windows 10 Mobile Build 10.0.10250.0 und höher erhalten Versionsnummer 1.1.5.0 <br> -   Geräte unter Windows 10 Mobile Build >= 10.0.10240.0 und < 10.010250.0 erhalten Versionsnummer 1.1.0.0 
| 4          | -   Paketversion: 2.0.0.0 <br> -   Gerätefamilie: Windows.Universal, minVersion 10.0.10240.0   | -   Alle Kunden mit allen Gerätefamilien unter Windows 10 Build v10.0.10240.0 und höher erhalten Paket 2.0.0.0 | 

> [!NOTE]
>  In allen Fällen erhalten Kunden Geräte das Paket mit der höchstmöglichen Versionsnummer, für die Sie qualifiziert sind. In der dritten Übermittlung oben erhalten beispielsweise alle Desktopgeräte v1.1.10.0, auch wenn sie die Betriebssystemversion 10.0.10250.0 oder höher haben und somit auch mit v1.1.5.0 kompatibel wären. Da 1.1.10.0 für sie die höchsten Versionsnummer ist, erhalten sie dieses Paket.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Verwenden der Versionsnummern zum Durchführen eines Rollbacks auf ein vorheriges Paket für Neuanschaffungen

Wenn Sie Kopien der Pakete aufbewahren, haben Sie die Möglichkeit, das Paket Ihrer APP im Store auf ein früheres Windows 10-Paket zurückzusetzen, wenn Sie Probleme mit einem Release entdecken sollten. Dies ist eine temporäre Methode, um die Unterbrechung auf Ihre Kunden zu beschränken, während Sie sich Zeit nehmen, das Problem zu beheben.

Erstellen Sie zu diesem Zweck eine neue [Übermittlung](app-submissions.md). Entfernen Sie das problematische Paket und laden Sie das alte Paket hoch, das Sie im Store bereitstellen möchten. Kunden, die bereits das Paket erhalten haben, für das Sie einen Rollback durchführen, weisen immer noch das problematische Paket auf (da das ältere Paket eine frühere Versionsnummer besitzt). Dadurch wird verhindert, dass jemand das problematische Paket erhält, und die App ist im Store weiterhin verfügbar.

Um das Problem für die Kunden zu beheben, die das problematische Paket bereits erhalten haben, können Sie ein neues Windows 10-Paket mit einer höheren Versionsnummer als das ungültige Paket übermitteln, sobald dies möglich ist. Danach durchläuft die Übermittlung den Zertifizierungsprozess, und alle Kunden werden auf das neue Paket aktualisiert, da es eine höhere Versionsnummer aufweist.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Versionsnummern für Windows 8.1 (und früher) und Windows Phone 8.1-Pakete

> [!IMPORTANT]
> Sie können keine neuen XAP-Pakete mehr hochladen, die mit den Windows Phone 8. x SDK (s) erstellt wurden. Apps, die bereits mit XAP-Paketen im Speicher gespeichert sind, funktionieren weiterhin auf Windows 10 Mobile-Geräten. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Bei APPX-Paketen für Windows Phone 8.1-Pakete muss die Versionsnummer des Pakets in einer neuen Übermittlung immer höher sein als die des in der letzten Übermittlung (oder vorherigen Übermittlung) enthaltenen Pakets.

Für AppX-Pakete, die auf Windows 8 und Windows 8.1 abzielen, gilt die gleiche Regel pro Architektur: die Versionsnummer des Pakets in einer neuen Übermittlung muss immer größer sein als der des Pakets, das zuletzt im Speicher für die gleiche Architektur veröffentlicht wurde.

Außerdem muss die Versionsnummer von Windows 8.1-Paketen stets höher sein als die Versionsnummern aller Windows 8-Pakete für dieselbe App. Mit anderen Worten: Die Versionsnummer eines von Ihnen übermittelten Windows 8-Pakets muss niedriger sein als die Versionsnummer eines Windows 8.1-Paket, das Sie für dieselbe App übermittelt haben.

> [!NOTE]
> Wenn Ihre APP auch über Windows 10-Pakete verfügt, muss die Versionsnummer der Windows 10-Pakete höher sein als die für die Windows 8-, Windows 8.1-und/oder Windows Phone 8,1-Pakete. Weitere Informationen finden Sie unter [Hinzufügen von Paketen für Windows 10 zu einer zuvor veröffentlichten App](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Im folgenden finden Sie einige Beispiele dafür, was in verschiedenen Versionsnummern-Update Szenarios für Pakete mit Windows 8 und Windows 8.1 geschieht.

| Version der App im Store  | Hochgeladene Version | Nachdem sich die neue Version im Speicher befindet, wird Sie in einem neuen Erwerb installiert. | Nachdem sich die neue Version im Speicher befindet, wird Sie aktualisiert, wenn die APP bereits von einem Kunden verwendet wird. |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nothing                                     | x86, v1.0.0.0               | x86, v1.0.0.0 auf x86- und x64-PCs                                                | Nichts. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 für die Architektur des Kunden                                                   | Nichts. Die Versionsnummern sind identisch. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 für Kunden mit einem x86-PC <br> v1.0.0.1 für Kunden mit einem x64-PC                 | Nichts für Kunden, die die App auf einem x86-PC ausführen. <br> v1.0.0.0 wird für Kunden, die die App auf einem x64-PC ausführen, auf v1.0.0.1 aktualisiert. <br> **Hinweis**    Wenn die x86-Version der APP auf einem x64-Computer ausgeführt wird, wird die APP erst dann auf die x64-Version aktualisiert, wenn der Kunde deinstalliert und neu installiert wird. |
| Nothing                                     | Neutral, v1.0.0.1           | Neutral, v1.0.0.1 auf allen PCs                                                         | Nichts. |
| Neutral, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 für die Architektur des PC des Kunden.          | Nichts. Kunden mit der neutralen Version v1.0.0.1 verwenden weiter diese Version der App. |
| Neutral, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 für die Architektur des PC des Kunden. | Nichts für Kunden mit der neutralen Version v1.0.0.1. <br> v1.0.0.0 wird für Kunden, die die Version v1.0.0.0 der App für die spezifische Architektur ihres PC verwenden, auf v1.0.0.1 aktualisiert. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 für die Architektur des PC des Kunden.  | v1.0.0.1 wird für Kunden, die die Version v1.0.0.1 der App für die spezifische Architektur ihres PC verwenden, auf v1.0.0.2 aktualisiert. |

> [!NOTE]
> Anders als AppX-Pakete werden die Versionsnummern in XAP-Paketen nicht berücksichtigt, wenn bestimmt wird, welches Paket einem bestimmten Kunden bereitgestellt werden soll. Um ein Kunde von einem XAP-Paket auf ein neueres zu aktualisieren, stellen Sie sicher, dass die ältere XAP-Datei in der neuen Übermittlung entfernt wird.
