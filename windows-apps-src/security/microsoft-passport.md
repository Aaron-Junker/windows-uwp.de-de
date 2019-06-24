---
title: Windows Hello
description: In diesem Artikel wird die neue Windows-Hello-Technologie beschrieben, die im Lieferumfang des Windows 10-Betriebssystems enthalten ist. Zudem wird erörtert, wie Entwickler diese Technologie implementieren können, um ihre UWP (Universelle Windows-Plattform)-Apps und Back-End-Dienste zu schützen. Der Artikel hebt die spezifischen Funktionen dieser Technologien hervor, die dabei helfen, aus der Verwendung herkömmlicher Anmeldeinformationen erwachsende Bedrohungen zu mindern. Darüber hinaus bietet er eine Anleitung dazu, wie diese Technologien als Bestandteil Ihrer Windows 10-Einführung entworfen und bereitgestellt werden.
ms.assetid: 0B907160-B344-4237-AF82-F9D47BCEE646
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: 6e69d3489bcc41f40eca07aff628425d34819c4b
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320591"
---
# <a name="windows-hello"></a>Windows Hello

Dieser Artikel beschreibt die neue Windows Hello Technologie, die ist im Lieferumfang des Betriebssystems Windows 10 und erläutert, wie Entwickler diese Technologie zum Schutz ihrer apps der universellen Windows-Plattform (UWP) und Back-End-Dienste implementieren können. Der Artikel hebt die spezifischen Funktionen dieser Technologien hervor, die dabei helfen, aus der Verwendung herkömmlicher Anmeldeinformationen erwachsende Bedrohungen zu mindern. Darüber hinaus bietet er eine Anleitung dazu, wie diese Technologien als Bestandteil Ihrer Windows 10-Einführung entworfen und bereitgestellt werden.

