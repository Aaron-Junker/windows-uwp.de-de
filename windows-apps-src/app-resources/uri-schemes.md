---
description: Es gibt mehrere URI (Uniform Resource Identifier)-Schemen, die Sie verwenden können, um auf Dateien aus Ihrem App Paket, dem App-Ordner oder der Cloud zu verweisen. Sie können auch ein URI-Schema verwenden, um auf Zeichenfolgen zu verweisen, die von den App-Ressourcendateien (.resw) geladen wurden.
title: URI-Schemas
template: detail.hbs
ms.date: 10/16/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 8806992ebb7f4335ca0a748c1b2bce4a6de39fae
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031544"
---
# <a name="uri-schemes"></a>URI-Schemas

Es gibt mehrere URI (Uniform Resource Identifier)-Schemen, die Sie verwenden können, um auf Dateien aus Ihrem App Paket, dem App-Ordner oder der Cloud zu verweisen. Sie können auch ein URI-Schema verwenden, um auf Zeichenfolgen zu verweisen, die von den App-Ressourcendateien (.resw) geladen wurden. Sie können diese URI-Schemas im Code, in Ihrem XAML-Markup, im App-Paket Manifest oder in ihren Kachel-und Popup Benachrichtigungs Vorlagen verwenden.

## <a name="common-features-of-the-uri-schemes"></a>Allgemeine Funktionen der URI-Schemas

Alle in diesem Thema beschriebenen Schemas befolgen typische URI-Schema Regeln für Normalisierung und Ressourcen Abruf. Die generische Syntax eines URIs finden Sie unter [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) .

