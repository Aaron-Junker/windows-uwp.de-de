---
author: DelfCo
Description: "Globalisierung bedeutet das Entwerfen und Entwickeln einer App für verschiedene globale Märkte, ohne dass Änderungen oder Anpassungen erforderlich sind."
Search.SourceType: Video
title: Globalisierung und Lokalisierung
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: fa7757a8657abfc84ee905cb05b0b3addc59f64e
ms.lasthandoff: 02/07/2017

---

# <a name="globalization-and-localization"></a>Globalisierung und Lokalisierung
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Windows wird auf der ganzen Welt von Benutzern mit unterschiedlicher Kultur, Herkunft und Sprache verwendet. Benutzer sprechen beliebige Sprache oder sogar mehrere Sprachen. Die Benutzer sind über die ganze Welt verteilt, und jede Sprache kann ortsabhängig überall gesprochen werden. Sie können die Marktchancen Ihrer App erweitern, indem Sie mithilfe der *Globalisierung* und *Lokalisierung* eine variable App entwickeln.

**Globalisierung** beschreibt den Entwurf und die Entwicklung der App für verschiedene globale Märkte, ohne dass Änderungen oder Anpassungen erforderlich sind.

Sie haben u. a. folgende Möglichkeiten:

-   Entwerfen des App-Layouts in Anpassung an die verschiedenen Textlängen und Schriftgrade anderer Sprachen in Beschriftungen und Textzeichenfolgen
-   Verwenden von Text und kulturspezifischen Bildern aus Ressourcen, die an verschiedene lokale Märkte angepasst werden können, anstatt hartcodierte Elemente im Code oder Markup der App zu programmieren
-   Verwenden von Globalisierungs-APIs zum Anzeigen von Daten, die in verschiedenen Regionen unterschiedlich formatiert sind, z. B. numerische Werte, Datumsangaben, Uhrzeiten und Währungen

**Lokalisierung** beschreibt die Anpassung der App an die sprachlichen, kulturellen und politischen Anforderungen bestimmter lokaler Märkte.

Beispiel:

-   Übersetzen von Text und Beschriftungen der App für den neuen Markt und Erstellen separater Ressourcen für die jeweilige Sprache
-   Ändern kulturspezifischer Bilder nach Bedarf und Platzieren der Bilder in separaten Ressourcen

Das folgende Video enthält eine kurze Einführung dazu, wie Sie Ihre App für die weltweite Verbreitung vorbereiten: [Einführung in die Globalisierung und Lokalisierung](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

## <a name="articles"></a>Artikel
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Artikel</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Empfohlene und nicht empfohlene Vorgehensweisen](guidelines-and-checklist-for-globalizing-your-app.md)</p></td>
<td align="left"><p>Befolgen Sie diese bewährten Methoden beim Globalisieren Ihrer Apps für eine größere Zielgruppe und beim Lokalisieren Ihrer Apps für einen bestimmten Markt.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Verwenden weltweit einsetzbarer Formate](use-global-ready-formats.md)</p></td>
<td align="left"><p>Entwickeln Sie eine für den globalen Markt geeignete App mit geeigneter Formatierung für Datumsangaben, Uhrzeiten, Zahlen und Währungen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Verwalten von Sprache und Region](manage-language-and-region.md)</p></td>
<td align="left"><p>Mithilfe der verschiedenen Sprach- und Regionseinstellungen von Windows können Sie die Auswahl von UI-Ressourcen und die Formatierung der UI-Elemente der App steuern.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Verwenden von Mustern zum Formatieren von Datums- und Uhrzeitwerten](use-patterns-to-format-dates-and-times.md)</p></td>
<td align="left"><p>Verwenden Sie die [<strong>Windows.Globalization.DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859)-API mit benutzerdefinierten Mustern, um Datums- und Uhrzeitwerte im gewünschten Format anzuzeigen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](adjust-layout-and-fonts--and-support-rtl.md)</p></td>
<td align="left"><p>Entwickeln Sie Ihre App so, dass die Layouts und Schriftarten mehrerer Sprachen unterstützt werden – also beispielsweise auch die Flussrichtung von rechts nach links (right-to-left, RTL).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Vorbereiten Ihrer App für die Lokalisierung](prepare-your-app-for-localization.md)</p></td>
<td align="left"><p>Bereiten Sie Ihre App für die Lokalisierung vor, um sie an andere Märkte, Sprachen oder Regionen anzupassen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Aufnehmen von UI-Zeichenfolgen in Ressourcen](put-ui-strings-into-resources.md)</p></td>
<td align="left"><p>Nehmen Sie Zeichenfolgenressourcen für Ihre Benutzeroberfläche in Ressourcendateien auf. Sie können dann in Ihrem Code oder Markup auf diese Zeichenfolgen verweisen.</p></td>
</tr>
</tbody>
</table>

 

Weitere Informationen finden Sie in der ursprünglich für Windows 8.x erstellten Dokumentation, die weiterhin für universelle Windows-Plattform-Apps (UWP) und Windows 10 gilt.

-   [Globalisierung Ihrer App](https://msdn.microsoft.com/library/windows/apps/xaml/hh965328)
-   [Sprachabgleich](https://msdn.microsoft.com/library/windows/apps/xaml/jj673578.aspx)
-   [NumeralSystem-Werte](https://msdn.microsoft.com/library/windows/apps/xaml/jj236471.aspx)
-   [Internationale Schriftarten](https://msdn.microsoft.com/library/windows/apps/xaml/dn263115.aspx)
-   [App-Ressourcen und Lokalisierung](https://msdn.microsoft.com/library/windows/apps/xaml/hh710212.aspx)

 

 




