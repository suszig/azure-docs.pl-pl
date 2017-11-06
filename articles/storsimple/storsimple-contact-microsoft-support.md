---
title: Logowania z biletu pomocy technicznej dla serii StorSimple 8000 | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć żądanie obsługi i rozpocząć sesję pomocy technicznej na urządzeniu StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d25fb4902d37faca5358e5a9010e9e146e344e1
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a>Skontaktuj się z pomocą techniczną firmy Microsoft dla Twojego urządzenia StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [skontaktuj się z pomocą techniczną firmy Microsoft dla Twojego urządzenia StorSimple](storsimple-8000-contact-microsoft-support.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

Jeśli wystąpią problemy z rozwiązania Microsoft Azure StorSimple można utworzyć żądania obsługi technicznej. W ramach sesji online ze specjalistą pomocy technicznej może również należy rozpocząć sesję pomocy technicznej na urządzeniu StorSimple. W tym artykule przedstawiono:

* Jak utworzyć żądanie pomocy technicznej.
* Jak rozpocząć sesję pomocy technicznej w interfejsie programu Windows PowerShell urządzenia StorSimple.

Przegląd [StorSimple 8000 serii Obsługa SLA oraz informacje](https://msdn.microsoft.com/library/mt433077.aspx) przed utworzeniem żądania pomocy technicznej.

## <a name="create-a-support-request"></a>Utwórz żądanie obsługi
Wykonaj poniższe kroki, aby utworzyć żądanie pomocy technicznej:

#### <a name="to-create-a-support-request"></a>Aby utworzyć żądanie obsługi
1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), w prawym górnym rogu kliknij nazwę konta, a następnie kliknij przycisk **skontaktuj się z pomocą techniczną firmy Microsoft**.
   
    ![MS skontaktuj się z pomocą techniczną przez ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. Nastąpi przekierowanie do nowego portalu Azure (portal.azure.com). Kliknij przycisk **nowy obsługuje żądania** kafelka.
   
    ![MS skontaktuj się z pomocą techniczną przez nowego portalu](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    Po prawej stronie ekranu **nowy obsługuje żądania** pojawi się okienko. 
   
    ![Nowe okienko żądania obsługi](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. W **podstawy** okna dialogowego należy wykonać następujące czynności:                                
   
   1. Z **wydawania typu** listy rozwijanej wybierz **techniczne**.
   2. Wybierz **subskrypcji** z listy rozwijanej.
   3. Z **usługi** listy rozwijanej wybierz **StorSimple**. 
   4. Wybierz **plan pomocy technicznej** z listy rozwijanej. Należy plan płatnej pomocy technicznej, aby włączyć pomocy technicznej.
4. Kliknij przycisk **Dalej**. **Problem** zostanie wyświetlone okno dialogowe.
   
    ![Nowe okienko żądania obsługi](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. W **Problem** okna dialogowego należy wykonać następujące czynności:
   
   1. Wybierz **ważność** poziomu z listy rozwijanej.
   2. Wybierz **typ problemu** z listy rozwijanej.
   3. Wybierz **kategorii** z listy rozwijanej. 
   4. W **szczegóły** polu Zwięźle opisz swój problem.
   5. W **okresie** polu, określ datę, godzinę i strefę czasową, który odpowiada ostatniego wystąpienia problemu.
   6. W obszarze **przekazywania pliku**, kliknij ikonę folderu, aby przejść do pakietu programu obsługi.
   7. Wybierz **udostępnianie informacji diagnostycznych** pole wyboru.
6. Kliknij przycisk **Dalej**. **Informacje kontaktowe** zostanie wyświetlone okno dialogowe.
   
    ![Nowe okienko żądania obsługi](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. Wprowadź swoje informacje kontaktowe i wybierz metodę kontaktu (telefonu lub poczty e-mail). 
8. Wybierz **Zapisz zmiany dotyczące kontaktu dla przyszłych żądań obsługi** pole wyboru.
9. Kliknij przycisk **Utwórz**.

Gdy prześlesz żądanie z pracownikiem pomocy technicznej skontaktuje się z Tobą tak szybko, jak to możliwe, aby przejść do żądania.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Rozpocznij sesję pomocy technicznej w programie Windows PowerShell dla urządzenia StorSimple
Aby rozwiązać problemy, które mogą pojawić się z urządzeniem StorSimple, konieczne będzie współpracować z zespołu Support firmy Microsoft. Microsoft Support konieczne może być zastosowanie sesji pomocy technicznej do logowania się do urządzenia. 

Wykonaj poniższe kroki, aby rozpocząć sesję pomocy technicznej:

#### <a name="to-start-a-support-session"></a>Aby rozpocząć sesję pomocy technicznej
1. Dostęp do urządzenia, za pomocą konsoli szeregowej lub za pośrednictwem sesji telnet z komputera zdalnego. Aby to zrobić, wykonaj kroki opisane w [przy użyciu programu PuTTY można nawiązać połączenia z konsolą szeregową urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. W sesji, który zostanie otwarty, naciśnij klawisz **Enter** klawisz, aby wyświetlić wiersz polecenia.
3. W menu konsoli szeregowej wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**.
4. W wierszu polecenia wpisz następujące hasło: 
   
    `Password1`
5. W wierszu polecenia wpisz następujące polecenie:
   
    `Enable-HcsSupportAccess`
6. Do użytkownika zostanie wyświetlone zaszyfrowanego ciągu. Skopiuj ten ciąg do edytora tekstu, takiego jak Notatnik.
7. Zapisz ten ciąg i wysłać go w wiadomości e-mail do firmy Microsoft Support. 

> [!IMPORTANT]
> Dostęp do pomocy technicznej można wyłączyć, uruchamiając `Disable-HcsSupportAccess`. Urządzenia StorSimple spróbuje również wyłączyć dostęp do pomocy technicznej 8 godzin po sesji zostało zainicjowane. Jest najlepszym rozwiązaniem jest zmiana poświadczeń urządzenia StorSimple po zainicjowaniu sesji pomocy technicznej.
> 
> 