Alle URI-Schemas definieren den hierarchischen Teil pro [RFC 3986](https://www.ietf.org/rfc/rfc3986.txt) als Autoritäts-und Pfad Komponenten des URI.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Dies bedeutet, dass im Wesentlichen drei Komponenten für einen URI vorhanden sind. Unmittelbar nach den beiden Schrägstrichen des URI- *Schemas* ist eine Komponente (die leer sein kann) als *Autorität* bezeichnet. Und unmittelbar darauf folgt der *Pfad* . Wenn Sie den URI als Beispiel nehmen, lautet das Schema "" `http://www.contoso.com/welcome.png` `http://` , die Autorität ist " `www.contoso.com` ", und der Pfad ist " `/welcome.png` ". Ein weiteres Beispiel ist der URI `ms-appx:///logo.png` , bei dem die Autoritäts Komponenten leer sind und einen Standardwert annimmt.

Die fragmentkomponente wird von der Schema spezifischen Verarbeitung der in diesem Thema erwähnten URIs ignoriert. Beim Abrufen und Vergleichen von Ressourcen hat die fragmentkomponente keine Auswirkungen. Allerdings können Ebenen oberhalb der spezifischen Implementierung das Fragment interpretieren, um eine sekundäre Ressource abzurufen.

Der Vergleich erfolgt in Byte nach der Normalisierung aller IRI-Komponenten.

## <a name="case-insensitivity-and-normalization"></a>Nichtbeachtung der Groß-/Kleinschreibung und Normalisierung

Alle in diesem Thema beschriebenen URI-Schemas befolgen typische URI-Regeln (RFC 3986) für die Normalisierung und den Ressourcen Abruf für Schemas. Die normalisierte Form dieser URIs verwaltet die Groß-/Kleinschreibung und die prozentuale Decodieren von nicht reservierten Zeichen in RFC 3986.

Für alle in diesem Thema beschriebenen URI- *Schemas* werden von Schema, *Autorität* und *Pfad* weder die Groß-/Kleinschreibung als auch die Groß-/Kleinschreibung beachtet. **Hinweis** Die einzige Ausnahme von dieser Regel ist die *Autorität* von `ms-resource` , bei der die Groß-/Kleinschreibung beachtet wird.

## <a name="ms-appx-and-ms-appx-web"></a>MS-AppX und MS-AppX-Web

Verwenden Sie das `ms-appx` `ms-appx-web` URI-Schema oder, um auf eine Datei zu verweisen, die aus dem Paket Ihrer APP stammt (siehe [Verpacken von apps](../packaging/index.md)). Dateien in Ihrem App-Paket sind in der Regel statische Bilder, Daten, Code und Layoutdateien. Das `ms-appx-web` Schema greift auf dieselben Dateien wie zu `ms-appx` , aber im webdepot. Beispiele und weitere Informationen finden Sie unter [verweisen auf ein Bild oder ein anderes Asset aus XAML-Markup und-Code](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Schema Name (MS-AppX und MS-AppX-Web)

Der URI-Schema Name ist die Zeichenfolge "MS-AppX" oder "MS-AppX-Web".

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autorität (MS-AppX und MS-AppX-Web)

Die Autorität ist der Name der Paket Identität, der im Paket Manifest definiert ist. Daher ist Sie sowohl im URI als auch im IRI-Formular (Internationalized Resource Identifier) auf den Zeichensatz beschränkt, der in einem Paket Identitäts Namen zulässig ist. Der Paketname muss der Name eines der Pakete im Paket Abhängigkeits Diagramm der aktuellen APP sein.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Wenn ein beliebiges anderes Zeichen in der Autorität angezeigt wird, schlagen Abruf und Vergleich fehl. Der Standardwert für die Autorität ist das Paket, das zurzeit in der app ausgeführt wird.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Benutzerinformationen und Port (MS-AppX und MS-AppX-Web)

Das `ms-appx` Schema definiert im Gegensatz zu anderen gängigen Schemas keine Benutzerinformationen oder eine Port Komponente. Da " @" and " :" nicht als gültige Autorität-Werte zulässig ist, schlägt die Suche fehl, wenn Sie eingeschlossen werden. Jedes der folgenden Fehler schlägt fehl.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Path (MS-AppX und MS-AppX-Web)

Die Pfadkomponente entspricht der generischen RFC 3986-Syntax und unterstützt nicht-ASCII-Zeichen in IRIS. Die Pfadkomponente definiert den logischen oder physischen Dateipfad einer Datei. Diese Datei befindet sich in einem Ordner, der dem installierten Speicherort des App-Pakets für die von der Autorität angegebene App zugeordnet ist.

Wenn sich der Pfad auf einen physischen Pfad und Dateinamen bezieht, wird das physische dateiasset abgerufen. Wenn jedoch keine solche physische Datei gefunden wird, wird die tatsächliche Ressource, die während des Abrufs zurückgegeben wird, mithilfe der Inhalts Aushandlung zur Laufzeit bestimmt. Diese Bestimmung basiert auf app-, Betriebssystem-und Benutzereinstellungen, wie z. b. Sprache, Anzeige Skalierungsfaktor, Design, hohem Kontrast und anderen Lauf Zeit Kontexten. Beispielsweise kann eine Kombination aus den Sprachen der APP, den Anzeigeeinstellungen des Systems und den Einstellungen für den hohen Kontrast des Benutzers berücksichtigt werden, wenn der tatsächliche Ressourcen Wert festgelegt wird, der abgerufen werden soll.

```xml
ms-appx:///images/logo.png
```

Der obige URI kann tatsächlich eine Datei innerhalb des aktuellen App-Pakets mit dem folgenden physischen Dateinamen abrufen.

<blockquote>
<pre>
\Images\fr-FR\logo.scale-100_contrast-white.png
</blockquote>
</pre>

Natürlich können Sie dieselbe physische Datei auch abrufen, indem Sie direkt mit dem vollständigen Namen darauf verweisen.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

Die Pfadkomponente von `ms-appx(-web)` ist, wie bei generischen URIs, Groß-/Kleinschreibung beachtet. Wenn jedoch das zugrunde liegende Dateisystem, mit dem auf die Ressource zugegriffen wird, Groß-/Kleinschreibung nicht beachtet, wie z. b. für NTFS, wird das Abrufen der Ressource ohne Beachtung der Groß-/Kleinschreibung

Die normalisierte Form des URIs verwaltet die Groß-/Kleinschreibung und die prozentuale decodierungen (ein "%"-Symbol, gefolgt von der zweistelligen hexadezimalen Darstellung) RFC 3986 nicht reservierte Zeichen. Die Zeichen "?", "#", "/", "*" und "" (doppelte Anführungszeichen) müssen in einem Pfad Prozent codiert sein, um Daten wie Datei-oder Ordnernamen darzustellen. Alle Prozent codierten Zeichen werden vor dem Abruf entschlüsselt. Verwenden Sie daher diesen URI, um eine Datei mit dem Namen "Hello # World.html" abzurufen.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Abfrage (MS-AppX und MS-AppX-Web)

Abfrage Parameter werden beim Abrufen von Ressourcen ignoriert. Die normalisierte Form von Abfrage Parametern verwaltet die Groß-/Kleinschreibung. Abfrage Parameter werden während des Vergleichs nicht ignoriert.

## <a name="ms-appdata"></a>ms-appdata

Verwenden `ms-appdata` Sie das URI-Schema, um auf Dateien zu verweisen, die aus den lokalen, Roaming-und temporären Daten Ordnern der APP stammen. Weitere Informationen zu diesen app-Daten Ordnern finden Sie unter [Speichern und Abrufen von Einstellungen und anderen APP-Daten](../design/app-settings/store-and-retrieve-app-data.md).

Das `ms-appdata` URI-Schema führt die Aushandlung von Lauf Zeit Inhalten nicht durch, die von [MS-AppX und MS-AppX-Web](#ms-appx-and-ms-appx-web) erledigt werden. Sie können jedoch auf den Inhalt von [resourcecontext. qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) reagieren und die entsprechenden Ressourcen aus app-Daten mit dem vollständigen physischen Dateinamen im URI laden.

### <a name="scheme-name-ms-appdata"></a>Schema Name (MS-APPDATA)

Der URI-Schema Name ist die Zeichenfolge "MS-AppData".

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autorität (MS-APPDATA)

Die Autorität ist der Name der Paket Identität, der im Paket Manifest definiert ist. Daher ist Sie sowohl im URI als auch im IRI-Formular (Internationalized Resource Identifier) auf den Zeichensatz beschränkt, der in einem Paket Identitäts Namen zulässig ist. Der Paketname muss der Name des Pakets der aktuellen APP sein.

```xml
ms-appdata://Contoso.MyApp/
```

Wenn ein beliebiges anderes Zeichen in der Autorität angezeigt wird, schlagen Abruf und Vergleich fehl. Der Standardwert für die Autorität ist das Paket, das zurzeit in der app ausgeführt wird.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Benutzerinformationen und Port (MS-APPDATA)

Das `ms-appdata` Schema definiert im Gegensatz zu anderen gängigen Schemas keine Benutzerinformationen oder eine Port Komponente. Da " @" and " :" nicht als gültige Autorität-Werte zulässig ist, schlägt die Suche fehl, wenn Sie eingeschlossen werden. Jedes der folgenden Fehler schlägt fehl.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Path (MS-APPDATA)

Die Pfadkomponente entspricht der generischen RFC 3986-Syntax und unterstützt nicht-ASCII-Zeichen in IRIS. Innerhalb des Ordners " [Windows. Storage. ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) " befinden sich drei Reservierte Ordner für den lokalen, Roaming-und temporären Zustands Speicher. Das `ms-appdata` Schema ermöglicht den Zugriff auf Dateien und Ordner an diesen Speicherorten. Das erste Segment der Pfadkomponente muss den jeweiligen Ordner auf folgende Weise angeben. Daher ist die "Path-Empty"-Form von "hier-Part" nicht zulässig.

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

Die Pfadkomponente von `ms-appdata` ist, wie bei generischen URIs, Groß-/Kleinschreibung beachtet. Wenn jedoch das zugrunde liegende Dateisystem, mit dem auf die Ressource zugegriffen wird, Groß-/Kleinschreibung nicht beachtet, wie z. b. für NTFS, wird das Abrufen der Ressource ohne Beachtung der Groß-/Kleinschreibung

Die normalisierte Form des URIs verwaltet die Groß-/Kleinschreibung und die prozentuale decodierungen (ein "%"-Symbol, gefolgt von der zweistelligen hexadezimalen Darstellung) RFC 3986 nicht reservierte Zeichen. Die Zeichen "?", "#", "/", "*" und "" (doppelte Anführungszeichen) müssen in einem Pfad Prozent codiert sein, um Daten wie Datei-oder Ordnernamen darzustellen. Alle Prozent codierten Zeichen werden vor dem Abruf entschlüsselt. Verwenden Sie daher diesen URI, um eine lokale Datei mit dem Namen "Hello # World.html" abzurufen.

```xml
ms-appdata://local/Hello%23World.html
```

Das Abrufen der Ressource und die Identifizierung des Pfad Segments der obersten Ebene werden nach der Normalisierung der Punkte (".. /./b/c"). Daher können sich URIs nicht selbst aus einem der reservierten Ordner heraus positionieren. Daher ist der folgende URI nicht zulässig.

```xml
ms-appdata:///local/../hello/logo.png
```

Dieser URI ist jedoch zulässig (wenn redundant).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>Abfrage (MS-APPDATA)

Abfrage Parameter werden beim Abrufen von Ressourcen ignoriert. Die normalisierte Form von Abfrage Parametern verwaltet die Groß-/Kleinschreibung. Abfrage Parameter werden während des Vergleichs nicht ignoriert.

## <a name="ms-resource"></a>MS-Ressource

Verwenden Sie das `ms-resource` URI-Schema, um auf Zeichen folgen zu verweisen, die aus den Ressourcen Dateien (. resw) Ihrer APP geladen wurden. Beispiele und weitere Informationen zu Ressourcen Dateien finden Sie unter Lokalisieren von Zeichen folgen [in der Benutzeroberfläche und im App-Paket Manifest](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Schema Name (MS-Resource)

Der URI-Schema Name ist die Zeichenfolge "MS-Resource".

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autorität (MS-Resource)

Die Autorität ist die im Paket Ressourcen Index (PRI) definierte Ressourcen Zuordnung auf oberster Ebene, die in der Regel dem im Paket Manifest definierten Paket Identitäts Namen entspricht. Siehe [Verpacken von apps](../packaging/index.md)). Daher ist Sie sowohl im URI als auch im IRI-Formular (Internationalized Resource Identifier) auf den Zeichensatz beschränkt, der in einem Paket Identitäts Namen zulässig ist. Der Paketname muss der Name eines der Pakete im Paket Abhängigkeits Diagramm der aktuellen APP sein.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Wenn ein beliebiges anderes Zeichen in der Autorität angezeigt wird, schlagen Abruf und Vergleich fehl. Der Standardwert für die-Autorität ist der Paketname der aktuell laufenden app, der Groß-und Kleinschreibung beachtet.

```xml
ms-resource:///
```

Bei der Zertifizierungsstelle wird die Groß-/Kleinschreibung beachtet, und das normalisierte Formular behält den Fall Bei der Suche nach einer Ressource erfolgt die Groß-/Kleinschreibung jedoch nicht.

### <a name="user-info-and-port-ms-resource"></a>Benutzerinformationen und Port (MS-Resource)

Das `ms-resource` Schema definiert im Gegensatz zu anderen gängigen Schemas keine Benutzerinformationen oder eine Port Komponente. Da " @" and " :" nicht als gültige Autorität-Werte zulässig ist, schlägt die Suche fehl, wenn Sie eingeschlossen werden. Jedes der folgenden Fehler schlägt fehl.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Path (MS-Resource)

Der Pfad identifiziert den hierarchischen Speicherort der [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) -Unterstruktur (siehe [Resource Management System](/previous-versions/windows/apps/jj552947(v=win.10))) und den darin enthaltenen [namedresource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) . In der Regel entspricht dies dem Dateinamen (ausgenommen Erweiterung) einer Ressourcen Datei (. resw) und dem Bezeichner einer darin enthaltenen Zeichen folgen Ressource.

Beispiele und weitere Informationen finden Sie unter Lokalisieren von Zeichen folgen [in Ihrer Benutzeroberfläche und App-Paket Manifest](localize-strings-ui-manifest.md) und [Unterstützung von Kacheln und Popup Benachrichtigungen für Sprache, Skalierung und hohen Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

Die Pfadkomponente von `ms-resource` ist, wie bei generischen URIs, Groß-/Kleinschreibung beachtet. Der zugrunde liegende Abruf bewirkt jedoch eine [comparestringordinal](/windows/desktop/api/winstring/nf-winstring-windowscomparestringordinal) , bei der *ignoreCase* auf festgelegt ist `true` .

Die normalisierte Form des URIs verwaltet die Groß-/Kleinschreibung und die prozentuale decodierungen (ein "%"-Symbol, gefolgt von der zweistelligen hexadezimalen Darstellung) RFC 3986 nicht reservierte Zeichen. Die Zeichen "?", "#", "/", "*" und "" (doppelte Anführungszeichen) müssen in einem Pfad Prozent codiert sein, um Daten wie Datei-oder Ordnernamen darzustellen. Alle Prozent codierten Zeichen werden vor dem Abruf entschlüsselt. Verwenden Sie daher diesen URI, um eine Zeichen folgen Ressource aus einer Ressourcen Datei mit dem Namen abzurufen `Hello#World.resw` .

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Abfrage (MS-Resource)

Abfrage Parameter werden beim Abrufen von Ressourcen ignoriert. Die normalisierte Form von Abfrage Parametern verwaltet die Groß-/Kleinschreibung. Abfrage Parameter werden während des Vergleichs nicht ignoriert. Abfrage Parameter werden groß-und Kleinschreibung verglichen.

Entwickler bestimmter Komponenten, die oberhalb dieser URI-Verarbeitung angeordnet sind, können die Abfrage Parameter so verwenden, wie Sie sichtbar sind.

## <a name="related-topics"></a>Verwandte Themen

* [Uniform Resource Identifier (URI): generische Syntax](https://www.ietf.org/rfc/rfc3986.txt)
* [Verpacken von Apps](../packaging/index.md)
* [Verweisen auf ein Bild oder ein anderes Objekt aus XAML-Markup und Code](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Speichern und Abrufen von Einstellungen und anderen App-Daten](../design/app-settings/store-and-retrieve-app-data.md)
* [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest](localize-strings-ui-manifest.md)
* [Ressourcenverwaltungssystem](/previous-versions/windows/apps/jj552947(v=win.10))
* [Unterstützung für Kachel-und Popup Benachrichtigungen für Sprache, Skalierung und hohen Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
