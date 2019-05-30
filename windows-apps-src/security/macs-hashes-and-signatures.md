---
title: MACs, Hashes und Signaturen
description: In diesem Thema wird erläutert, wie Nachrichtenauthentifizierungscodes (MACs), Hashes und Signaturen in UWP-Apps verwendet werden können, um die Manipulation von Nachrichten zu erkennen.
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: 78b14023f61dd3f8c27bc31f5876407ff0ed0366
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371204"
---
# <a name="macs-hashes-and-signatures"></a>MACs, Hashes und Signaturen




In diesem Thema wird erläutert, wie Nachrichtenauthentifizierungscodes (MACs), Hashes und Signaturen in UWP-Apps verwendet werden können, um die Manipulation von Nachrichten zu erkennen.

## <a name="message-authentication-codes-macs"></a>Nachrichtenauthentifizierungscodes (MACs)


Die Verschlüsselung hilft dabei, nicht autorisierte Personen davon abzuhalten, eine Nachricht zu lesen, kann jedoch nichts dagegen ausrichten, wenn Personen die Nachricht manipulieren. Eine so veränderte Nachricht kann Kosten verursachen, selbst wenn die Veränderung keinen echten Inhalt vermittelt. Ein Nachrichtenauthentifizierungscode (Message Authentication Code, MAC) kann die Manipulation von Nachrichten verhindern. Nehmen wir folgendes Beispiel:

-   Sven und Andrea geben einander einen geheimen Schlüssel weiter und einigen sich darauf, die MAC-Funktion zu nutzen.
-   Sven schreibt eine Nachricht und gibt sie zusammen mit dem geheimen Schlüssel in eine MAC-Funktion ein, um einen MAC-Wert zu erhalten.
-   Bob sendet die \[unverschlüsselte\] Meldung und dem MAC Wert Alice über ein Netzwerk.
-   Andrea gibt den geheimen Schlüssel und die Nachricht ebenfalls in eine MAC-Funktion ein. Sie vergleicht den MAC-Wert, den sie erhält, mit dem MAC-Wert, den Sie von Sven erhalten hat. Stimmen die Werte überein, wurde die Nachricht beim Verwenden nicht manipuliert.

Beachten Sie, dass Eve, die den Austausch zwischen Sven und Andrea heimlich mitverfolgt, die Nachricht so nicht manipulieren kann. Eve hat keinen Zugriff auf den privaten Schlüssel und kann daher keinen MAC-Wert generieren, der die manipulierte Nachricht bei Andrea als unverändert erscheinen lassen könnte.

Die Erstellung eines MAC-Werts gewährleistet somit, dass die ursprüngliche Nachricht nicht geändert wurde. Zudem beweist der Einsatz eines freigegebenen geheimen Schlüssels, dass der Nachrichtenhash von jemandem signiert wurde, der Zugriff auf den privaten Schlüssel hatte.

Sie können [**MacAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.MacAlgorithmProvider) zur Enumerierung der verfügbaren MAC-Algorithmen und zur Generierung eines symmetrischen Schlüssels verwenden. Die statischen Methoden der [**CryptographicEngine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine)-Klasse können zudem zur Durchführung der erforderlichen Verschlüsselung genutzt werden, durch den Sie den MAC-Wert erhalten.

Digitale Signaturen von öffentlichen Schlüsseln sind das Äquivalent der Nachrichtenauthentifizierungscodes (MACs) von privaten Schlüsseln. Obwohl MACs private Schlüssel verwenden, um dem Nachrichtenempfänger zu ermöglichen, Nachrichten zu überprüfen und sicherzugehen, dass diese bei der Übertragung nicht verändert wurden, verwenden Signaturen ein Schlüsselpaar aus einem privaten und einem öffentlichen Schlüssel.

In diesem Beispielcode ist die Verwendung der [**MacAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.MacAlgorithmProvider)-Klasse zum Erstellen eines Nachrichtenauthentifizierungscodes mit Hash (Hashed Message Authentication Code, HMAC) dargestellt.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Storage.Streams;

