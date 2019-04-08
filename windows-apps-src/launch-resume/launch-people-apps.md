---
title: Starten der Kontakte-App
description: In diesem Thema wird das URI-Schema „ms-people“ beschrieben. Ihre App kann dieses URI-Schema verwenden, um die Kontakte-App für bestimmte Aktionen zu starten.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ab10acab42ab3f03121a7c5a462cb651b0f3f31b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595565"
---
# <a name="launch-the-people-app"></a>Starten der Kontakte-App

In diesem Thema wird beschrieben, die **ms-Personen:** URI-Schema. Ihre App kann dieses URI-Schema verwenden, um die Kontakte-App für bestimmte Aktionen zu starten.

## <a name="ms-people-uri-scheme-reference"></a>ms-people: URI-Schema-Verweis

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
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
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
<td align="left">MS-Benutzer: Viewcontact? Kontakt-ID =&lt;Contactid&gt;&amp;AggregatedId =&lt;Aggid&gt;&amp;"PhoneNumber" = &lt;Phonenum&gt;&amp;-e-Mail =&lt;-e-Mail &gt; &amp;ContactName =&lt;Namen&gt;&amp;Kontakt =&lt;Contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Startet mit einer „Kontakt speichern“-Seite innerhalb der Kontakte-App, um den angegeben Kontakt mit der angegebenen Telefonnummer oder E-Mail-Adresse zu speichern.
<div class="alert">
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Die Reihenfolge der Parameter spielt keine Rolle.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
<tr class="even">
<td align="left">Startet mit der Seite „Neuen Kontakt hinzufügen“ innerhalb der Kontakte-App, um den angegeben Kontakt zu speichern.
<div class="alert"><p>Verwenden Sie <a href="https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a>, um die Seite „Neuen Kontakt speichern“ zu öffnen. Mit <strong>LaunchUriAsync</strong> wird nur die Hauptseite der Kontakte-App gestartet.</p>
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Die Reihenfolge der Parameter spielt keine Rolle.</p>
<p>Die unterstützten Parametern können in beliebiger Kombination verwendet werden.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savecontacttask?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
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
<th align="left">Beschreibung</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>SearchString</b></td>
<td align="left"><p>Optional.</p>
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
<th align="left">Beschreibung</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>ContactId</b></td>
<td align="left"><p>Optional.</p>
<p>Die Kontakt-ID des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Optional.</p>
<p>Die Telefonnummer des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>E-Mail</b></td>
<td align="left"><p>Optional.</p>
<p>Die E-Mail-Adresse des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Optional.</p>
<p>Der Name des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Kontakt</b></td>
<td align="left"><p>Optional.</p>
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
<th align="left">Beschreibung</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Optional.</p>
<p>Die Telefonnummer des Kontakts.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>E-Mail</b></td>
<td align="left"><p>Optional.</p>
<p>Die E-Mail-Adresse des Kontakts.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Optional.</p>
<p>Der Name des Kontakts.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>ms-people:savecontacttask: Parameterverweis

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Beschreibung</th>

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Firma</b></td>
<td align="left"><p>Optional.</p>
<p>Firmenname des Kontakts.</p></td>

</tr>
<tr class="even">
<td align="left"><b>Vorname</b></td>
<td align="left"><p>Optional.</p>
<p>Vorname des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>Optional.</p>
<p>Ort der Wohnanschrift.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>Optional.</p>
<p>Land der Wohnanschrift.</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>Optional.</p>
<p>Bundesland der Wohnanschrift.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>Optional.</p>
<p>Straße der Wohnanschrift.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>Optional.</p>
<p>Postleitzahl der Wohnanschrift.</p></td>

</tr>
<tr class="even">
<td align="left"><b>"HomePhone"</b></td>
<td align="left"><p>Optional.</p>
<p>Private Telefonnummer des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>Optional.</p>
<p>Berufsbezeichnung des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>"LastName"</b></td>
<td align="left"><p>Optional.</p>
<p>Nachname des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>middleName</b></td>
<td align="left"><p>Optional.</p>
<p>Zweiter Vorname des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>"Mobilephone"</b></td>
<td align="left"><p>Optional.</p>
<p>Mobiltelefonnummer des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>der Spitzname</b></td>
<td align="left"><p>Optional.</p>
<p>Spitzname des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Hinweise</b></td>
<td align="left"><p>Optional.</p>
<p>Notizen zum Kontakt.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>Optional.</p>
<p>Weitere E-Mail-Adresse des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>Optional.</p>
<p>Private E-Mail-Adresse des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Suffix</b></td>
<td align="left"><p>Optional.</p>
<p>Suffix des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Titel</b></td>
<td align="left"><p>Optional.</p>
<p>Anrede des Kontakts.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Website</b></td>
<td align="left"><p>Optional.</p>
<p>Webseite des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressCity</b></td>
<td align="left"><p>Optional.</p>
<p>Ort der Geschäftsadresse.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressCountry</b></td>
<td align="left"><p>Optional.</p>
<p>Land der Geschäftsadresse.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressState</b></td>
<td align="left"><p>Optional.</p>
<p>Bundesland der Geschäftsadresse.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressStreet</b></td>
<td align="left"><p>Optional.</p>
<p>Straße der Geschäftsadresse.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>Optional.</p>
<p>Postleitzahl der Geschäftsadresse.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Geschäftlichen</b></td>
<td align="left"><p>Optional.</p>
<p>Geschäftliche E-Mail-Adresse des Kontakts.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Tel_geschäftlich</b></td>
<td align="left"><p>Optional.</p>
<p>Geschäftliche Telefonnummer des Kontakts.</p></td>
</tr>
</tbody>
</table>
