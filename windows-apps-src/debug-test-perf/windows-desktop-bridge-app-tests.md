---
ms.assetid: 2f76c520-84a3-4066-8eb3-ecc0ecd198a7
title: Tests für Windows Desktop Bridge-Apps
description: Verwende die integrierten Tests von Desktop-Brücke, um sicherzustellen, dass deine Desktopanwendung für die Konvertierung in eine UWP-App optimiert ist.
ms.date: 12/18/2017
ms.topic: article
keywords: Windows 10, UWP, App-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 37c382fb81a4527b730840142643ff72b9020127
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730296"
---
# <a name="windows-desktop-bridge-app-tests"></a>Tests für Windows Desktop Bridge-Apps

[Desktop-Brücke-Apps](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) sind Windows-Desktopanwendungen, die mit der [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) in UWP-Apps (Universelle Windows-Plattform) konvertiert wurden. Nach der Konvertierung wird die Windows-Desktopanwendung gepackt, gewartet und als UWP-App-Paket (APPX- oder APPXBUNDLE-Datei) für Windows 10 Desktop bereitgestellt.

## <a name="required-versus-optional-tests"></a>Obligatorische im Vergleich zu optionalen Tests
Optionale Tests für Windows Desktop-Brücke-Apps dienen nur zu Informationszwecken und nicht zur Bewertung Ihrer App während des Aufnahmeverfahrens für den Microsoft Store. Wir empfehlen das Prüfen dieser Testergebnisse, um Apps mit höherer Qualität zu erstellen. Die gesamten Kriterien für die Aufnahme in den Microsoft Store werden von den obligatorischen Tests und nicht von den optionalen Tests bestimmt.

## <a name="current-optional-tests"></a>Aktuelle optionale Tests

### <a name="1-digitally-signed-file-test"></a>1. Test der digitalen Signatur der Datei 
**Hintergrund**  
Dieser Test stellt sicher, dass alle portierbaren ausführbaren Dateien (PE-Dateien) eine gültige Signatur enthalten. Bei digital signierten Dateien wissen Benutzer, dass es sich bei der Software um Originalsoftware handelt.

**Testdetails**  
Der Test überprüft alle portierbaren ausführbaren Dateien im Paket und die Header auf eine Signatur. Alle PE-Dateien sollten digital signiert sein. Eine Warnung wird generiert, wenn PE-Dateien nicht signiert sind.
 
**Korrekturmaßnahmen**  
Digital signierte Dateien werden immer empfohlen. Weitere Informationen findest du unter [Einführung in das Codesignieren](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85)).

### <a name="2-file-association-verbs"></a>2. Dateizuordnungsverben 
**Hintergrund**  
Dieser Test überprüft die Registrierung des Pakets auf registrierte Dateizuordnungsverben. 

**Testdetails**  
Konvertierte Desktopanwendungen können um eine große Palette von Windows-Runtime-APIs erweitert werden. Dieser Test überprüft, dass die UWP-Binärdateien in der App keine Nicht-Windows-Runtime-APIs aufrufen. In UWP-Binärdateien ist das Flag **AppContainer** festgelegt.

**Korrekturmaßnahmen**  
Siehe [Desktop-zu-UWP-Brücke: App-Erweiterungen](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions) findest du eine Erläuterung dieser Erweiterungen und ihrer ordnungsgemäßen Verwendung. 

### <a name="3-debug-configuration-test"></a>3. Test der Debugkonfiguration
Dieser Test stellt sicher, dass es sich beim MSIX- oder APPX-Paket nicht um einen Debugbuild handelt.
 
**Hintergrund**  
Um für den Microsoft Store zertifiziert zu werden, dürfen Apps nicht zum Debuggen kompiliert werden und nicht auf die Debugversionen einer ausführbaren Datei verweisen. Darüber hinaus müssen Sie den Code für die App optimiert erstellen, damit dieser Test bestanden wird.
 
**Testdetails**  
Testen Sie die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt und dass die App mit keinen Debugframeworks verknüpft ist.
 
