---
Description: In diesem Thema werden die formatspezifischen Indexer beschrieben, die das Tool MakePri.exe verwendet, um seinen Ressourcenindex zu generieren.
title: Formatspezifische Indexer für MakePri.exe
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 1a245c4ec0280f687cf34e85123960e64fe36a57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645875"
---
# <a name="makepriexe-format-specific-indexers"></a>Formatspezifische Indexer für MakePri.exe

In diesem Thema werden die formatspezifischen Indexer beschrieben, die das Tool [MakePri.exe](compile-resources-manually-with-makepri.md) verwendet, um seinen Ressourcenindex zu generieren.

> [!NOTE]
> MakePri.exe installiert ist, wenn Sie aktivieren die **Windows SDK für verwaltete UWP-Apps** Option während der Installation von Windows Software Development Kit. Es wird auf den Pfad installiert `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (sowie in Ordnern, die mit dem Namen für die anderen Architekturen). Beispiel: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

MakePri.exe wird in der Regel mit den Befehlen `new`, `versioned` oder `resourcepack` verwendet. Siehe [Befehlszeilenoptionen für MakePri.exe](makepri-exe-command-options.md). In diesen Fällen indiziert das Tool Quelldateien und generiert einen Ressourcenindex. MakePri.exe verwendet eine Reihe individueller Indexer, um die verschiedenen Quellressourcendateien oder Ressourcencontainer zu lesen. Der einfachste Indexer ist der Ordnerindexer. Er indiziert den Inhalt eines Ordners, beispielsweise `.jpg`- oder `.png`-Bilder.

Formatspezifische Indexer werden durch Angabe von `<indexer-config>`-Elementen in einem `<index>`-Element der [MakePri.exe-Konfigurationsdatei](makepri-exe-configuration.md) identifiziert. Das Attribut `type` identifiziert den verwendeten formatspezifischen Indexer.

Bei Ressourcencontainern, die während der Indizierung vorgefunden werden, wird der Inhalt indiziert. Die Container selbst werden dem Index nicht hinzugefügt. Beispielsweise werden vom Ordnerindexer gefundene `.resjson`-Dateien von einem `.resjson`-Indexer im Detail indiziert. Die `.resjson`-Datei ist in diesem Fall nicht im Index enthalten. **Hinweis:** Dazu muss ein `<indexer-config>`-Element für den Indexer, der diesem Container zugeordnet ist, in der Konfigurationsdatei enthalten sein.

Die für eine Containerentität&mdash;  wie einen Ordner oder `.resw` eine &mdash;-Datei gefundenen Qualifizierer werden normalerweise auf alle enthaltenen Ressourcen angewendet, also beispielsweise auf die Dateien im Ordner oder die Zeichenfolgen in der `.resw`-Datei.

## <a name="folder"></a>Ordner

Der Ordnerindexer wird mit dem `type`-Attribut FOLDER angegeben. Er indiziert den Inhalt eines Ordners und bestimmt Ressourcenbezeichner anhand der Ordner- und Dateinamen. Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema attributeFormDefault=\"unqualified\" elementFormDefault=\"qualified\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">\
    <xs:simpleType name=\"ExclusionTypeList\">\
        <xs:restriction base=\"xs:string\">\
            <xs:enumeration value=\"path\"/>\
            <xs:enumeration value=\"extension\"/>\
            <xs:enumeration value=\"name\"/>\
            <xs:enumeration value=\"tree\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:complexType name=\"FolderExclusionType\">\
        <xs:attribute name=\"type\" type=\"ExclusionTypeList\" use=\"required\"/>\
        <xs:attribute name=\"value\" type=\"xs:string\" use=\"required\"/>\
        <xs:attribute name=\"doNotTraverse\" type=\"xs:boolean\" use=\"required\"/>\
        <xs:attribute name=\"doNotIndex\" type=\"xs:boolean\" use=\"required\"/>\
    </xs:complexType>\
    <xs:simpleType name=\"IndexerConfigFolderType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:sequence>\
                <xs:element name=\"exclude\" type=\"FolderExclusionType\" minOccurs=\"0\" maxOccurs=\"unbounded\"/>\
            </xs:sequence>\
            <xs:attribute name=\"type\" type=\"IndexerConfigFolderType\" use=\"required\"/>\
            <xs:attribute name=\"foldernameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"filenameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>
```

Das Attribut `qualifierDelimiter` bestimmt das Zeichen, nach dem Qualifizierer in einem Dateinamen angegeben werden (die Dateinamenerweiterung wird ignoriert). Der Standardwert ist ".".