Beachten Sie, dass der Schwerpunkt dieses Artikels auf der App-Entwicklung liegt. Weitere Informationen zu den Architektur- und Implementierungsdetails von Windows Hello finden Sie unter [Windows-Hello-Anleitung auf TechNet](https://docs.microsoft.com/windows/keep-secure/microsoft-passport-guide).

Ein vollständiges Codebeispiel finden Sie im [Windows-Hello-Codebeispiel auf GitHub](https://go.microsoft.com/fwlink/?LinkID=717812).

Ein schrittweises Vorgehen zum Erstellen einer UWP-App mit Windows Hello und dem zugrunde liegenden Authentifizierungsdienst finden Sie in den Artikeln [Windows-Hello-Anmelde-App](microsoft-passport-login.md) und [Windows-Hello-Anmeldedienst](microsoft-passport-login-auth-service.md).

## <a name="1-introduction"></a>1 Einführung

Eine grundlegende Annahme über die Informationssicherheit besteht darin, dass ein System denjenigen ermitteln kann, der es verwendet. Durch das Ermitteln eines Benutzers kann das System entscheiden, ob der Benutzer ordnungsgemäß identifiziert wurde (wird auch als Authentifizierung bezeichnet), und anschließend wird bestimmt, welche Aktionen der ordnungsgemäß authentifizierte Benutzer ausführen darf (Autorisierung). Die große Mehrheit der weltweit bereitgestellten Computersysteme ist davon abhängig, dass Authentifizierungs- und Autorisierungsentscheidungen auf der Grundlage von Benutzeranmeldeinformationen getroffen werden. Demzufolge bilden wiederverwendbare, vom Benutzer erstellte Kennwörter die Sicherheitsbasis für diese Systeme. Die oftmals zitierte Regel, dass die Authentifizierung etwas beinhalten kann, was man „weiß, hat oder ist“, fasst das Problem gut zusammen: Ein wiederverwendbares Kennwort ist an sich schon ein Authentifizierungsfaktor. Jeder, der das Kennwort kennt, kann sich demnach als der Benutzer ausgeben, dem das Kennwort gehört.

### <a name="11-problems-with-traditional-credentials"></a>1.1 Probleme mit herkömmlichen Anmeldeinformationen

Seit der Mitte der 1960er Jahre, als Fernando Corbató und sein Team im Massachusetts Institute of Technology die Einführung des Kennworts meisterten, müssen Benutzer und Administratoren mit der Verwendung von Kennwörtern für die Benutzerauthentifizierung und -autorisierung leben. Mit der Zeit hat sich der Leistungsstandard für die Kennwortspeicherung und die Verwendung ein wenig verbessert (beispielsweise durch Kennworthashing und Salt. Es gibt jedoch weiterhin zwei Probleme. Kennwörter lassen sich leicht klonen und stehlen. Außerdem werden sie durch Implementierungsfehler möglicherweise unsicher. Zudem müssen die Benutzer das Praktische und die Sicherheit ausgleichen.

#### <a name="111-credential-theft"></a>1.1.1. Diebstahl von Anmeldeinformationen

Das größte Risiko von Kennwörtern ist einfach: Ein Angreifer kann sie leicht stehlen. Jeder Ort, an dem ein Kennwort eingegeben, verarbeitet oder gespeichert wird, ist anfällig. So kann ein Angreifer beispielsweise eine Sammlung an Kennwörtern oder Hashwerten von einem Authentifizierungsserver stehlen, indem er Lauschangriffe auf Netzwerkdatenverkehr eines Anwendungsservers ausführt, Schadsoftware in eine Anwendung oder auf einem Gerät einschleust, die Benutzertastaturanschläge auf einem Gerät aufzeichnet oder dem Benutzer zusieht, welche Zeichen er eingibt. Diese sind nur die häufigsten Angriffsmethoden.

Ein weiteres verwandtes Risiko besteht im sogenannten „Credential Replay“ (Wiedergabe von Anmeldeinformationen). Hierbei erfasst ein Angreifer gültige Anmeldeinformationen, indem er einen Lauschangriff auf ein unsicheres Netzwerk ausführt und die Anmeldeinformationen später wiedergibt, um einen gültigen Benutzer zu imitieren. Die meisten Authentifizierungsprotokolle (einschließlich Kerberos und OAuth) schützen vor Replay-Angriffen, indem ein Zeitstempel im Anmeldeinformationsaustauschprozess eingefügt wird. Dadurch wird jedoch nur das Token geschützt, das vom Authentifizierungssystem ausgestellt wird, nicht aber das Kennwort, das vom Benutzer bereitgestellt wird, um sich ursprünglich anzumelden.

#### <a name="112-credential-reuse"></a>1.1.2 Wiederverwendung von Anmeldeinformationen

Der häufige Ansatz, eine E-Mail-Adresse als den Benutzernamen zu verwenden, verschlimmert das Problem nur noch. Ein Angreifer, der eine Kombination aus Benutzernamen und Kennwort erfolgreich aus einem kompromittierten System wiederhergestellt hat, kann anschließend dieselbe Kombination auf anderen Systemen probieren. Diese Taktik funktioniert erstaunlicherweise oft. Auf diese Weise können Angreifer ein kompromittiertes System als Sprungbrett zu anderen Systemen verwenden. Zudem führt das Verwenden von E-Mail-Adressen als Benutzernamen zu weiteren Problemen, die später in dieser Anleitung erörtert werden.

### <a name="12-solving-credential-problems"></a>1.3 Beheben von Anmeldeinformationsproblemen

Das Lösen der Probleme, die Kennwörter mit sich bringen, ist nicht einfach. Die Verschärfung von Kennwortrichtlinien allein wird das Problem nicht beheben: Die Benutzer könnten ihre Kennwörter einfach wiederverwenden, teilen oder aufschreiben. Auch wenn Benutzerschulungen für die Authentifizierungssicherheit wichtig sind, wird das Problem durch die Schulung allein ebenfalls nicht beseitigt.

Windows Hello ersetzt Kennwörter durch eine sichere zweistufige Authentifizierung (2FA). Dabei werden vorhandene Anmeldeinformationen überprüft und gerätespezifische Anmeldeinformationen erstellt, die durch eine Benutzergeste (Biometrie- oder PIN-basiert) geschützt sind. 


## <a name="2-what-is-windows-hello"></a>2 Was ist Windows Hello?

Windows Hello ist der durch Microsoft für das neue in Windows 10 integrierte biometrische Anmeldesystem festgelegte Name. Da es direkt im Betriebssystem integriert ist, ermöglicht Windows Hello die Gesichts- oder Fingerabdruckidentifikation zum Entsperren von Benutzergeräten. Die Authentifizierung erfolgt, wenn der Benutzer seine eindeutige biometrische ID übermittelt, um auf die gerätespezifischen Anmeldeinformationen zuzugreifen. Demzufolge kann sich ein Angreifer, der das Gerät gestohlen hat, nicht anmelden, es sei denn, der Angreifer verfügt über die PIN. Der sichere Windows-Anmeldeinformationsspeicher schützt die biometrischen Daten auf dem Gerät. Durch das Verwenden von Windows Hello zum Entsperren eines Geräts erhält autorisierte Benutzer Zugriff auf seine sämtlichen Windows-Funktionen, -Apps, -Daten, -Websites und -Dienste.

Der Windows Hello-Authentifikator wird auch als Hello bezeichnet. Ein Hello ist für die Kombination aus einem einzelnen Gerät und einem bestimmten Benutzer eindeutig. Es wird nicht auf mehreren Geräten verwendet, nicht für einen Server oder eine aufrufende App freigegeben und kann von einem Gerät nicht einfach extrahiert werden. Wenn mehrere Benutzer ein Gerät gemeinsam verwenden, muss jeder Benutzer ein eigenes Konto einrichten. Jedes Konto erhält ein eindeutiges Hello für dieses Gerät. Sie können sich ein Hello als Token vorstellen, das Sie zum Entsperren (oder Freigeben) von gespeicherten Anmeldeinformationen verwenden können. Mit dem Hello an sich werden Sie nicht für eine App oder einen Dienst authentifiziert, es gibt jedoch die dafür benötigten Anmeldeinformationen frei. Anders ausgedrückt, handelt es sich beim Hello nicht um Benutzeranmeldeinformationen, sondern um einen zweiten Faktor für die Authentifizierung.

### <a name="21-windows-hello-authentication"></a>2.1 Windows Hello-Authentifizierung

Windows Hello bietet Geräten eine zuverlässige Möglichkeit, einzelne Benutzer zu erkennen. Dies betrifft den ersten Teil des Wegs zwischen einem Benutzer und einem angeforderten Dienst- oder Datenelement. Nachdem das Gerät den Benutzer erkannt hat, muss es den Benutzer jedoch erst noch authentifizieren, bevor es entscheidet, ob er auf die angeforderte Ressource zugreifen darf. Windows Hello bietet sichere 2FA, die vollständig in Windows integriert ist, und ersetzt wiederverwendbare Kennwörter durch eine Kombination aus einem bestimmten Gerät und einer biometrischen Geste oder PIN.

Bei Windows Hello handelt es sich jedoch nicht um einen bloßen Ersatz für herkömmliche 2FA-Systeme. Konzeptionell gesehen ähnelt es Smartcards. Die Authentifizierung wird mithilfe von kryptografischen Primitiven ausgeführt, anstelle Zeichenfolgen zu vergleichen. Zudem sind die Schlüssel des Benutzers in der vor Manipulationen geschützten Hardware sicher. Windows Hello erfordert keine zusätzlichen Infrastrukturkomponenten, wie sie für die Smartcardbereitstellung benötigt werden. Insbesondere benötigen Sie keine Public Key-Infrastruktur (PKI) zum Verwalten von Zertifikaten, wenn Sie derzeit keine haben. Windows Hello kombiniert die wichtigsten Vorteile von Smartcards – flexible Bereitstellung für virtuelle Smartcards und stabile Sicherheit für physische Smartcards – jedoch ohne die Nachteile.

### <a name="22-how-windows-hello-works"></a>2.2 Wie funktioniert Windows Hello

Wenn Benutzer Windows Hello auf ihrem Computer einrichten, generiert es ein neues Schlüsselpaar bestehend aus öffentlichem und privatem Schlüssel auf dem Gerät. Das [Trusted Platform Module](https://docs.microsoft.com/windows/keep-secure/trusted-platform-module-overview) (TPM) generiert und schützt diesen privaten Schlüssel. Wenn das Gerät nicht über einen TPM-Chip verfügt, wird der private Schlüssel verschlüsselt und durch Software geschützt. Zusätzlich generieren TPM-aktivierte Geräte einen Datenblock, der für die Bestätigung verwendet werden kann, dass ein Schlüssel an TPM gebunden ist. Die Nachweisinformationen können in Ihrer Lösung verwendet werden, um z. B. zu entscheiden, ob dem Benutzer eine andere Autorisierungsstufe gewährt wird.

Zum Aktivieren von Windows Hello auf einem Gerät muss der Benutzer entweder sein Azure Active Directory-Konto oder sein Microsoft-Konto in den Windows-Einstellungen verbinden.

#### <a name="221-how-keys-are-protected"></a>2.2.1 Schutz von Schlüsseln

Bei jedem Generieren von Schlüsselmaterialien muss dieses gegen Angriffe geschützt werden. Die zuverlässigste Möglichkeit diesbezüglich ist über spezialisierte Hardware möglich. Lange Zeit wurden Hardwaresicherheitsmodule (Hardware Security Modules, HSMs) verwendet, um Schlüssel für sicherheitskritische Anwendungen zu generieren, zu speichern und zu verarbeiten. Bei Smartcards handelt es sich um einen besonderen HSM-Typ, ebenso wie Geräte, die mit dem Trusted Computing Group TPM-Standard konform sind. Nach Möglichkeit profitiert die Windows-Hello-Implementierung von der integrierten TPM-Hardware, um Schlüssel zu generieren, zu speichern und zu verarbeiten. Allerdings benötigen Windows Hello und Windows Hello for Work kein integriertes TPM.

Microsoft empfiehlt, nach Möglichkeit TPM-Hardware zu verwenden. Das TPM schützt vor einer Vielzahl von unbekannten und potenziellen Angriffen einschließlich Brute-Force-Angriffen auf die PIN. Das TPM stellt auch nach einer Kontosperre eine zusätzliche Schutzebene bereit. Wenn das Schlüsselmaterial von TPM gesperrt wurde, muss der Benutzer die PIN zurücksetzen. Beim Zurücksetzen der PIN werden alle mit dem alten Schlüsselmaterial verschlüsselten Schlüssel und Zertifikate entfernt.

#### <a name="222-authentication"></a>2.2.2 Authentifizierung

Wenn ein Benutzer auf geschütztes Schlüsselmaterial zugreifen möchte, beginnt der Authentifizierungsprozess damit, dass der Benutzer das Gerät durch Eingabe einer PIN oder eine geometrische Geste entsperrt. Dieser Vorgang wird auch als Freigeben des Schlüssels bezeichnet.

Eine Anwendung kann niemals die Schlüssel einer anderen Anwendung bzw. ein Benutzer niemals die Schlüssel anderer Benutzer verwenden. Diese Schlüssel werden zum Signieren von Anforderungen verwendet, die an den Identitätsanbieter oder IDP gesendet werden, um Zugriff auf angegebene Ressourcen zu erhalten. Anwendungen können bestimmte APIs verwenden, um Vorgänge anzufordern, für die Schlüsselmaterial für bestimmte Aktionen erforderlich sind. Für den Zugriff über diese APIs ist keine explizite Überprüfung anhand einer Benutzergeste erforderlich, und das Schlüsselmaterial wird gegenüber der anfordernden Anwendung nicht offengelegt. Stattdessen fordert die Anwendung eine bestimmte Aktion, wie das Signieren eines Datenelements, und die Windows-Hello-Ebene führt die Aktion aus und gibt die Ergebnisse zurück.

### <a name="23-getting-ready-to-implement-windows-hello"></a>2.3 Vorbereitung der Implementierung von Windows Hello

Nachdem wir nun ein grundlegendes Verständnis der Arbeitsweise von Windows Hello besitzen, befassen wir uns damit, wie wir sie in unseren eigenen Anwendungen implementieren.

Es gibt verschiedene Szenarien, die mithilfe von Windows Hello implementiert werden können. Beispielsweise die Anmeldung bei der App auf einem Gerät. Das andere häufige Szenario wäre dagegen die Authentifizierung bei einem Dienst. Anstelle von Benutzernamen und Kennwort verwenden Sie Windows Hello. In den folgenden Kapiteln besprechen wir die Implementierung einiger verschiedener Szenarien, z. B. die Authentifizierung gegenüber Diensten mit Windows Hello und die Umwandlung eines vorhandenen Benutzername-/Kennwortsystems in ein Windows-Hello-System.

Beachten Sie schließlich, dass die Windows-Hello-API die Verwendung des Windows-10-SDKs erfordern, die dem Betriebssystem entspricht, unter dem die App verwendet wird. Mit anderen Worten: Das Windows-SDK 10.0.10240 muss für Apps verwendet werden, die unter Windows 10 bereitgestellt werden, und 10.0.10586 muss für Apps verwendet werden, die für Windows 10, Version 1511 bereitgestellt werden.

## <a name="3-implementing-windows-hello"></a>3 Implementieren von Windows Hello

In diesem Kapitel beginnen wir mit einem „frischen“ Szenario ohne vorhandenes Authentifizierungssystem und erläutern die Implementierung von Windows Hello.

Im nächsten Abschnitt wird die Migration von einem System mit vorhandenem Benutzernamen/Kennwort behandelt. Auch wenn Sie am nächsten Kapitel mehr interessiert sind, sollten Sie sich dieses ansehen, um ein grundlegendes Verständnis des Prozesses und des erforderlichen Codes zu erhalten.

### <a name="31-enrolling-new-users"></a>3.1 Registrieren neuer Benutzer

Wir beginnen mit einem völlig neuen Dienst, der Windows Hello verwendet, und einem hypothetischen neuen Benutzer, der zur Registrierung auf einem neuen Gerät bereit ist.

Im ersten Schritt wird sichergestellt, dass der Benutzer Windows Hello verwenden kann. Die App überprüft die Benutzereinstellungen und Computerfunktionen, um sicherzustellen, dass Benutzer-ID-Schlüssel erstellt werden können. Wenn die App ermittelt, dass der Benutzer Windows Hello noch nicht aktiviert hat, fordert sie ihn dazu auf, Windows Hello vor der Verwendung der App einzurichten.

Zum Aktivieren von Windows Hello muss der Benutzer lediglich eine PIN unter Windows-Einstellungen einrichten, es sei denn, der Benutzer hat diese bereits auf der Willkommensseite (Out of Box Experience, OOBE) eingerichtet.

Die folgenden Codezeilen zeigen eine einfache Methode zum Überprüfen, ob der Benutzer Windows Hello eingerichtet hat.

```csharp
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
if (!keyCredentialAvailable)
{
    // User didn't set up PIN yet
    return;
}
```

Im nächsten Schritt werden die Informationen angefordert, mit denen sich der Benutzer bei Ihrem Dienst registriert. Sie können den Vornamen, Nachnamen, die E-Mail-Adresse und einen eindeutigen Benutzernamen vom Benutzer anfordern. Die E-Mail-Adresse können Sie ggf. als eindeutigen Bezeichner verwenden.

In diesem Szenario verwenden wir die E-Mail-Adresse als eindeutigen Bezeichner für den Benutzer. Nachdem sich der Benutzer registriert hat, sollten Sie eine Validierungs-E-Mail senden, um sicherzustellen, dass die Adresse gültig ist. Dadurch erhalten Sie einen Mechanismus zum Zurücksetzen des Kontos, falls dies erforderlich ist.

Wenn der Benutzer seine PIN eingerichtet hat, erstellt die App das [**KeyCredential**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.KeyCredential) des Benutzers. Die App ruft außerdem die optionalen Schlüsselnachweisinformationen ab, um einen kryptografischen Beweis dafür zu erhalten, dass der Schlüssel im TPM generiert wird. Der generierte öffentliche Schlüssel und optional der Nachweis werden an den Back-End-Server gesendet, um das verwendete Gerät zu registrieren. Jedes Schlüsselpaar, das auf den einzelnen Geräten generiert wird, ist eindeutig.

Der Code zum Erstellen von [**KeyCredential**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.KeyCredential) sieht wie folgt aus:

```csharp
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

[  **RequestCreateAsync**](https://docs.microsoft.com/previous-versions/windows/dn973048(v=win.10)) ist der Teil, durch den öffentlicher und privater Schlüssel erstellt werden. Wenn das Gerät über den richtigen TPM-Chip verfügt, fordern die APIs den TPM-Chip zum Erstellen des privaten und öffentlichen Schlüssels an und speichern das Ergebnis. Wenn kein TPM-Chip verfügbar ist, erstellt das Betriebssystem das Schlüsselpaar im Code. Die App hat keine Möglichkeit, direkt auf die erstellten privaten Schlüssel zuzugreifen. Bei der Erstellung von Schlüsselpaaren geht es auch um die resultierenden Nachweisinformationen. (Weitere Informationen zu Nachweisen finden Sie im nächsten Abschnitt.)

Nachdem das Schlüsselpaar und die Nachweisinformationen auf dem Gerät erstellt wurden, müssen der öffentliche Schlüssel, die optionalen Nachweisinformationen und der eindeutige Bezeichner (z. B. die E-Mail-Adresse) an den Back-End-Registrierungsdienst gesendet und im Back-End gespeichert werden.

Damit der Benutzer über mehrere Geräte auf die App zugreifen kann, muss der Back-End-Dienst mehrere Schlüssel für denselben Benutzer speichern können. Jeder Schlüssel ist für jedes Gerät eindeutig. Deshalb werden alle Schüssel gespeichert, die demselben Benutzer zugeordnet sind. Ein Gerätebezeichner dient zur Optimierung der Serverseite bei der Benutzerauthentifizierung. Darauf gehen wir im nächsten Kapitel genauer ein.

Ein Beispieldatenbankschema zum Speichern dieser Informationen im Back-End kann wie folgt aussehen:

![Windows Hello-Beispieldatenbankschema](images/passport-db.png)

Die Registrierungslogik kann wie folgt aussehen:

![Windows Hello-Registrierungslogik](images/passport-registration.png)

Die von Ihnen erfassten Registrierungsinformationen umfassen natürlich viel mehr Identifikationsinformationen, als wir in dieses einfache Szenario einbinden. Wenn Ihre App beispielsweise auf einen gesicherten Dienst – z. B. Onlinebanking – zugreift, müssen Sie beim Anmeldevorgang einen Identitätsnachweis und andere Dinge anfordern. Nachdem alle Bedingungen erfüllt wurden, wird der öffentliche Schlüssel dieses Benutzers im Back-End gespeichert und für Überprüfungszwecke verwendet, wenn der Benutzer den Dienst das nächste Mal verwendet.

```csharp
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Credentials;

static async void RegisterUser(string AccountId)
{
    var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
    if (!keyCredentialAvailable)
    {
        // The user didn't set up a PIN yet
        return;
    }

    var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
    if (keyCreationResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = keyCreationResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var keyAttestationResult = await userKey.GetAttestationAsync();
        IBuffer keyAttestation = null;
        IBuffer certificateChain = null;
        bool keyAttestationIncluded = false;
        bool keyAttestationCanBeRetrievedLater = false;

        keyAttestationResult = await userKey.GetAttestationAsync();
        KeyCredentialAttestationStatus keyAttestationRetryType = 0;

        if (keyAttestationResult.Status == KeyCredentialAttestationStatus.Success)
        {
            keyAttestationIncluded = true;
            keyAttestation = keyAttestationResult.AttestationBuffer;
            certificateChain = keyAttestationResult.CertificateChainBuffer;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.TemporaryFailure)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.TemporaryFailure;
            keyAttestationCanBeRetrievedLater = true;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.NotSupported)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.NotSupported;
            keyAttestationCanBeRetrievedLater = true;
        }
    }
    else if (keyCreationResult.Status == KeyCredentialStatus.UserCanceled ||
        keyCreationResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        // Show error message to the user to get confirmation that user
        // does not want to enroll.
    }
}
```

#### <a name="311-attestation"></a>3.1.1 Nachweis

Beim Erstellen des Schlüsselpaars besteht auch die Möglichkeit, die vom TPM-Chip generierten Nachweisinformationen anzufordern. Diese optionalen Informationen können im Rahmen des Registrierungsvorgangs an den Server gesendet werden. Der TPM-Schlüsselnachweis ist ein Protokoll, durch das kryptografisch überprüft wird, ob ein Schlüssel TPM-gebunden ist. Mithilfe dieses Nachweistyps kann gewährleistet werden, dass ein bestimmter kryptografischer Vorgang im TPM eines bestimmten Computers ausgeführt wurde.

Wenn der Server den generierten RSA-Schlüssel, die Nachweisanweisung und das AIK-Zertifikat empfängt, überprüft er folgende Voraussetzungen:

- Die AIK-Zertifikatsignatur muss gültig sein.
- Das AIK-Zertifikat muss mit einer vertrauenswürdigen Stammzertifizierungsstelle verkettet sein.
- Das AIK-Zertifikat und dessen Kette müssen für EKU-OID „2.23.133.8.3“ aktiviert sein (der Anzeigename lautet „Attestation Identity Key-Zertifikat“).
- Das AIK-Zertifikat muss zeitlich gültig sein.
- Die Zertifikate aller ausstellenden Zertifizierungsstellen in der Kette müssen zeitlich gültig und dürfen nicht widerrufen sein.
- Die Nachweisanweisung muss das richtige Format aufweisen.
- Die Signatur für den [**KeyAttestation**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.KeyAttestationHelper)-BLOB verwendet einen öffentlichen AIK-Schlüssel.
- Der im [**KeyAttestation**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.KeyAttestationHelper)-BLOB enthaltene öffentliche Schlüssel muss mit dem öffentlichen RSA-Schlüssel übereinstimmen, den der Client zusammen mit der Nachweisanweisung gesendet hat.

Ihre App weist dem Benutzer je nach diesen Umständen möglicherweise eine andere Autorisierungsstufe zu. Wenn beispielsweise eine dieser Voraussetzungen nicht erfüllt ist, registriert die App den Benutzer möglicherweise nicht bzw. schränkt dessen Befugnisse ein.

### <a name="32-logging-on-with-windows-hello"></a>3.2 Anmeldung mit Windows Hello

Nachdem sich der Benutzer in Ihrem System registriert ist, kann er die App nutzen. Je nach Szenario können Sie die Authentifizierung des Benutzers anfordern, bevor er mit der App-Nutzung beginnen darf, oder die Authentifizierung danach mithilfe der Back-End-Dienste anfordern.

### <a name="33-force-the-user-to-sign-in-again"></a>3.3 Erzwingen der erneuten Benutzeranmeldung

In einigen Szenarien sollte der Benutzer nachweisen, dass es sich um die derzeit angemeldete Person handelt, bevor er auf die App zugreift bzw. in manchen Fällen bevor er eine bestimmte Aktion innerhalb der App ausführt. Bevor beispielsweise eine Banking-App den Befehl zum Überweisen von Geld an den Server sendet, sollten Sie sicherstellen, dass es sich um den richtigen Benutzer handelt, anstatt einer Person, die ein angemeldetes Gerät gefunden hat und versucht, eine Transaktion ausführen. Sie können eine erneute Benutzeranmeldung in der App erzwingen, indem Sie die [**UserConsentVerifier**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier)-Klasse verwenden. Durch die folgende Codezeile wird die erneute Eingabe von Anmeldeinformationen erzwungen.

Durch die folgende Codezeile wird die erneute Eingabe von Anmeldeinformationen erzwungen.

```csharp
UserConsentVerificationResult consentResult = await UserConsentVerifier.RequestVerificationAsync("userMessage");
if (consentResult.Equals(UserConsentVerificationResult.Verified))
{
    // continue
}
```

Natürlich können Sie auch den Abfrage-/Rückmeldungsmechanismus verwenden, bei dem der Server den Benutzer zur Eingabe seines PIN-Codes von biometrischen Anmeldeinformationen auffordert. Dies hängt von dem Szenario ab, das Sie als Entwickler implementieren müssen. Dieser Mechanismus wird im folgenden Abschnitt beschrieben.

### <a name="34-authentication-at-the-backend"></a>3.4 Authentifizierung im Back-End

Wenn eine App versucht, auf einen geschützten Back-End-Dienst zuzugreifen, sendet dieser eine Abfrage an die App. Die App verwendet den privaten Schlüssel des Benutzers, um die Abfrage zu signieren, und sendet sie an den Server zurück. Da der Server den öffentlichen Schlüssel für diesen Benutzer gespeichert hat, stellt er anhand standardmäßiger Krypto-APIs sicher, dass die Nachricht tatsächlich mit dem richtigen privaten Schlüssel signiert wurde. Auf dem Client erfolgt die Signierung mithilfe der Windows-Hello-API; der Entwickler selbst erhält niemals Zugriff auf den privaten Schlüssel eines Benutzers.

Neben der Schlüsselüberprüfung kann der Dienst auch den Schlüsselnachweis überprüfen, um festzustellen, ob Beschränkungen hinsichtlich der Speicherung der Schlüssel auf dem Gerät bestehen. Wenn das Gerät beispielsweise TPM zum Schützen der Schlüssel verwendet, ist die Sicherheit höher als bei Geräten, auf denen Schlüssel ohne TPM gespeichert werden. Die Back-End-Logik könnte z. B. festlegen, dass der Benutzer Geld nur bis zu einer bestimmten Höhe überweisen darf, wenn kein TPM zur Risikominderung eingesetzt wird.

Ein Nachweis ist nur für Geräte mit einem TPM-Chip der Version 2.0 oder höher verfügbar. Daher müssen Sie berücksichtigen, dass diese Informationen nicht immer auf jedem Gerät zur Verfügung stehen.

Der Clientworkflow sieht ggf. wie das folgende Diagramm aus:

![Windows Hello-Clientworkflow](images/passport-client-workflow.png)

Wenn die App den Dienst im Back-End aufruft, sendet der Server eine Abfrage. Die Abfrage ist mit dem folgenden Code signiert:

```csharp
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

if (openKeyResult.Status == KeyCredentialStatus.Success)
{
    var userKey = openKeyResult.Credential;
    var publicKey = userKey.RetrievePublicKey();
    var signResult = await userKey.RequestSignAsync(message);

    if (signResult.Status == KeyCredentialStatus.Success)
    {
        return signResult.Result;
    }
    else if (signResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {

    }
}
```

Durch die erste Zeile [**KeyCredentialManager.OpenAsync**](https://docs.microsoft.com/uwp/api/windows.security.credentials.keycredentialmanager.openasync) wird das Betriebssystem aufgefordert, den Schlüsselhandle zu öffnen. Wenn dieser Schritt erfolgreich ist, können Sie die Abfragenachricht mit der [**KeyCredential.RequestSignAsync**](https://docs.microsoft.com/uwp/api/windows.security.credentials.keycredential.requestsignasync)-Methode signieren. Dadurch wird das Betriebssystem aufgefordert, die PIN oder biometrische Daten des Benutzers über Windows Hello anzufordern. Der Entwickler hat zu keiner Zeit Zugriff auf den privaten Schlüssel des Benutzers. Dieser wird durch die APIs gesichert.

Die APIs fordern das Betriebssystem auf, die Abfrage mit dem privaten Schlüssel zu signieren. Das System fordert dann vom Benutzer einen PIN-Code oder konfigurierte biometrische Anmeldeinformationen an. Wenn die richtigen Informationen eingegeben werden, kann das System den TPM-Chip zum Ausführen der kryptografischen Funktionen und zum Signieren der Abfrage auffordern. (Falls kein TPM verfügbar ist, verwenden Sie die Fallback-Softwarelösung.) Der Client muss die signierte Abfrage zurück an den Server senden.

In diesem Sequenzdiagramm ist ein grundlegender Abfrage/Rückmeldungsablauf dargestellt:

![Windows Hello-Abfrage/Rückmeldung](images/passport-challenge-response.png)

Als nächstes muss der Server die Signatur überprüfen. Wenn Sie den öffentlichen Schlüssel anfordern, und senden es an den Server zur zukünftigen Überprüfung verwenden, ist es in einem Blob ASN. 1-codierte PublicKeyInfo. Bei Betrachtung der [Windows Hello-Codebeispiel auf GitHub](https://go.microsoft.com/fwlink/?LinkID=717812), sehen Sie, dass es sind Hilfsklassen zum Umschließen Crypt32-Funktionen, um der ASN. 1-codierte Blob in ein Blob CNG zu übersetzen, die häufiger verwendet wird. Das BLOB enthält den Algorithmus des öffentlichen Schlüssels, also RSA, und den öffentlichen RSA-Schlüssel.

In diesem Beispiel ist der Grund konvertieren wir die ASN. 1-codierte-Blob in ein Blob von CNG, damit es mit CNG verwendet (/ Windows/Desktop/SecCNG/Cng-Portal) sein kann und die BCrypt-API. Wenn Sie sich das Blob CNG ansehen, zeigen sie Sie auf den zugehörigen [BCRYPT_KEY_BLOB Struktur](/windows/desktop/api/bcrypt/ns-bcrypt-_bcrypt_key_blob). Dieser API-Oberfläche kann für die Authentifizierung und Verschlüsselung in Windows-Anwendungen verwendet werden. ASN. 1 ist ein dokumentierten Standard für die Kommunikation von Datenstrukturen, die serialisiert werden können, und sie wird häufig verwendet, in der Kryptografie mit öffentlichem Schlüssel und Zertifikate. Daher Informationen des öffentliche Schlüssels wird auf diese Weise zurückgegeben. Der öffentliche Schlüssel ist ein RSA-Schlüssel. und der Algorithmus, den Windows Hello verwendet, wenn sie die Daten signiert wird.

Sobald das CNG-BLOB verfügbar ist, müssen Sie die signierte Abfrage mit dem öffentlichen Schlüssel des Benutzers abgleichen. Da jeder seine eigene System- oder Back-End-Technologie verwendet, gibt es keine allgemeine Vorgehensweise zur Implementierung dieser Logik. Wir verwenden SHA256 als Hashalgorithmus und Pkcs1 für SignaturePadding, um sicherzustellen, dass Sie diese Funktionen zur Überprüfung der signierten Antwort vom Client verwenden. An dieser Stelle verweisen wir erneut auf das Beispiel, in dem die Vorgehensweise mit .NET 4.6 auf dem Server beschrieben wird. In der Regel werden jedoch etwa folgende Schritte ausgeführt:

```csharp
using (RSACng pubKey = new RSACng(publicKey))
{
    retval = pubKey.VerifyData(originalChallenge, responseSignature, HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1);
}
```

Wir lesen den gespeicherten öffentlichen Schlüssel, bei dem es sich um einen RSA-Schlüssel handelt. Wir gleichen die Nachricht mit der signierten Abfrage mit dem öffentlichen Schlüssel ab. Nach erfolgreicher Überprüfung wird der Benutzer autorisiert. Sobald der Benutzer authentifiziert wurde, kann die App die Back-End-Dienste wie gewohnt aufrufen.

Der vollständige Code sieht etwa wie folgt aus:

```csharp
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Security.Credentials;

static async Task<IBuffer> GetAuthenticationMessageAsync(IBuffer message, String AccountId)
{
    var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

    if (openKeyResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = openKeyResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var signResult = await userKey.RequestSignAsync(message);
        if (signResult.Status == KeyCredentialStatus.Success)
        {
            return signResult.Result;
        }
        else if (signResult.Status == KeyCredentialStatus.UserCanceled)
        {
            // Launch app-specific flow to handle the scenario 
            return null;
        }
    }
    else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
    {
        // PIN reset has occurred somewhere else and key is lost.
        // Repeat key registration
        return null;
    }
    else
    {
        // Show custom UI because unknown error has happened.
        return null;
    }
}
```

Die Implementierung des richtigen Abfrage-/Rückmeldungsmechanismus wird in diesem Dokument nicht erörtert. Dennoch erfordert dieses Thema Aufmerksamkeit, um erfolgreich einen sicheren Mechanismus zu erstellen, der Replay- oder Man-in-the-Middle-Angriffe wirksam abwehrt.

### <a name="35-enrolling-another-device"></a>3.5 Registrieren eines anderen Geräts

Heutzutage ist es üblich, dass Benutzer über mehrere Geräte verfügen, auf denen die gleichen Apps installiert sind. Wie funktioniert dies bei der Verwendung von Windows Hello mit mehreren Geräten?

Bei Verwendung von Windows Hello erstellt jedes Gerät eine eindeutige Kombination bestehend aus einem privaten und einem öffentlichen Schlüssel. Wenn ein Benutzer in der Lage sein soll, mehrere Geräte zu verwenden, müssen im Back-End folglich mehrere öffentliche Schlüssel dieses Benutzers gespeichert werden können. Im Datenbankdiagramm in Abschnitt 2.1 finden Sie ein Beispiel für die Tabellenstruktur.

Die Registrierung eines weiteren Geräts funktioniert fast genauso wie die erstmalige Registrierung eines Benutzers. Sie müssen weiterhin sicherstellen, dass es sich bei dem Benutzer, der sich für dieses neue Gerät registriert, tatsächlich um den Benutzer handelt, der er zu sein vorgibt. Sie können dazu einen beliebigen aktuellen Mechanismus für die zweistufige Authentifizierung verwenden. Es gibt mehrere Möglichkeiten zur sicheren Umsetzung dieses Schritts. Die Vorgehensweise hängt vom jeweiligen Szenario ab.

Wenn Sie z. B. immer noch Anmeldenamen und Kennwort verwenden, können Sie den Benutzer darüber authentifizieren und ihn zur Verwendung einer seiner Überprüfungsmethoden wie SMS oder E-Mail auffordern. Wenn Sie keinen Benutzernamen und kein Kennwort besitzen, können Sie auch eines der bereits registrierten Geräte verwenden, um eine Benachrichtigung an die App auf dem Gerät zu senden. Ein Beispiel dafür ist die MSA-Authentifikator-App. Kurz gesagt: Sie sollten einen gängigen 2FA-Mechanismus verwenden, um zusätzliche Geräte für den Benutzer zu registrieren.

Der Code zum Registrieren des neuen Geräts entspricht daher genau dem Code, der für die erstmalige Registrierung des Benutzers (innerhalb der App) verwendet wird.

```csharp
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

Damit der Benutzer einfacher erkennen kann, welche Geräte registriert sind, können Sie den Gerätenamen oder einen anderen Bezeichner mit der Registrierung senden. Dies ist z. B. auch hilfreich, wenn Sie einen Dienst für das Back-End implementieren möchten, durch den die Benutzer die Registrierung bei Geräteverlust aufheben können.

### <a name="36-using-multiple-accounts-in-your-app"></a>3.6 Verwenden mehrerer Konten in der App

Neben der Unterstützung von mehreren Geräten für ein Konto ist es auch gängig, mehrere Konten in einer App zu unterstützen. Ein Beispiel dafür ist die Anmeldung an mehreren Twitter-Konten innerhalb Ihrer App. Mit Windows Hello können Sie mehrere Schlüsselpaare erstellen und mehrere Konten in Ihrer App unterstützen.

Eine Möglichkeit besteht darin, den im vorherigen Kapitel beschriebenen Benutzernamen oder eindeutigen Bezeichner in einem isolierten Speicher zu speichern. Daher wird jedes Mal, wenn Sie ein neues Konto erstellen, die Konto-ID im isolierten Speicher abgelegt.

Auf der App-Benutzeroberfläche bieten Sie dem Benutzer an, eines der zuvor erstellten Konten auszuwählen oder sich mit einem neuen Konto zu registrieren. Die Erstellung eines neuen Kontos ist identisch mit den oben beschriebenen Schritten. Für die Auswahl eines Kontos müssen die gespeicherten Konten auf dem Bildschirm aufgelistet werden. Nachdem der Benutzer ein Konto ausgewählt hat, wird er über die Konto-ID bei der App angemeldet:

```csharp
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);
```

Die übrigen Schritte wurden oben bereits beschrieben. Es sei noch einmal betont, dass alle diese Konten durch dieselbe PIN oder biometrische Geste geschützt werden, da sie in diesem Szenario auf einem Gerät mit demselben Windows-Konto verwendet werden.

## <a name="4-migrating-an-existing-system-to-windows-hello"></a>4 Migrieren eines vorhandenen Systems zu Windows Hello

In diesem kurzen Abschnitt gehen wir davon aus, dass eine UWP (Universelle Windows-Plattform)-App vorhanden ist und das Back-End-System eine Datenbank verwendet, in der der Benutzername und der Kennworthash gespeichert sind. Diese Apps erfassen beim Start der App die Anmeldeinformationen des Benutzers und verwenden diese, wenn das Back-End-System die Authentifizierungsaufforderung zurückgibt.

In diesem Abschnitt wird beschrieben, welche Elemente geändert oder ersetzt werden müssen, damit Windows Hello funktioniert.

Die meisten der Verfahren haben wir bereits in den vorhergehenden Kapiteln behandelt. Das Hinzufügen von Windows Hello zu Ihrem vorhandenen System beinhaltet das Hinzufügen einiger verschiedener Workflows im Registrierungs- und Authentifizierungsteil Ihres Codes.

Ein Ansatz hierfür ist, den Benutzer entscheiden zu lassen, wann das Upgrade durchgeführt wird. Wenn Sie nach der Anmeldung des Benutzers an der App erkennen, dass Windows Hello von der App und dem Betriebssystem unterstützt wird, können Sie den Benutzer fragen, ob er seine Anmeldeinformationen auf dieses moderne und sicherheitsoptimierte System umstellen möchte. Anhand des folgenden Codes können Sie überprüfen, ob der Benutzer Windows Hello verwenden kann.

```csharp
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
```

Die Benutzeroberfläche sieht ungefähr wie folgt aus:

![Windows-Hello-Benutzeroberfläche](images/passport-ui.png)

Wenn sich der Benutzer entschließt, auf Windows Hello umzusteigen, müssen Sie die vorstehend beschriebene [**KeyCredential**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.KeyCredential) erstellen. Der Registrierungsserver im Back-End fügt der Datenbank den öffentlichen Schlüssel und die optionale Nachweisanweisung hinzu. Da der Benutzer bereits anhand von Benutzernamen und Kennwort authentifiziert wurde, kann der Server die neuen Anmeldeinformationen mit den aktuellen Benutzerinformationen in der Datenbank verknüpfen. Das Datenbankmodell kann dem oben beschriebenen Beispiel entsprechen.

Wenn die App die [**KeyCredential**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.KeyCredential) des Benutzers erstellen konnte, speichert sie die Benutzer-ID im isolierten Speicher, damit der Benutzer dieses Konto aus der Liste auswählen kann, wenn er die App das nächste Mal startet. Ab diesem Punkt folgt die Steuerung exakt den Beispielen in früheren Kapiteln.

Der letzte Schritt bei der Migration zu einem vollständigen Windows-Hello-Szenario besteht darin, den Anmeldenamen und die Kennwortoption in der App zu deaktivieren und die gespeicherten Kennworthashwerte aus der Datenbank zu entfernen.

## <a name="5-summary"></a>5 Zusammenfassung

Mit Windows 10 wird eine höhere Sicherheitsebene eingeführt, die außerdem einfach umzusetzen ist. Windows Hello bietet ein neues biometrisches Anmeldesystem, das den Benutzer erkennt und alle Versuche, die Identifizierung zu umgehen, aktiv verhindert. Es kann dann mehrere Ebenen für Schlüssel und Zertifikate bieten, die nie eingeblendet oder außerhalb des TPMs verwendet werden können. Eine weitere Sicherheitsebene wird außerdem durch die optionale Verwendung von Attestation Identity Keys (AIK) und Zertifikaten bereitgestellt.

Dieser Leitfaden enthält Informationen zum Entwurf und zur Bereitstellung dieser Technologien. Damit können Sie als Entwickler Ihre Windows 10-Rollouts im Handumdrehen mit sicherer Authentifizierung ausstatten, die den Schutz von Apps und Back-End-Diensten gewährleistet. Der erforderliche Code ist überschaubar und leicht verständlich. Die eigentliche Arbeit überlassen Sie einfach Windows 10.

Flexible Implementierungsoptionen ermöglichen Windows Hello, das vorhandene Authentifizierungssystem zu ersetzen oder es einzubinden. Der Bereitstellung erfolgt schnell und kosteneffizient. Zur Bereitstellung von Windows 10-Sicherheit ist keine zusätzliche Infrastruktur erforderlich. Durch die Integration von Microsoft Hello in das Betriebssystem ist Windows 10 die sicherste Antwort auf Authentifizierungsprobleme, mit denen Entwickler heute konfrontiert sind.

Mission erfüllt! Sie haben das Internet gerade sicherer gemacht!

## <a name="6-resources"></a>6 Ressourcen

### <a name="61-articles-and-sample-code"></a>6.1 Artikel und Beispielcode

- [Windows Hello-Übersicht](https://support.microsoft.com/help/17215)
- [Details zur Implementierung für Windows Hello](https://docs.microsoft.com/windows/keep-secure/microsoft-passport-guide)
- [Windows Hello-Codebeispiel auf GitHub](https://go.microsoft.com/fwlink/?LinkID=717812)

### <a name="62-terminology"></a>6.2 Terminologie

| | |
|-|-|
| AIK | Ein Attestation Identity Key (AIK) liefert einen solchen kryptografischen Nachweis (TPM-Schlüsselnachweis), indem die Eigenschaften des nicht migrierbaren Schlüssel signiert und Eigenschaften und Signatur der vertrauenden Seite zur Überprüfung überlassen werden. Die resultierende Signatur wird als „Nachweisanweisung“ bezeichnet. Da die Signatur mithilfe des privaten AIK-Schlüssels erstellt wird – der nur in dem TPM verwendet werden kann, von dem er erstellt wurde – kann die vertrauende Seite sicher davon ausgehen, dass der nachgewiesene Schlüssel nicht migrierbar ist und außerhalb dieses TPMs nicht verwendet werden kann. |
| AIK-Zertifikat | Ein AIK-Zertifikat wird verwendet, um das Vorhandensein eines AIKs in einem TPM nachzuweisen. Außerdem wird damit nachgewiesen, dass die anderen vom AIK zertifizierten Schlüssel ebenfalls von diesem TPM stammen. |
| IDP | Ein IDP (Identity Provider) ist ein Identitätsanbieter. Ein Beispiel ist der von Microsoft für Microsoft-Konten erstellte IDP. Jedes Mal, wenn eine Anwendung sich mit einem Microsoft-Konto (MSA) authentifizieren muss, kann sie den MSA-IDP aufrufen. |
| PKI | Public Key-Infrastruktur (PKI) bezeichnet üblicherweise eine von einer Organisation gehostete Umgebung, bei der die Organisation selbst für die Erstellung und den Widerruf von Schlüsseln usw. verantwortlich ist. |
| TPM | Mit dem TPM (Trusted Platform Module) können kryptografische Schlüsselpaare bestehend aus einem öffentlichen und privaten Schlüssel so erstellt werden, dass der private Schlüssel außerhalb des TPMs niemals aufgedeckt oder verwendet werden kann (nicht migrierbarer Schlüssel). |
| TPM-Schlüsselnachweis | Ein Protokoll, durch das kryptografisch geprüft wird, ob ein Schlüssel TPM-gebunden ist. Mithilfe dieses Nachweistyps kann gewährleistet werden, dass ein bestimmter kryptografischer Vorgang im TPM eines bestimmten Computers ausgeführt wurde. |

## <a name="related-topics"></a>Verwandte Themen

* [Windows Hello Login-app](microsoft-passport-login.md)
* [Windows Hello-Anmeldedienst](microsoft-passport-login-auth-service.md)