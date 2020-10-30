---
description: In diesem Thema werden die Format spezifischen Indexer beschrieben, die vom MakePri.exe Tool verwendet werden, um den Index von Ressourcen zu generieren.
title: Formatspezifische Indexer für „MakePri.exe“
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 3794d369ae9d47cfc7aad1b24ca2768b04024581
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031693"
---
# <a name="makepriexe-format-specific-indexers"></a>Formatspezifische Indexer für „MakePri.exe“

In diesem Thema werden die Format spezifischen Indexer beschrieben, die vom [MakePri.exe](compile-resources-manually-with-makepri.md) Tool verwendet werden, um den Index von Ressourcen zu generieren.

> [!NOTE]
> MakePri.exe wird installiert, wenn Sie bei der Installation des Windows Software Development Kit die Option **Windows SDK für verwaltete UWP-apps** aktivieren. Es wird im Pfad installiert `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (sowie in Ordnern, die für die anderen Architekturen benannt sind). Beispiel: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

MakePri.exe wird in der Regel mit `new` den `versioned` Befehlen, oder verwendet `resourcepack` . Weitere Informationen finden Sie unter [MakePri.exe Befehlszeilenoptionen](makepri-exe-command-options.md). In diesen Fällen werden Quelldateien indiziert, um einen Index von Ressourcen zu generieren. MakePri.exe verwendet verschiedene einzelne Indexer, um unterschiedliche Quell Ressourcen Dateien oder Container für Ressourcen zu lesen. Der einfachste Indexer ist der ordnerindexer, der den Inhalt eines Ordners (z `.jpg` . b `.png` . oder Bilder) indiziert.

Sie identifizieren Format spezifische Indexer, indem Sie `<indexer-config>` Elemente in einem `<index>` Element der [MakePri.exe Konfigurationsdatei](makepri-exe-configuration.md)angeben. Das `type` -Attribut identifiziert den Format spezifischen Indexer, der verwendet wird.

Ressourcen Container, die während der Indizierung gefunden werden, erhalten in der Regel ihren Inhalt indiziert, anstatt dem Index selbst hinzugefügt zu werden Beispielsweise `.resjson` können Dateien, die der Ordner Indexer findet, von einem Indexer weiter indiziert werden `.resjson` . in diesem Fall wird die `.resjson` Datei selbst nicht im Index angezeigt. **Beachten Sie** , dass ein- `<indexer-config>` Element für den Indexer, der diesem Container zugeordnet ist, in die Konfigurationsdatei eingeschlossen werden muss.

In der Regel werden Qualifizierer für eine enthaltende Entität, z. b. &mdash; ein Ordner oder eine `.resw` Datei, auf alle darin enthaltenen &mdash; Ressourcen, z. b. die Dateien innerhalb des Ordners, oder die Zeichen folgen in der `.resw` Datei angewendet.

## <a name="folder"></a>Ordner

Der ordnerindexer wird durch ein- `type` Attribut des-Ordners identifiziert. Er indiziert den Inhalt eines Ordners und bestimmt Ressourcen Qualifizierer aus dem Ordner und den Dateinamen. Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:simpleType name="ExclusionTypeList">
        <xs:restriction base="xs:string">
            <xs:enumeration value="path"/>
            <xs:enumeration value="extension"/>
            <xs:enumeration value="name"/>
            <xs:enumeration value="tree"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:complexType name="FolderExclusionType">
        <xs:attribute name="type" type="ExclusionTypeList" use="required"/>
        <xs:attribute name="value" type="xs:string" use="required"/>
        <xs:attribute name="doNotTraverse" type="xs:boolean" use="required"/>
        <xs:attribute name="doNotIndex" type="xs:boolean" use="required"/>
    </xs:complexType>
    <xs:simpleType name="IndexerConfigFolderType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="exclude" type="FolderExclusionType" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
            <xs:attribute name="type" type="IndexerConfigFolderType" use="required"/>
            <xs:attribute name="foldernameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="filenameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Das- `qualifierDelimiter` Attribut gibt das Zeichen an, nach dem Qualifizierer in einem Dateinamen angegeben werden und die Erweiterung ignoriert wird. Der Standard ist ".".

## <a name="pri"></a>PRI

Der PRI-Indexer wird durch ein- `type` Attribut von PRI identifiziert. Der Inhalt einer PRI-Datei wird indiziert. Sie verwenden Sie in der Regel, wenn Sie die Ressource, die in einer anderen Assembly, dll, SDK oder Klassenbibliothek enthalten ist, in den PRI der APP indizieren.

Alle Ressourcennamen, Qualifizierer und Werte, die in der PRI-Datei enthalten sind, werden direkt in der neuen PRI-Datei verwaltet. Die Ressourcen Zuordnung auf oberster Ebene wird jedoch im endgültigen PRI nicht beibehalten. Ressourcen Zuordnungen werden zusammengeführt.

```xml
<xs:schema id="prifile" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigPriType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((p|P)(r|R)(i|I))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigPriType" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## <a name="priinfo"></a>Priinfo

