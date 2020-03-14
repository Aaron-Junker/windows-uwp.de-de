---
Description: Sie können das exakte Datum und die exakte Uhrzeit festlegen, an dem bzw. zu der Ihre App im Store verfügbar werden soll. Damit haben Sie mehr Flexibilität und die Möglichkeit, das Datum für verschiedene Märkte anzupassen.
title: Konfigurieren des genauen Veröffentlichungszeitplans
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Zeitplan, Veröffentlichung, Datum, starten
ms.localizationpriority: medium
ms.openlocfilehash: eebd98d8e1ce39ef8d9876ab4749bcc76012f9fa
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210386"
---
# <a name="configure-precise-release-scheduling"></a>Konfigurieren des genauen Veröffentlichungszeitplans

Im Abschnitt **Zeitplan** auf der Seite [Preise und Verfügbarkeit](set-app-pricing-and-availability.md) können Sie das genaue Datum und die genaue Uhrzeit festlegen, an dem bzw. zu der Ihre App im Store verfügbar gemacht werden soll, was Ihnen mehr Flexibilität und das Anpassen des Datums für verschiedene Märkte ermöglicht.

> [!NOTE]
> Obwohl dieses Thema auf Apps verweist, verwendet der Veröffentlichungszeitplan für die Add-On-Übermittlungen das gleiche Verfahren.

Sie können darüber hinaus ein Datum festlegen, ab dem das Produkt nicht mehr im Store verfügbar sein soll. Beachten Sie, dass dies bedeutet, dass das Produkt nicht mehr im Store durch Suchen oder beim Surfen gefunden werden kann. Aber alle Kunden mit einem direkten Link können den Store-Eintrag des Produkts anzeigen. Sie können es nur herunterladen, wenn sie das Produkt bereits besitzen oder einen [Werbecode](generate-promotional-codes.md) haben und ein Windows 10-Gerät verwenden.