namespace SampleMacAlgorithmProvider
{
    sealed partial class MacAlgProviderApp : Application
    {
        public MacAlgProviderApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Initialize the hashing process.
            String strMsg = "This is a message to be authenticated";
            String strAlgName = MacAlgorithmNames.HmacSha384;
            IBuffer buffMsg;
            CryptographicKey hmacKey;
            IBuffer buffHMAC;

            // Create a hashed message authentication code (HMAC)
            this.CreateHMAC(
                strMsg,
                strAlgName,
                out buffMsg,
                out hmacKey,
                out buffHMAC);

            // Verify the HMAC.
            this.VerifyHMAC(
                buffMsg,
                hmacKey,
                buffHMAC);
        }

        void CreateHMAC(
            String strMsg,
            String strAlgName,
            out IBuffer buffMsg,
            out CryptographicKey hmacKey,
            out IBuffer buffHMAC)
        {
            // Create a MacAlgorithmProvider object for the specified algorithm.
            MacAlgorithmProvider objMacProv = MacAlgorithmProvider.OpenAlgorithm(strAlgName);

            // Demonstrate how to retrieve the name of the algorithm used.
            String strNameUsed = objMacProv.AlgorithmName;

            // Create a buffer that contains the message to be signed.
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
            buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a key to be signed with the message.
            IBuffer buffKeyMaterial = CryptographicBuffer.GenerateRandom(objMacProv.MacLength);
            hmacKey = objMacProv.CreateKey(buffKeyMaterial);

            // Sign the key and message together.
            buffHMAC = CryptographicEngine.Sign(hmacKey, buffMsg);

            // Verify that the HMAC length is correct for the selected algorithm
            if (buffHMAC.Length != objMacProv.MacLength)
            {
                throw new Exception("Error computing digest");
            }
         }

