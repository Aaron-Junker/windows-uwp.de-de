---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: In diesem Abschnitt wird beschrieben, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) PlayReady-geschützte Medieninhalte hinzufügen.
title: PlayReady DRM
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e82a0ffc0ddcf2ac1973ba446ec50dfc61a7cd1a
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361569"
---
# <a name="playready-drm"></a>PlayReady DRM



In diesem Abschnitt wird beschrieben, wie Sie Ihrer UWP-App (Universelle Windows-Plattform) PlayReady-geschützte Medieninhalte hinzufügen.

PlayReady DRM ermöglicht Entwicklern das Erstellen von UWP-Apps, die PlayReady-geschützte Inhalte für den Benutzer bereitstellen und gleichzeitig vom Inhaltsanbieter definierte Regeln erzwingen können. Dieser Abschnitt beschreibt die Änderungen an den Microsoft PlayReady-DRM für Windows 10 "und" Vorgehensweise: Ändern Sie Ihre PlayReady-UWP-app, um die Änderungen, die aus der vorherigen Version von Windows 8.1, Windows 10-Version zu unterstützen.
 
| Thema                                                                     | Beschreibung                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Hardware DRM](hardware-drm.md)                                           | Dieses Thema enthält eine Übersicht über das Hinzufügen der hardwarebasierten Verwaltung digitaler Rechte (Digital Rights Management, DRM) mit PlayReady zu Ihrer UWP-App.                                                                                                                                                                 |
| [Adaptive streaming mit PlayReady](adaptive-streaming-with-playready.md) | In diesem Artikel wird beschrieben, wie Sie einer UWP-App (Universelle Windows-Plattform) adaptives Streaming von Multimediainhalten mit Microsoft PlayReady-Inhaltsschutz hinzufügen. Dieses Feature unterstützt derzeit die Wiedergabe von Inhalten über HTTP Live Streaming (HLS) und Dynamic Streaming over HTTP (DASH). |

## <a name="whats-new-in-playready-drm"></a>Neuigkeiten bei PlayReady DRM

Die folgende Liste beschreibt die neuen Features und Änderungen an der PlayReady-DRM für Windows 10.

-   Hardwarebasierte Verwaltung digitaler Rechte (Hardware Digital Rights Management, HWDRM) wurde hinzugefügt.

    Die Unterstützung für hardwarebasierten Inhaltsschutz ermöglicht die sichere Wiedergabe von High-Definition (HD)- und Ultra-High-Definition (UHD)-Inhalten auf mehreren Geräteplattformen. Schlüsselmaterial (einschließlich privater Schlüssel, Inhaltsschlüssel und anderer Schlüsselmaterialien zum Ableiten oder Entsperren dieser Schlüssel) sowie entschlüsselte komprimierte und nicht komprimierte Videobeispiele werden durch Hardwaresicherheit geschützt. Bei Verwendung des Hardware-DRM sind unbekannte Aktivierungen (Wiedergabe auf unbekannter Ausgabe/Wiedergabe auf unbekannter Ausgabe mit „downres”) irrelevant, da die HWDRM-Pipeline die verwendete Ausgabe immer kennt. Weitere Informationen finden Sie unter [Hardware-DRM](hardware-drm.md).

