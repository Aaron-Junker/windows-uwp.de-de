---
title: Vorbereiten Ihrer Anwendung auf den Wechsel der japanischen Ära
description: Hier erhalten Sie Informationen zum im Mai 2019 anstehenden Wechsel der japanischen Ära, und wie Sie Ihre Anwendung darauf entsprechend vorbereiten können.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 04/26/2019
ms.topic: article
keywords: Windows 10, UWP, Lokalisierbarkeit, Lokalisierung, japanisch, Ära
ms.localizationpriority: high
ms.openlocfilehash: 367dbf3b3ea1ef5f2853820f617f3a484ef863cf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174434"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>Vorbereiten Ihrer Anwendung auf den Wechsel der japanischen Ära

> [!NOTE]
> Am 1. April 2019 wurde der Name der neuen Ära verkündet: Reiwa (令和). Am 25. April veröffentlichte Microsoft Pakete für verschiedene Windows-Betriebssysteme mit dem aktualisierten Registrierungsschlüssel mit dem Namen der neuen Ära. Aktualisieren Sie Ihr Gerät, und überprüfen Sie die Registrierung, um festzustellen, ob sie den neuen Schlüssel aufweist: Testen Sie anschließend Ihre Anwendung. Sehen Sie [diesen Supportartikel](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068) ein, um sich zu vergewissern, dass Ihr Betriebssystem den aktualisierten Registrierungsschlüssel empfangen haben sollte.

Der japanische Kalender ist in Ären unterteilt, und für den Großteil des modernen Computerzeitalters haben wir uns in der Heisei-Ära befunden. Am 1. Mai 2019 beginnt jedoch eine neue Ära. Da dies das erste Mal seit Jahrzehnten ist, dass ein Wechsel der Ären stattfindet, muss Software, die den japanischen Kalender unterstützt, getestet werden, um sicherzustellen, dass sie zu Beginn der neuen Ära ordnungsgemäß funktioniert.

In den folgenden Abschnitten erfahren Sie, was Sie tun können, um Ihre Anwendung auf die neue Ära vorzubereiten und zu testen.

> [!NOTE]
> Wir empfehlen, hierfür eine Testmaschine zu verwenden, da die von Ihnen vorgenommenen Änderungen das Verhalten des gesamten Computers beeinflussen.

## <a name="add-a-registry-key-for-the-new-era"></a>Hinzufügen eines Registrierungsschlüssels für die neue Ära

> [!NOTE]
> Die folgenden Anweisungen beziehen sich auf Geräte, die noch nicht mit dem neuen Registrierungsschlüssel aktualisiert wurden. Überprüfen Sie zunächst, ob Ihr Gerät den neuen Registrierungsschlüssel enthält. Falls dies nicht der Fall ist, führen Sie entsprechend den folgenden Anweisungen einen Test aus.

Es ist wichtig, auf Kompatibilitätsprobleme zu testen, bevor der Ärawechsel stattgefunden hat. Dies können Sie nun mithilfe des Namens der neuen Ära vornehmen. Fügen Sie dazu mithilfe des **Registrierungs-Editors** einen Registrierungsschlüssel für die neue Ära hinzu:

1. Navigieren Sie zu **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**.
2. Wählen Sie **Bearbeiten > Neu > Zeichenfolge** aus, und weisen Sie ihr den Namen **2019 05 01** zu.
3. Klicken Sie mit der rechten Maustaste auf den Schlüssel, und wählen Sie **Ändern** aus.
4. Geben Sie im Feld **Wertdaten** den Wert **令和_令_Reiwa_R** ein (Sie können den Wert der Einfachheit halber von hier kopieren und einfügen).

Weitere Informationen finden Sie unter [Umgang mit Ären im japanischen Kalender](/windows/desktop/Intl/era-handling-for-the-japanese-calendar), wo Sie weitere Einzelheiten über das Format für diese Registrierungsschlüssel erhalten.

Am 1. April 2019 wurde der Name der neuen Ära verkündet. Am 25. April wurde ein Update mit einem neuen Registrierungsschlüssel für unterstützte Windows-Versionen mit dem Namen veröffentlicht. Dies ermöglicht es Ihnen zu überprüfen, ob er von Ihrer Anwendung ordnungsgemäß verarbeitet wird. Dieses Update wird für unterstützte frühere Versionen von Windows 10 sowie Windows 8 und 7 bereitgestellt.

