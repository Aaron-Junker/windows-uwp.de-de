---
Description: Es gibt mehrere URI (Uniform Resource Identifier)-Schemen, die Sie verwenden können, um auf Dateien aus Ihrem App Paket, dem App-Ordner oder der Cloud zu verweisen. Sie können auch ein URI-Schema verwenden, um auf Zeichenfolgen zu verweisen, die von den App-Ressourcendateien (.resw) geladen wurden.
title: URI-Schemen
template: detail.hbs
ms.date: 10/16/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: b449179468d26c357e69ad1d8868004cadd6e2fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632055"
---
# <a name="uri-schemes"></a>URI-Schemen

Es gibt mehrere URI (Uniform Resource Identifier)-Schemen, die Sie verwenden können, um auf Dateien aus Ihrem App Paket, dem App-Ordner oder der Cloud zu verweisen. Sie können auch ein URI-Schema verwenden, um auf Zeichenfolgen zu verweisen, die von den App-Ressourcendateien (.resw) geladen wurden. Sie können diese URI-Schemen in Ihrem Code, im XAML-Markup, in Ihrem App-Paketmanifest oder in der Kachel und Popupbenachrichtigungsvorlage verwenden.

## <a name="common-features-of-the-uri-schemes"></a>Allgemeine Features der URI-Schemen