-   PlayReady ist keine AppX-Framework-Komponente mehr, sondern stattdessen eine integrierte Betriebssystemkomponente. Der Namespace wurde von **Microsoft.Media.PlayReadyClient** in [**Windows.Media.Protection.PlayReady**](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady) geändert.
-   Die folgenden Header definieren die PlayReady-Fehlercodes sind jetzt Bestandteil des Windows Software Development Kit (SDK): Windows.Media.Protection.PlayReadyErrors.h und Windows.Media.Protection.PlayReadyResults.h.
-   Unterstützt das proaktive Abrufen nicht persistenter Lizenzen.

    Das proaktive Abrufen nicht persistenter Lizenzen wurde von Vorgängerversionen von PlayReady DRM nicht unterstützt. Diese Funktion wurde in dieser Version hinzugefügt. Mit ihrer Hilfe kann die Zeit bis zum ersten Frame verkürzt werden. Weitere Informationen finden Sie unter [Proaktives Abrufen einer nicht persistenten Lizenz vor der Wiedergabe](#proactively-acquire-a-non-persistent-license-before-playback).

-   Unterstützt das Abrufen mehrerer Lizenzen in einer Nachricht.

    Ermöglicht der Client-App das Abrufen mehrerer nicht persistenter Lizenzen in einer Lizenzabrufnachricht. Da Lizenzen für mehrere Inhalte abgerufen werden, während der Benutzer noch die Inhaltsbibliothek durchsucht, kann dies die Zeit bis zum ersten Frame verkürzen. Diese Funktion verhindert, dass beim Abrufen der Lizenz eine Verzögerung erfolgt, wenn der Benutzer die wiederzugebenden Inhalte auswählt. Darüber hinaus können Audio- und Videodatenströme verschlüsselt werden, um Schlüssel zu trennen, indem ein Inhaltsheader aktiviert wird, der mehrere Schlüsselkennungen (KIDs) enthält. Dadurch können mit einem einzigen Lizenzabruf alle Lizenzen für alle Datenströme in einer Inhaltsdatei abgerufen werden, anstatt zu diesem Zweck benutzerdefinierte Logik und mehrere Lizenzabrufanforderungen verwenden zu müssen.

-   Unterstützung für den Echtzeitablauf, d. h. einer Lizenz mit begrenzter Dauer (Limited Duration License, LDL), wurde hinzugefügt.

    Bietet die Möglichkeit, einen Echtzeitablauf für Lizenzen festzulegen und während der Wiedergabe einen reibungslosen Wechsel zwischen einer ablaufenden Lizenz und einer anderen (gültigen) Lizenz sicherzustellen. In Kombination mit dem Abruf mehrerer Lizenzen in einer Nachricht kann eine App so mehrere LDLs asynchron abrufen, während der Benutzer noch die Inhaltsbibliothek durchsucht, und nur eine Lizenz mit längerer Dauer abrufen, sobald der Benutzer den wiederzugebenden Inhalt ausgewählt hat. Die Wiedergabe wird dann schneller gestartet (weil bereits eine Lizenz verfügbar ist), und da die App beim Ablauf der LDL bereits eine Lizenz mit längerer Dauer abgerufen hat, wird die Wiedergabe unterbrechungsfrei bis zum Ende des Inhalts fortgesetzt.

-   Nicht persistente Lizenzketten wurden hinzugefügt.
-   Unterstützung für zeitbasierte Einschränkungen (einschließlich Ablauf, Ablauf nach der ersten Wiedergabe und Echtzeitablauf) für nicht persistente Lizenzen wurde hinzugefügt.
-   Richtlinienunterstützung für HDCP Typ 1 (Version 2.2 unter Windows 10) wurde hinzugefügt.

    Weitere Informationen finden Sie unter [Zu berücksichtigende Informationen](#things-to-consider).

-   Miracast ist jetzt als Ausgabe implizit.
-   Sicheres Beenden wurde hinzugefügt.

    Dank des sicheren Beendens kann ein PlayReady-Gerät einem Medienstreamingdienst zuverlässig bestätigen, dass die Medienwiedergabe für einen bestimmten Inhalt beendet wurde. Mit dieser Funktion wird sichergestellt, dass Ihre Medienstreamingdienste präzise Erzwingung und Berichte zu Nutzungseinschränkungen auf verschiedenen Geräten für ein bestimmtes Konto bereitstellen.

-   Audio- und Videolizenztrennung wurde hinzugefügt.

    Separate Spuren verhindern, dass Videos als Audio decodiert werden. Dadurch wird ein stabilerer Inhaltsschutz gewährleistet. Neue Standards erfordern separate Schlüssel für Audio- und Videospuren.

-   MaxResDecode-Feature hinzugefügt.

    Dieses Feature wurde hinzugefügt, um die Wiedergabe von Inhalten auf eine maximale Auflösung zu beschränken, auch wenn der Benutzer einen leistungsfähigeren Schlüssel (aber keine Lizenz) besitzt. Dadurch werden Szenarien unterstützt, in denen mehrere Datenstromgrößen mit einem einzelnen Schlüssel codiert werden.

PlayReady DRM wurden die folgenden neuen Schnittstellen, Klassen und Enumerationen hinzugefügt:

-   [**IPlayReadyLicenseAcquisitionServiceRequest**](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady.IPlayReadyLicenseAcquisitionServiceRequest) interface
-   [**IPlayReadyLicenseSession** ](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady.IPlayReadyLicenseSession) Schnittstelle
-   [**IPlayReadySecureStopServiceRequest**](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady.IPlayReadySecureStopServiceRequest) interface
-   [**PlayReadyLicenseSession** ](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady.PlayReadyLicenseSession) Klasse
-   [**PlayReadySecureStopIterable** ](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady.PlayReadySecureStopIterable) Klasse
-   [**PlayReadySecureStopIterator**](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady.PlayReadySecureStopIterator) class
-   [**PlayReadyHardwareDRMFeatures**](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady.PlayReadyHardwareDRMFeatures) enumerator

Es wurde ein neues Beispiel erstellt, um die Verwendung der neuen Features von PlayReady DRM zu veranschaulichen. Das Beispiel können Sie hier [https://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](https://go.microsoft.com/fwlink/p/?linkid=331670) herunterladen.

## <a name="things-to-consider"></a>Zu berücksichtigende Informationen

-   PlayReady DRM unterstützt jetzt HDCP Typ 1 (unterstützt in HDCP-Version 2.1 oder höher). PlayReady enthält in der Lizenz eine zu erzwingende HDCP-Typeinschränkungsrichtlinie für das Gerät. Unter Windows 10 erzwingt diese Richtlinie die Einbindung von HDCP 2.2 oder höher. Dieses Feature kann in der PlayReady Server v3.0 SDK-Lizenz aktiviert werden. (Der Server steuert diese Richtlinie in der Lizenz mit der GUID der HDCP-Typeinschränkung). Weitere Informationen finden Sie unter [PlayReady-Kompatibilität und Stabilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).
-   Windows Media Video (auch bekannt als VC-1) wird nicht vom Hardware-DRM unterstützt (siehe [Außerkraftsetzen des Hardware-DRM](hardware-drm.md#override-hardware-drm)).
-   PlayReady DRM unterstützt jetzt den High Efficiency Video Coding (HEVC/H.265)-Videokomprimierungsstandard. Zur Unterstützung von HEVC muss Ihre App Inhalte mit Common Encryption Scheme (CENC) Version 2 verwenden, d. h. die Slice-Header des Inhalts bleiben unverschlüsselt. Finden Sie in ISO/IEC 23001-7-Informationstechnologie – MPEG Systems Technologies – Teil 7: Common Encryption in ISO base Media-Datei-Format-Dateien (ISO/IEC 23001-Spec-Version-7:2015 oder höher ist erforderlich.) Weitere Informationen. Microsoft empfiehlt außerdem die Verwendung von CENC Version 2 für alle HWDRM-Inhalte. Zudem wird HEVC nicht von allen Hardware-DRM-Typen unterstützt (siehe [Außerkraftsetzen des Hardware-DRM](hardware-drm.md#override-hardware-drm)).
-   Um bestimmte neue Features von PlayReady 3.0 nutzen zu können (u. a. SL3000 für hardwarebasierte Clients, Abruf mehrerer nicht persistenter Lizenzen in einer Lizenzabrufnachricht und zeitbasierte Einschränkungen für nicht persistente Lizenzen), muss für den PlayReady-Server Microsoft PlayReady Server Software Development Kit v3.0.2769 oder höher verwendet werden.
-   Je nachdem, welche Ausgabeschutzrichtlinie in der Inhaltslizenz festgelegt ist, kann die Medienwiedergabe für Endbenutzer fehlschlagen, wenn die verbundene Ausgabe der Benutzer diese Anforderungen nicht erfüllt. In der folgenden Tabelle sind die Fehler aufgeführt, die in diesem Zusammenhang häufig auftreten. Weitere Informationen finden Sie unter [PlayReady-Kompatibilität und Stabilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).

| Fehler                                                   | Wert      | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | Die Ausgabeschutzrichtlinie der Lizenz erfordert, dass der Monitor HDCP einbindet, die Einbindung war aber nicht möglich.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_RICHTLINIE\_NICHT UNTERSTÜTZT                              | 0xC00D7159 | Die Ausgabeschutzrichtlinie der Lizenz erfordert, dass der Monitor HDCP Typ 1 einbindet, die Einbindung war aber nicht möglich.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_AUSGABE\_PROTECTION\_ANFORDERUNGEN\_NICHT\_MET | 0x8004CD22 | Dieser Fehlercode tritt nur bei der Verwendung des Hardware-DRM auf. Die Ausgabeschutzrichtlinie der Lizenz erfordert, dass der Monitor HDCP einbindet oder die effektive Auflösung des Inhalts verringert, die Einbindung von HDCP war jedoch nicht möglich, und die effektive Auflösung des Inhalts konnte nicht verringert werden, weil das Hardware-DRM die Verringerung der effektiven Auflösung von Inhalten nicht unterstützt. Bei Verwendung des Software-DRM wird der Inhalt wiedergegeben. Weitere Informationen finden Sie unter [Überlegungen zur Verwendung des Hardware-DRM](hardware-drm.md#considerations-for-using-hardware-drm). |
| FEHLER\_GRAFIKEN\_OPM\_NICHT\_UNTERSTÜTZT                    | 0xc0262500 | Der Grafiktreiber unterstützt Ausgabeschutz nicht. Dies kann z. B. der Fall sein, wenn der Monitor über VGA verbunden ist oder kein entsprechender Grafiktreiber für die digitale Ausgabe installiert ist. In letzterem Fall ist in der Regel der Treiber „Microsoft Basic Display Adapter” installiert, und das Problem kann durch Installieren eines entsprechenden Grafiktreibers behoben werden.                                                                                                                                                  |

## <a name="output-protection"></a>Ausgabeschutz

Im folgenden Abschnitt wird das Verhalten bei Verwendung von PlayReady DRM für Windows 10 mit Ausgabeschutzrichtlinien in einer PlayReady-Lizenz beschrieben.

PlayReady DRM unterstützt die Ausgabeschutzebenen in der **erweiterbaren Medienrechtespezifikation von Microsoft PlayReady**. Dieses Dokument ist Teil des Dokumentationspakets, das zusammen mit lizenzierten PlayReady-Produkten bereitgestellt wird.

> [!NOTE]
> Die zulässigen Werte für Ausgabesicherheitsebenen, die von einem Lizenzserver festgelegt werden können, unterliegen den [PlayReady-Kompatibilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).

PlayReady DRM ermöglicht die Wiedergabe von Inhalten mit Ausgabeschutzrichtlinien nur über Ausgabeanschlüsse gemäß Angabe in den PlayReady-Kompatibilitätsregeln. Weitere Informationen zu in den PlayReady-Kompatibilitätsregeln angegebenen Ausgabeanschlussbedingungen finden Sie in den [definierten Bedingungen für PlayReady-Kompatibilität und Stabilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).

Dieser Abschnitt konzentriert sich auf Ausgabeschutzszenarien mit PlayReady DRM für Windows 10 und PlayReady Hardware DRM für Windows 10 (auch auf einigen Windows-Clients verfügbar). Mit PlayReady HWDRM wird der gesamte Ausgabeschutz innerhalb der Windows-TEE-Implementierung erzwungen (siehe [Hardware-DRM](hardware-drm.md)). Aus diesem Grund unterscheidet sich das Verhalten in einigen Fällen von der Verwendung von PlayReady SWDRM (Software-DRM):

* Unterstützung für die Schutzebene (OPL) für nicht komprimierte digitale Videos 270 Grad: PlayReady HWDRM für Windows 10-ab-Lösung unterstützt und erzwingt, dass HDCP (High-Bandwidth Digital Content Protection) beteiligt ist. Es empfiehlt sich, bei HD-Inhalten für das HWDRM einen OPL-Wert zu verwenden, der größer als 270 ist (dies ist jedoch nicht zwingend erforderlich). Darüber hinaus sollten Sie die HDCP-Typeinschränkung in der Lizenz festlegen (HDCP-Version 2.2 oder höher).
* Im Gegensatz zum SWDRM wird beim HWDRM der Ausgabeschutz für alle Monitore basierend auf dem langsamsten Monitor erzwungen. Wenn der Benutzer beispielsweise zwei Monitore angeschlossen hat und nur einer davon HDCP unterstützt, ist die Wiedergabe nicht möglich, falls die Lizenz HDCP erfordert. Dies gilt auch dann, wenn der Inhalt nur auf dem Monitor gerendert wird, der HDCP unterstützt. Beim SWDRM wird der Inhalt wiedergegeben, solange das Rendering nur auf dem Monitor erfolgt, der HDCP unterstützt.
* Es ist nicht garantiert, dass das HWDRM vom Client verwendet wird und dass das Verfahren sicher ist, es sei denn, von den Inhaltsschlüsseln und -lizenzen werden die folgenden Bedingungen erfüllt:
    * Die für den Videoinhaltsschlüssel verwendete Lizenz muss mindestens die Sicherheitsstufe 3.000 besitzen.
    * Audiodaten müssen mit einem anderen Inhaltsschlüssel verschlüsselt werden als Videodaten, und die für Audiodaten verwendete Lizenz muss mindestens die Sicherheitsstufe 2.000 besitzen. Alternativ können Audiodaten auch unverschlüsselt bleiben.
* In allen SWDRM-Szenarien darf die Sicherheitsebene der PlayReady-Lizenz für den Audio- und/oder Videoinhaltsschlüssel maximal 2.000 betragen.

### <a name="output-protection-levels"></a>Ausgabeschutzebenen

Die folgende Tabelle veranschaulicht die Zuordnungen zwischen verschiedenen OPLs in der PlayReady-Lizenz und deren Erzwingung durch PlayReady DRM für Windows 10:

#### <a name="video"></a>Video

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Komprimierte digitale Videos</th>
        <th colspan="2">Unkomprimierte digitale Videos</th>
        <th>TV (analog)</th>
    </tr>
    <tr>
        <th>Beliebig</th>
        <th colspan="2">HDMI, DVI, DisplayPort, MHL</th>
        <th>Komponente, Composite</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">Nicht zutreffend\*</td>
        <td colspan="2">Inhalt wird übergeben.</td>
        <td>Inhalt wird übergeben.</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">Nicht zutreffend\*</td>
        <td>Inhalt wird übergeben, wenn CGMS-A CopyNever eingebunden wird oder CGMS-A nicht eingebunden werden kann.</td>
    </tr>
    <tr>
        <th>200</th>
        <td>Inhalt wird übergeben, wenn CGMS-A CopyNever eingebunden wird.</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">Versucht, HDCP einzubinden, der Inhalt wird jedoch unabhängig vom Ergebnis übergeben.</td>
        <td rowspan="5">Nicht zutreffend\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>SWDRM</b>: Versucht, HDCP zu erfassen. Sollte die Einbindung von HDCP nicht möglich sein, schränkt der PC die effektive Auflösung auf 520.000 Pixel pro Frame ein und übergibt den Inhalt.</td>
        <td><b>HWDRM</b>: Inhalt wird mit HDCP übergeben. Wenn die Einbindung von HDCP nicht möglich ist, wird die Wiedergabe über HDMI/DVI-Anschlüsse blockiert.</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **Wenn HDCP typeinschränkung nicht definiert ist:** Inhalt wird mit HDCP übergeben. Sollte die Einbindung von HDCP nicht möglich sein, wird die Wiedergabe über HDMI/DVI-Anschlüsse blockiert.
            </p>
            <p>
                **Wenn die Einschränkung des Typs HDCP definiert ist**: Pass-Inhalte mit HDCP 2.2 und Inhalts-Stream-Typ, die auf 1 festgelegt werden. Wenn die Einbindung von HDCP nicht möglich ist oder der Inhaltsdatenstromtyp nicht auf 1 festgelegt werden kann, wird die Wiedergabe über HDMI/DVI-Anschlüsse blockiert.
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">Windows 10 übergibt komprimierte digitale Videoinhalte niemals an Ausgaben, unabhängig vom nachfolgenden OPL-Wert. Weitere Informationen zu komprimierten digitalen Videoinhalten finden Sie in den <a href="https://www.microsoft.com/playready/licensing/compliance/">Kompatibilitätsregeln für PlayReady-Produkte</a>.</td>
        <td colspan="2" rowspan="2">Nicht zutreffend\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Nicht alle Werte für die Schutzebenen für die Ausgabe können durch einen Lizenzserver festgelegt werden. Weitere Informationen finden Sie unter [PlayReady-Kompatibilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).

#### <a name="audio"></a>Audio

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Komprimierte digitale Audiodaten</th>
        <th>Unkomprimierte digitale Audiodaten</th>
        <th>Analog- oder USB-Audio</th>
    </tr>
    <tr>
        <th>HDMI, DisplayPort, MHL</th>
        <th>HDMI, DisplayPort, MHL</th>
        <th>Beliebig</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">Inhalt wird übergeben.</td>
        <td>Inhalt wird übergeben.</td>
        <td rowspan="5">Inhalt wird übergeben.</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>Inhalt wird übergeben, wenn HDCP für HDMI, DisplayPort oder MHL eingebunden wird oder SCMS eingebunden und auf „CopyNever“ festgelegt wird.</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Inhalt wird übergeben, wenn HDCP für HDMI, DisplayPort oder MHL eingebunden wird.</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

PlayReady DRM ermöglicht die Wiedergabe von Inhalten per Miracast-Ausgabe, sobald HDCP 2.0 oder höher eingebunden wird. Unter Windows 10 gilt Miracast jedoch als *digitale* Ausgabe. Weitere Informationen zu Miracast-Szenarien finden Sie in den [PlayReady-Kompatibilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/). Die folgende Tabelle veranschaulicht die Zuordnungen zwischen verschiedenen OPLs in der PlayReady-Lizenz und deren Erzwingung durch PlayReady DRM für Miracast-Ausgaben:

<table>
    <tr>
        <th>OPL</th>
        <th>Komprimierte digitale Audiodaten</th>
        <th>Unkomprimierte digitale Audiodaten</th>
        <th>Komprimierte digitale Videos</th>
        <th>Unkomprimierte digitale Videos</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
        <td>Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
        <td rowspan="6">Nicht zutreffend\*</td>
        <td>Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">Inhalt wird NICHT übergeben.</td>
        <td rowspan="2">Nicht zutreffend\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">Nicht zutreffend\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.</td>
        <td>Inhalt wird NICHT übergeben.</td>
        <td>
            <p>
                **Wenn HDCP typeinschränkung nicht definiert ist:** Inhalt wird übergeben, wenn HDCP 2.0 oder höher eingebunden wird. Wenn die Einbindung nicht erfolgreich ist, wird der Inhalt NICHT übergeben.
            </p>
            <p>
                **Wenn HDCP typeinschränkung definiert ist:** Pass-Inhalte mit HDCP 2.2 und Inhalts-Stream-Typ, die auf 1 festgelegt werden. Wenn die Einbindung von HDCP nicht möglich ist oder der Inhaltsdatenstromtyp nicht auf 1 festgelegt werden kann, wird der Inhalt NICHT übergeben.
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">Nicht zutreffend\*</td>
        <td rowspan="2">Windows 10 übergibt komprimierte digitale Videoinhalte niemals an Ausgaben, unabhängig vom nachfolgenden OPL-Wert. Weitere Informationen zu komprimierten digitalen Videoinhalten finden Sie in den <a href="https://www.microsoft.com/playready/licensing/compliance/">Kompatibilitätsregeln für PlayReady-Produkte</a>.</td>
        <td rowspan="2">Nicht zutreffend\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Nicht alle Werte für die Schutzebenen für die Ausgabe können durch einen Lizenzserver festgelegt werden. Weitere Informationen finden Sie unter [PlayReady-Kompatibilitätsregeln](https://www.microsoft.com/playready/licensing/compliance/).

### <a name="additional-explicit-output-restrictions"></a>Zusätzliche explizite Ausgabeeinschränkungen

Die folgende Tabelle beschreibt die Implementierung expliziter Einschränkungen des Ausgabeschutzes für digitale Videos von PlayReady DRM für Windows 10:

<table>
    <tr>
        <th>Szenario</th>
        <th>GUID</th>
        <th>Situation</th>
        <th>Folge</th>
    </tr>
    <tr>
        <th>Maximale effektive Auflösung (Decodierungsgröße)</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>Verbundene Ausgabe: digitale Videoausgabe, Miracast, HDMI, DVI usw.</td>
        <td>
            <p>
                Inhalt wird übergeben, wenn folgende Einschränkungen vorliegen:  
            </p>
            <ul>
                <li>(a) Die Breite des Frames muss kleiner oder gleich der maximalen Framebreite (in Pixel) und die Höhe des Frames muss kleiner oder gleich der maximalen Framehöhe (in Pixel) sein. Oder:</li>
                <li>(a) Die Höhe des Frames muss kleiner oder gleich der maximalen Framebreite (in Pixel) und die Breite des Frames muss kleiner oder gleich der maximalen Framehöhe (in Pixel) sein.</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>HDCP-Typeinschränkung</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>Verbundene Ausgabe: digitale Videoausgabe, Miracast, HDMI, DVI usw.</td>
        <td>Inhalt wird mit HDCP 2.2 und Inhaltsdatenstrom-Typ „1“ übergeben. Sollte die Einbindung von HDCP 2.2 nicht möglich sein oder der Inhaltsdatenstrom-Typ nicht auf „1“ festgelegt werden können, wird der Inhalt NICHT übergeben. Außerdem muss die Ausgabeschutzebene für unkomprimierte digitale Videos muss mindestens auf 271 festgelegt werden.</td>
    </tr>
</table>
<br/>

Die folgende Tabelle beschreibt die Implementierung expliziter Einschränkungen des Ausgabeschutzes für analoge Videos von PlayReady DRM für Windows 10.

<table>
    <tr>
        <th>Szenario</th>
        <th>GUID</th>
        <th>Situation</th>
        <th colspan="2">Folge</th>
    </tr>
    <tr>
        <th>Analoger Computermonitor</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>Verbundene Ausgabe lautet: VGA, DVI&ndash;analog, etc.</td>
        <td><b>SWDRM:</b> PC beschränken die effektive Auflösung zu 520,000 Epx pro Frame wird, und übergeben von Inhalt</td>
        <td><b>HWDRM:</b> Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>Analoge Komponente</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>Verbundene Ausgabe: Komponente</td>
        <td><b>SWDRM:</b> PC beschränken die effektive Auflösung zu 520,000 Epx pro Frame wird, und übergeben von Inhalt</td>
        <td><b>HWDRM:</b> Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th rowspan="2">Analoge TV-Ausgaben</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>OPL für Analog-TV ist kleiner als 151.</td>
        <td colspan="2">CGMS-A muss eingebunden werden.</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>OPL für Analog-TV ist kleiner als 101, und „2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3“ ist nicht in der Lizenz enthalten.</td>
        <td colspan="2">Einbindung von CGMS-A muss versucht werden, der Inhalt kann jedoch unabhängig vom Ergebnis wiedergegeben werden.</td>
    </tr>
    <tr>
        <th>Automatische Verstärkungsregelung und Farbstreifen</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>Inhalt wird mit einer Auflösung von maximal 520.000 Pixeln an eine analoge TV-Ausgabe übergeben.</td>
        <td colspan="2">Die automatische Verstärkungsregelung wird nur für Komponentenvideos und den PAL-Modus festgelegt, wenn die Auflösung weniger als 520.000 Pixel beträgt. Automatische Verstärkungsregelung und Farbstreifeninformationen für NTSC werden festgelegt, wenn die Auflösung weniger als 520.000 Pixel beträgt (siehe Tabelle 3.5.7.3. in den Kompatibilitätsregeln).</td>
    </tr>
    <tr>
        <th>Nur digitale Ausgabe</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>Die verbundene Ausgabe ist analog.</td>
        <td colspan="2">Inhalt wird nicht übergeben.</td>
    </tr>
</table>
<br/>

> [!NOTE]
> Wenn für die Wiedergabe ein Adapterdongle wie Mini-DisplayPort auf VGA verwendet wird, wird die Ausgabe von Windows 10 als digitale Videoausgabe betrachtet, sodass keine Richtlinien für analoge Videos durchgesetzt werden können.

Die folgende Tabelle beschreibt die Implementierung von PlayReady DRM für Windows 10 für die Wiedergabe in anderen Fällen:

<table>
    <tr>
        <th>Szenario</th>
        <th>GUID</th>
        <th>Situation</th>
        <th colspan="2">Folge</th>
    </tr>
    <tr>
        <th>Unbekannte Ausgabe</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>Die Ausgabe kann nicht vernünftig ermittelt werden, oder für den Grafiktreiber ist kein OPM möglich.</td>
        <td><b>SWDRM:</b> Inhalt wird übergeben.</td>
        <td><b>HWDRM:</b> Inhalt wird NICHT übergeben.</td>
    </tr>
    <tr>
        <th>Unbekannte Ausgabe mit Einschränkung</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>Die Ausgabe kann nicht vernünftig ermittelt werden, oder für den Grafiktreiber ist kein OPM möglich.</td>
        <td><b>SWDRM:</b> PC beschränken die effektive Auflösung zu 520,000 Epx pro Frame wird, und übergeben von Inhalt</td>
        <td><b>HWDRM:</b> Inhalt wird NICHT übergeben.</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>Vorraussetzungen

Bevor Sie mit der Erstellung Ihrer PlayReady-geschützten UWP-App beginnen, müssen Sie die folgende Software auf Ihrem System installieren:

-   Windows 10.
-   Wenn Sie die Beispiele für PlayReady-DRM für UWP-apps kompilieren, müssen Sie Microsoft Visual Studio 2015 oder höher verwenden, um die Beispiele zu kompilieren. Sie können immer noch Microsoft Visual Studio 2013 verwenden, um die Beispiele von PlayReady-DRM für Windows 8.1 Store-Apps zu kompilieren.

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](https://go.microsoft.com/fwlink/p/?LinkId=626876).-->

## <a name="playready-uwp-app-migration-guide"></a>Migrationshandbuch für UWP-Apps mit PlayReady

Dieser Abschnitt enthält Informationen zum Migrieren Ihrer vorhandenen PlayReady Windows 8.x Store-apps auf Windows 10.

Der Namespace für PlayReady-UWP-apps auf Windows 10 mehr den **Microsoft.Media.PlayReadyClient** zu [ **Windows.Media.Protection.PlayReady**](https://docs.microsoft.com/uwp/api/Windows.Media.Protection.PlayReady). Sie müssen also in Ihrem Code den alten Namespace suchen und durch den neuen Namespace ersetzen. Sie verweisen weiterhin auf eine WINMD-Datei. Es ist Teil des windows.media.winmd auf dem Windows 10-Betriebssystem. Sie ist in „windows.winmd“ als Teil des TH Windows SDK enthalten. Bei UWP wird darauf in „windows.foundation.univeralappcontract.winmd“ verwiesen.

Zum Wiedergeben von PlayReady-geschützten High-Definition (HD)-Inhalten (1080p) und Ultra-High-Definition (UHD)-Inhalten müssen Sie das Hardware-DRM mit PlayReady implementieren. Informationen zum Implementieren des Hardware-DRM mit PlayReady finden Sie unter [Hardware-DRM](hardware-drm.md).

Manche Inhalte werden nicht vom Hardware-DRM unterstützt. Informationen zum Deaktivieren des Hardware-DRM und Aktivieren des Software-DRM finden Sie unter [Außerkraftsetzen des Hardware-DRM](hardware-drm.md#override-hardware-drm).

Stellen Sie im Medienschutz-Manager sicher, dass für Ihren Code die folgenden Einstellungen festgelegt sind (sofern noch nicht geschehen):

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>Proaktives Abrufen einer nicht persistenten Lizenz vor der Wiedergabe

In diesem Abschnitt wird beschrieben, wie Sie vor dem Start der Wiedergabe proaktiv nicht persistente Lizenzen abrufen.

In früheren Versionen von PlayReady DRM konnten nicht persistente Lizenzen nur reaktiv während der Wiedergabe abgerufen werden. In dieser Version können Sie nicht persistente Lizenzen proaktiv abrufen, bevor die Wiedergabe beginnt.

1.  Erstellen Sie proaktiv eine Wiedergabesitzung, in der die nicht persistente Lizenz gespeichert werden kann. Zum Beispiel:

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  Binden Sie die Wiedergabesitzung an die Lizenzabrufklasse. Zum Beispiel:

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  Erstellen Sie eine Lizenzserviceanfrage. Zum Beispiel:

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  Führen Sie den Lizenzerwerb mit der Serviceanfrage aus, die Sie in Schritt 3 erstellt haben. Die Lizenz wird in der Wiedergabesitzung gespeichert.
5.  Binden Sie die Wiedergabesitzung an die Medienquelle für die Wiedergabe. Zum Beispiel:

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## <a name="query-for-protection-capabilities"></a>Abfrage der Schutzmöglichkeiten
Ab Windows 10, Version 1703, können Sie Hardware-DRM-Funktionen abfragen, z. B. für Decodierung, Auflösung und Ausgabeschutz (HDCP). Abfragen erfolgen mit der Methode [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported). Diese erwartet eine Zeichenfolge, welche die Funktionen angibt, nach deren Unterstützung gefragt wird, und eine Zeichenfolge, die das Schlüsselsystem angibt, für das die Abfrage gilt. Eine Liste der unterstützten Zeichenfolgenwerte finden Sie auf der API-Referenzseite für [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported). Das folgende Codebeispiel veranschaulicht die Verwendung dieser Methode.  

    ```cs
    using namespace Windows::Media::Protection;

    ProtectionCapabilities^ sr = ref new ProtectionCapabilities();

    ProtectionCapabilityResult result = sr->IsTypeSupported(
    L"video/mp4; codecs=\"avc1.640028\"; features=\"decode-bpp=10,decode-fps=29.97,decode-res-x=1920,decode-res-y=1080\"",
    L"com.microsoft.playready");

    switch (result)
    {
        case ProtectionCapabilityResult::Probably:
        // Queue up UHD HW DRM video
        break;

        case ProtectionCapabilityResult::Maybe:
        // Check again after UI or poll for more info.
        break;

        case ProtectionCapabilityResult::NotSupported:
        // Do not queue up UHD HW DRM video.
        break;
    }
    ```
## <a name="add-secure-stop"></a>Hinzufügen des sicheren Beendens

In diesem Abschnitt wird beschrieben, wie Sie Ihrer UWP-App die Funktion für sicheres Beenden hinzufügen.

Dank des sicheren Beendens kann ein PlayReady-Gerät einem Medienstreamingdienst zuverlässig bestätigen, dass die Medienwiedergabe für einen bestimmten Inhalt beendet wurde. Mit dieser Funktion wird sichergestellt, dass Ihre Medienstreamingdienste präzise Erzwingung und Berichte zu Nutzungseinschränkungen auf verschiedenen Geräten für ein bestimmtes Konto bereitstellen.

Es gibt zwei primäre Szenarien für das Senden einer Abfrage für sicheres Beenden:

-   Wenn die Mediendarstellung beendet wird, weil das Ende des Inhalts erreicht wurde, oder wenn die Mediendarstellung vor ihrem Ende vom Benutzer beendet wurde.
-   Wenn die vorherige Sitzung unerwartet beendet wurde (z. B. aufgrund eines System- oder App-Absturzes). Die App muss beim Starten oder Herunterfahren alle ausstehenden Sitzungen für sicheres Beenden abfragen und von anderen Medienwiedergaben getrennte Abfragen senden.

Eine Beispielimplementierung für das sichere Beenden finden Sie in der Datei „securestop.cs” im PlayReady-Beispiel unter [https://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](https://go.microsoft.com/fwlink/p/?linkid=331670).

## <a name="use-playready-drm-on-xbox-one"></a>Verwenden von PlayReady DRM auf Xbox One

Um PlayReady-DRM in einer UWP-app auf der Xbox One verwenden zu können, müssen Sie zuerst zum Registrieren Ihrer [Partner Center](https://partner.microsoft.com/dashboard) -Konto, das Sie verwenden, um die app für die Autorisierung zum Verwenden von PlayReady zu veröffentlichen. Hierzu stehen Ihnen zwei Möglichkeiten zur Verfügung.

* Ihr Microsoft-Kontakt kann die Berechtigung anfordern.
* Gelten für die Autorisierung durch den Namen der Partner Center-Konto und das Unternehmen um senden [ pronxbox@microsoft.com ](mailto:pronxbox@microsoft.com).

Wenn Sie die Autorisierung erhalten haben, müssen Sie dem App-Manifest eine zusätzliche `<DeviceCapability>` hinzufügen. Sie müssen diese manuell hinzufügen, da derzeit im App Manifest Designer keine Einstellung verfügbar ist. Führen Sie folgende Schritte durch, um dies zu konfigurieren:

1. Öffnen Sie das Projekt in Visual Studio, öffnen Sie den **Solution Explorer**, und klicken Sie mit der rechten Maustaste auf **Package.appxmanifest**.
2. Wählen Sie **Öffnen mit...**  und anschließend **XML (Text) Editor**, und klicken Sie auf **OK**.
3. Fügen Sie zwischen den `<Capabilities>`-Tags die folgende `<DeviceCapability>` ein:

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. Speichern Sie die Datei.

Beachten Sie bei der Verwendung von PlayReady auf Xbox One jedoch auch Folgendes: Auf Entwicklungskits besteht eine SL150-Beschränkung (d. h. SL2000- oder SL3000-Inhalte können nicht wiedergegeben werden). Einzelhandelsgeräte können Inhalte mit höheren Sicherheitsebenen wiedergeben. Sie müssen jedoch SL150-Inhalte verwenden, um Ihre App in einem Entwicklungskit zu verwenden. Zum Testen dieser Inhalte stehen Ihnen folgende Möglichkeiten zur Verfügung:

* Verwenden Sie speziell zusammengestellte Testinhalte, die SL150-Lizenzen erfordern.
* Implementieren Sie eine Logik, sodass nur bestimmte authentifizierte Testkonten SL150-Lizenzen für bestimmte Inhalte erhalten können.

Gehen Sie so vor, wie es für Ihr Unternehmen und Ihr Produkt am praktischsten ist.


## <a name="see-also"></a>Siehe auch
- [Medienwiedergabe](media-playback.md)