        public void VerifyHMAC(
            IBuffer buffMsg,
            CryptographicKey hmacKey,
            IBuffer buffHMAC)
        {
            // The input key must be securely shared between the sender of the HMAC and 
            // the recipient. The recipient uses the CryptographicEngine.VerifySignature() 
            // method as follows to verify that the message has not been altered in transit.
            Boolean IsAuthenticated = CryptographicEngine.VerifySignature(hmacKey, buffMsg, buffHMAC);
            if (!IsAuthenticated)
            {
                throw new Exception("The message cannot be verified.");
            }
        }
    }
}
```

## <a name="hashes"></a>Hashes


Eine kryptografische Hashfunktion gibt für einen an sie übergebenen Datenblock beliebiger Länge eine Bitzeichenfolge fester Größe zurück. Hashfunktionen werden normalerweise zum Signieren von Daten verwendet. Da die meisten Signaturvorgänge für öffentliche Schlüssel rechtenintensiv sind, ist das Signieren eines Nachrichtenhashes in der Regel effizienter als das Signieren der ursprünglichen Nachricht. Im folgenden Verfahren wird ein gängiges Szenario vorgestellt, das für diese Zwecke aber vereinfacht wurde:

-   Sven und Andrea geben einander einen geheimen Schlüssel weiter und einigen sich darauf, die MAC-Funktion zu nutzen.
-   Sven schreibt eine Nachricht und gibt sie zusammen mit dem geheimen Schlüssel in eine MAC-Funktion ein, um einen MAC-Wert zu erhalten.
-   Bob sendet die \[unverschlüsselte\] Meldung und dem MAC Wert Alice über ein Netzwerk.
-   Andrea gibt den geheimen Schlüssel und die Nachricht ebenfalls in eine MAC-Funktion ein. Sie vergleicht den MAC-Wert, den sie erhält, mit dem MAC-Wert, den Sie von Sven erhalten hat. Stimmen die Werte überein, wurde die Nachricht beim Verwenden nicht manipuliert.

Beachten Sie, dass Andrea eine unverschlüsselte Nachricht gesendet hat. Nur der Hash war verschlüsselt. Durch das Verfahren wird nur sichergestellt, dass die ursprüngliche Nachricht nicht verändert wurde und – durch die Verwendung von Andreas öffentlichem Schlüssel – dass der Nachrichtenhash von jemandem mit Zugriff auf Andreas privaten Schlüssel signiert wurde (wahrscheinlich von Andrea).

Sie können die [**HashAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider)-Klasse verwenden, um die verfügbaren Hashalgorithmen aufzulisten und einen [**CryptographicHash**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash)-Wert zu erstellen.

Digitale Signaturen von öffentlichen Schlüsseln sind das Äquivalent der Nachrichtenauthentifizierungscodes (MACs) von privaten Schlüsseln. Während MACs private Schlüssel verwenden, um einem Nachrichtenempfänger das Überprüfen der Nachrichtenintegrität zu ermöglichen, verwenden Signaturen ein privates/öffentliches Schlüsselpaar.

Mit dem [**CryptographicHash**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash)-Objekt kann ein Hash mehrmals auf unterschiedliche Daten angewendet werden, ohne dass das Objekt jedes Mal neu erstellt werden muss. Die [**Append**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.core.cryptographichash.append)-Methode fügt einem Puffer, auf den ein Hash angewendet werden soll, neue Daten hinzu. Die [**GetValueAndReset**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.core.cryptographichash.getvalueandreset)-Methode wendet den Hash auf die Daten an und setzt das Objekt zur weiteren Verwendung zurück. Dies wird im folgenden Beispiel gezeigt.

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the hashing algorithm to use.
    String strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    String strMsg1 = "This is message 1.";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    String strMsg2 = "This is message 2.";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Hash message 3.
    String strMsg3 = "This is message 3.";
    IBuffer buffMsg3 = CryptographicBuffer.ConvertStringToBinary(strMsg3, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg3);
    IBuffer buffHash3 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    String strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    String strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
    String strHash3 = CryptographicBuffer.EncodeToBase64String(buffHash3);
}

```

## <a name="digital-signatures"></a>Digitale Signaturen


Digitale Signaturen von öffentlichen Schlüsseln sind das Äquivalent der Nachrichtenauthentifizierungscodes (MACs) von privaten Schlüsseln. Während MACs private Schlüssel verwenden, um einem Nachrichtenempfänger das Überprüfen der Nachrichtenintegrität zu ermöglichen, verwenden Signaturen ein privates/öffentliches Schlüsselpaar.

Da die meisten Signaturvorgänge für öffentliche Schlüssel rechtenintensiv sind, ist das Signieren eines Nachrichtenhashes in der Regel aber effizienter als das Signieren der ursprünglichen Nachricht. Der Absender erstellt einen Nachrichtenhash, signiert ihn und sendet sowohl die Signatur als auch die (unverschlüsselte) Nachricht. Der Empfänger berechnet einen Hash für die Nachricht, entschlüsselt die Signatur und vergleicht die entschlüsselte Signatur mit dem Hashwert. Stimmen sie überein, kann der Empfänger relativ sicher sein, dass die Nachricht tatsächlich vom Absender stammt und während der Übertragung nicht manipuliert wurde.

Durch das Signieren wird nur sichergestellt, dass die ursprüngliche Nachricht nicht verändert wurde und – durch die Verwendung des öffentlichen Schlüssels des Absenders – dass der Nachrichtenhash von jemandem mit Zugriff auf den privaten Schlüssel signiert wurde.

Sie können ein [**AsymmetricKeyAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.AsymmetricKeyAlgorithmProvider)-Objekt verwenden, um die verfügbaren Signaturalgorithmen aufzulisten und ein Schlüsselpaar zu generieren oder zu importieren. Sie können statische Methoden der [**CryptographicHash**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash)-Klasse zum Signieren einer Nachricht oder Überprüfen einer Signatur verwenden.