Der priinfo-Indexer wird durch ein- `type` Attribut von priinfo identifiziert. Der Inhalt einer detaillierten Dumpdatei wird indiziert. Sie führen eine ausführliche Dumpdatei aus, indem Sie `makepri dump` mit der- `/dt detailed` Option ausführen. Das Configuration-Element für den Indexer entspricht dem folgenden Schema.

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

Dieses Konfigurationselement ermöglicht optionale Attribute, um das Verhalten des priinfo-Indexers zu konfigurieren. Der Standardwert von `emitStrings` und `emitPaths` ist `true` . Wenn `emitStrings` ist `true` , werden Ressourcen Kandidaten, bei denen das- `type` Attribut auf "String" festgelegt ist, im Index eingeschlossen. andernfalls werden Sie ausgeschlossen. Wenn ' emitpath ' ist `true` , werden Ressourcen Kandidaten, bei denen das- `type` Attribut auf ' Path ' festgelegt ist, im Index eingeschlossen. andernfalls werden Sie ausgeschlossen.

Im folgenden finden Sie eine Beispielkonfiguration, die Zeichen folgen-Ressourcentypen umfasst, aber Pfad Ressourcentypen überspringt.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

Um indiziert zu werden, muss eine Dumpdatei mit der-Erweiterung enden `.pri.xml` und muss dem folgenden Schema entsprechen.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >
  <xs:simpleType name="candidateType">
    <xs:restriction base="xs:string">
      <xs:pattern value="Path|String"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="scopeType">
    <xs:sequence>
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:anyAttribute processContents="skip" />
              </xs:complexType>
            </xs:element>
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">
                    <xs:complexType>
                      <xs:sequence>
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">
                          <xs:complexType>
                            <xs:attribute name="name" type="xs:string" use="required" />
                            <xs:attribute name="value" type="xs:string" use="required" />
                            <xs:attribute name="priority" type="xs:integer" use="required" />
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />
                            <xs:attribute name="index" type="xs:integer" use="required" />
                          </xs:complexType>
                        </xs:element>
                      </xs:sequence>
                      <xs:anyAttribute processContents="skip" />
                    </xs:complexType>
                  </xs:element>
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:attribute name="type" type="candidateType" use="required" />
              </xs:complexType>
            </xs:element>
          </xs:sequence>
          <xs:attribute name="name" use="required" type="xs:string" />
          <xs:anyAttribute processContents="skip" />
        </xs:complexType>
      </xs:element>
    </xs:sequence>
    <xs:attribute name="name" use="required" type="xs:string" />
    <xs:anyAttribute processContents="skip" />
  </xs:complexType>
  <xs:element name="PriInfo">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="PriHeader" >
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
        <xs:element name="QualifierInfo">
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="ResourceMap">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="VersionInfo">
                <xs:complexType>
                  <xs:anyAttribute processContents="skip" />
                </xs:complexType>
              </xs:element>
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />
            </xs:sequence>
            <xs:attribute name="name" type="xs:string" use="required" />
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

MakePri.exe unterstützt die dumptypen "Basic", "detailliert", "Schema" und "Zusammenfassung". Wenn Sie MakePri.exe konfigurieren möchten, um den dumptyp auszugeben, den der priinfo-Indexer lesen kann, schließen Sie "/DumpType ausführlich" ein, wenn Sie den `dump` Befehl verwenden.