Standardmäßig (es sei denn, Sie haben eine der Optionen für **Make this app available but not discoverable in the Store** im Abschnitt [Sichtbarkeit](choose-visibility-options.md#discoverability) ausgewählt) wird Ihre App für Kunden zur Verfügung gestellt, sobald sie die Zertifizierung bestanden und den Veröffentlichungsprozess abgeschlossen hat. Um ein anderes Datum auszuwählen, wählen Sie **Optionen anzeigen**, um diesen Abschnitt zu erweitern.

Beachten Sie, dass Sie Datumsangaben im Abschnitt **Zeitplan** nicht konfigurieren können, wenn Sie eine der Optionen für **Make this app available but not discoverable in the Store** im Abschnitt [Sichtbarkeit](choose-visibility-options.md#discoverability) ausgewählt haben, da ihre App nicht für Kunden veröffentlicht wird. Daher muss kein Veröffentlichungsdatum konfiguriert werden.

> [!IMPORTANT]
> Das im Abschnitt "Zeitplan" angegebene Datum gilt nur für Kunden unter Windows 10.
>
>Wenn Ihre zuvor veröffentlichte App frühere Betriebssystemversionen unterstützt, werden alle von Ihnen ausgewählten **Erfassungs** Datumsangaben nicht für diese Kunden angewendet. Sie können die APP weiterhin abrufen (es sei denn, Sie übermitteln ein Update mit einer neuen Auswahl im Bereich " [Sichtbarkeit](choose-visibility-options.md#discoverability) ", oder wenn Sie auf der Seite " **App-Übersicht** " die Option "App nicht **verfügbar** " auswählen).

## <a name="base-schedule"></a>Basiszeitplan

Die für den Basiszeitplan getroffene Auswahl gilt für alle Märkte, in denen Ihre App verfügbar ist, es sei denn, Sie fügen später ein Datum für bestimmte Märkte (oder Marktgruppen) durch die Auswahl von [Customize for specific markets](#customize-the-schedule-for-specific-markets) hinzu.

Sie sehen hier zwei Optionen: **Release** und **Stop acquisition**. 

## <a name="release"></a>Version

In der Dropdownliste **Release** können Sie festlegen, wann Ihre App im Store verfügbar sein soll. Dies bedeutet, dass die App im Store durch Suchen oder beim Surfen gefunden werden kann und dass Kunden den Store-Eintrag anzeigen und die App erwerben können.

>[!NOTE]
> Nachdem Ihre App veröffentlicht und im Store verfügbar gemacht wurde, können Sie kein Datum für **Release** mehr auswählen (da die App bereits veröffentlicht worden ist).

Hier sind die Optionen, die Sie für den **Releasezeitplan** eines Produkts konfigurieren können:
- **so bald wie möglich**: Das Produkt wird sofort nach der Zertifizierung und Veröffentlichung verfügbar gemacht. Dies ist die Standardoption.
- **at**: Das Produkt wird am ausgewählten Datum und zur ausgewählten Uhrzeit verfügbar gemacht. Ihnen stehen zwei zusätzliche Optionen zur Auswahl:
   - **UTC**: Die ausgewählte Zeit ist UTC-Zeit (Universal Coordinated Time, Koordinierte Weltzeit), damit die App überall zur gleichen Zeit verfügbar gemacht wird.
   - **Lokal**: Die ausgewählte Zeit ist die in jeder Zeitzone für einen Markt verwendete Uhrzeit. (Beachten Sie, dass für Märkte, die mehr als eine Zeitzone umfassen, nur eine Zeitzone in diesem Markt verwendet wird. Für den USA wird die Zeitzone Eastern verwendet. Eine umfassende Liste der Zeitzonen wird weiter unten auf dieser Seite angezeigt.)
- **nicht geplant**: Die App wird nicht im Store verfügbar sein. Wenn Sie diese Option auswählen, können Sie die App im Store später zur Verfügung stellen, indem Sie eine neue Übermittlung erstellen und eine der anderen Optionen auswählen.

## <a name="stop-acquisition"></a>Beenden des Erwerbs

Im Dropdownmenü **Stop acquisition** können Sie ein Datum und eine Uhrzeit festlegen, an dem bzw. zu der neue Kunden die App nicht mehr im Store erwerben oder ihren Eintrag anzeigen dürfen. Dies kann nützlich sein, wenn Sie präzise steuern möchten, wann eine App neuen Kunden nicht mehr angeboten wird, z. B. wenn Sie die Verfügbarkeit zwischen mehr als einer Ihrer Apps koordinieren.

Die Standardeinstellung für **Stop acquisition** ist "Never". Wählen Sie zum Ändern in der Dropdownliste **at** aus, und geben Sie wie oben beschrieben Datum und Uhrzeit ein. Am ausgewählten Datum und zur ausgewählten Uhrzeit können Kunden die App nicht mehr erwerben.

Es ist wichtig zu verstehen, dass diese Option die gleichen Auswirkungen hat wie das auswählen, dass **diese APP erkennbar** ist, aber im [Sichtbarkeits](choose-visibility-options.md#discoverability) Abschnitt nicht verfügbar ist, und die Auswahl von **Übernahme aufheben: jeder Kunde mit einem direkten Link kann die Store-Auflistung des Produkts anzeigen, aber Sie kann nur heruntergeladen werden, wenn Sie zuvor das Produkt besaßen, oder ein Windows 10-Gerät verwendet wird.** Wenn Sie eine App überhaupt nicht mehr für Kunden anbieten möchten, klicken Sie in der App-Übersicht auf **Make app unavailable**. Weitere Informationen finden Sie unter [Entfernen einer App aus dem Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Wenn Sie ein Datum auswählen, um **den Erwerb zu beenden**, und später feststellen, dass Sie die App wieder verfügbar machen möchten, können Sie eine neue Übermittlung erstellen und für **Stop acquisition** wieder **Never** festlegen. Die App wird wieder verfügbar, nachdem Ihre aktualisierte Übermittlung veröffentlicht wurde.

## <a name="customize-the-schedule-for-specific-markets"></a>Anpassen des Zeitplans für bestimmte Märkte 

Die oben ausgewählten Optionen gelten standardmäßig für alle Märkte, in denen Ihre App angeboten wird. Wenn Sie den Preis für die einzelnen Märkte anpassen möchten, klicken Sie auf **Customize for specific markets**. Das Popupfenster **Market selection** wird mit allen Märkten angezeigt, in denen Sie Ihre App verfügbar machen möchten. Wenn Sie Märkte im Abschnitt [Märkte](define-pricing-and-market-selection.md) ausgeschlossen haben, werden diese Märkte hier nicht angezeigt. 

Um einen Zeitplan für einen Markt hinzuzufügen, wählen Sie ihn aus, und klicken Sie auf **Speichern**. Sie sehen dann die gleichen Optionen **Release** und **Stop acquisition** wie oben beschrieben, aber die getroffene Auswahl wird nur auf diesen Markt angewendet.

Um einen Zeitplan hinzuzufügen, der für mehrere Märkte gilt, erstellen Sie eine *Marktgruppe*. Dazu wählen Sie die Märkte aus, die Sie aufnehmen möchten, und geben einen Namen für die Gruppe ein. (Dieser Name dient nur zu Referenzzwecken und ist nicht für alle Kunden sichtbar.) Beispiel: Wenn Sie eine Marktgruppe für Nordamerika erstellen möchten, können Sie **Kanada**, **Mexiko** und **USA** auswählen und sie **Nordamerika** nennen oder einen anderen Namen auswählen. Wenn Sie Ihre Marktgruppe erstellt haben, klicken Sie auf **Speichern**. Sie sehen dann die gleichen Optionen **Release** und **Stop acquisition** wie oben beschrieben, aber die getroffene Auswahl wird nur auf diese Marktgruppe angewendet.

Um einen benutzerdefinierten Zeitplan für einen zusätzlichen Markt oder eine weitere Marktgruppe hinzuzufügen, klicken Sie einfach erneut auf **Customize for specific markets**, und wiederholen Sie diese Schritte. Wählen Sie zum Ändern der in einer Marktgruppe enthaltenen Märkte den Namen aus. Um den benutzerdefinierten Zeitplan für eine Marktgruppe (oder einen einzelnen Markt) zu entfernen, klicken Sie auf **Entfernen**.

> [!NOTE]
> Ein Markt kann nicht zu mehr als einer der Marktgruppen gehören, die Sie im Abschnitt **Zeitplan** verwenden. 

## <a name="global-time-zones"></a>Globale Zeitzonen

Im folgenden finden Sie eine Tabelle, in der gezeigt wird, welche bestimmten Zeitzonen auf den einzelnen Märkten verwendet werden. wenn ihre Übermittlung z. b. die lokale Zeit (z. b. das Release um 1Am lokal) verwendet, können Sie herausfinden, welche Zeit auf den einzelnen Märkten veröffentlicht wird. Dies ist besonders hilfreich bei Märkten, die mehr als einmal z haben. eines, z. b. Kanada.

| Markt | Zeitzone |
|--------|-----------|
| Afghanistan  |  (UTC + 04:30) Uler |
| Albanien  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Algerien  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Amerikanisch-Samoa  |  (UTC + 13:00) Samoa |
| Andorra  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Angola  |  (UTC + 01:00) West-Zentralafrika |
| Anguilla  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Antarktis  |  (UTC + 12:00) Auckland, Wellington |
| Antigua und Barbuda  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Argentinien  |  (UTC-03:00) Stadt Buenos Aires |
| Armenien  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Aruba  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Australien  |  (UTC + 10:00) Canberra, Melbourne, Sydney |
| Österreich  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Aserbaidschan  |  (UTC + 04:00) Baku |
| Bahamas  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Bahrain  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Bangladesch  |  (UTC + 06:00) Kas |
| Barbados  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Belarus  |  (UTC + 03:00) Minsk |
| Belgien  |  (UTC + 01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Belize  |  (UTC-06:00) Zentralzeit (USA & Kanada) |
| Benin  |  (UTC + 01:00) West-Zentralafrika |
| Bermuda  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Bhutan  |  (UTC + 06:00) Kas |
| Bolivarische Republik Venezuela  |  (UTC-04:00) Verfolgt |
| Bolivien  |  (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Bonaire, St. Eustatius und Saba  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Bosnien und Herzegowina  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Botsuana  |  (UTC + 01:00) West-Zentralafrika |
| Bouvetinsel  |  (UTC + 00:00) Monrovia, Reykjavik |
| Brasilien  |  (UTC-03:00) Brasiliens |
| Britisches Territorium im Indischen Ozean  |  (UTC + 06:00) Kas |
| Britische Jungferninseln  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Brunei  |  (UTC + 08:00) Irkutsk |
| Bulgarien  |  (UTC + 02:00) Chisinau |
| Burkina Faso  |  (UTC + 00:00) Monrovia, Reykjavik |
| Burundi  |  (UTC + 02:00) Harare, Pretoria |
| CÃ ́te Ivoire  |  (UTC + 00:00) Monrovia, Reykjavik |
| Kambodscha  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Kamerun  |  (UTC + 01:00) West-Zentralafrika |
| Kanada  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Republik Cabo Verde  |  (UTC-01:00) Cabo Verde ist. |
| Kaimaninseln  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Zentralafrikanische Republik  |  (UTC + 01:00) West-Zentralafrika |
| Tschad  |  (UTC + 01:00) West-Zentralafrika |
| Chile  |  (UTC-04:00) Santiago |
| China  |  (UTC + 08:00) Peking, Chongqing, Hongkong, urumschlag |
| Weihnachtsinsel  |  (UTC + 07:00) Krasnojarsk |
| Kokosinseln  |  (UTC + 06:30) Yangon (Rangoon) |
| Kolumbien  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Komoren  |  (UTC + 03:00) Hauptsitz |
| Kongo  |  (UTC + 01:00) West-Zentralafrika |
| Kongo, Demokratische Republik  |  (UTC + 01:00) West-Zentralafrika |
| Cookinseln  |  (UTC-10:00) IRONMAN |
| Costa Rica  |  (UTC-06:00) Zentralzeit (USA & Kanada) |
| Kroatien  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Curraã, AO  |  (UTC-04:00) Cuiabá |
| Zypern  |  (UTC + 02:00) Chisinau |
| Tschechische Republik  |  (UTC + 01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Dänemark  |  (UTC + 01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Dschibuti  |  (UTC + 03:00) Hauptsitz |
| Dominica  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Dominikanische Republik  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Ecuador  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Ägypten  |  (UTC + 02:00) Chisinau |
| El Salvador  |  (UTC-06:00) Zentralzeit (USA & Kanada) |
| Äquatorialguinea  |  (UTC + 01:00) West-Zentralafrika |
| Eritrea  |  (UTC + 03:00) Hauptsitz |
| Estland  |  (UTC + 02:00) Chisinau |
| Äthiopien  |  (UTC + 03:00) Hauptsitz |
| Falklandinseln (Malwinen)  |  (UTC-04:00) Santiago |
| Färöer  |  (UTC + 00:00) Dublin, Edinburgh, Lissabon, London |
| Fidschi  |  (UTC + 12:00) Abschloss |
| Finnland  |  (UTC + 02:00) Helsinki, Kiew, Riga, Sofia, Tallinn, Vilnius |
| France  |  (UTC + 01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Französisch-Guyana  |  (UTC-03:00) Cayenne, Fortaleza |
| Französisch-Polynesien  |  (UTC-10:00) IRONMAN |
| Französische Süd- und Antarktisgebiete  |  (UTC + 05:00) Ashgabat, Tashkent |
| Gabun  |  (UTC + 01:00) West-Zentralafrika |
| Gambia  |  (UTC + 00:00) Monrovia, Reykjavik |
| Georgia  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Germany  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Ghana  |  (UTC + 00:00) Monrovia, Reykjavik |
| Gibraltar  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Griechenland  |  (UTC + 02:00) Athen, Bukarest |
| Grönland  |  (UTC + 00:00) Monrovia, Reykjavik |
| Grenada  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Guadeloupe  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Guam  |  (UTC + 10:00) Guam, Port Moresby |
| Guatemala  |  (UTC-06:00) Zentralzeit (USA & Kanada) |
| Guernsey  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guinea  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guinea-Bissau  |  (UTC + 00:00) Monrovia, Reykjavik |
| Guyana  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Haiti  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Heard und McDonaldinseln  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Heiliger Stuhl (Vatikanstadt)  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Honduras  |  (UTC-06:00) Zentralzeit (USA & Kanada) |
| Hongkong (SAR)  |  (UTC + 08:00) Peking, Chongqing, Hongkong, urumschlag |
| Ungarn  |  (UTC + 01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Island  |  (UTC + 00:00) Monrovia, Reykjavik |
| Indien  |  (UTC + 05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Indonesien  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Irak  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Irland  |  (UTC + 00:00) Dublin, Edinburgh, Lissabon, London |
| Israel  |  (UTC + 02:00) Emer |
| Italien  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Jamaika  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Japan  |  (UTC + 09:00) Osaka, Sapporo, Tokio |
| Jersey  |  (UTC + 00:00) Monrovia, Reykjavik |
| Jordanien  |  (UTC + 02:00) Chisinau |
| Kasachstan  |  (UTC + 05:00) Ashgabat, Tashkent |
| Kenia  |  (UTC + 03:00) Hauptsitz |
| Kiribati  |  (UTC + 14:00) Kiritimati-Insel |
| Korea  |  (UTC + 09:00) Seoul |
| Kuwait  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Kirgisistan  |  (UTC + 06:00) Astana |
| Laos  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Lettland  |  (UTC + 02:00) Chisinau |
| Libanon  |  (UTC + 02:00) Chisinau |
| Lesotho  |  (UTC + 02:00) Harare, Pretoria |
| Liberia  |  (UTC + 00:00) Monrovia, Reykjavik |
| Libyen  |  (UTC + 02:00) Chisinau |
| Liechtenstein  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Litauen  |  (UTC + 02:00) Chisinau |
| Luxemburg  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Macau SAR  |  (UTC + 08:00) Peking, Chongqing, Hongkong, urumschlag |
| Mazedonien (ehem. jugoslawische Republik Mazedonien)  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Madagaskar  |  (UTC + 03:00) Hauptsitz |
| Malawi  |  (UTC + 02:00) Harare, Pretoria |
| Malaysia  |  (UTC + 08:00) Kuala Lumpur, Singapur |
| Malediven  |  (UTC + 05:00) Ashgabat, Tashkent |
| Mali  |  (UTC + 00:00) Monrovia, Reykjavik |
| Malta  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Man, Isle of  |  (UTC + 00:00) Dublin, Edinburgh, Lissabon, London |
| Marshallinseln  |  (UTC + 12:00) Petropawlowsk-Kamchatsky-Old |
| Martinique  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Mauretanien  |  (UTC + 00:00) Monrovia, Reykjavik |
| Mauritius  |  (UTC + 04:00) Port Louis |
| Mayotte  |  (UTC + 03:00) Hauptsitz |
| Mexiko  |  (UTC-06:00) Guadalajara, Mexiko-Stadt, Monterrey |
| Mikronesien  |  (UTC + 10:00) Guam, Port Moresby |
| Republik Moldau  |  (UTC + 02:00) Chisinau |
| Monaco  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Mongolei  |  (UTC + 07:00) Krasnojarsk |
| Montenegro  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Montserrat  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Marokko  |  (UTC + 01:00) Casablanca |
| Mosambik  |  (UTC + 02:00) Harare, Pretoria |
| Myanmar  |  (UTC + 06:30) Yangon (Rangoon) |
| Namibia  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Nauru  |  (UTC + 12:00) Petropawlowsk-Kamchatsky-Old |
| Nepal  |  (UTC + 05:45) Katmandu |
| Niederlande  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Neukaledonien  |  (UTC + 11:00) Solomon ist., New Caledonia |
| Neuseeland  |  (UTC + 12:00) Auckland, Wellington |
| Nicaragua  |  (UTC-06:00) Zentralzeit (USA & Kanada) |
| Niger  |  (UTC + 01:00) West-Zentralafrika |
| Nigeria  |  (UTC + 01:00) West-Zentralafrika |
| Niue  |  (UTC + 13:00) Samoa |
| Norfolkinsel  |  (UTC + 11:00) Solomon ist., New Caledonia |
| Nördliche Marianen  |  (UTC + 10:00) Guam, Port Moresby |
| Norwegen  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Oman  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Pakistan  |  (UTC + 05:00) Islamabad, Karatschi |
| Palau  |  (UTC + 09:00) Osaka, Sapporo, Tokio |
| Palästinensische Behörde  |  (UTC + 02:00) Chisinau |
| Panama  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Papua-Neuguinea  |  (UTC + 10:00) Wladiwostok |
| Paraguay  |  (UTC-04:00) Asunción |
| Peru  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Philippinen  |  (UTC + 08:00) Kuala Lumpur, Singapur |
| Pitcairninseln  |  (UTC-08:00) Pacific Time (USA und Canada) |
| Polen  |  (UTC + 01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Portugal  |  (UTC + 00:00) Dublin, Edinburgh, Lissabon, London |
| Katar  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Réunion  |  (UTC + 04:00) Port Louis |
| Rumänien  |  (UTC + 02:00) Chisinau |
| Zeile  |  (UTC-07:00) Mountain Time (USA & Kanada) |
| Russische Föderation  |  (UTC + 03:00) Moskau, St. Petersburg |
| Ruanda  |  (UTC + 02:00) Harare, Pretoria |
| SÃ £ o TomÃ © und prãncipe  |  (UTC + 00:00) Monrovia, Reykjavik |
| St. BarthÃ © lemy  |  (UTC + 04:00) Eriwan |
| St. Helena, Ascension und Tristan da Cunha  |  (UTC + 00:00) Dublin, Edinburgh, Lissabon, London |
| St. Kitts und Nevis  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| St. Lucia  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Saint-Martin (französischer Teil)  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| St. Pierre und Miquelon  |  (UTC-02:00) Mid-Atlantik-alt |
| St. Vincent und die Grenadinen  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Samoa  |  (UTC + 13:00) Samoa |
| San Marino  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Saudi-Arabien  |  (UTC + 03:00) Kuwait, Riad |
| Senegal  |  (UTC + 00:00) Monrovia, Reykjavik |
| Serbien  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Seychellen  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Sierra Leone  |  (UTC + 00:00) Monrovia, Reykjavik |
| Singapur  |  (UTC + 08:00) Kuala Lumpur, Singapur |
| St. Maarten (niederländischer Teil)  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Slowakei  |  (UTC + 01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Slowenien  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Salomonen  |  (UTC + 11:00) Solomon ist., New Caledonia |
| Somalia  |  (UTC + 03:00) Hauptsitz |
| Südafrika  |  (UTC + 02:00) Harare, Pretoria |
| Südgeorgien und die Südlichen Sandwichinseln  |  (UTC-02:00) Mid-Atlantik-alt |
| Spanien  |  (UTC + 01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Sri Lanka  |  (UTC + 05:30) Chennai, Kolkata, Mumbai, New Delhi |
| Suriname  |  (UTC-03:00) Cayenne, Fortaleza |
| Svalbard und Jan Mayen  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Swasiland  |  (UTC + 02:00) Harare, Pretoria |
| Schweden  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Schweiz  |  (UTC + 01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Taiwan  |  (UTC + 08:00) Provinz |
| Tadschikistan  |  (UTC + 05:00) Ashgabat, Tashkent |
| Tansania  |  (UTC + 03:00) Hauptsitz |
| Thailand  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Timor-Leste  |  (UTC + 09:00) Seoul |
| Togo  |  (UTC + 00:00) Monrovia, Reykjavik |
| Tokelau  |  (UTC + 13:00) Nuku ' alofa |
| Tonga  |  (UTC + 13:00) Nuku ' alofa |
| Trinidad und Tobago  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Tunesien  |  (UTC + 01:00) Sarajevo, Skopje, Warschau, Zagreb |
| Türkei  |  (UTC + 03:00) Bul |
| Turkmenistan  |  (UTC + 05:00) Ashgabat, Tashkent |
| Turks- und Caicosinseln  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Tuvalu  |  (UTC + 12:00) Petropawlowsk-Kamchatsky-Old |
| Kleinere Amerikanische Überseeinseln  |  (UTC + 13:00) Samoa |
| Amerikanische Jungferninseln  |  (UTC-04:00) Atlantik Zeit (Kanada) |
| Uganda  |  (UTC + 03:00) Hauptsitz |
| Ukraine  |  (UTC + 02:00) Chisinau |
| Vereinigte Arabische Emirate  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Großbritannien  |  (UTC + 00:00) Dublin, Edinburgh, Lissabon, London |
| USA  |  (UTC-05:00) Eastern Time (USA & Kanada) |
| Uruguay  |  (UTC-03:00) Brasiliens |
| Usbekistan  |  (UTC + 05:00) Ashgabat, Tashkent |
| Vanuatu  |  (UTC + 11:00) Solomon ist., New Caledonia |
| Vietnam  |  (UTC + 07:00) Bangkok, Hanoi, Jakarta |
| Wallis und Futuna  |  (UTC + 12:00) Petropawlowsk-Kamchatsky-Old |
| Westliche Sahara (umstritten)  |  (UTC + 00:00) Dublin, Edinburgh, Lissabon, London |
| Jemen  |  (UTC + 04:00) Abu Dhabi, Muskat |
| Sambia  |  (UTC + 02:00) Harare, Pretoria |
| Simbabwe  |  (UTC + 02:00) Harare, Pretoria |