**Korrekturmaßnahmen**  
* Erstelle die App als Releasebuild, bevor du die App an den Microsoft Store übermittelst.
* Stellen Sie sicher, dass Sie die richtige .NET Framework-Version installiert haben.
* Stellen Sie sicher, dass die App nicht über Links zu Debugversionen eines Frameworks verfügt und dass die Erstellung mit einer Releaseversion erfolgt. Wenn diese App .NET-Komponenten enthält, sollten Sie sich vergewissern, dass Sie die richtige Version des .NET-Frameworks installiert haben.

### <a name="4-package-sanity-test"></a>4. Tests der Paketintegrität
#### <a name="41-archive-files-usage"></a>4.1 Archivdateiverwendung

**Hintergrund**  
Dieser Test unterstützt dich dabei, bessere Desktop-Brücke-Apps zu erstellen, die auf Computern unter [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) ausgeführt werden sollen.

**Testdetails**  
Dieser Test überprüft alle ausführbaren Dateien in Archivdateien oder selbstextrahierenden Inhalten. Da ausführbare Dateien in dieser Art von Inhalten während der Aufnahme in den Microsoft Store nicht signiert werden, wird die App auf Systemen unter Windows 10 S möglicherweise nicht wie erwartet ausgeführt.
 
**Korrekturmaßnahmen**
* Überprüfe die vom Test markierten Dateien, um festzustellen, ob es Auswirkungen auf deine App in einer Windows 10 S-Umgebung gibt.
* Wenn deine App betroffen ist, entferne die ausführbaren Dateien aus den archivierten Dateien, und verwende keine selbstextrahierenden Archive, um ausführbare Dateien auf dem Datenträger zu speichern. Dies sollte verhindern, dass App-Funktionalität verlorengeht.

#### <a name="42-blocked-executables"></a>4.2 Gesperrte ausführbare Dateien

**Hintergrund**  
Dieser Test unterstützt dich dabei, bessere Desktop-Brücke-Apps zu erstellen, die auf Computern unter [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) ausgeführt werden sollen. 

**Testdetails**  
Dieser Test überprüft, ob die App versucht, ausführbare Dateien zu starten, was auf Systemen unter Windows 10 S nur beschränkt erlaubt ist. Apps, die solche Aufrufe benötigen, werden auf Systemen unter Windows 10 S möglicherweise nicht wie erwartet ausgeführt. 

**Korrekturmaßnahmen**  
* Ermittle, welche der vom Test gekennzeichneten Einträge Aufrufe zum Start einer ausführbaren Datei darstellen, die nicht Teil deiner App ist. Entferne diese Aufrufe. 
* Wenn gekennzeichnete Dateien Teil deiner Anwendung sind, kannst du die Warnung ignorieren.


## <a name="current-required-tests"></a>Aktuelle obligatorische Tests

### <a name="1-app-capabilities-test-special-use-capabilities"></a>1. Test von App-Funktionen (Funktionen mit speziellem Zweck)

**Hintergrund**  
Sonderfunktionen sind für sehr spezielle Szenarien vorgesehen. Diese Funktionen dürfen nur mit Unternehmenskonten genutzt werden. 

**Testdetails**  
Es wird überprüft, ob von der App die folgenden Funktionen deklariert werden: 
* EnterpriseAuthentication
* SharedUserCertificates
* DocumentsLibrary

Falls eine oder mehrere dieser Funktionen deklariert werden, wird Benutzern während des Tests eine Warnung angezeigt. 

**Korrekturmaßnahmen**  
Sie können erwägen, die Sonderfunktion zu entfernen, wenn diese für die App nicht erforderlich ist. Die Verwendung dieser Funktionen unterliegt außerdem einer weiteren Prüfung anhand der Richtlinie für die Aufnahme.

### <a name="2-app-manifest-resources-tests"></a>2. Tests von App-Manifestressourcen 
#### <a name="21-app-resources-validation"></a>2.1 App-Ressourcenüberprüfung
Deine App wird ggf. nicht ordnungsgemäß installiert, wenn die im App-Manifest deklarierten Zeichenfolgen oder Bilder falsch sind. Wenn die App mit diesen Fehlern installiert wird, werden das Logo der App oder andere Bilder möglicherweise nicht richtig angezeigt.    

**Testdetails**  
Prüft die im App-Manifest definierten Ressourcen, um sicherzustellen, dass sie vorhanden und gültig sind.

**Korrekturmaßnahme**  
Orientiere dich an der folgenden Tabelle.

