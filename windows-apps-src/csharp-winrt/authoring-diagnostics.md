---
description: C#/WinRT Diagnose Fehlermeldungen für erstellte Komponenten
title: Diagnostizieren von c#-/WinRT-Komponenten Fehlern
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d1e5eda34a8cbce70edc812ba27437090393ca0
ms.sourcegitcommit: 457239a6907198bc2948322a07c484dd8a170b33
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230019"
---
# <a name="diagnose-cwinrt-component-errors"></a>Diagnostizieren von c#-/WinRT-Komponenten Fehlern

In diesem Artikel finden Sie weitere Informationen zu Einschränkungen bei Windows-Runtime-Komponenten, die mit c#/WinRT. geschrieben wurden. Es erweitert die Informationen, die in Fehlermeldungen aus c#/WinRT werden, wenn ein Autor seine Komponente erstellt. Für vorhandene UWP-.net Native verwaltete Komponenten werden die Metadaten für c# WinRT-Komponenten mit [Winmdexp.exe](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool), einem .NET-Tool, generiert. Nachdem Windows-Runtime Unterstützung von .net entkoppelt ist, stellt c#/WinRT die Tools bereit, mit denen Sie eine winmd-Datei aus der Komponente generieren können. Der Windows-Runtime verfügt über mehr Einschränkungen für Code als eine c#-Klassenbibliothek, und der c#-/WinRT-Diagnose Scanner warnt Sie vor der Erstellung einer winmd-Datei.

In diesem Artikel werden die Fehler beschrieben, die in Ihrem Build aus c#/WinRT. gemeldet wurden. Dieser Artikel dient als aktualisierte Version der Informationen zu Einschränkungen für [vorhandene UWP-.net Native verwaltete Komponenten](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) , die das Winmdexp.exe Tool verwenden.