## <a name="pri"></a>PRI

Der PRI-Indexer wird mit dem `type`-Attribut PRI angegeben. Er indiziert die Inhalte von PRI-Dateien. Normalerweise werden damit Ressourcen in anderen Assemblys, DLLs, SDKs oder in Klassenbibliotheken in der PRI-Datei der App indiziert.

Alle Ressourcennamen, Qualifizierer und Werte in der PRI-Datei werden direkt in der neuen PRI-Datei beibehalten. Die Ressourcenzuordnung der obersten Ebene wird jedoch nicht in der abschließenden PRI-Datei beibehalten. Ressourcenzuordnungen werden zusammengeführt.

```xml
<xs:schema id=\"prifile\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigPriType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((p|P)(r|R)(i|I))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigPriType\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

## <a name="priinfo"></a>PriInfo

Der PriInfo-Indexer wird mit dem `type`-Attribut PRIINFO angegeben. Er indiziert den Inhalt einer ausführlichen Dumpdatei. Durch den Aufruf von `makepri dump` mit der Option `/dt detailed` wird eine detaillierte Dumpdatei erstellt. Der Konfigurationselement für den Indexer entspricht dem folgenden Schema.

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

Dieses Konfigurationselement ermöglicht die Verwendung optionaler Attribute, um das Verhalten des PriInfo-Indexers zu konfigurieren. Der Standardwert für `emitStrings` und `emitPaths` ist `true`. Wenn `emitStrings` den Wert `true` besitzt, werden Ressourcenkandidaten mit dem `type`-Attribut „String” in den Index aufgenommen. Andernfalls werden sie ausgeschlossen. Wenn „emitPaths” den Wert `true` besitzt, werden Ressourcenkandidaten mit dem `type`-Attribut „Path” in den Index aufgenommen. Andernfalls werden sie ausgeschlossen.

Im folgenden Konfigurationsbeispiel werden String-Ressourcentypen aufgenommen und Path-Ressourcentypen übersprungen.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

Voraussetzung für die Indizierung einer Dumpdatei ist, dass sie über die Erweiterung `.pri.xml` verfügt und dem folgenden Schema entspricht.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >\
  <xs:simpleType name="candidateType">\
    <xs:restriction base="xs:string">\
      <xs:pattern value="Path|String"/>\
    </xs:restriction>\
  </xs:simpleType>\
  <xs:complexType name="scopeType">\
    <xs:sequence>\
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>\
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">\
        <xs:complexType>\
          <xs:sequence>\
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:anyAttribute processContents="skip" />\
              </xs:complexType>\
            </xs:element>\
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">\
                    <xs:complexType>\
                      <xs:sequence>\
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">\
                          <xs:complexType>\
                            <xs:attribute name="name" type="xs:string" use="required" />\
                            <xs:attribute name="value" type="xs:string" use="required" />\
                            <xs:attribute name="priority" type="xs:integer" use="required" />\
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />\
                            <xs:attribute name="index" type="xs:integer" use="required" />\
                          </xs:complexType>\
                        </xs:element>\
                      </xs:sequence>\
                      <xs:anyAttribute processContents="skip" />\
                    </xs:complexType>\
                  </xs:element>\
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:attribute name="type" type="candidateType" use="required" />\
              </xs:complexType>\
            </xs:element>\
          </xs:sequence>\
          <xs:attribute name="name" use="required" type="xs:string" />\
          <xs:anyAttribute processContents="skip" />\
        </xs:complexType>\
      </xs:element>\
    </xs:sequence>\
    <xs:attribute name="name" use="required" type="xs:string" />\
    <xs:anyAttribute processContents="skip" />\
  </xs:complexType>\
  <xs:element name="PriInfo">\
    <xs:complexType>\
      <xs:sequence>\
        <xs:element name="PriHeader" >\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="QualifierInfo">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="ResourceMap">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:element name="VersionInfo">\
                <xs:complexType>\
                  <xs:anyAttribute processContents="skip" />\
                </xs:complexType>\
              </xs:element>\
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />\
            </xs:sequence>\
            <xs:attribute name="name" type="xs:string" use="required" />\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
      </xs:sequence>\
    </xs:complexType>\
  </xs:element>\
</xs:schema>
```