Nachdem Sie das Testen Ihrer Anwendung abgeschlossen haben, können Sie den Platzhalter-Registrierungsschlüssel löschen. Auf diese Weise wird sichergestellt, dass der neue Registrierungsschlüssel, der bei der Aktualisierung von Windows hinzugefügt wird, nicht beeinträchtigt wird.

## <a name="change-your-devices-calendar-format"></a>Ändern des Kalenderformats Ihres Geräts

Nachdem Sie den Registrierungsschlüssel für die neue Ära hinzugefügt haben, müssen Sie Ihr Gerät für die Verwendung des japanischen Kalenders konfigurieren. Sie benötigen dazu kein Gerät in japanischer Sprache. Um gründliche Tests durchzuführen, können Sie auch das japanische Language Pack installieren; dies ist für die grundlegenden Tests jedoch nicht erforderlich.

So konfigurieren Sie Ihr Gerät für die Verwendung des japanischen Kalenders:

1. Öffnen Sie **intl.cpl** (suchen Sie in der Windows-Suchleiste danach).
2. Wählen Sie in der Dropdownliste **Format** den Eintrag **Japanisch (Japan)** aus.
3. Wählen Sie **Zusätzliche Einstellungen** aus.
4. Wählen Sie die Registerkarte **Datum** aus.
5. Wählen Sie in der Dropdownliste **Kalendertyp** den Eintrag **和暦** (*Wareki*, den japanischen Kalender) aus. Es sollte die zweite Option sein.
6. Klicken Sie auf **OK**.
7. Klicken Sie im Fenster **Region** auf **OK**.

Ihr Gerät sollte nun für die Verwendung des japanischen Kalenders konfiguriert sein und die in der Registrierung eingetragenen Ären anzeigen. Nachstehend sehen Sie ein Beispiel dafür, was Sie möglicherweise in der rechten unteren Ecke des Bildschirms sehen:

![Datum und Uhrzeit im japanischen Kalenderformat](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>Einstellen der Uhr des Geräts

Um zu überprüfen, ob Ihre Anwendung mit der neuen Ära funktioniert, müssen Sie die Uhr Ihres Computers auf den 1. Mai 2019 oder auf ein späteres Datum stellen. Die folgenden Anweisungen gelten für Windows 10. Windows 8 und 7 sollten jedoch ähnlich funktionieren:

1. Klicken Sie mit der rechten Maustaste auf den Datums-/Uhrzeitbereich in der unteren rechten Ecke des Bildschirms.
2. Wählen Sie **Datum/Uhrzeit ändern** aus.
3. Wählen Sie in der Einstellungs-App unter **Datum und Uhrzeit ändern** die Option **Ändern** aus.
4. Ändern Sie das Datum auf den 1. Mai 2019 oder später.

> [!NOTE]
> Sie können das Datum gemäß den Organisationseinstellungen möglicherweise nicht ändern. Wenn dies der Fall ist, wenden Sie sich an Ihren Administrator. Sie können jedoch auch den Platzhalter-Registrierungsschlüssel bearbeiten, sodass Sie über ein bereits akzeptiertes Datum verfügen.

## <a name="test-your-application"></a>Testen Ihrer Anwendung

Testen Sie nun, wie Ihre Anwendung mit der neuen Ära umgeht. Überprüfen Sie Stellen, an denen das Datum angezeigt wird, z. B. bei Zeitstempeln und in Datumsauswahlfeldern. Stellen Sie sicher, dass die Ära vor dem 1. Mai 2019 (Heisei, 平成) und danach (Reiwa, 令和) korrekt ist.

### <a name="gannen-"></a>*Gannen* (元年)

Das Format für den japanischen Kalender lautet generell **&lt;Name der Ära&gt; &lt;Jahr der Ära&gt;** . Das Jahr 2018 ist z. B. **Heisei 30** (平成30年).  Das erste Jahr einer Ära ist jedoch ein Sonderfall. Anstelle von **&lt;Name der Ära&gt; 1** hat es das Format **&lt;Name der Ära&gt; 元年** (*Gannen*). Dementsprechend wäre das erste Jahr der Heisei-Ära 平成元年 (*Heisei Gannen*). Vergewissern Sie sich, dass Ihre Anwendung das erste Jahr der neuen Ära richtig verarbeitet und ordnungsgemäß 令和元年 ausgibt.

## <a name="related-apis"></a>Zugehörige APIs

Es gibt mehrere WinRT-, .NET- und Win32-APIs, die aktualisiert werden, um mit dem Wechsel der Ären umgehen zu können. Sie müssen sich daher keine Sorgen machen, sofern Sie diese APIs verwenden. Auch wenn Sie sich vollständig auf diese APIs verlassen, ist es dennoch eine gute Idee, Ihre Anwendung zu testen und sicherzustellen, dass sie sich erwartungsgemäß verhält, insbesondere, wenn Sie sie für etwas Besonderes wie Parsing verwenden.

Aktualisierungen des Betriebssystems und der SDKs finden Sie unter [Updates für den Wechsel der japanischen Ära im Mai 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change).

Die folgenden APIs werden betroffen sein:

### <a name="winrt"></a>WinRT

* [Windows.Globalization-Namespace](/uwp/api/windows.globalization)
  * [Calendar-Klasse](/uwp/api/windows.globalization.calendar)
    * [AddDays-Methode](/uwp/api/windows.globalization.calendar.adddays)
    * [AddEras-Methode](/uwp/api/windows.globalization.calendar.adderas)
    * [AddHours-Methode](/uwp/api/windows.globalization.calendar.addhours)
    * [AddMinutes-Methode](/uwp/api/windows.globalization.calendar.addminutes)
    * [AddMonths-Methode](/uwp/api/windows.globalization.calendar.addmonths)
    * [AddNanoseconds-Methode](/uwp/api/windows.globalization.calendar.addnanoseconds)
    * [AddPeriods-Methode](/uwp/api/windows.globalization.calendar.addperiods)
    * [AddSeconds-Methode](/uwp/api/windows.globalization.calendar.addseconds)
    * [AddWeeks-Methode](/uwp/api/windows.globalization.calendar.addweeks)
    * [AddYears-Methode](/uwp/api/windows.globalization.calendar.addyears)
    * [Era-Eigenschaft](/uwp/api/windows.globalization.calendar.era)
    * [EraAsString-Methode](/uwp/api/windows.globalization.calendar.eraasstring)
    * [FirstYearInThisEra-Eigenschaft](/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [LastEra-Eigenschaft](/uwp/api/windows.globalization.calendar.lastera)
    * [LastYearInThisEra-Eigenschaft](/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [NumberOfYearsInThisEra-Eigenschaft](/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Windows.Globalization.DateTimeFormatting-Namespace](/uwp/api/windows.globalization.datetimeformatting)
  * [DateTimeFormatter-Klasse](/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Format-Methode](/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [System-Namespace](/dotnet/api/system)
  * [DateTime-Struktur](/dotnet/api/system.datetime)
  * [DateTimeOffset-Struktur](/dotnet/api/system.datetimeoffset)
* [System.Globalization-Namespace](/dotnet/api/system.globalization)
  * [Calendar-Klasse](/dotnet/api/system.globalization.calendar)
  * [DateTimeFormatInfo-Klasse](/dotnet/api/system.globalization.datetimeformatinfo)
  * [JapaneseCalendar-Klasse](/dotnet/api/system.globalization.japanesecalendar)
  * [JapaneseLunisolarCalendar-Klasse](/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [datetimeapi.h-Header](/windows/desktop/api/datetimeapi/)
  * [GetDateFormatA-Funktion](/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
  * [GetDateFormatEx-Funktion](/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
  * [GetDateFormatW-Funktion](/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [winnls.h-Header](/windows/desktop/api/winnls/)
  * [EnumDateFormatsA-Funktion](/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
  * [EnumDateFormatsExA-Funktion](/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
  * [EnumDateFormatsExEx-Funktion](/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
  * [EnumDateFormatsExW-Funktion](/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
  * [EnumDateFormatsW-Funktion](/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
  * [GetCalendarInfoA-Funktion](/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
  * [GetCalendarInfoEx-Funktion](/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
  * [GetCalendarInfoW-Funktion](/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>Siehe auch

* [Umgang mit Ären im japanischen Kalender](/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [Das Y2K-Phänomen im japanischen Kalender](/archive/blogs/shawnste/the-japanese-calendars-y2k-moment)
* [Verwenden der Registrierung zum Testen der neuen japanischen Ära unter Windows](/archive/blogs/shawnste/using-the-registry-to-test-the-new-japanese-era-on-windows)
* [Gannen vs Ichinen](/archive/blogs/shawnste/gannen-vs-ichinen)
* [Updates für den Wechsel der japanischen Ära im Mai 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Umgang mit einer neuen Ära im japanischen Kalender in .NET](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)