Fehlermeldung | Kommentare
--------------|---------
Das Bild "{Bildname}" definiert sowohl einen Scale- als auch einen TargetSize-Qualifizierer. Es darf jedoch jeweils nur ein Qualifizierer definiert sein. | Sie können Bilder für unterschiedliche Auflösungen anpassen. In der tatsächlichen Meldung enthält „{image name}“ den Namen des Bilds mit dem Fehler. Stellen Sie sicher, dass für jedes Bild entweder „Scale” oder „TargetSize” als Qualifizierer definiert ist. 
Das Bild "{Bildname}" überschreitet die Größenbeschränkung von {Größenangabe}.  | Stellen Sie sicher, dass alle Bilder der App den Größenbeschränkungen entsprechen. In der tatsächlichen Meldung enthält „{image name}“ den Namen des Bilds mit dem Fehler. 
Das Bild "{Bildname}" fehlt im Paket.  | Ein erforderliches Bild fehlt. In der tatsächlichen Meldung enthält "{Bildname}“ den Namen des fehlenden Bilds. 
Das Bild "{Bildname}" ist keine gültige Bilddatei.  | Stellen Sie sicher, dass alle Bilder der App den Einschränkungen für Dateiformattypen entsprechen. In der tatsächlichen Meldung enthält „{image name}“ den Namen des ungültigen Bilds. 
Das Bild „BadgeLogo“ enthält einen ungültigen ABGR-Wert {value} an der Position (x, y). Das Pixel muss weiß (##FFFFFF) oder transparent (00######) sein.  | Das Signallogo ist ein Bild, das neben der Signalbenachrichtigung angezeigt wird, um die App auf dem Sperrbildschirm zu identifizieren. Dieses Bild muss einfarbig sein (es darf nur weiße und transparente Pixel enthalten). In der tatsächlichen Meldung enthält {Wert} den Namen des ungültigen Farbwerts im Bild. 
Das Bild „BadgeLogo“ enthält einen ABGR-Wert {Wert} an der Position (x, y), der für ein Bild mit hohem Kontrast (Weiß) ungültig ist. Das Pixel muss (##2A2A2A) oder dunkler oder transparent (00######) sein.  | Das Signallogo ist ein Bild, das neben der Signalbenachrichtigung angezeigt wird, um die App auf dem Sperrbildschirm zu identifizieren. Da das Signallogo bei hohem Kontrast (weiß) auf einem weißen Hintergrund angezeigt wird, muss es sich um eine dunkle Version des normalen Signallogos handeln. Bei hohem Kontrast (Weiß) darf das Signallogo nur Pixel enthalten, die dunkler als (##2A2A2A) oder transparent sind. In der tatsächlichen Meldung enthält {Wert} den Namen des ungültigen Farbwerts im Bild. 
Für das Bild muss mindestens eine Variante ohne TargetSize-Qualifizierer definiert sein. Sie müssen einen Scale-Qualifizierer definieren oder „Scale” und „TargetSize” nicht angeben. In diesem Fall wird „Scale-100” verwendet.  | Weitere Informationen findest du in den Anleitungen unter [Reaktionsfähiges Design](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design) und [App-Ressourcen](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data). 
Das Paket enthält keine Datei „resources.pri”.  | Wenn das App-Manifest lokalisierbaren Inhalt enthält, müssen Sie sicherstellen, dass das Paket der App eine gültige Datei „resources.pri“ enthält. 
Die Datei „resources.pri“ muss eine Ressourcenzuordnung enthalten, bei der der Name dem Paketnamen „{vollständiger Paketname}“ entspricht.  | Dieser Fehler wird angezeigt, wenn das Manifest geändert wird und der Name der Ressourcenzuordnung in „resources.pri“ dem Paketnamen im Manifest nicht mehr entspricht. In der tatsächlichen Meldung enthält „{vollständiger Paketname}“ den Paketnamen, den „resources.pri“ enthalten muss. Um diesen Fehler zu beheben, musst du die Datei „resources.pri“ neu erstellen. Am besten erstellst du dazu das App-Paket neu. 
Für die Datei „resources.pri“ darf „Automatisch zusammenführen“ nicht aktiviert sein.  | „MakePRI.exe“ unterstützt eine Option mit dem Namen AutoMerge. Standardmäßig ist AutoMerge deaktiviert. Falls aktiviert, führt AutoMerge die App-Sprachpaketressourcen zur Laufzeit in einer einzelnen Datei (resources.pri) zusammen. Für Apps, die du über den Microsoft Store vertreiben möchtest, wird dies nicht empfohlen. Die Datei „resources.pri“ einer über den Microsoft Store vertriebenen App muss sich im Stammverzeichnis des App-Pakets befinden und alle von der App unterstützten Sprachverweise enthalten. 
Die Zeichenfolge „{string}“ entspricht nicht der Längenbeschränkung von maximal {number} Zeichen.  | Weitere Informationen finden Sie unter [App-Paketanforderungen](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements). In der tatsächlichen Meldung wird „{string}“ durch die Zeichenfolge mit dem Fehler ersetzt, und {number} enthält die maximale Länge. 
Die Zeichenfolge „{string}“ darf keine führenden/nachgestellten Leerzeichen enthalten.  | Das Schema für die Elemente im App-Manifest lässt führende oder nachgestellte Leerzeichen nicht zu. In der tatsächlichen Meldung wird „{string}“ durch die Zeichenfolge mit dem Fehler ersetzt. Stellen Sie sicher, dass keiner der lokalisierten Werte der Manifestfelder in „resources.pri“ führende oder nachgestellte Leerzeichen enthält. 
Die Zeichenfolge darf nicht leer sein (Länge größer 0 (null)).  | Weitere Informationen finden Sie unter [App-Paketanforderungen](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements). 
In der Datei „resources.pri” ist keine Standardressource angegeben.  | Weitere Informationen findest du in der Anleitung unter [App-Ressourcen](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data). In der Standardbuildkonfiguration nimmt Visual Studio nur Bildressourcen mit der Skalierung 200 % in das App-Paket auf, wenn ein Bündel generiert wird, andere Ressourcen werden im Ressourcenpaket abgelegt. Stelle sicher, dass du entweder Bildressourcen mit der Skalierung 200 % einschliest oder dein Projekt für die Aufnahme der vorhandenen Ressourcen konfigurierst. 
In der Datei „resources.pri“ ist kein Ressourcenwert angegeben.  | Stellen Sie sicher, dass für das App-Manifest gültige Ressourcen in „resources.pri“ definiert sind. 
Die Größe der Bilddatei {Dateiname} muss unter 204800 Bytes liegen.  | Verringern Sie die Größe der angegebenen Bilder. 
Die Datei {Dateiname} darf keinen Abschnitt mit umgekehrter Zuordnung enthalten.  | Die umgekehrte Zuordnung wird zwar während des Debuggens mit F5 in Visual Studio beim Aufrufen von „makepri.exe“ generiert, sie kann jedoch entfernt werden, indem „makepri.exe“ beim Generieren einer PRI-Datei ohne den Parameter „/m“ ausgeführt wird. 



#### <a name="22-branding-validation"></a>2.2 Überprüfung des Brandings
**Hintergrund**  
Desktop-Brücke-Apps sollten vollständig und betriebsbereit sein. Apps, für die Standardbilder (aus Vorlagen oder SDK-Beispielen) verwendet werden, verfügen über eine schlechte Benutzeroberfläche und können im Store-Katalog nicht leicht identifiziert werden.

**Testdetails**  
Bei diesem Test wird sichergestellt, dass die von der App verwendeten Bilder keine Standardbilder aus SDK-Beispielen oder aus Visual Studio sind. 

**Korrekturmaßnahmen**  
Ersetzen Sie Standardbilder durch eigene Bilder, die über Aussagekraft für Ihre App verfügen.

### <a name="3-package-compliance-tests"></a>3. Tests der Erfüllung der Paketanforderungen
#### <a name="31-app-manifest"></a>3.1 App-Manifest
Überprüft die Inhalte des App-Manifests auf Richtigkeit.

**Hintergrund**  
Apps müssen ein korrekt formatiertes App-Manifest besitzen.

**Testdetails**  
Überprüft das App-Manifest, um sicherzustellen, dass der Inhalt der Beschreibung in den [App-Paketanforderungen](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) entspricht. Die folgenden Prüfungen werden in diesem Test ausgeführt:
* **Dateierweiterungen und Protokolle**  
Deine App kann die Dateitypen deklarieren, denen sie zugeordnet werden kann. Eine Deklaration einer großen Anzahl ungewöhnlicher Dateitypen sorgt für eine schlechtere Benutzererfahrung. Mit diesem Test wird die Anzahl von Dateierweiterungen beschränkt, die einer App zugeordnet werden können.
* **Framework-Abhängigkeitsregel**  
Mit diesem Test wird die Anforderung erzwungen, dass Apps geeignete Abhängigkeiten von der UWP deklarieren. Für den Test tritt ein Fehler auf, wenn eine unzulässige Abhängigkeit besteht. Bei einem Konflikt zwischen der Betriebssystemversion, auf die die App abzielt, und den eingerichteten Frameworkabhängigkeiten schlägt der Test fehl. Der Test schlägt ebenfalls fehl, wenn sich die App auf eine „Vorschauversion“ der Framework-DLLs bezieht.
* **Überprüfung der prozessübergreifenden Kommunikation (Inter-process Communication, IPC)**  
Dieser Test setzt die Anforderung durch, dass Desktop-Brücke-Apps außerhalb des App-Containers nicht mit Desktopkomponenten kommunizieren. Die prozessübergreifende Kommunikation ist nur für quergeladene Apps vorgesehen. Apps, die für [**ActivatableClassAttribute**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-activatableclassattribute) den Namen `DesktopApplicationPath` angeben, bestehen diesen Test nicht.  

**Korrekturmaßnahme**  
Gleichen Sie das App-Manifest mit den [App-Paketanforderungen](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) ab.


#### <a name="32-application-count"></a>3.2 Anzahl von Anwendungen
Dieser Test stellt sicher, dass ein App-Paket (.appx, App Bundle) genau eine Anwendung enthält. 

**Hintergrund**  
Dieser Test wird gemäß Store-Richtlinie implementiert. 

**Testdetails**  
Dieser Test stellt sicher, dass die Gesamtzahl der APPX-Pakete im Bündel kleiner als 512 ist und dass nur ein „Hauptpaket“ im Bündel vorhanden ist. Es wird auch überprüft, dass die Revisionsnummer der Paketversion auf 0 festgelegt ist. 

**Korrekturmaßnahmen**  
Stelle sicher, dass das App-Paket und das Bündel die unter **Testdetails** angegebenen Anforderungen erfüllen.


#### <a name="33-registry-checks"></a>3.3 Registrierungsprüfungen
**Hintergrund**  
Bei diesem Test wird überprüft, ob die Anwendung neue Dienste oder Treiber installiert oder aktualisiert.

**Testdetails**  
Der Test sucht in der Datei „registry.dat“ nach Updates zu bestimmten Registrierungspfaden, die angeben, ob neue Dienste oder Treiber registriert wurden. Wenn die App versucht, einen Treiber oder Dienst zu installieren, schlägt der Test fehl.  

**Korrekturmaßnahmen**  
Überprüfe die Fehler, und entferne die betreffenden Dienste oder Treiber, wenn sie nicht erforderlich sind. Wenn die App von diesen Elementen abhängig ist, musst du die App für die Aufnahme in den Store überarbeiten.


### <a name="4-platform-appropriate-files-test"></a>4. Test auf für Plattform geeignete Dateien
Apps, von denen gemischte Binärdateien installiert werden, stürzen je nach der Prozessorarchitektur von Benutzern möglicherweise ab oder werden nicht richtig ausgeführt. 

**Hintergrund**  
Bei diesem Test werden die Binärdateien in einem App-Paket auf Architekturkonflikte geprüft. Ein App-Paket sollte keine Binärdateien enthalten, die unter der im Manifest angegebenen Prozessorarchitektur nicht verwendet werden können. Das Einfügen von nicht unterstützten Binärdateien kann zum Absturz der App oder zu einem unnötigen Anstieg der Paketgröße einer App führen. 

**Testdetails**  
Es wird überprüft, ob die Bitanzahl jedes Headers der portierbaren ausführbaren Dateien korrekt ist, wenn Querverweise mit der Prozessorarchitekturdeklaration des App-Pakets eingerichtet werden. 

**Korrekturmaßnahmen**  
Befolgen Sie diese Richtlinien, um sicherzustellen, dass Ihr App-Paket nur Dateien enthält, die von der im App-Manifest angegebenen Architektur unterstützt werden: 
* Wenn die Zielprozessorarchitektur der App den Prozessortyp „Neutral“ aufweist, kann die App keine x86-, x64- oder ARM-Binärdateien oder -Abbilddateien enthalten.
* Wenn die Zielprozessorarchitektur der App den Prozessortyp "x86" aufweist, darf das App-Paket nur x86-Binärdateien oder -Abbilddateien enthalten. Wenn das Paket x64- oder ARM-Binärtypen oder -Imagetypen enthält, tritt beim Test ein Fehler auf.
* Wenn die Zielprozessorarchitektur der App den Prozessortyp "x64" aufweist, muss das App-Paket x64-Binärdateien oder -Abbilddateien enthalten. Beachten Sie, dass das Paket in diesem Fall auch x86-Dateien enthalten kann. Für die Hauptoberfläche der App sollte jedoch die x64-Binärdatei genutzt werden. Falls das Paket ARM-Binärdateien oder -Abbilddateien oder *nur* x86-Binärdateien oder -Abbilddateien enthält, tritt beim Test ein Fehler auf.
* Wenn die Zielprozessorarchitektur der App den Prozessortyp "ARM" aufweist, darf das App-Paket nur ARM-Binärdateien oder -Abbilddateien enthalten. Wenn das Paket x64- oder x86-Binärdateien oder -Abbilddateien enthält, tritt beim Test ein Fehler auf. 

### <a name="5-supported-api-test"></a>5. Test der unterstützten APIs
Überprüft die App, um festzustellen, ob nicht kompatible APIs verwendet werden. 

**Hintergrund**  
Desktop-Brücke-Apps können zusammen mit modernen APIs (UWP-Komponenten) einige ältere Win32-APIs nutzen. Dieser Test ermittelt verwaltete Binärdateien, die nicht unterstützte APIs verwenden.
 
**Testdetails**  
Dieser Test überprüft alle UWP-Komponenten in der App:
* Es wird sichergestellt, dass die im App-Paket enthaltenen verwalteten Binärdateien keine Abhängigkeiten von einer Win32-API aufweisen, die für die Entwicklung von UWP-Apps nicht unterstützt wird. Dazu wird jeweils die Importadressentabelle der Binärdatei überprüft.
* Stellt sicher, dass eine verwaltete Binärdatei des App-Pakets keine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist. 

**Korrekturmaßnahmen**  
Dies kann korrigiert werden, indem sichergestellt wird, dass die App als Releasebuild und nicht als ein Debugbuild kompiliert wurde. 

> [!NOTE]
> Der Debugbuild einer App besteht diesen Test nicht, auch wenn die App nur [APIs für UWP-Apps](https://docs.microsoft.com/uwp/) verwendet. Prüfe die Fehlermeldungen, um die vorliegende API zu ermitteln, die keine für UWP-Apps zulässige API ist. 

> [!NOTE]
> C++-Apps, die in einer Debugkonfiguration erstellt wurden, bestehen diesen Test nicht. Dies gilt auch, wenn für die Konfiguration nur APIs aus dem Windows SDK für UWP-Apps verwendet werden. Weitere Informationen findest du unter [Alternativen zu Windows-APIs in UWP-Apps](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps).

### <a name="6-user-account-control-uac-test"></a>6. Test der Benutzerkontensteuerung (User Account Control; UAC)  

**Hintergrund**  
Stellt sicher, dass die App zur Laufzeit nicht die Benutzerkontensteuerung anfordert.

**Testdetails**  
Eine App kann gemäß Microsoft Store-Richtlinie nicht erhöhte Administratorrechte oder UIAccess anfordern. Erhöhte Sicherheitsberechtigungen werden nicht unterstützt. 

**Korrekturmaßnahmen**  
Apps müssen als interaktiver Benutzer ausgeführt werden. Weitere Details findest du unter [Übersicht über die Benutzeroberflächenautomatisierungs-Sicherheit](https://docs.microsoft.com/dotnet/framework/ui-automation/ui-automation-security-overview?redirectedfrom=MSDN).

 
### <a name="7-windows-runtime-metadata-validation"></a>7. Windows-Runtime-Metadatenvalidierung
**Hintergrund**  
Es wird sichergestellt, dass die Komponenten, die als Teil einer App mitgeliefert werden, mit dem UWP-Typsystem kompatibel sind.

**Testdetails**  
Bei diesem Test wird eine Reihe von Flags im Zusammenhang mit der richtigen Verwendung von Typen gesetzt.

**Korrekturmaßnahmen**  
* **ExclusiveTo-Attribute**  
Stelle sicher, dass von UWP-Klassen keine Schnittstellen implementiert werden, die für eine andere Klasse als ExclusiveTo markiert sind.
* **Allgemeine Richtigkeit der Metadaten**  
Stelle sicher, dass der zum Generieren deiner Typen verwendete Compiler in Bezug auf die UWP-Spezifikationen auf dem neuesten Stand ist.
* **Eigenschaften**  
Stelle sicher, dass alle Eigenschaften in einer UWP-Klasse eine `get`-Methode aufweisen (`set`-Methoden sind optional). Stelle für alle Eigenschaften sicher, dass der von der `get`-Methode zurückgegebene Typ dem Typ des Eingabeparameters der `set`-Methode entspricht.
* **Speicherort von Typen**  
Stelle sicher, dass die Metadaten für alle UWP-Typen in der WINMD-Datei enthalten sind, die im App-Paket über den längsten Namen mit Namespaceübereinstimmung verfügt.
* **Richtige Groß-/Kleinschreibung von Typnamen**  
Stelle sicher, dass alle UWP-Typen eindeutige Namen ohne Berücksichtigung von Groß-/Kleinschreibung in deinem App-Paket aufweisen. Vergewissern Sie sich außerdem, dass UWP-Typnamen im App-Paket nicht auch als Namespacename verwendet werden.
* **Richtigkeit von Typnamen**  
Stelle sicher, dass im globalen Namespace oder im Windows-Namespace der obersten Ebene keine UWP-Typen vorhanden sind.
 

### <a name="8-windows-security-features-tests"></a>8. Test der Windows-Sicherheitsfeatures
Durch Ändern der standardmäßigen Windows-Sicherheitsvorkehrungen können Kunden einem erhöhten Risiko ausgesetzt werden. 

#### <a name="81-banned-file-analyzer"></a>8.1 Untersuchung auf gesperrte Dateien
**Hintergrund**  
Bestimmte Dateien wurden im Hinblick auf wichtige Aspekte wie Sicherheit, Zuverlässigkeit oder andere Verbesserungen aktualisiert. Windows Desktop-Brücke-Apps müssen die neuesten Versionen dieser Dateien enthalten, da veraltete Versionen ein Risiko darstellen. Diese Dateien werden im Zertifizierungskit für Windows-Apps blockiert, um sicherzustellen, dass von allen Entwicklern aktuelle Versionen verwendet werden.

**Testdetails**  
Beim Test auf unzulässige Dateien im Windows-Zertifizierungskit für Apps wird derzeit eine Überprüfung auf folgende Dateien durchgeführt:
* *Bing.Maps.JavaScript\js\veapicore.js*  
Bei dieser Überprüfung tritt normalerweise ein Fehler auf, wenn von einer App anstelle der aktuellen offiziellen Version eine Release Preview-Version der Datei verwendet wird. 

**Korrekturmaßnahmen**  
Verwende zum Beheben dieses Fehlers die aktuelle Version des [Bing Maps SDK](https://www.bingmapsportal.com/) für UWP-Apps.

#### <a name="82-private-code-signing"></a>8.2 Private Codesignatur
Überprüft, ob innerhalb des App-Pakets Binärdateien für private Codesignaturen vorhanden sind. 

**Hintergrund**  
Signaturdateien für privaten Code sollten privat bleiben, da sie im Fall einer Gefährdung zu bösartigen Zwecken missbraucht werden könnten. 

**Testdetails**  
Überprüft, ob innerhalb des App-Pakets Dateien mit der Erweiterung „.pfx“ oder „.snk“ vorhanden sind, die darauf hinweisen, dass private Signaturschlüssel verwendet werden. 

**Korrekturmaßnahmen**  
Entferne alle Signaturschlüssel für privaten Code (wie z. B. PFX- und SNK-Dateien) aus dem Paket.


## <a name="related-topics"></a>Zugehörige Themen

* [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies)
