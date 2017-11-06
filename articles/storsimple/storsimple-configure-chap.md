---
title: "Konfigurowanie protokołu CHAP dla urządzenia z serii StorSimple 8000 | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak skonfigurować protokół uwierzytelniania typu Challenge Handshake (CHAP) na urządzeniu StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 83ad256522ca19a19b3fe46fcc48e9cb37cbe246
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>Konfigurowanie protokołu CHAP dla urządzenia StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [Konfigurowanie protokołu CHAP dla urządzenia StorSimple](storsimple-8000-configure-chap.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

W tym samouczku przedstawiono sposób konfigurowania protokołu CHAP dla urządzenia StorSimple. Procedury szczegółowo opisane w tym artykule dotyczą z serii StorSimple 8000, a także urządzenia StorSimple 1200.

Protokół uwierzytelniania typu Challenge Handshake oznacza protokołu CHAP. Jest schemat uwierzytelniania używany przez serwery do weryfikacji tożsamości klientów zdalnych. Weryfikacja jest oparta na wspólne hasło lub klucz tajny. Protokół CHAP, może być jednokierunkowe (jednokierunkowe) lub wzajemne (dwukierunkowe). Jednokierunkowe CHAP jest podczas obiekt docelowy uwierzytelniania inicjatora. Wzajemne lub wstecznego protokołu CHAP, wymaga z drugiej strony, obiekt docelowy uwierzytelniania inicjatora i następnie inicjatora uwierzytelniania obiektu docelowego. Inicjator może być realizowane bez uwierzytelnienia docelowego. Jednak docelowym może być realizowane tylko wtedy, gdy uwierzytelnianie inicjatora również jest zaimplementowana. 

Jako najlepsze rozwiązanie zaleca się używać protokołu CHAP w celu zwiększenia bezpieczeństwa iSCSI.

> [!NOTE]
> Należy pamiętać, że protokół IPSEC nie jest obecnie obsługiwane urządzenia StorSimple.
> 
> 

Ustawienia protokołu CHAP na urządzeniu StorSimple można skonfigurować w następujący sposób:

* Uwierzytelnianie jednokierunkowe lub jednokierunkowe
* Dwukierunkowy lub uwierzytelniania wzajemnego lub wstecznego

W każdym z tych przypadków portal dla urządzenia i oprogramowania inicjatora iSCSI serwera musi być skonfigurowana. Szczegółowe informacje na temat tej konfiguracji zostały opisane w poniższych samouczka.

## <a name="unidirectional-or-one-way-authentication"></a>Uwierzytelnianie jednokierunkowe lub jednokierunkowe
W przypadku uwierzytelniania jednokierunkowe inicjator jest uwierzytelniany element docelowy. To uwierzytelnianie wymaga skonfigurowania ustawień inicjatora protokołu CHAP na urządzeniu StorSimple i oprogramowaniem iSCSI Initiator na hoście. Szczegółowe procedury dotyczące hosta systemu Windows i urządzeń StorSimple, na których zostały opisane dalej.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Aby móc skonfigurować urządzenie do uwierzytelniania jednokierunkowe
1. W klasycznym portalu Azure na **urządzeń** kliknij przycisk **Konfiguruj** kartę.
   
    ![Inicjatora protokołu CHAP](./media/storsimple-configure-chap/IC740943.png)
2. Przewiń w dół na tej stronie, a następnie w **inicjatora protokołu CHAP** sekcji:
   
   1. Podaj nazwę użytkownika dla użytkownika inicjatora protokołu CHAP.
   2. Podaj hasło dla użytkownika inicjatora protokołu CHAP.
      
    > [!IMPORTANT]
    > Nazwa użytkownika protokołu CHAP musi zawierać mniej niż 233 znaków. Hasło protokołu CHAP musi należeć do zakresu od 12 do 16 znaków. Dłużej nazwa użytkownika lub hasło spowoduje niepowodzenie uwierzytelniania na hoście systemu Windows.
   
   3. Potwierdź hasło.
3. Kliknij pozycję **Zapisz**. Zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **OK**, aby zapisać zmiany.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Aby skonfigurować uwierzytelnianie jednokierunkowe na serwerze hosta systemu Windows
1. Na serwerze hosta z systemem Windows uruchom inicjatora iSCSI.
2. W **właściwości inicjatora iSCSI** okna, wykonaj następujące czynności:
   
   1. Kliknij przycisk **odnajdywania** kartę.
      
       ![Właściwości inicjatora iSCSI](./media/storsimple-configure-chap/IC740944.png)
   2. Kliknij przycisk **odnajdź Portal**.
3. W **odnajdowanie portalu obiektu docelowego** okno dialogowe:
   
   1. Określ adres IP urządzenia.
   2. Kliknij przycisk **zaawansowane**.
      
       ![Odnajdowanie portalu obiektu docelowego](./media/storsimple-configure-chap/IC740945.png)
4. W **Zaawansowane ustawienia** okno dialogowe:
   
   1. Wybierz **Włącz protokół CHAP logowania** pole wyboru.
   2. W **nazwa** pola, podaj nazwę użytkownika, który określony dla inicjatora protokołu CHAP w klasycznym portalu.
   3. W **klucz tajny obiektu docelowego** pola, podaj hasło określone dla inicjatora protokołu CHAP w klasycznym portalu.
   4. Kliknij przycisk **OK**.
      
       ![Zaawansowane ustawienia ogólne](./media/storsimple-configure-chap/IC740946.png)
5. Na **celów** karcie **właściwości inicjatora iSCSI** okna, stan urządzenia powinny się wyświetlać jako **połączony**. Jeśli używasz urządzenia StorSimple 1200 następnie każdy wolumin zostanie zainstalowany jako obiektu docelowego iSCSI w sposób przedstawiony poniżej. W związku z tym kroki 3 i 4 należy powtórzyć dla każdego woluminu.
   
    ![Woluminów zainstalowanych jako osobne cele](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Jeśli zmienisz nazwę iSCSI, Nowa nazwa będzie służyć do nowej sesji iSCSI. Nowe ustawienia nie są ponownie używane istniejące sesje aż do wylogowywania i logowania.
   > 
   > 

Aby uzyskać więcej informacji o konfigurowaniu protokołu CHAP na serwerze hosta systemu Windows, przejdź do [uwagi dodatkowe](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Dwukierunkowy lub wzajemnego uwierzytelniania
W przypadku uwierzytelniania dwukierunkowego docelowy umożliwiają uwierzytelnienie inicjatora a następnie inicjatora obiektu docelowego. Wymaga to użytkownikowi na konfigurowanie ustawień inicjatora protokołu CHAP, a także ustawieniach wstecznego protokołu CHAP oprogramowania inicjatora urządzenia i iSCSI na hoście. W poniższych procedurach opisano kroki, aby skonfigurować uwierzytelnianie wzajemne, na urządzeniu i na hoście systemu Windows.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Aby móc skonfigurować urządzenie do wzajemnego uwierzytelniania
1. W klasycznym portalu Azure na **urządzeń** kliknij przycisk **Konfiguruj** kartę.
   
    ![Obiektu docelowego protokołu CHAP](./media/storsimple-configure-chap/IC740948.png)
2. Przewiń w dół na tej stronie, a następnie w **docelowego protokołu CHAP** sekcji:
   
   1. Podaj **nazwy użytkownika** dla danego urządzenia.
   2. Podaj **wstecznego protokołu CHAP hasła** dla danego urządzenia.
   3. Potwierdź hasło.
3. W **inicjatora protokołu CHAP** sekcji:
   
   1. Podaj **nazwy użytkownika** dla danego urządzenia.
   2. Podaj **hasło** dla danego urządzenia.
   3. Potwierdź hasło.
4. Kliknij pozycję **Zapisz**. Zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **OK**, aby zapisać zmiany.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Aby skonfigurować uwierzytelnianie dwukierunkowe na serwerze hosta systemu Windows
1. Na serwerze hosta z systemem Windows uruchom inicjatora iSCSI.
2. W **właściwości inicjatora iSCSI** okna, kliknij przycisk **konfiguracji** kartę.
3. Kliknij przycisk **protokołu CHAP**.
4. W **wzajemnego protokołu CHAP klucz tajny inicjatora iSCSI** okno dialogowe:
   
   1. Typ **wstecznego protokołu CHAP hasła** skonfigurowanego w klasycznym portalu Azure.
   2. Kliknij przycisk **OK**.
      
       ![wzajemne tajny protokołu CHAP inicjatora iSCSI](./media/storsimple-configure-chap/IC740949.png)
5. Kliknij przycisk **cele** kartę.
6. Kliknij przycisk **Connect** przycisku. 
7. W **połączyć z docelowym** okno dialogowe, kliknij przycisk **zaawansowane**.
8. W **właściwości zaawansowane** okno dialogowe:
   
   1. Wybierz **Włącz protokół CHAP logowania** pole wyboru.
   2. W **nazwa** pola, podaj nazwę użytkownika, który określony dla inicjatora protokołu CHAP w klasycznym portalu.
   3. W **klucz tajny obiektu docelowego** pola, podaj hasło określone dla inicjatora protokołu CHAP w klasycznym portalu.
   4. Wybierz **wykonaj wzajemnego uwierzytelniania** pole wyboru.
      
       ![Ustawienia zaawansowane wzajemnego uwierzytelniania.](./media/storsimple-configure-chap/IC740950.png)
   5. Kliknij przycisk **OK** w celu ukończenia konfiguracji protokołu CHAP

Aby uzyskać więcej informacji o konfigurowaniu protokołu CHAP na serwerze hosta systemu Windows, przejdź do [uwagi dodatkowe](#additional-considerations).

## <a name="additional-considerations"></a>Dodatkowe zagadnienia
**Szybkie połączenie** funkcja nie obsługuje połączeń, które mają włączony protokół CHAP. Po włączeniu protokołu CHAP, upewnij się, że używasz **Connect** przycisku, który jest dostępny na **cele** kartę, aby połączyć się z obiektem docelowym.

![Łączenie do obiektu docelowego](./media/storsimple-configure-chap/IC740947.png)

W **nawiązywanie połączenia z docelowym** okno dialogowe jest wyświetlone, wybierz **Dodaj to połączenie do listy ulubionych elementów docelowych** pole wyboru. Dzięki temu, że przy każdym ponownym uruchomieniu komputera próby odzyskania połączenia do ulubionych obiektów docelowych iSCSI.

## <a name="errors-during-configuration"></a>Błędy podczas konfiguracji
Jeśli konfiguracja protokołu CHAP jest niepoprawny, a następnie może powodować zobacz **niepowodzenie uwierzytelniania** komunikat o błędzie.

## <a name="verification-of-chap-configuration"></a>Weryfikacja konfiguracji protokołu CHAP
Aby sprawdzić, czy CHAP jest używany przez wykonanie następujących kroków.

#### <a name="to-verify-your-chap-configuration"></a>Aby sprawdzić konfigurację protokołu CHAP
1. Kliknij przycisk **Ulubione obiekty docelowe**.
2. Wybierz cel, dla której jest włączone uwierzytelnianie.
3. Kliknij przycisk **szczegóły**.
   
    ![Inicjator właściwości ulubionych obiektów docelowych iSCSI](./media/storsimple-configure-chap/IC740951.png)
4. W **ulubionych szczegóły docelowej** okna dialogowego Zanotuj wpis w **uwierzytelniania** pola. Jeśli konfiguracja zakończyło się pomyślnie, powinien powiedzieć **protokołu CHAP**.
   
    ![Szczegóły ulubiony obiekt docelowy](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [zabezpieczenia usługi StorSimple](storsimple-security.md).
* Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