Mehrere Elemente der `.pri.xml` Datei werden von MakePri.exe übersprungen. Diese Elemente werden entweder während der Indizierung berechnet oder in der MakePri.exe Konfigurationsdatei angegeben. Ressourcennamen, Qualifizierer und Werte, die in der Dumpdatei enthalten sind, werden direkt in der neuen PRI-Datei verwaltet. Die Ressourcen Zuordnung der obersten Ebene wird jedoch im endgültigen PRI nicht beibehalten. Ressourcen Zuordnungen werden als Teil der Indizierung zusammengeführt.

Dies ist ein Beispiel für eine gültige Zeichen folgen-typressource aus einer Dumpdatei.

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

Dies ist ein Beispiel für eine gültige path type-Ressource mit zwei Kandidaten aus einer Dumpdatei.

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>Resfiles

Der resfiles-Indexer wird durch ein- `type` Attribut von resfiles identifiziert. Der Inhalt einer Datei wird indiziert `.resfiles` . Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema id="resx" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResFilesType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResFilesType" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Eine `.resfiles` Datei ist eine Textdatei, die eine flache Liste mit Dateipfaden enthält. Eine `.resfiles` Datei kann "//"-Kommentare enthalten. Hier sehen Sie ein Beispiel.

<blockquote>
<pre>
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
</pre>
</blockquote>

## <a name="resjson"></a>ResJSON

Der resjson-Indexer wird durch das- `type` Attribut resjson identifiziert. Der Inhalt einer `.resjson` Datei, die eine Zeichen folgen-Ressourcen Datei ist, wird indiziert. Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema id="resjson" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResJsonType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResJsonType" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Eine `.resjson` Datei enthält JSON-Text (siehe [den Medientyp "application/json" für JavaScript Object Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)). Die Datei muss ein einzelnes JSON-Objekt mit hierarchischen Eigenschaften enthalten. Jede Eigenschaft muss entweder ein anderes JSON-Objekt oder ein Zeichen folgen Wert sein.

JSON-Eigenschaften, deren Namen mit einem Unterstrich ("_") beginnen, werden nicht in die endgültige PRI-Datei kompiliert, sondern in der Protokolldatei beibehalten.

Die Datei kann auch "//"-Kommentare enthalten, die während der Verarbeitung ignoriert werden.

Das `initialPath` Attribut fügt alle Ressourcen unter diesem anfänglichen Pfad an, indem es dem Namen der Ressource vorangestellt wird. Dies wird normalerweise verwendet, wenn Klassen Bibliotheksressourcen indiziert werden. Der Standardwert lautet „blank“.

## <a name="resw"></a>ResW

Der resw-Indexer wird durch ein `type` resw-Attribut identifiziert. Der Inhalt einer `.resw` Datei, die eine Zeichen folgen-Ressourcen Datei ist, wird indiziert. Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema id="resw" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResxType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(w|W))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResxType" use="required"/>
            <xs:attribute name="convertDotsToSlashes" type="xs:boolean" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Eine `.resw` Datei ist eine XML-Datei, die dem folgenden Schema entspricht.

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

Das `convertDotsToSlashes` Attribut konvertiert alle Punktzeichen ("."), die in Ressourcennamen (Attribute für Datenelement Namen) gefunden werden, in einen Schrägstrich "/", außer wenn die Punktzeichen zwischen "[" und "]" liegen.

Das `initialPath` Attribut fügt alle Ressourcen unter diesem anfänglichen Pfad an, indem es dem Namen der Ressource vorangestellt wird. Dies wird in der Regel verwendet, wenn Klassen Bibliotheksressourcen indiziert werden. Der Standardwert lautet „blank“.

## <a name="related-topics"></a>Verwandte Themen

* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
* [Befehlszeilenoptionen für „MakePri.exe“](makepri-exe-command-options.md)
* [Konfigurationsdatei für „MakePri.exe“](makepri-exe-configuration.md)
* [Der Anwendungs-/JSON-Medientyp für JavaScript Object Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)
