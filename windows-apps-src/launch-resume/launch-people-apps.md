---
title: Starten der Kontakte-App
description: In diesem Thema wird das URI-Schema „ms-people“ beschrieben. Ihre App kann dieses URI-Schema verwenden, um die Kontakte-App für bestimmte Aktionen zu starten.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dd1048ad0d3c5d542c5d7fb398261f3e29316396
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162614"
---
# <a name="launch-the-people-app"></a>Starten der Kontakte-App

In diesem Thema wird das **ms-people:**-URI-Schema beschrieben. Ihre App kann dieses URI-Schema verwenden, um die Kontakte-App für bestimmte Aktionen zu starten.

## <a name="ms-people-uri-scheme-reference"></a>ms-people: URI-Schemaverweis

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ergebnisse</th>
<th align="left">URI-Schema</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Damit können andere Apps die Hauptseite der Kontakte-App starten.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Damit können andere Apps die Einstellungsseite der Kontakte-App starten.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Damit können andere Apps einen Suchbegriff bereitstellen, der in der Kontakte-App mit der Ergebnisseite der Suche gestartet wird.
<div class="alert">
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Wenn Sie die Syntax nicht ordnungsgemäß eingeben oder den Wert der Suchzeichenfolge vergessen, wird standardmäßig eine vollständige Liste der Kontakte ohne Filterung zurückgegeben.</p>
</div>
<div>
</div></td>
<td align="left">MS-People: Search? SearchString = &lt; contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Startet mit einer vorhandenen Kontaktkarte, wenn der Kontakt gefunden wurde. Oder startet mit einer temporären Kontaktkarte, wenn kein Kontakt gefunden wird. Wenn kein Eingabeparameter angegeben wird, wird die Kontakte-App mit einer Kontaktliste gestartet.
<div class="alert">
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Die Reihenfolge der Parameter spielt keine Rolle.</p>
<p>Wenn mehr als eine Übereinstimmung vorliegt, wird die erste Übereinstimmung des Kontakts zurückgegeben.</p>
</div>
<div> 
</div></td>
<td align="left">MS-People: viewcontact? ContactId = &lt; ContactId &gt; &amp; aggregatedid = &lt; aggid &gt; &amp; PhoneNumber = &lt; phoneNum &gt; &amp; Email = &lt; Email &gt; &amp; ContactName = &lt; Name &gt; &amp; Contact = &lt; contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Startet mit einer „Kontakt speichern“-Seite innerhalb der Kontakte-App, um den angegeben Kontakt mit der angegebenen Telefonnummer oder E-Mail-Adresse zu speichern.
<div class="alert">
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Die Reihenfolge der Parameter spielt keine Rolle.</p>
</div>
<div>
</div></td>
<td align="left">MS-People: SaveTo Contact? PhoneNumber = &lt; phoneNum &gt; &amp; Email = &lt; Email &gt; &amp; ContactName = &lt; Name&gt;</td>
</tr>
<tr class="even">
<td align="left">Öffnet die Seite neuen Kontakt hinzufügen innerhalb der People-APP, um den gegebenen Kontakt zu speichern.
<div class="alert"><p>Öffnen Sie mithilfe von <a href="/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">launchuriforresulzasync</a> die Seite neuen Kontakt speichern. Wenn Sie <strong>launchuriasync</strong> verwenden, wird nur die Hauptseite der People-App gestartet.</p>
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Die Reihenfolge der Parameter spielt keine Rolle.</p>
<p>Sie können eine beliebige Kombination unterstützter Parameter verwenden.</p>
</div>
<div>
</div></td>
<td align="left">MS-People: savecontacttask? PhoneNumber = &lt; phoneNum &gt; &amp; Email = &lt; Email &gt; &amp; ContactName = &lt; Name&gt;</td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesearch-parameter-reference"></a>ms-people:search: Parameterverweis

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">BESCHREIBUNG</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>SearchString</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die Suchzeichenfolge für die Informationen zur Kontaktsuche.</p>
<p>Die Telefonnummer oder den Namen des Kontakts.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peopleviewcontact-parameter-reference"></a>ms-people:viewcontact: Parameterverweis

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">BESCHREIBUNG</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>ContactId</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die Kontakt-ID des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die Telefonnummer des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>E-Mail</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die E-Mail-Adresse des Kontakts.</p></td>
<td align="left"><p>MS-People: viewcontact? E-Mail =johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der Name des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Kontakt</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Das Kontaktobjekt.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavetocontact-parameter-reference"></a>ms-people:savetocontact: Parameterverweis

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">BESCHREIBUNG</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die Telefonnummer des Kontakts.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>E-Mail</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die E-Mail-Adresse des Kontakts.</p></td>
<td align="left"><p>MS-People: SaveTo Contact? E-Mail =johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der Name des Kontakts.</p></td>
<td align="left"><p>MS-People: SaveTo Contact? Email = johnsmith@contsco.com &amp; ContactName = John %20% Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>MS-People: savecontacttask: Parameter Verweis

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">BESCHREIBUNG</th>

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Company</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Firmenname des Kontakts.</p></td>

</tr>
<tr class="even">
<td align="left"><b>Vorname</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der Vorname des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Stadt der Privatadresse.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Land der Privatadresse.</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der Status der Privatadresse.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Straße der Privatadresse.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Homeaddresszipcode</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die Postleitzahl der Privatadresse.</p></td>

</tr>
<tr class="even">
<td align="left"><b>"HomePhone"</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Telefonnummer des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die Position des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Nachname</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Nachname des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>MiddleName</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der zweite Name des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>MobilePhone</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Mobiltelefonnummer des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Namen</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der Spitzname des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Notizen</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Hinweise zum Kontakt.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Otheremail</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Andere e-Mail-Adresse des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Personalemail</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Persönliche e-Mail-Adresse des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Suffix</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Suffix des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Titel</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der Titel des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Website</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Website des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Workaddresscity</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Stadt der Arbeitsadresse.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Workaddresscountry</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Land der Arbeitsadresse.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Workaddressstate</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Der Status der Arbeitsadresse.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Workaddressstreet</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Straße der Arbeitsadresse.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Workaddresszipcode</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Die Postleitzahl der Arbeitsadresse.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Workemail</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Geschäftliche e-Mail-Adresse des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Arbeits Telefon</b></td>
<td align="left"><p>Dies ist optional.</p>
<p>Geschäftliche Telefonnummer des Kontakts.</p></td>
</tr>
</tbody>
</table>