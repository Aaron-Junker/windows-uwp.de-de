---
title: EDS-Autorisierung
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
description: " EDS-Autorisierung"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3e5c5ef3bf3c864215544391bc291a26f6c05d0f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607605"
---
# <a name="eds-authorization"></a>EDS-Autorisierung
 
  * [Einführung](#ID4EN)
  * [Autorisierung](#ID4EFB)
  * [3.0-Token: Mehrere Benutzer im Vergleich zu Einzelbenutzermodus](#ID4EEC)
  * [Unterstützt die EDS mit mehreren Benutzern?](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>Einführung
 
Unterhaltung Discovery Services (EDS) 3.1 wird anonymen Datenverkehr nicht unterstützt. Authentifizierung ist für alle Anforderungen an EDS erforderlich. EDS erfordert eine XToken von Aufrufern, um Clients zu authentifizieren. Diese Token werden vom XSTS erstellt und über verschiedene Xbox Authentication Services (XAS) abgerufen werden können. Es gibt separate Authentifizierungsdienste für Geräte, Benutzer und Titeln, die die Identität des Tokens definiert werden.
 
XSTS ist der Gatekeeper für Xbox LIVE. Es ist die erste Verteidigungslinie zu bestimmen, ob ein Benutzer oder Gerät für die Xbox LIVE-Dienste die Verbindung autorisiert ist. Nach der Authentifizierung des Benutzers, generiert der XSTS ein XToken, mit denen sie sich sicher an eine beliebige Komponente für den Dienst zu identifizieren. Diese XToken ist Ihre Passport Gültigkeitsdauer.
 
Personen und Dinge, unsere Dienste verwenden möchten. Und wir möchten die meisten dieser Dinge und Personen, um unsere Dienste verwenden zu können. Aber wie wir sicherstellen, dass die Dinge Personen vorgeben sind nicht und die Personen, die sie vorgibt, tatsächlich sind? Wir stellen sie mit dem Token, die sie verwenden können, um sich für andere Personen zu identifizieren.
 
Diese Token werden durch die XSTS erstellt und in der Regel als XTokens bezeichnet. XToken ist ein weitgefasster Begriff, mit dem Token zu erfassen, die eine Vielzahl von Faktoren enthalten und können in vielen verschiedenen Formen stammen, aber sie sind alle erstellt, signiert, und optional durch den Server XSTS verschlüsselt. Intern verwendet XSTS MXAN zum Erstellen und formatieren die Token an. MXAN ist die einzige Komponente, die jemals eine XToken Informationen extrahiert. Nutzen die Token Services übergeben MXAN gebrochen, werden die Anforderungsheader. Die Token verarbeitet und überprüft werden, und die Ansprüche im Token ausgedrückt werden an den Dienst zurückgegeben werden. Der Dienst kann dann diese Anspruchstypen Werte an, um dem aufrufenden Benutzer oder Gerät zu identifizieren, und führen Aktionen, die anhand dieser Informationen verwenden.
 
Der basic-Identitätstoken - werden für Benutzer, Geräte und Titel - von der Xbox Authentication Services (XAS) bereitgestellt. Jede XAS ist verantwortlich für das Generieren eines Identitätstokens Werten für verschiedene Ansprüche an, die sie für verantwortlich sind.
 
   * XASD (XAS für Geräte): ein DToken bietet eine geräteidentität erstellt
   * XASU (XAS für Benutzer): erstellt eine UToken bietet eine Benutzeridentität
   * XAST (XAS für Titel): erstellt eine ist TToken bietet eine Title-Identität
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>Autorisierung
 
   * Rufen Sie ein oder mehrere Identitätstoken. Sie können eine XToken mit nur einer einzelnen D, U und T-Token anfordern. Sie müssen mindestens eine der D oder u angeben 
     * Fordern Sie eine DToken vom XASD Gerätedetails für die Authentifizierung bereitstellen
     * Fordern Sie eine UToken von XASU mit eine Form der Benutzerauthentifizierung ein. Die Benutzerauthentifizierung wird wahrscheinlich in Form von ein MSA (RPS)-Token bereitgestellt.
     * Fordern Sie ein ist TToken von XAST an. Titel, die verfügbar sind, abhängig von der Plattform, die derzeit ausgeführt werden, damit Sie eine DToken XAST auch bereitstellen müssen.
  
   * Erstellen Sie eine XSTS-Anforderung.
 
     * Definieren Sie die abhängige Partei, von der Sie ein Token anfordert.
     * Füllen Sie die Anforderungseigenschaften mit den D, U und/oder T-Token.
    * Führen Sie die XSTS-Anforderung, und speichern Sie die resultierende XToken. Die zurückgegebene XToken enthält (höchstens) aller Geräte, Benutzer und Titel Identitätsinformationen aus der Identitätstoken und alle weiteren Ansprüche (aktuellen Abonnementstatus, Benutzergruppen, usw.).
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0-Token: Mehrere Benutzer im Vergleich zu Einzelbenutzermodus
 
Dies ist die Form des 3.0 Tokens: `XBL3.0 x=&lt;hash>;&lt;token>`
 
Je nachdem auf die &lt;Hash >, das Token wird anders behandelt:
 
   * Wenn &lt;Hash > ist gleich * (Sternchen), wird kein bestimmter Benutzer die Anforderung ausführt, und alle Benutzer in das Token in der deserialisierten Prinzipal vorhanden sind. Dies ist die "true" Mehrbenutzer-Form.
   * Wenn &lt;Hash > ist gleich - (Bindestrich), und klicken Sie dann keine Benutzer die Anforderung ausführen. Alle Benutzer in der deserialisierten Prinzipal entfernt werden.
   * Wenn &lt;Hash > ist nicht gleich * oder – es ist ein Bezeichner, der angibt, welcher Benutzer in das Token die Anforderung stammt. Nur der angegebene Benutzer wird in der deserialisierten Prinzipal vorhanden sein. Alle anderen Benutzer werden entfernt. Dies ist der Einzelbenutzer-3.0-Token.
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>Unterstützt die EDS mit mehreren Benutzern?
 * Die Antwort ist keine. Im Fall, der beschrieben wurde, wird die-Konsole immer Benutzertoken für die einzelnen senden. Auch wenn mehrere Benutzer angemeldet vorhanden sind, muss ein angegebene "Aufrufer", wobei alle anderen Identitäten gelöscht werden vorhanden sein.
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>Weitere Informationen 

[Marketplace-URIs](../uri/marketplace/atoc-reference-marketplace.md)

   