MakePri unterstützt die Dumptypen „Basic”, „Detailed”, „Schema” und „Summary”. Damit MakePri.exe einen Dumptyp ausgibt, den der PriInfo-Indexer lesen kann, müssen Sie die Zeichenfolge „/DumpType Detailed” in den `dump`-Befehl aufnehmen.

Mehrere Elemente der `.pri.xml`-Datei werden von MakePri.exe übersprungen. Diese Elemente werden entweder bei der Indizierung erstellt oder sind in der MakePri.exe-Konfigurationsdatei angegeben. Ressourcennamen, Qualifizierer und Werte in der Dumpdatei werden direkt in der neuen PRI-Datei beibehalten. Die Ressourcenzuordnung der obersten Ebene wird jedoch nicht in der abschließenden PRI-Datei verwaltet. Ressourcenzuordnungen werden bei der Indizierung zusammengeführt.

Hier ein Beispiel für eine gültige Ressource vom Typ „String” aus einer Dumpdatei:

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

Hier ein Beispiel für eine gültige Ressource vom Typ „Path” aus einer Dumpdatei:

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

## <a name="resfiles"></a>ResFiles

Der ResFiles-Indexer wird mit dem `type`-Attribut RESFILES angegeben. Er indiziert die Inhalte einer `.resfiles`-Datei. Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema id=\"resx\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResFilesType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResFilesType\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Die `.resfiles`-Datei ist eine Textdatei mit einer flachen Liste von Dateipfaden. Eine `.resfiles`-Datei kann Kommentare mit Schrägstrichen ("//") enthalten. Hier sehen Sie ein Beispiel.

```
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
```

## <a name="resjson"></a>ResJSON

Der ResJSON-Indexer wird mit dem `type`-Attribut RESJSON angegeben. Er indiziert die Inhalte einer `.resjson`-Datei, bei der es sich um eine Zeichenfolgen-Ressourcendatei handelt. Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema id=\"resjson\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResJsonType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResJsonType\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Eine `.resjson`-Datei enthält JSON-Text (siehe [Anwendungs-/JSON-Medientyp für JavaScript Object Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)). Die Datei muss ein einzelnes JSON-Objekt mit hierarchischen Eigenschaften enthalten. Jede Eigenschaft muss ein anderes JSON-Objekt oder ein Zeichenfolgenwert sein.

JSON-Eigenschaften, deren Name mit einem Unterstrich (_) beginnt, werden nicht in die abschließende PRI-Datei kompiliert, sondern in der Protokolldatei beibehalten.

Die Datei kann auch „// ”-Kommentare enthalten. Diese werden bei der Analyse ignoriert.

Das `initialPath`-Attribut platziert alle Ressourcen unter diesem Anfangspfad. Zu diesem Zweck wird das Attribut dem Namen der Ressource vorangestellt. Diese Vorgehensweise findet normalerweise bei der Indizierung der Ressourcen von Klassenbibliotheken Anwendung. Die Standardeinstellung ist leer.

## <a name="resw"></a>ResW

Der ResW-Indexer wird mit dem `type`-Aattribut RESW angegeben. Er indiziert die Inhalte einer `.resw`-Datei, bei der es sich um eine Zeichenfolgen-Ressourcendatei handelt. Das zugehörige Konfigurationselement entspricht dem folgenden Schema.

```xml
<xs:schema id=\"resw\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResxType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(w|W))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResxType\" use=\"required\"/>\
            <xs:attribute name=\"convertDotsToSlashes\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Eine `.resw`-Datei ist eine XML-Datei, die dem folgendem Schema entspricht.

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

Das Attribut `convertDotsToSlashes` bewirkt, dass alle Punkte (.) in Ressourcennamen (Namensattribute von Datenelementen) in einen Schrägstrich (/) konvertiert werden. Dies gilt jedoch nicht für Punkte zwischen eckigen Klammern ([ und ]).

Das `initialPath`-Attribut platziert alle Ressourcen unter diesem Anfangspfad. Zu diesem Zweck wird das Attribut dem Namen der Ressource vorangestellt. Diese Vorgehensweise findet normalerweise bei der Indizierung der Ressourcen von Klassenbibliotheken Anwendung. Die Standardeinstellung ist leer.

## <a name="related-topics"></a>Verwandte Themen

* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
* [MakePri.exe-Befehlszeilenoptionen](makepri-exe-command-options.md)
* [MakePri.exe-Konfigurationsdatei](makepri-exe-configuration.md)
* [Die Anwendung/Json Medientyp für JavaScript Object Notation (JSON)](https://www.ietf.org/rfc/rfc4627.txt)