Alle in diesem Thema beschriebenen Schemen folgen den typischen URI-Schema-Regeln für die Normalisierung und den Ressourcenabruf. Weitere Informationen für die allgemeine Syntax für eine URI finden Sie unter [RFC 3986](https://go.microsoft.com/fwlink/p/?LinkId=263444).

Bei allen URI-Schemen wird der hierarchische Teil gemäß [RFC 3986](https://go.microsoft.com/fwlink/p/?LinkId=263444) als die Autoritäts- und Pfadkomponenten des URI definiert:

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Dies bedeutet, dass es im Wesentlichen drei Komponenten für einen URI gibt. Unmittelbar nach den Schrägstrichen des URI *Schemas* ist eine Komponente (ggf. leer), die als *authority* bezeichnet ist. Und direkt danach befindet sich *path*. Als Beispiel: Bei der URI `http://www.contoso.com/welcome.png` ist das Schema "`http://`", die Autorität "`www.contoso.com`" und der Pfad "`/welcome.png`". Ein weiteres Beispiel ist die URI `ms-appx:///logo.png`, wobei die Autorität für die Komponenten leer ist und einen Standardwert annimmt.

Die Fragment-Komponente wird bei der schemenspezifischen Verarbeitung der hier aufgeführten URIs ignoriert. Beim Ressourcenabruf und Vergleich hat die Fragment-Komponente keine Bedeutung. Ebenen über einer bestimmten Implementierung können die Fragment-Komponente jedoch so interpretieren, dass sie eine sekundäre Ressource abruft.

Der Vergleich findet byteweise nach der Normalisierung aller IRI-Komponenten statt.

## <a name="case-insensitivity-and-normalization"></a>Das Ignorieren der Groß-/Kleinschreibung und die Normalisierung

Alle in diesem Thema beschriebenen URI-Schemen folgen den typischen URI-Regeln (RFC 3986) für die Normalisierung und den Ressourcenabruf für Schemen. Bei der normalisierten Form der URI bleibt die Groß-/Kleinschreibung erhalten, und von nicht reservierten RFC 3986-Zeichen werden die Prozentzeichen entfernt.

Für alle in diesem Thema beschriebenen URI-Schemen ignorieren *Schema*, *Autorität* und *Pfad* entweder standardmäßig die Groß- und Kleinschreibung oder werden andernfalls vom System als Ignorieren der Groß- und Kleinschreibung verarbeitet. **Hinweis:** Die einzige Ausnahme von dieser Regel ist die *Autorität*`ms-resource`, bei die Schreibweise von Bedeutung ist.

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx und ms-appx-web

Verwenden Sie `ms-appx` oder `ms-appx-web`-URI-Schemen, um auf eine Datei zu verweisen, die aus Ihrem App-Paket stammt (siehe [Verpacken von Apps](../packaging/index.md)). Bei diesen Dateien Ihres App-Pakets handelt es sich in der Regel um statische Bilder, Daten, Code und Layoutdateien. Das `ms-appx-web`-Schema greift auf die gleichen Dateien wie `ms-appx` zu, jedoch im Webdepot. Beispiele und weitere Informationen finden Sie unter [Verweisen auf ein Bild oder eine Ressource aus XAML-Markup und Code](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Schemaname (ms-appx und ms-appx-web)

Die Zeichenfolge "ms-appx" oder "ms-appx-web" stellt den URI-Schemanamen dar.

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autorität (ms-appx und ms-appx-web)

Die Autorität ist der Paketidentitätsname, der im Paketmanifest definiert ist. Sie ist daher sowohl in der URI- als auch der IRI (Internationalized Resource Identifier)-Form auf den für den Paketidentitätsnamen zulässigen Zeichensatz beschränkt. Der Paketname muss ein Namen des Pakets im Paketabhängigkeitsdiagramms der ausgeführten App sein.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Werden in der Autorität andere Zeichen angegeben, schlagen Abruf und Vergleich fehl. Der Standardwert für die Autorität ist das derzeit ausgeführte App-Paket.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Benutzerinformationen und Port (ms-appx und ms-appx-web)

Das `ms-appx`-Schema definiert im Gegensatz zu anderen beliebten Schemen keine Benutzerinformationen oder Portkomponente. Da "@" and ":" nicht als gültige Autoritätswerte zulässig sind, schlägt die Suche fehl, wenn diese Zeichen enthalten sind. Folgende Beispiele schlagen fehl.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Pfad (ms-appx und ms-appx-web)

Die Pfadkomponente entspricht der generischen RFC 3986-Syntax und unterstützt Nicht-ASCII-Zeichen in IRIs. Die Pfadkomponente definiert den logischen oder physischen Dateipfad einer Datei. Diese Datei ist in einem Ordner enthalten, der mit dem Installationsort des Pakets einer von der Autorität angegebenen App verknüpft ist.

Wenn der Pfad auf einem physischen Pfad und Dateiname verweist, wird die physische Dateiressource abgerufen. Wenn keine physische Datei gefunden wird, wird die tatsächlich während des Abrufs zurückgegebene Ressource zur Laufzeit mittels Inhaltsaushandlung ermittelt. Diese Ermittlung basiert auf App, Betriebssystem und Benutzereinstellungen wie Sprache, Skalierungsfaktor, Designs, hoher Kontrast und andere Laufzeitkontexte. Eine Kombination der Sprachen der App, die Anzeigeeinstellungen des Systems und die hohen Kontrasteinstellungen des Benutzers werden beispielsweise  berücksichtigt, wenn der tatsächliche abzurufende Ressourcenwert ermittelt wird:

```xml
ms-appx:///images/logo.png
```

Die oben genannte URI kann tatsächlich eine Datei im aktuellen App-Paket mit folgendem physischen Dateinamen abrufen.

```
\Images\fr-FR\logo.scale-100_contrast-white.png
```

Sie können natürlich auch die gleiche physische Datei abrufen, indem Sie direkt mit dem vollständigen Namen darauf verweisen.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

Bei der Pfadkomponente von `ms-appx(-web)` muss wie bei generischen URIs die Groß-/Kleinschreibung beachtet werden. Wenn beim zugrunde liegenden Dateisystem, von dem auf die Ressource zugegriffen wird, wie bei NTFS die Groß-/Kleinschreibung nicht berücksichtigt wird, wird auch beim Abrufen der Ressource die Groß-/Kleinschreibung ignoriert.

Bei der normalisierten Form des URI bleibt die Groß-/Kleinschreibung erhalten, und von nicht reservierten RFC 3986-Zeichen werden die Prozentzeichen entfernt. Die Zeichen "?", "#", "/", "*" und '”' (doppelte Anführungszeichen) müssen in einem Pfad mit einem Prozentzeichen versehen werden, um Daten wie Datei- oder Ordnernamen anzugeben. Alle mit Prozentzeichen versehenen Zeichen werden vor dem Abrufen decodiert. Verwenden Sie daher zum Abrufen der Datei "Hello#World.html" diese URI.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Abfrage (ms-appx und ms-appx-web)

Abfrageparameter werden während des Abrufens von Ressourcen ignoriert. Bei der normalisierten Form von Abfrageparametern wird die Groß-/Kleinschreibung erhalten. Abfrageparameter werden beim Vergleich nicht ignoriert.

## <a name="ms-appdata"></a>ms-appdata

Verweisen Sie mithilfe des `ms-appdata`-URI Schemas auf App-Dateien, die aus den lokalen und temporären Datenordnern sowie Roamingdatenordnern stammen. Weitere Informationen zu diesen App-Datendateien finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](../design/app-settings/store-and-retrieve-app-data.md).

Das `ms-appdata`-URI-Schema führt keine Laufzeit mittels Inhaltsaushandlung durch, die [ms-appx and ms-appx-web](#ms-appx-and-ms-appx-web) durchführen. Sie können allerdings auf den Inhalt des [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) reagieren und die richtigen Ressourcen über den gesamten physischen Dateiname im URI in App-Daten laden.

### <a name="scheme-name-ms-appdata"></a>Schemaname (ms-appdata)

Die Zeichenfolge „ms-appdata” stellt den URI-Schemanamen dar.

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autorität (ms-appdata)

Die Autorität ist der Paketidentitätsname, der im Paketmanifest definiert ist. Sie ist daher sowohl in der URI- als auch der IRI (Internationalized Resource Identifier)-Form auf den für den Paketidentitätsnamen zulässigen Zeichensatz beschränkt. Der Paketname muss der Name des aktuellen Pakets der ausgeführten App sein.

```xml
ms-appdata://Contoso.MyApp/
```

Werden in der Autorität andere Zeichen angegeben, schlagen Abruf und Vergleich fehl. Der Standardwert für die Autorität ist das derzeit ausgeführte App-Paket.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Benutzerinformationen und Port (ms-appdata)

Das `ms-appdata`-Schema definiert im Gegensatz zu anderen beliebten Schemen keine Benutzerinformationen oder Portkomponente. Da "@" and ":" nicht als gültige Autoritätswerte zulässig sind, schlägt die Suche fehl, wenn diese Zeichen enthalten sind. Folgende Beispiele schlagen fehl.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Pfad (ms-appdata)

Die Pfadkomponente entspricht der generischen RFC 3986-Syntax und unterstützt Nicht-ASCII-Zeichen in IRIs. Am Speicherort [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) befinden sich drei reservierte Ordner für lokale und temporäre Statusspeicher sowie für Roamingstatusspeicher. Das Schema `ms-appdata` ermöglicht den Zugriff auf Dateien und Ordner an diesen Speicherorten. Im ersten Segment der Pfadkomponente muss der bestimmte Ordner in folgendem Format angegeben werden. Daher ist die Form „path-empty” von „hier-part” nicht zulässig.

Lokaler Ordner.

```xml
ms-appdata:///local/
```

Temporärer Ordner.

```xml
ms-appdata:///temp/
```

Roamingordner.

```xml
ms-appdata:///roaming/
```

Bei der Pfadkomponente von `ms-appdata` muss wie bei generischen URIs die Groß-/Kleinschreibung beachtet werden. Wenn beim zugrunde liegenden Dateisystem, von dem auf die Ressource zugegriffen wird, wie bei NTFS die Groß-/Kleinschreibung nicht berücksichtigt wird, wird auch beim Abrufen der Ressource die Groß-/Kleinschreibung ignoriert.

Bei der normalisierten Form des URI bleibt die Groß-/Kleinschreibung erhalten, und von nicht reservierten RFC 3986-Zeichen werden die Prozentzeichen entfernt. Die Zeichen "?", "#", "/", "*" und '”' (doppelte Anführungszeichen) müssen in einem Pfad mit einem Prozentzeichen versehen werden, um Daten wie Datei- oder Ordnernamen anzugeben. Alle mit Prozentzeichen versehenen Zeichen werden vor dem Abrufen decodiert. Verwenden Sie daher zum Abrufen der lokalen Datei "Hello#World.html" diese URI.

```xml
ms-appdata://local/Hello%23World.html
```

Der Abruf der Ressource und die Identifikation des Segments für den Pfad auf oberster Ebene werden nach der Normalisierung von Punkten behandelt (".././b/c"). Daher können bei URIs keine Punkte für die reservierten Ordner verwendet werden. Die folgenden URI sind also nicht zulässig.

```xml
ms-appdata:///local/../hello/logo.png
```

Diese URI ist jedoch zulässig (wenn auch redundant).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>Abfrage (ms-appdata)

Abfrageparameter werden während des Abrufens von Ressourcen ignoriert. Bei der normalisierten Form von Abfrageparametern wird die Groß-/Kleinschreibung erhalten. Abfrageparameter werden beim Vergleich nicht ignoriert.

## <a name="ms-resource"></a>ms-resource

Verwenden Sie das URI-Schema `ms-resource` , um auf Zeichenfolgen zu verweisen, die aus den Ressourcendateien Ihrer App (.resw) geladen wurden. Weitere Beispiele und Informationen zu Ressourcendateien (.resw) finden Sie unter [Lokalisieren der Zeichenfolge im Paketmanifest der Benutzeroberfläche und der App](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Schemaname (ms-resource)

Die Zeichenfolge „ms-resource” stellt den URI-Schemanamen dar.

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autorität (ms-resource)

Bei der Autorität handelt es sich um die im Package Resource Index (PRI) definierte Ressourcenzuordnung der obersten Ebene, die in der Regel dem Paketidentitätsnamen entspricht, der im Paketmanifest festgelegt ist. Siehe [Verpacken von Apps](../packaging/index.md). Sie ist daher sowohl in der URI- als auch der IRI (Internationalized Resource Identifier)-Form auf den für den Paketidentitätsnamen zulässigen Zeichensatz beschränkt. Der Paketname muss ein Namen des Pakets im Paketabhängigkeitsdiagramms der ausgeführten App sein.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Werden in der Autorität andere Zeichen angegeben, schlagen Abruf und Vergleich fehl. Der Standardwert für die Autorität ist der die Groß-/Kleinschreibung beachtende Name der aktuell ausgeführten App.

```xml
ms-resource:///
```

Bei der Autorität wird die Groß-/Kleinschreibung berücksichtigt und bei der normalisierten Form die Groß-/Kleinschreibung erhalten. Bei der Suche nach einer Ressource wird die Groß-/Kleinschreibung jedoch ignoriert.

### <a name="user-info-and-port-ms-resource"></a>Benutzerinformationen und Port (ms-resource)

Das `ms-resource`-Schema definiert im Gegensatz zu anderen beliebten Schemen keine Benutzerinformationen oder Portkomponente. Da "@" and ":" nicht als gültige Autoritätswerte zulässig sind, schlägt die Suche fehl, wenn diese Zeichen enthalten sind. Folgende Beispiele schlagen fehl.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Pfad (ms-resource)

Der Pfad gibt den hierarchischen Ort der [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live)-Unterstruktur (siehe [Resource Management System](https://msdn.microsoft.com/library/windows/apps/jj552947)) und das darin enthaltene [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live)-Element an. Dies entspricht in der Regel dem Dateinamen (ohne Erweiterung) der Ressourcendatei (.resw) und dem Bezeichner der darin enthaltenen Zeichenfolgenressource.

Beispiele und weitere Informationen finden Sie unter [Lokalisieren der Zeichenfolge im Paketmanifest der Benutzeroberfläche und der App](localize-strings-ui-manifest.md) und [Unterstützte Kachel- und Popupbenachrichtigungen für Sprache, Skalierungsfaktor und hohen Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

Bei der Pfadkomponente von `ms-resource` muss wie bei generischen URIs die Groß-/Kleinschreibung beachtet werden. Das zugrunde liegende abrufen wird jedoch eine [CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628) mit *IgnoreCase* festgelegt `true`.

Bei der normalisierten Form des URI bleibt die Groß-/Kleinschreibung erhalten, und von nicht reservierten RFC 3986-Zeichen werden die Prozentzeichen entfernt. Die Zeichen "?", "#", "/", "*" und '”' (doppelte Anführungszeichen) müssen in einem Pfad mit einem Prozentzeichen versehen werden, um Daten wie Datei- oder Ordnernamen anzugeben. Alle mit Prozentzeichen versehenen Zeichen werden vor dem Abrufen decodiert. Daher zum Abrufen von einer Zeichenfolgenressource aus einer Ressourcendatei namens `Hello#World.resw`, diesen URI verwenden.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Abfrage (ms-resource)

Abfrageparameter werden während des Abrufens von Ressourcen ignoriert. Bei der normalisierten Form von Abfrageparametern wird die Groß-/Kleinschreibung erhalten. Abfrageparameter werden beim Vergleich nicht ignoriert. Beim Vergleichen von Abfrageparametern wird die Groß-/Kleinschreibung berücksichtigt.

Entwickler bestimmter Komponenten, die sich in Ebenen über dieser URI-Analyse befinden, können die Abfrageparameter flexibel verwenden.

## <a name="related-topics"></a>Verwandte Themen

* [Uniform Resource Identifier (URI): Generische Syntax](https://go.microsoft.com/fwlink/p/?LinkId=263444)
* [Verpacken von Apps](../packaging/index.md)
* [Verweisen auf ein Bild oder eine andere Anlage aus XAML-Markup und code](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Speichern und Abrufen von Einstellungen und anderen App-Daten](../design/app-settings/store-and-retrieve-app-data.md)
* [Lokalisieren von Zeichenfolgen auf der Benutzeroberfläche und im App-Paketmanifest](localize-strings-ui-manifest.md)
* [Ressourcenverwaltungssystem](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [Kachel- und popupbildgrößen benachrichtigungsunterstützung für Sprache, Skalierung und hoher Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)