Suchen Sie nach dem Fehlermeldungs Text (ohne bestimmte Werte für Platzhalter) oder nach der Nachrichtennummer. Wenn Sie die benötigten Informationen hier nicht finden, können Sie uns bei der Verbesserung der Dokumentation helfen, indem Sie die Schaltfläche "Feedback" am Ende dieses Artikels verwenden. Fügen Sie in Ihrem Feedback die Fehlermeldung ein. Alternativ dazu können Sie im [c#-/WinRT](https://github.com/microsoft/CsWinRT)-Repository einen Fehler melden.

In diesem Artikel werden Fehlermeldungen nach Szenario organisiert.

## <a name="implementing-interfaces-that-arent-valid-windows-runtime-interfaces"></a>Implementieren von Schnittstellen, die keine gültigen Windows-Runtime Schnittstellen sind

C#-/WinRT Komponenten können bestimmte Windows-Runtime Schnittstellen nicht implementieren, z. b. die Windows-Runtime Schnittstellen, die asynchrone Aktionen oder Vorgänge darstellen (**iasyncaction**, **iasyncactionwithprogress \<TProgress\>**, **iasyncoperation \<TResult\>** oder **iasyncoperationwithprogress \<TResult,TProgress\>**). Verwenden Sie stattdessen die **asyncinfo** -Klasse zum Erstellen von asynchronen Vorgängen in Windows-Runtime-Komponenten. Hinweis: Hierbei handelt es sich nicht um die einzigen ungültigen Schnittstellen, z. b. kann eine Klasse **System. Exception** nicht implementieren.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1008</p></td>
<td><p>Windows-Runtime Komponententyp {0} kann die Schnittstelle {1} nicht implementieren, da die Schnittstelle keine gültige Windows-Runtime Schnittstelle ist.</p></td>
</tr>
</tbody>
</table>

## <a name="attribute-related-errors"></a>Attribut bezogene Fehler

In der Windows-Runtime können überladene Methoden die gleiche Anzahl von Parametern nur dann aufweisen, wenn eine Überladung als Standard Überladung angegeben wird. Verwenden Sie das-Attribut **Windows. Foundation. Metadata. defauldeverload** (CsWinRT1015, 1016).

Wenn Arrays als Eingaben oder Ausgaben in Funktionen oder Eigenschaften verwendet werden, müssen Sie entweder schreibgeschützt oder schreibgeschützt sein (cswinrt 1025). Die Attribute " **System. Runtime. InteropServices. windowsruntime. leseronlyarray** " und " **System. Runtime. InteropServices. windowsruntime. Write-onlyarray** " werden für Sie bereitgestellt. Die bereitgestellten Attribute dienen nur zur Verwendung in Parametern des Arraytyps (CsWinRT1026), und pro Parameter (CsWinRT1023) sollte nur eine Anwendung angewendet werden.

Sie müssen kein beliebiges Attribut auf einen als **out** markierten Array Parameter anwenden, da davon ausgegangen wird, dass er schreibgeschützt ist. Wenn Sie ihn in diesem Fall als schreibgeschützt versehen (CsWinRT1024), wird eine Fehlermeldung angezeigt.

Die Attribute " **System. Runtime. InteropServices. InAttribute** " und " **System. Runtime. InteropServices. OutAttribute** " dürfen nicht für Parameter eines beliebigen Typs verwendet werden (CsWinRT1021, 1022).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1015</p></td>
<td><p>In Class {2} : {0} über Ladungen mit mehreren Parametern von ' {1} ' werden mit Windows. Foundation. Metadata. defauldeverloadattribute versehen. Das-Attribut kann nur auf eine Überladung der-Methode angewendet werden.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1016</p></td>
<td><p>In Class {2} : für die {0} Parameter Überladungen von {1} muss genau eine Methode als Standard Überladung angegeben werden, indem Sie mit dem Attribut Windows. Foundation. Metadata. defauldeverloadattribute versehen wird.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1021</p></td>
<td><p>Die Methode ' ' {0} weist den Parameter ' {1} ' auf, bei dem es sich um ein Array handelt, das entweder System. Runtime. InteropServices. InAttribute oder System. Runtime. InteropServices. OutAttribute aufweist. In der Windows-Runtime müssen Array Parameter entweder "Read-Event Array" oder "write-on" aufweisen. Entfernen Sie diese Attribute, oder ersetzen Sie sie bei Bedarf durch das entsprechende Windows-Runtime-Attribut.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1022</p></td>
<td><p>Die Methode ' ' {0} weist den Parameter ' {1} ' mit System. Runtime. InteropServices. InAttribute oder System. Runtime. InteropServices. OutAttribute auf. Windows-Runtime unterstützt nicht das Markieren von Parametern mit System. Runtime. InteropServices. InAttribute oder System. Runtime. InteropServices. OutAttribute. Entfernen Sie eventuell System.Runtime.InteropServices.InAttribute, und ersetzen Sie System.Runtime.InteropServices.OutAttribute stattdessen durch den 'out'-Modifizierer.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1023</p></td>
<td><p>Die Methode ' ' {0} weist den Parameter ' {1} ' auf, bei dem es sich um ein Array handelt, das sowohl ' Read onlyarray ' als auch ' Write Items ' aufweist. Die Inhalte von Arrayparametern müssen in der Windows-Runtime entweder lesbar oder schreibbar sein. Entfernen Sie eines der Attribute aus '{1}'.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1024</p></td>
<td><p>Die Methode ' ' {0} weist den Ausgabeparameter ' {1} ' auf, bei dem es sich um ein Array handelt, das jedoch über das Attribut ' Read ' verfügt. Die Inhalte von Ausgabearrays sind in der Windows-Runtime schreibbar.  Entfernen Sie das Attribut aus '{1}'.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1025</p></td>
<td><p>Die Methode ' ' {0} weist den Parameter ' {1} ' auf, der ein Array ist. Die Inhalte von Array-Parametern müssen in der Windows-Runtime entweder lesbar oder schreibbar sein. Wenden Sie entweder "Read-lyarray" oder "Write" auf " {1} " an.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1026</p></td>
<td><p>Die Methode ' ' {0} weist den Parameter ' {1} ' auf, bei dem es sich nicht um ein Array handelt, das entweder ein ' Read onlyarray '-Attribut oder ein ' Write '-Attribut aufweist. Der Windows-Runtime unterstützt nicht das Markieren von nicht-Array-Parametern mit "Read onlyarray" oder "Beschreib teonlyarray".</p></td>
</tr>
</tbody>
</table>

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>Namespacefehler und ungültige Namen für die Ausgabedatei

In der Windows-Runtime müssen sich alle öffentlichen Typen in einer Windows-Metadatendatei (. winmd) in einem Namespace befinden, der den winmd-Dateinamen oder in den Subnamespaces des Datei namens freigibt. Wenn Ihr Visual Studio-Projekt z. b. den Namen a. b hat (das heißt, die Windows-Runtime Komponente ist a. b. winmd), kann Sie die öffentlichen Klassen A. b. Class1 und a. b. C. Klasse2 enthalten, aber keine. class3 oder D. Class4.

> [!NOTE]
> Diese Einschränkungen gelten nur für öffentliche Typen, nicht für die privaten Typen, die in der Implementierung verwendet werden.

Im Fall von A. class3 können Sie class3 entweder in einen anderen Namespace verschieben oder den Namen der Windows-Runtime Komponente in eine winmd-Komponente ändern. Im vorherigen Beispiel kann A.Class3 nicht vom Code, der A.B.WINMD aufruft, gefunden werden.

Im Fall von "d. Class4" darf kein Dateiname "d. Class4" und Klassen im Namespace "A. B" enthalten, sodass das Ändern des Namens der Windows-Runtime Komponente keine Option ist. Sie können D. Class4 entweder in einen anderen Namespace verschieben oder in eine andere Windows-Runtime Komponente einfügen.

Das Dateisystem kann nicht zwischen Groß-und Kleinschreibung unterscheiden, sodass Namespaces, die sich nach Groß-/Kleinschreibung unterscheiden, unzulässig sind (CsWinRT1002).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1001</p></td>
<td><p>Ein öffentlicher Typ hat einen Namespace (" {1} "), der kein gemeinsames Präfix mit anderen Namespaces (" {0} ") aufweist. Alle Typen in einer Windows-Metadatendatei müssen sich in einem Subnamespace des im Dateinamen enthaltenen Namespace befinden.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1002</p></td>
<td><p>Es wurden mehrere Namespaces mit dem Namen ' {0} ' gefunden; Namespace Namen dürfen sich nicht nur in der Groß-/Kleinschreibung im Windows-Runtime unterscheiden.</p></td>
</tr>
</tbody>
</table>

## <a name="exporting-types-that-arent-valid-windows-runtime-types"></a>Exportieren von Typen, die keine zulässigen Windows-Runtime-Typen sind

Die öffentliche Schnittstelle der Komponente darf nur Windows-Runtime Typen verfügbar machen. .Net bietet jedoch Zuordnungen für eine Reihe häufig verwendeter Typen, die in .net und der Windows-Runtime etwas unterschiedlich sind. Dadurch kann der .NET-Entwickler mit vertrauten Typen arbeiten, anstatt neue zu erlernen. Sie können diese zugeordneten .NET Framework-Typen in der öffentliche Schnittstelle der Komponente verwenden. Weitere Informationen finden Sie unter [Deklarieren von Typen in Windows-Runtime Komponenten](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#declaring-types-in-windows-runtime-components), [übergeben von Windows-Runtime Typen an verwalteten Code](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#passing-windows-runtime-types-to-managed-code)und .net-Zuordnungen [von Windows-Runtime Typen](../winrt-components/net-framework-mappings-of-windows-runtime-types.md).

Viele dieser Zuordnungen sind Schnittstellen. IList wird z \<T\> . b. der IVector-Schnittstelle Windows-Runtime \<T\> . Wenn Sie "List" \<string\> anstelle von "IList" \<string\> als Parametertyp verwenden, stellt c#/WinRT eine Liste von Alternativen bereit, die alle zugeordneten Schnittstellen enthält, die von List implementiert werden \<T\> . Bei Verwendung von geschachtelten generischen Typen, wie z. b. List \<Dictionary\<int, string\> \> , bietet c#/WinRT Optionen für jede Schachtelungs Ebene. Diese Listen können recht umfangreich sein.

Im Allgemeinen sollte die Schnittstelle ausgewählt werden, die dem Typ am nächsten ist. Beispielsweise ist für Dictionary \<int, string\> die beste Wahl die wahrscheinlichste IDictionary \<int, string\> .

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1006</p></td>
<td><p>Der-Member ' ' {0} weist in der Signatur den Typ ' {1} ' auf. Der Typ " {1} " ist kein gültiger Windows-Runtime Typ. Der Typ (oder seine generischen Parameter) implementiert jedoch Schnittstellen, die Windows-Runtime Typen gültig sind. Ändern Sie ggf. den Typ " {1} in der Element Signatur in einen der folgenden Typen aus System. Collections. Generic: {2} .</p></td>
</tr>
</tbody>
</table>

In der Windows-Runtime müssen Arrays in Element Signaturen eindimensional mit einer unteren Grenze von 0 (null) sein. Es sind keine Typen von Netz Arrays wie `myArray[][]` (CsWinRT1017) und `myArray[,]` (CsWinRT1018) zulässig.

> [!NOTE]
> Diese Einschränkung gilt nicht für Arrays, die Sie intern in der Implementierung verwenden.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1017</p></td>
<td><p>{0}Die Methode verfügt über ein Array vom Typ {1} in der Signatur. Arrays in Windows-Runtime Methoden Signatur können nicht eingefügt werden.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1018</p></td>
<td><p>Die Methode ' {0} ' weist in der Signatur ein mehrdimensionales Array vom Typ ' ' {1} auf. Arrays in Signaturen von Windows-Runtime-Methoden müssen eindimensional sein.</p></td>
</tr>
</tbody>
</table>

## <a name="structures-that-contain-fields-of-disallowed-types"></a>Strukturen, die Felder mit unzulässigen Typen enthalten

In der Windows-Runtime kann eine Struktur nur Felder enthalten, und nur Strukturen können Felder enthalten. Diese Felder müssen öffentlich sein. Zu den gültigen Feldtypen zählen Strukturen, Enumerationen und primitive Typen.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1007</p></td>
<td><p>Die Struktur {0} enthält keine öffentlichen Felder. Windows-Runtime Strukturen müssen mindestens ein öffentliches Feld enthalten.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1011</p></td>
<td><p>Struktur {0} hat ein nicht öffentliches Feld. Alle Felder müssen für Windows-Runtime Strukturen öffentlich sein.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1012</p></td>
<td><p>Struktur {0} hat ein Konstantenfeld. Konstanten können nur in Windows-Runtime Enumerationen angezeigt werden.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1013</p></td>
<td><p>Struktur {0} hat ein Feld vom Typ {1} ; {1} ist kein gültiger Windows-Runtime Feldtyp. Die Felder in einer Windows-Runtime-Struktur müssen vom Typ UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum sein oder selbst eine Struktur darstellen.</p></td>
</tr>
</tbody>
</table>

## <a name="parameter-name-conflicts-with-generated-code"></a>Parameter Name steht in Konflikt mit generiertem Code.

In der Windows-Runtime werden Rückgabewerte als Ausgabeparameter betrachtet, und die Namen der Parameter müssen eindeutig sein. Standardmäßig gibt c#/WinRT den Rückgabewert des Namens an `__retval` . Wenn die Methode einen Parameter mit dem Namen hat `__retval` , erhalten Sie die Fehlermeldung CsWinRT1010. Um dies zu korrigieren, benennen Sie den Parameter mit einem anderen Namen als `__retvalue` .

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1010</p></td>
<td><p>Der Parameter Name in der-Methode ist mit dem Parameternamen des {1} {0} Rückgabewerts identisch, der in der generierten c#-/WinRT-Interop verwendet wird. verwenden Sie einen anderen Parameternamen.</p></td>
</tr>
</tbody>
</table>

## <a name="miscellaneous"></a>Sonstiges

Weitere Einschränkungen in einer von c#/WinRT erstellten Komponente sind:

- Sie können keine überladenen Operatoren für öffentliche Typen verfügbar machen.
- Klassen und Schnittstellen können nicht generisch sein.
- Klassen müssen versiegelt sein.
- Parameter können nicht als Verweis übermittelt werden, z. b. mit dem **ref** -Schlüsselwort.
- Eigenschaften müssen über eine öffentliche get-Methode verfügen.
- Es muss mindestens ein öffentlicher Typ (Klasse oder Schnittstelle) im Namespace Ihrer Komponente vorhanden sein.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1014</p></td>
<td><p>" {0} " ist eine Operator Überladung. Verwaltete Typen können in der Windows-Runtime keine überladenen Operatoren verfügbar machen.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1004</p></td>
<td><p>Der Typ {0} ist generisch. Windows-Runtime Typen können nicht generisch sein.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1005</p></td>
<td><p>Das Exportieren nicht versiegelter Typen wird in cswinrt nicht unterstützt. Markieren Sie den Typ {0} als versiegelt.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1020</p></td>
<td><p>Die Methode ' ' {0} weist den markierten Parameter ' ' auf {1} `ref` . Verweis Parameter sind in Windows-Runtime nicht zulässig.</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1000</p></td>
<td><p>Die-Eigenschaft {0} weist keine öffentliche Getter-Methode auf. Windows-Runtime unterstützt keine Setter-only-Eigenschaften.</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1003</p></td>
<td><p>Windows-Runtime Komponenten müssen mindestens einen öffentlichen Typ aufweisen.</p></td>
</tr>
</tbody>
</table>

In der Windows-Runtime kann eine Klasse nur über einen Konstruktor mit einer bestimmten Anzahl von Parametern verfügen. Beispielsweise können Sie keinen Konstruktor mit einem einzelnen Parameter vom Typ " **String** " und einen anderen Konstruktor mit einem einzelnen Parameter vom Typ " **int**" haben. Die einzige Problem Umgehung besteht darin, für jeden Konstruktor eine andere Anzahl von Parametern zu verwenden.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Fehlernummer</p></th>
<th><p>Meldungstext</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1009</p></td>
<td><p>Klassen können nicht über mehrere Konstruktoren mit der gleichen Stelligkeit in der Windows-Runtime verfügen, und die Klasse {0} verfügt über mehrfach {1} ermittelungsekonstruktoren.</p></td>
</tr>
</tbody>
</table>


