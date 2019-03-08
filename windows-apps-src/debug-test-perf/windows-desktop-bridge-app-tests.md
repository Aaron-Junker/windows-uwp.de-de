---
ms.assetid: 2f76c520-84a3-4066-8eb3-ecc0ecd198a7
title: Tests für Windows Desktop Bridge-Apps
description: Verwenden Sie die Desktop-Brücke integrierter Tests, um sicherzustellen, dass Ihre desktop-app für die Konvertierung in eine UWP-app optimiert ist.
ms.date: 12/18/2017
ms.topic: article
keywords: Windows 10, Uwp, app-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 42ec5c1e91fbeebcaad68f346f317893fdfb2e1c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606885"
---
# <a name="windows-desktop-bridge-app-tests"></a>Tests für Windows Desktop Bridge-Apps

[Apps für Desktop-Brücke](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root) sind Windows-desktopanwendungen konvertiert an universelle Windows-Plattform (UWP)-apps, die mit der [Desktop-Brücke](https://developer.microsoft.com/en-us/windows/bridges/desktop). Nach der Konvertierung wird die Windows-Desktopanwendung gepackt, gewartet und als UWP-App-Paket (eine APPX- oder APPXBUNDLE-Datei) für Windows 10 Desktop bereitgestellt.

## <a name="required-versus-optional-tests"></a>Obligatorische im Vergleich zu optionalen Tests
Optionale Tests für Windows Desktop-Brücke-apps dienen nur zur Information und werden nicht zum Auswerten von Ihrer app während des onboardings mit Microsoft Store verwendet werden. Es wird empfohlen, untersuchen diese Testergebnisse, um eine bessere Qualitäts-apps zu erstellen. Die gesamten Kriterien für die Aufnahme in den Windows Store werden von den obligatorischen Tests und nicht von den optionalen Tests bestimmt.

## <a name="current-optional-tests"></a>Aktuelle optionale Tests

### <a name="1-digitally-signed-file-test"></a>1. Test digital signierter Dateien 
**Hintergrund**  
Dieser Test stellt sicher, dass alle portierbaren ausführbaren Dateien (PE-Dateien) eine gültige Signatur enthalten. Bei digital signierten Dateien wissen Benutzer, dass es sich bei der Software um Originalsoftware handelt.

**Testdetails**  
Der Test überprüft alle portierbaren ausführbaren Dateien im Paket und überprüft die Kopfzeilen auf eine Signatur. Alle PE-Dateien sollten digital signiert werden. Eine Warnung wird generiert, wenn PE-Dateien nicht signiert sind.
 
**Korrekturmaßnahmen**  
Digital signierte Dateien werden immer empfohlen. Weitere Informationen finden Sie unter [Einführung in die Codesignatur](https://msdn.microsoft.com/en-us/library/ms537361(v=vs.85).aspx).

### <a name="2-file-association-verbs"></a>2. Verben für Datei-Zuordnung 
**Hintergrund**  
Dieser Test überprüft die Registrierung des Pakets auf registrierte Dateizuordnungsverben. 

**Testdetails**  
Konvertierte Desktopanwendungen können um eine große Palette von UWP-APIs (Universelle Windows-Plattform) erweitert werden. Dieser Test überprüft, dass die UWP-Binärdateien in der App keine Nicht-UWP-APIs aufrufen. UWP-Binärdateien weisen das **AppContainer**-Kennzeichen auf.

**Korrekturmaßnahmen**  
Finden Sie unter [UWP Bridge Desktop: App-Erweiterungen](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-extensions) eine Erläuterung der diese Erweiterungen und wie sie ordnungsgemäß verwendet. 

### <a name="3-debug-configuration-test"></a>3. Test der Debugkonfiguration
Dieser Test stellt sicher, dass es sich bei der App nicht um einen Debugbuild handelt.
 
**Hintergrund**  
Für die Zertifizierung für die Microsoft Store-apps müssen nicht kompiliert werden, zum Debuggen, und sie müssen nicht auf die Debugversionen einer ausführbaren Datei verweisen. Darüber hinaus müssen Sie den Code für die App optimiert erstellen, damit dieser Test bestanden wird.
 
**Testdetails**  
Testen Sie die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt und dass die App mit keinen Debugframeworks verknüpft ist.
 
**Korrekturmaßnahmen**  
* Erstellen Sie die app als ein Releasebuild erstellt, bevor Sie sie an den Microsoft Store übermitteln.
* Stellen Sie sicher, dass Sie die richtige .NET Framework-Version installiert haben.
* Stellen Sie sicher, dass die App nicht über Links zu Debugversionen eines Frameworks verfügt und dass die Erstellung mit einer Releaseversion erfolgt. Wenn diese App .NET-Komponenten enthält, sollten Sie sich vergewissern, dass Sie die richtige Version des .NET-Frameworks installiert haben.

### <a name="4-package-sanity-test"></a>4. Testen der Paket-integritätsprüfung
#### <a name="41-archive-files-usage"></a>4.1 Archivdateiverwendung

**Hintergrund**  
Dieser Test unterstützt Sie dabei, bessere Desktop-Brücke-Apps zu erstellen, die auf Computern unter [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) ausgeführt werden sollen.

**Testdetails**  
Dieser Test überprüft alle ausführbaren Dateien in Archivdateien oder selbstextrahierenden Inhalten. Da ausführbare Dateien innerhalb dieser Art von Inhalten während der Übernahme in den Windows Store nicht signiert werden, wird die App auf Systemen unter Windows 10 S möglicherweise nicht wie erwartet ausgeführt.
 
**Korrekturmaßnahmen**
* Überprüfen Sie die vom Test markierten Dateien, um festzustellen, ob es Auswirkungen auf Ihre App in einer Windows 10 S-Umgebung gibt.
* Wenn Ihre App betroffen ist, entfernen Sie die ausführbaren Dateien aus den archivierten Dateien, und verwenden Sie keine selbstextrahierenden Archive, um ausführbare Dateien auf dem Datenträger zu speichern. Dies sollte verhindern, dass App-Funktionalität verlorengeht.

#### <a name="42-blocked-executables"></a>4.2 Gesperrte ausführbare Dateien

**Hintergrund**  
Dieser Test unterstützt Sie dabei, bessere Desktop-Brücke-Apps zu erstellen, die auf Computern unter [Windows 10 S](https://www.microsoft.com/windows/windows-10-s) ausgeführt werden sollen. 

**Testdetails**  
Dieser Test überprüft, ob die App versucht, ausführbare Dateien zu starten, was auf Systemen unter Windows 10 S nur beschränkt erlaubt ist. Apps, die solche Aufrufe benötigen, werden auf Systemen unter Windows 10 S möglicherweise nicht wie erwartet ausgeführt. 

**Korrekturmaßnahmen**  
* Ermitteln Sie, welche der vom Test gekennzeichneten Einträge Aufrufe zum Start einer ausführbaren Datei darstellen, die nicht Teil Ihrer App ist. Entfernen Sie diese Aufrufe. 
* Wenn gekennzeichnete Dateien Teil Ihrer Anwendung sind, können Sie die Warnung ignorieren.


## <a name="current-required-tests"></a>Aktuelle obligatorische Tests

### <a name="1-app-capabilities-test-special-use-capabilities"></a>1. App-Funktionen testen (besondere Verwendung Funktionen)

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

### <a name="2-app-manifest-resources-tests"></a>2. App-manifest Ressourcen tests 
#### <a name="21-app-resources-validation"></a>2.1 App-Ressourcenüberprüfung
Ihre App wird ggf. nicht ordnungsgemäß installiert, wenn die im App-Manifest deklarierten Zeichenfolgen oder Bilder falsch sind. Wenn die App mit diesen Fehlern installiert wird, werden das Logo der App oder andere Bilder möglicherweise nicht richtig angezeigt.    

**Testdetails**  
Prüft die im App-Manifest definierten Ressourcen, um sicherzustellen, dass sie vorhanden und gültig sind.

**Korrekturmaßnahme**  
Orientieren Sie sich an der folgenden Tabelle.

Fehlermeldung | Anmerkungen
--------------|---------
Das Bild "{Bildname}" definiert sowohl einen Scale- als auch einen TargetSize-Qualifizierer. Es darf jedoch jeweils nur ein Qualifizierer definiert sein. | Sie können Bilder für unterschiedliche Auflösungen anpassen. In der tatsächlichen Meldung enthält „{image name}“ den Namen des Bilds mit dem Fehler. Stellen Sie sicher, dass für jedes Bild entweder „Scale” oder „TargetSize” als Qualifizierer definiert ist. 
Das Bild "{Bildname}" überschreitet die Größenbeschränkung von {Größenangabe}.  | Stellen Sie sicher, dass alle Bilder der App den Größenbeschränkungen entsprechen. In der tatsächlichen Meldung enthält „{image name}“ den Namen des Bilds mit dem Fehler. 
Das Bild "{Bildname}" fehlt im Paket.  | Ein erforderliches Bild fehlt. In der tatsächlichen Meldung enthält "{Bildname}“ den Namen des fehlenden Bilds. 
Das Bild "{Bildname}" ist keine gültige Bilddatei.  | Stellen Sie sicher, dass alle Bilder der App den Einschränkungen für Dateiformattypen entsprechen. In der tatsächlichen Meldung enthält „{image name}“ den Namen des ungültigen Bilds. 
Das Bild „BadgeLogo“ enthält einen ungültigen ABGR-Wert {value} an der Position (x, y). Das Pixel muss weiß (##FFFFFF) oder transparent (00######) sein.  | Das Signallogo ist ein Bild, das neben der Signalbenachrichtigung angezeigt wird, um die App auf dem Sperrbildschirm zu identifizieren. Dieses Bild muss einfarbig sein (es darf nur weiße und transparente Pixel enthalten). In der tatsächlichen Meldung enthält {Wert} den Namen des ungültigen Farbwerts im Bild. 
Das Bild „BadgeLogo“ enthält einen ABGR-Wert {Wert} an der Position (x, y), der für ein Bild mit hohem Kontrast (Weiß) ungültig ist. Das Pixel muss (##2A2A2A) oder dunkler oder transparent (00######) sein.  | Das Signallogo ist ein Bild, das neben der Signalbenachrichtigung angezeigt wird, um die App auf dem Sperrbildschirm zu identifizieren. Da das Signallogo bei hohem Kontrast (Weiß) auf einem weißen Hintergrund angezeigt wird, muss es sich um eine dunkle Version des normalen Signallogos handeln. Bei hohem Kontrast (Weiß) darf das Signallogo nur Pixel enthalten, die dunkler als (##2A2A2A) oder transparent sind. In der tatsächlichen Meldung enthält {Wert} den Namen des ungültigen Farbwerts im Bild. 
Für das Bild muss mindestens eine Variante ohne TargetSize-Qualifizierer definiert sein. Sie müssen einen Scale-Qualifizierer definieren oder „Scale” und „TargetSize” nicht angeben. In diesem Fall wird „Scale-100” verwendet.  | Weitere Informationen finden Sie in den Handbüchern unter [Reaktionsfähiges Design](https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx) und [App-Ressourcen](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data). 
Das Paket enthält keine Datei „resources.pri”.  | Wenn das App-Manifest lokalisierbaren Inhalt enthält, müssen Sie sicherstellen, dass das Paket der App eine gültige Datei „resources.pri“ enthält. 
Die Datei „resources.pri“ muss eine Ressourcenzuordnung enthalten, bei der der Name dem Paketnamen „{vollständiger Paketname}“ entspricht.  | Dieser Fehler wird angezeigt, wenn das Manifest geändert wird und der Name der Ressourcenzuordnung in „resources.pri“ dem Paketnamen im Manifest nicht mehr entspricht. In der tatsächlichen Meldung enthält „{vollständiger Paketname}“ den Paketnamen, den „resources.pri“ enthalten muss. Um diesen Fehler zu beheben, müssen Sie die Datei „resources.pri“ neu erstellen. Am besten erstellen Sie dazu das App-Paket neu. 
Für die Datei „resources.pri“ darf „Automatisch zusammenführen“ nicht aktiviert sein.  | „MakePRI.exe“ unterstützt eine Option mit dem Namen AutoMerge. Der Standardwert von AutoMerge ist Aus. Bei Aktivierung führt AutoMerge die App-Sprachpaketressourcen in einer einzelnen Datei „resources.pri“ zur Laufzeit zusammen. Wir nicht für apps zu empfehlen, die Sie über den Microsoft Store verteilen möchten. Die resources.pri einer App, die über den Microsoft Store verteilt werden muss im Stammverzeichnis des app Paket und alle Verweise, die von der app unterstützte Sprache. 
Die Zeichenfolge „{string}“ entspricht nicht der Längenbeschränkung von maximal {number} Zeichen.  | Weitere Informationen finden Sie unter [App-Paketanforderungen](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements). In der tatsächlichen Meldung wird „{string}“ durch die Zeichenfolge mit dem Fehler ersetzt, und {number} enthält die maximale Länge. 
Die Zeichenfolge „{string}“ darf keine führenden/nachgestellten Leerzeichen enthalten.  | Das Schema für die Elemente im App-Manifest lässt führende oder nachgestellte Leerzeichen nicht zu. In der tatsächlichen Meldung wird „{string}“ durch die Zeichenfolge mit dem Fehler ersetzt. Stellen Sie sicher, dass keiner der lokalisierten Werte der Manifestfelder in „resources.pri“ führende oder nachgestellte Leerzeichen enthält. 
Die Zeichenfolge darf nicht leer sein (Länge größer 0 (null)).  | Weitere Informationen finden Sie unter [App-Paketanforderungen](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements). 
In der Datei „resources.pri” ist keine Standardressource angegeben.  | Weitere Informationen finden Sie im Handbuch unter [App-Ressourcen](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data). In der Standardbuildkonfiguration nimmt Visual Studio nur Bildressourcen mit der Skalierung 200 % in das App-Paket auf, wenn ein Bündel generiert wird, andere Ressourcen werden im Ressourcenpaket abgelegt. Stellen Sie sicher, dass Sie entweder Bildressourcen mit der Skalierung 200 % einschließen oder Ihr Projekt für die Aufnahme der vorhandenen Ressourcen konfigurieren. 
In der Datei „resources.pri“ ist kein Ressourcenwert angegeben.  | Stellen Sie sicher, dass für das App-Manifest gültige Ressourcen in „resources.pri“ definiert sind. 
Die Bilddatei „{Dateiname}“ muss kleiner als 204.800 Bytes sein.  | Verringern Sie die Größe der angegebenen Bilder. 
Die Datei „{Dateiname}“ darf keinen Abschnitt mit umgekehrter Zuordnung enthalten.  | Die umgekehrte Zuordnung wird zwar während des Debuggens mit F5 in Visual Studio beim Aufrufen von „makepri.exe“ generiert, sie kann jedoch entfernt werden, indem „makepri.exe“ beim Generieren einer PRI-Datei ohne den Parameter „/m“ ausgeführt wird. 



#### <a name="22-branding-validation"></a>2.2 Branding-Validierung
**Hintergrund**  
Desktop-Brücke-Apps sollten vollständig und betriebsbereit sein. Apps, für die Standardbilder (aus Vorlagen oder SDK-Beispielen) verwendet werden, verfügen über eine schlechte Benutzeroberfläche und können im Store-Katalog nicht leicht identifiziert werden.

**Testdetails**  
Bei diesem Test wird sichergestellt, dass die von der App verwendeten Bilder keine Standardbilder aus SDK-Beispielen oder aus Visual Studio sind. 

**Korrekturmaßnahmen**  
Ersetzen Sie Standardbilder durch eigene Bilder, die über Aussagekraft für Ihre App verfügen.

### <a name="3-package-compliance-tests"></a>3. Paket-Konformitätstests
#### <a name="31-app-manifest"></a>3.1 App-Manifest
Überprüft die Inhalte des App-Manifests auf Korrektheit.

**Hintergrund**  
Apps müssen ein korrekt formatiertes App-Manifest besitzen.

**Testdetails**  
Überprüft das App-Manifest, um sicherzustellen, dass der Inhalt der Beschreibung in den [App-Paketanforderungen](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements) entspricht. Die folgenden Überprüfungen werden in diesem Test ausgeführt:
* **Dateierweiterungen und Protokolle**  
Ihre App kann die Dateitypen deklarieren, denen sie zugeordnet werden kann. Eine Deklaration über eine große Anzahl von ungewöhnlichen Dateitypen sorgt für eine schlechtere Benutzeroberfläche. Mit diesem Test wird die Anzahl von Dateierweiterungen beschränkt, die einer App zugeordnet werden können.
* **Framework-abhängigkeitsregel**  
Mit diesem Test wird die Anforderung durchgesetzt, dass die Apps geeignete Abhängigkeiten von der UWP deklarieren. Für den Test tritt ein Fehler auf, wenn eine unzulässige Abhängigkeit besteht. Liegt ein Konflikt zwischen der Betriebssystemversion, in der die App ausgeführt wird, und den bestehenden Frameworkabhängigkeiten vor, schlägt der Test fehl. Der Test schlägt ebenfalls fehl, wenn sich die App auf eine „Vorschauversion“ der Framework-DLLs bezieht.
* **Überprüfung der prozessübergreifenden Kommunikation (Interprocess Communication)**  
Dieser Test setzt die Anforderung durch, dass Desktop-Brücke-Apps außerhalb des App-Containers nicht mit Desktopkomponenten kommunizieren. Die prozessübergreifende Kommunikation ist nur für quergeladene Apps vorgesehen. Apps, die für [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) den Namen `DesktopApplicationPath` angeben, bestehen diesen Test nicht.  

**Korrekturmaßnahme**  
Gleichen Sie das App-Manifest mit den [App-Paketanforderungen](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements) ab.


#### <a name="32-application-count"></a>3.2 Anzahl der Anwendungen
Dieser Test stellt sicher, dass ein App-Paket (.appx, App Bundle) genau eine Anwendung enthält. 

**Hintergrund**  
Dieser Test wird gemäß der Store-Richtlinie implementiert. 

**Testdetails**  
Dieser Test stellt sicher, dass die Gesamtzahl der APPX-Pakete im Bündel kleiner als 512 ist und dass nur ein „Hauptpaket“ im Bündel vorhanden ist. Es wird auch überprüft, dass die Revisionsnummer der Paketversion auf 0 festgelegt ist. 

**Korrekturmaßnahmen**  
Stellen Sie sicher, dass das App-Paket und das Bündel die unter **Testdetails** angegebenen Anforderungen erfüllen.


#### <a name="33-registry-checks"></a>3.3 Registrierungsprüfungen
**Hintergrund**  
Bei diesem Test wird überprüft, ob die Anwendung neue Dienste oder Treiber installiert oder aktualisiert.

**Testdetails**  
Der Test sucht in der Datei „registry.dat“ nach Updates zu bestimmten Registrierungspfaden, die angeben, ob neue Dienste oder Treiber registriert wurden. Wenn die App versucht, einen Treiber oder Dienst zu installieren, schlägt der Test fehl.  

**Korrekturmaßnahmen**  
Überprüfen Sie die Fehler, und entfernen Sie die betreffenden Dienste oder Treiber, wenn sie nicht erforderlich sind. Wenn die App von diesen Elementen abhängig ist, müssen Sie die App für die Aufnahme im Store überarbeiten.


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
Dieser Test überprüft die UWP-Komponenten in der App:
* Stellt sicher, dass jeder verwalteten Binärdatei in der app-Paket eine Abhängigkeit auf eine Win32-API nicht, die nicht für die Entwicklung von UWP-Apps durch Aktivieren der Importadresstabelle der Binärdatei unterstützt wird.
* Stellt sicher, dass eine verwaltete Binärdatei des App-Pakets keine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist. 

**Korrekturmaßnahmen**  
Dies kann korrigiert werden, indem sichergestellt wird, dass die App als Releasebuild und nicht als ein Debugbuild kompiliert wurde. 

> [!NOTE]
> Debugbuild einer Anwendung können dieser Test schlägt fehl, selbst wenn die app nur verwendet [-APIs für UWP-apps](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx). Überprüfen Sie die Fehlermeldungen zur Identifizierung der API vorhanden, die nicht zulässige API für UWP-apps. 

> [!NOTE]
> C++-apps, die in einem Debug-Konfiguration erstellt werden werden dieser Test fehl, selbst wenn die Konfiguration nur die APIs aus dem Windows SDK für UWP-apps verwendet. Finden Sie unter [Alternativen zu Windows-APIs in UWP-apps](https://msdn.microsoft.com/library/windows/apps/hh464945.aspx) für Weitere Informationen.

### <a name="6-user-account-control-uac-test"></a>6. Benutzer-Konto (Control, UAC) testen  

**Hintergrund**  
Stellen Sie sicher, dass die App nicht die Benutzerkontensteuerung zur Laufzeit anfordert.

**Testdetails**  
Eine Anwendung kann nicht Admin Erhöhung der Rechte oder UIAccess gemäß Microsoft Store-Richtlinie anfordern. Erhöhte Sicherheitsberechtigungen werden nicht unterstützt. 

**Korrekturmaßnahmen**  
Apps müssen als interaktiver Benutzer ausgeführt werden. Weitere Details finden Sie unter [Sicherheit bei der Benutzeroberflächenautomatisierung – Übersicht](https://go.microsoft.com/fwlink/?linkid=839440).

 
### <a name="7-windows-runtime-metadata-validation"></a>7. Windows-Runtime-Metadatenvalidierung
**Hintergrund**  
Es wird sichergestellt, dass die Komponenten, die als Teil einer App mitgeliefert werden, mit dem UWP-Typsystem kompatibel sind.

**Testdetails**  
Bei diesem Test wird eine Reihe von Kennzeichen im Zusammenhang mit der richtigen Verwendung vergeben.

**Korrekturmaßnahmen**  
* **ExclusiveTo-Attribut**  
Stellen Sie sicher, dass von UWP-Klassen keine Schnittstellen implementiert werden, die für eine andere Klasse als „ExclusiveTo” gekennzeichnet sind.
* **Allgemeine Metadata Richtigkeit**  
Stellen Sie sicher, dass der zum Generieren der Typen verwendete Compiler in Bezug auf die UWP-Spezifikationen auf dem neuesten Stand ist.
* **Eigenschaften**  
Stellen Sie sicher, dass alle Eigenschaften in einer UWP-Klasse eine `get`-Methode aufweisen (`set`-Methoden sind optional). Stellen Sie für alle Eigenschaften sicher, dass der von der `get`-Methode zurückgegebene Typ dem Typ des Eingabeparameters der `set`-Methode entspricht.
* **Speicherort**  
Stellen Sie sicher, dass die Metadaten für alle UWP-Typen in der WINMD-Datei enthalten sind, die im App-Paket über den längsten Namen mit Namespaceübereinstimmung verfügt.
* **Type Name Groß-/Kleinschreibung**  
Stellen Sie sicher, dass alle UWP-Typen eindeutige Namen mit Groß-/Kleinschreibung im App-Paket aufweisen. Vergewissern Sie sich außerdem, dass UWP-Typnamen im App-Paket nicht auch als Namespacename verwendet werden.
* **Typ Richtigkeit des Namens**  
Stellen Sie sicher, dass im globalen Namespace oder im Windows-Namespace der obersten Ebene keine UWP-Typen vorhanden sind.
 

### <a name="8-windows-security-features-tests"></a>8. Tests der Windows-Sicherheitsfunktionen
Durch Ändern der standardmäßigen Windows-Sicherheitsvorkehrungen können Kunden einem erhöhten Risiko ausgesetzt werden. 

#### <a name="81-banned-file-analyzer"></a>8.1 Analyzer für gesperrte Dateien
**Hintergrund**  
Bestimmte Dateien wurden im Hinblick auf wichtige Aspekte wie Sicherheit, Zuverlässigkeit oder andere Verbesserungen aktualisiert. Windows Desktop-Brücke-Apps müssen die neuesten Versionen dieser Dateien enthalten, da veraltete Versionen ein Risiko darstellen. Diese Dateien werden im Zertifizierungskit für Windows-Apps blockiert, um sicherzustellen, dass von allen Entwicklern aktuelle Versionen verwendet werden.

**Testdetails**  
Beim Test auf unzulässige Dateien im Windows-Zertifizierungskit für Apps wird derzeit eine Überprüfung auf folgende Dateien durchgeführt:
* *Bing.Maps.JavaScript\js\veapicore.js*  
Für diese Überprüfung tritt normalerweise ein Fehler auf, wenn von einer App anstelle der aktuellen offiziellen Version eine Release Preview-Version der Datei verwendet wird. 

**Korrekturmaßnahmen**  
Um dies zu korrigieren, verwenden Sie die neueste Version von der [Bing Maps-SDK](https://go.microsoft.com/fwlink/p/?linkid=614880) für UWP-apps.

#### <a name="82-private-code-signing"></a>8.2 Private Codesignatur
Überprüft, ob innerhalb des App-Pakets Binärdateien für private Codesignaturen vorhanden sind. 

**Hintergrund**  
Signaturdateien für privaten Code sollten privat bleiben, da sie im Fall einer Gefährdung zu bösartigen Zwecken missbraucht werden könnten. 

**Testdetails**  
Überprüft, ob innerhalb des App-Pakets Dateien mit der Erweiterung „.pfx“ oder „.snk“ vorhanden sind, die darauf hinweisen, dass private Signaturschlüssel verwendet werden. 

**Korrekturmaßnahmen**  
Entfernen Sie alle Signaturschlüssel für privaten Code (wie z. B. PFX- und SNK-Dateien) aus dem Paket.


## <a name="related-topics"></a>Verwandte Themen

* [Microsoft Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944)
