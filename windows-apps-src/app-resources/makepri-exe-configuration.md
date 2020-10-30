---
description: In diesem Thema wird das Schema der MakePri.exe XML-Konfigurationsdatei beschrieben.
title: Konfigurationsdatei für „MakePri.exe“
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 5427927322f61f44cf3a8b53112f5f7811520290
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031723"
---
# <a name="makepriexe-configuration-file"></a>Konfigurationsdatei für „MakePri.exe“

In diesem Thema wird das Schema der [MakePri.exe](compile-resources-manually-with-makepri.md) XML-Konfigurationsdatei beschrieben. wird auch als PRI-Konfigurationsdatei bezeichnet. Das MakePri.exe Tool verfügt über einen [Befehl "conateconfig](makepri-exe-command-options.md#createconfig-command) ", mit dem Sie eine neue, initialisierte PRI-Konfigurationsdatei erstellen können.

> [!NOTE]
> MakePri.exe wird installiert, wenn Sie bei der Installation des Windows Software Development Kit die Option **Windows SDK für verwaltete UWP-apps** aktivieren. Es wird im Pfad installiert `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (sowie in Ordnern, die für die anderen Architekturen benannt sind). Beispiel: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

Die PRI-Konfigurationsdatei steuert, welche Ressourcen indiziert werden und wie. Die XML-Konfigurationsdatei muss dem folgenden Schema entsprechen.

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- Das- `default` Element gibt den Kontext (Sprache, Skalierung, Kontrast usw.) an, der zum Auflösen von Ressourcen verwendet werden soll, wenn der Lauf Zeit Kontext keinen Ressourcen Kandidaten entspricht. Da dieser Kontext zum Zeitpunkt der Erstellung angegeben wird und sich nicht ändert, werden Ressourcen in diesen Kontext aufgelöst, wenn Qualifizierer erstellt werden. Die übereinstimmende Bewertung wird zur Buildzeit gespeichert. Für jeden Qualifizierer muss ein Wert angegeben werden. Ausführliche Informationen zur Auswahl von Ressourcen finden Sie unter [resourcecontext](resource-management-system.md#resourcecontext) .
- Das- `index` Element definiert diskrete Indizierungs Durchgänge, die über die Assets durchgeführt werden. Jeder Indizierungs Durchlauf bestimmt die zu verwendenden [Format spezifischen Indexer](makepri-exe-format-specific-indexers.md) und die zu indizierenden Ressourcen.
- Das- `qualifiers` Element legt die anfänglichen Qualifizierer für die erste Datei bzw. den ersten Ordner fest, den andere Ressourcen erben Jedes qualifiziererelement muss einen gültigen Namen und Wert aufweisen (Weitere Informationen finden [Sie unter Anpassen von Ressourcen für Sprache, Skalierung, hoher Kontrast und andere Qualifizierer](tailor-resources-lang-scale-contrast.md)).
- Das- `root` Attribut ist der Pfad Stamm der physischen Datei für den Index Durchlauf. Sie kann relativ oder absolut sein. Wenn der Wert relativ ist, wird er an den Projektstamm angehängt, den Sie in der Befehlszeile angeben. Wenn der absolute Wert ist, wird er direkt als Index Durchlauf Stamm verwendet. Rückschräg Striche oder Schrägstriche sind zulässig. Nachfolgende Schrägstriche werden gekürzt. Der Stamm des Index Pass bestimmt den Ordner, in den alle Ressourcen als relativ betrachtet werden.
- Das- `startIndexAt` Attribut ist die anfängliche Seed-Datei oder der bei der Indizierung verwendete Ordner. Es ist relativ zum Index Pass Stamm. Ein leerer Wert geht von dem Index Pass-Stamm Ordner aus.

## <a name="default-pri-config-file"></a>Standardmäßige PRI-Konfigurationsdatei

MakePri.exe generiert diese neue, initialisierte PRI-Konfigurationsdatei, wenn der Befehl "| [ateconfig](makepri-exe-command-options.md#createconfig-command) " ausgegeben wird.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>Packaging-Element

Das- `packaging` Element definiert PRI-Split-Informationen. Das Schema für das `packaging` -Element wird sowohl für die automatische (Unterstützung für `autoResourcePackage` eine bestimmte Dimension) als auch für die manuelle Konfiguration definiert.

Dieses Beispiel zeigt, wie Sie `autoResourcePackage` eine bestimmte Dimension verwenden können.

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

In diesem Beispiel wird die Verwendung von Manual veranschaulicht `resourcePackage` .

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

MakePri.exe blockiert nicht explizit die Generierung von Ressourcen PRI-Dateien in einer bestimmten Dimension. Einschränkungen für einen bestimmten Satz von Dimensionen werden entweder durch MakeAppx.exe oder andere Tools in der Pipeline extern definiert und implementiert.

MakePri.exe analysiert das- `packaging` Element nach allen `index` Knoten, um alle Standard Qualifizierer aufzufüllen. MakePri.exe sammelt analysierte Informationen in diesen Datenstrukturen.

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>resources@isDeploymentMergeable-Attribut

Dieses Attribut legt ein Flag in der PRI-Datei fest, das bewirkt, dass

- Bereitstellung zusammenführen, um zu identifizieren, dass diese PRI-Datei zusammengeführt
- Getfullyqualifiedreferenzierung zum Zurückgeben eines Fehlers für den Fall, dass dieses Flag festgelegt ist und der Ressourcen-Manager mit einer Datei initialisiert wurde.

Der Standardwert dieses Attributs ist `true`. MakePri.exe legt das Flag nur dann in PRI fest, wenn Sie Windows 10 als Zielversion festlegen.

Es wird empfohlen, dass Sie `isDeploymentMergeable` für die Erstellung von Ressourcen Paketen (oder explizit auf) festlegen, `true` Wenn Sie Windows 10 als Ziel festlegen.

MakePri.exe fügt der Dumpdatei den Wert von hinzu `isDeploymentMergeable` , wenn `makepri dump` mit der- `/dt detailed` Option ausgeführt wird.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>resources@majorVersion-Attribut

Der Standardwert für dieses Attribut ist 1. Wenn Sie einen expliziten Wert angeben und außerdem die veraltete `/VersionMajor(vma)` Befehlszeilenoption für das MakePri.exe Tool verwenden, hat der Wert in der Konfigurationsdatei Vorrang.

Hier sehen Sie ein Beispiel.

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>resources@targetOsVersion-Attribut

Gibt die Version des Ziel Betriebssystems an. In der folgenden Tabelle sind die Werte aufgeführt, die unterstützt werden. der Standardwert ist 6.3.0.

| Wert | Bedeutung |
| ----- | ------- |
| 10.0.0 | Windows 10 |
| 6.3.0 (Standard) | Windows 8.1 |
| 6.2.1 | Windows 8 |

Hier sehen Sie ein Beispiel.

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**Hinweis** Windows ist in Bezug auf PRI-Dateien abwärts kompatibel. aber nicht immer vorwärts kompatibel.

MakePri.exe fügt der Dumpdatei den Wert von hinzu `targetOsVersion` , wenn `makepri dump` mit der- `/dt detailed` Option ausgeführt wird.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>Validierungs Fehlermeldungen

Im folgenden finden Sie einige Beispiel Fehlerbedingungen und die entsprechende Fehlermeldung.

| Bedingung | severity | `Message` |
| --------- | -------- | ------- |
| Eine targetosversion, die nicht mit einem der unterstützten Werte identisch ist, wird angegeben. | Fehler | Ungültige Konfiguration: Ungültige targetosversion angegeben. |
| Eine targetosversion von "6.2.1" wird angegeben, und `packaging` es ist ein-Element vorhanden. | Fehler | Ungültige Konfiguration: der Paketierungs Knoten wird mit dieser targetosversion nicht unterstützt. |
| Es wurden mehrere Modi in der Konfiguration gefunden. Beispielsweise sind Manual und autoresourcepackage angegeben. | Fehler | Ungültige Konfiguration: der "Packaging"-Knoten darf nicht mehr als einen Betriebsmodus aufweisen. |
| Unter Ressourcenpaket ist ein Standard Qualifizierer aufgeführt. | Fehler | Ungültige Konfiguration: <Qualifiername> = <QualifierValue> ist ein Standard Qualifizierer, und seine Kandidaten können keinem Ressourcenpaket hinzugefügt werden. |
| Der Qualifizierer autoresourcepackage enthält mehrere Qualifizierer. Beispielsweise language_scale. | Fehler | Ungültige Konfiguration: autoresourcepackage mit mehreren Qualifizierern wird nicht unterstützt. |
| Resourcepackage qualifierset enthält mehrere Qualifizierer. Beispiel: language-en-us_scale-100 | Fehler | Ungültige Konfiguration: qualifierset mit mehreren Qualifizierern wird nicht unterstützt. |
| Es wurde ein doppelter resourcepack-Name gefunden. | Fehler | Ungültige Konfiguration: Doppelter Name des Ressourcen Pakets <rpname> . |
| Derselbe qualifizierersatz, der in zwei Ressourcen Paketen definiert ist. | Fehler | Ungültige Konfiguration: Es wurden mehrere Instanzen von qualifierset " <qualifier tags> " gefunden. |
| Es wurden keine Kandidaten für das qualifierset gefunden, das für den Knoten "resourcepackage" aufgeführt ist. | Warnung | Ungültige Konfiguration: Es wurden keine Kandidaten für gefunden <Resource Package Name> . |
| Es wurden keine Kandidaten für den Qualifizierer gefunden, der unter dem Knoten "autoresourcepackage" aufgeführt | Warnung | Ungültige Konfiguration: für den Qualifizierer wurden keine Kandidaten gefunden <qualifier name> . Das Ressourcenpaket wurde nicht generiert. |
| Keiner der Modi wurde gefunden. Das heißt, es wurde ein leerer paketeknoten gefunden. | Warnung | Ungültige Konfiguration: Es wurde kein Paketmodus angegeben. |

## <a name="related-topics"></a>Verwandte Themen

* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
* [ Befehl "MakePri.exe Befehlszeilenoptionen" mit dem &mdash; Befehl "deeconfig"](makepri-exe-command-options.md#createconfig-command)
* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md)
* [Ressourcen Verwaltungs System &mdash; resourcecontext](resource-management-system.md#resourcecontext)
