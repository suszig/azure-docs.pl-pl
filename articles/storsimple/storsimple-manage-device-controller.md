---
title: "Zarządzanie kontrolerami urządzenia StorSimple | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zatrzymać, uruchom ponownie, zamknij lub zresetować kontrolerów urządzenia StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4ee989d0-956f-4c14-951e-fd4e490ea09d
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 8182d0ccf9fb57a9dba383d376d1c557845bcc8a
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="manage-your-storsimple-device-controllers"></a>Zarządzanie kontrolerów urządzenia StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [Zarządzanie kontrolerów urządzenia StorSimple](storsimple-8000-manage-device-controller.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
W tym samouczku opisano różne operacje, które można wykonać na kontrolerach urządzenia StorSimple. Kontrolery w urządzeniu StorSimple są kontrolery nadmiarowy (peer) w konfiguracji aktywny / pasywny. W danym momencie tylko jeden kontroler jest aktywna i przetwarza wszystkie operacje dysku i sieci. Inny kontroler jest w trybie pasywnym. W przypadku niepowodzenia aktywnego kontrolera pasywnym kontrolera staje się aktywny automatycznie.

Ten samouczek zawiera instrukcje krok po kroku, aby zarządzać kontrolery urządzeń przy użyciu:

* **Kontrolery** sekcji **konserwacji** strony w usłudze Menedżer StorSimple
* Program Windows PowerShell dla StorSimple.

Zaleca się, że zarządzać kontrolery urządzeń za pomocą usługi Menedżer StorSimple. Jeśli akcję można wykonać tylko przy użyciu programu Windows PowerShell dla urządzenia StorSimple, samouczka sprawia, że je zapisać.

Po przeczytaniu tego samouczka, będą mieć możliwość:

* Ponowne uruchamianie lub zamykanie kontrolera urządzenia StorSimple
* Wyłącz urządzenia StorSimple
* Resetowanie do ustawień fabrycznych urządzenia StorSimple

## <a name="restart-or-shut-down-a-single-controller"></a>Ponowne uruchamianie lub zamykanie jednego kontrolera
Zamknięcie lub ponowne uruchomienie kontrolera nie jest wymagana w ramach normalnego działania. Operacji zamknięcia na kontrolerze jednego urządzenia są często używane tylko w przypadkach, w których wymagana jest wymiana składnik sprzętowy urządzenia nie powiodło się. Ponowne uruchomienie kontrolera może być konieczne również w sytuacji, w których wpływ na wydajność w nadmiernego wykorzystania pamięci lub nieprawidłowo kontrolera. Może również musisz ponownie uruchomić kontrolera po wymianie pomyślne kontrolera, jeśli chcesz włączyć i zastąpionego kontroler testów.

Ponowne uruchamianie urządzenia nie jest znaczący wpływ na inicjatory połączone, przy założeniu, że jest dostępny kontroler pasywnych. Jeśli kontroler pasywnej nie jest dostępna lub włączenia wyłączone, a następnie ponowne uruchomienie kontrolera active może spowodować przerwy w świadczeniu usługi i przestoje.

> [!IMPORTANT]
> * **Działającego kontrolera powinien nigdy nie zostać usunięty, ponieważ skutkowałoby to utratą nadmiarowości i zwiększone ryzyko przestoju.**
> * Poniższa procedura ma zastosowanie tylko do urządzenia fizycznego StorSimple. Aby uzyskać informacje o tym, jak uruchomić, zatrzymać i uruchomić ponownie urządzenie wirtualne, zobacz [pracy z urządzeniem wirtualnym](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).
> 
> 

Można ponownie uruchomić lub zamknąć kontrolera jednego urządzenia za pomocą klasycznego portalu Azure usługi Menedżer StorSimple lub środowiska Windows PowerShell dla urządzenia StorSimple

Aby zarządzać kontrolerów urządzeń z klasycznego portalu Azure, wykonaj następujące kroki.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Aby ponownie uruchomić lub zamknąć kontrolera w klasycznym portalu
1. Przejdź do **urządzenia > Konserwacja**.
2. Przejdź do **stan sprzętu** i upewnij się, że jest w stanie kontrolerów na urządzeniu **dobra kondycja**.
   
    ![Sprawdź, czy kontrolery urządzeń StorSimple są w dobrej kondycji](./media/storsimple-manage-device-controller/IC766017.png)
3. W dolnej części **konserwacji** kliknij przycisk **Zarządzaj kontrolerami**.
   
    ![Zarządzanie kontrolerami urządzenia StorSimple](./media/storsimple-manage-device-controller/IC766018.png)</br>
   
   > [!NOTE]
   > Jeśli nie widzisz **Zarządzaj kontrolerami**, a następnie należy zainstalować aktualizacje. Aby uzyskać więcej informacji, zobacz [aktualizacja urządzenia StorSimple](storsimple-update-device.md).
   > 
   > 
4. W **Zmień ustawienia kontrolera** okna dialogowego pole, wykonaj następujące czynności:
   
   1. Z **Wybierz kontroler** listy rozwijanej wybierz kontroler, który chcesz zarządzać. Dostępne opcje to kontrolera 0 i kontrolera 1. Te kontrolery są również identyfikowane jako aktywnych lub pasywnych.
      
      > [!NOTE]
      > Nie można zarządzać kontrolera, jeśli jest niedostępna lub wyłączona wyłączone i nie zostanie wyświetlony na liście rozwijanej.
      > 
      > 
   2. Z **wybierz akcję** listy rozwijanej wybierz pozycję **ponowne uruchomienie kontrolera** lub **Zamknij kontroler**.
      
       ![Uruchom ponownie kontroler pasywnym urządzenia StorSimple](./media/storsimple-manage-device-controller/IC766020.png)
   3. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-device-controller/IC740895.png).

To spowoduje ponowne uruchamianie lub zamykanie kontrolera. Poniższa tabela zawiera podsumowanie szczegóły co się stanie, w zależności od opcji dokonanym w **Zmień ustawienia kontrolera** okno dialogowe.  

| Wybór # | Jeśli chcesz... | Dzieje się tak. |
| --- | --- | --- |
| 1. |Uruchom ponownie kontroler pasywnych. |Zadanie zostanie utworzone w celu ponownego uruchomienia kontrolera, a użytkownik otrzyma powiadomienie po pomyślnym utworzeniu zadania. Spowoduje to zainicjowanie ponowne uruchomienie kontrolera. Można monitorować proces ponownego uruchamiania, uzyskując dostęp do **usługi > pulpit nawigacyjny > Sprawdź dzienniki operacji** , a następnie filtrowanie według parametry specyficzne dla usługi. |
| 2. |Uruchom ponownie aktywnym kontrolerze. |Zostanie wyświetlone następujące ostrzeżenie: "po ponownym uruchomieniu aktywnym kontrolerze urządzenia zostaną przełączone awaryjnie do pasywnej kontrolera. Czy chcesz kontynuować?" </br>Jeśli wybierzesz kontynuować wykonywanie tej operacji, następne kroki będą takie same jak te używane do ponownego uruchamiania kontrolera pasywnym (zobacz wybór 1). |
| 3. |Zamknij pasywnym kontroler. |Zostanie wyświetlony następujący komunikat: "po zakończeniu zamknij należy na naciśnięcie przycisku zasilania na kontrolerze, aby ją włączyć. Czy na pewno chcesz zamknąć ten kontroler?" </br>Jeśli wybierzesz kontynuować wykonywanie tej operacji, następne kroki będą takie same jak te używane do ponownego uruchamiania kontrolera pasywnym (zobacz wybór 1). |
| 4. |Zamknij aktywny kontroler. |Zostanie wyświetlony następujący komunikat: "po zakończeniu zamknij należy na naciśnięcie przycisku zasilania na kontrolerze, aby ją włączyć. Czy na pewno chcesz zamknąć ten kontroler?" </br>Jeśli wybierzesz kontynuować wykonywanie tej operacji, następne kroki będą takie same jak te używane do ponownego uruchamiania kontrolera pasywnym (zobacz wybór 1). |

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Aby ponownie uruchomić lub zamknąć kontrolera w programie Windows PowerShell dla urządzenia StorSimple
Wykonaj poniższe kroki, aby zamknąć lub ponownie uruchomić pojedynczy kontroler na urządzeniu StorSimple z klasycznego portalu Azure.

1. Dostęp do urządzenia za pomocą konsoli szeregowej lub sesji telnet z komputera zdalnego. Połączenia z kontrolerem 0 lub 1 kontrolera, wykonując kroki opisane w [przy użyciu programu PuTTY można nawiązać połączenia z konsolą szeregową urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. W menu konsoli szeregowej wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**.
3. W komunikacie transparentu Zanotuj kontrolera są podłączone do (kontrolera 0 lub 1 kontrolera) i czy jest ona aktywnych lub pasywnych kontrolera (wstrzymania).
   
   * Można zamknąć jednego kontrolera, w wierszu polecenia wpisz:
     
       `Stop-HcsController`
     
       Kontroler, które są podłączone do zostanie zamknięty. Jeśli zatrzymasz aktywnym kontrolerze następnie go zostaną przełączone awaryjnie do pasywnych kontrolera przed jego zamknięciem.
   * Aby ponownie uruchomić kontrolera, w wierszu polecenia wpisz:
     
       `Restart-HcsController`
     
       Uruchom ponownie kontroler, które są podłączone do. Po ponownym uruchomieniu kontrolera active zostaną przełączone awaryjnie do pasywnej kontrolera przed ponownym uruchomieniem.

## <a name="shut-down-a-storsimple-device"></a>Wyłącz urządzenia StorSimple
W tej sekcji wyjaśniono, jak zamknąć uruchomionej lub urządzenie StorSimple nie powiodło się z komputerem zdalnym. Urządzenia jest wyłączona po zamykanie kontrolerów urządzeń. Zamknięcia urządzenia odbywa się, gdy urządzenie jest przenoszony fizycznie lub pochodzi z usługi.

> [!IMPORTANT]
> Przed zamknięciem urządzenia, Sprawdź kondycję składniki urządzenia. Przejdź do **urządzenia > konserwacja > Stan sprzętu** i sprawdź, czy stan LED wszystkich składników jest zielony. Dobra urządzenie ma stan zielony. Jeśli urządzenie jest wyłączone w celu zastąpienia nieprawidłowo działający składnik, pojawi się nie powiodło się (czerwony) lub obniżeniem statusu (żółty) dla odpowiednich składników.
> 
> 

#### <a name="to-shut-down-a-storsimple-device"></a>Aby wyłączyć urządzenia StorSimple
1. Użyj [ponowne uruchamianie lub zamykanie kontrolera](#restart-or-shut-down-a-single-controller) procedurę, aby zidentyfikować i zamknij pasywnym kontroler na urządzeniu. Tę operację można wykonać w klasycznym portalu Azure lub w programie Windows PowerShell dla urządzenia StorSimple.
2. Powtórz powyższy krok można zamknąć active kontrolera.
3. Teraz należy przyjrzeć się wstecz rzutu urządzenia. Po dwóch kontrolerów są całkowicie zamknięty, stan LED na obu kontrolerów powinny migający czerwony. Jeśli chcesz całkowicie wyłączyć urządzenia w tej chwili Przerzuć przełączniki zasilania na zasilania i chłodzenia modułów (PCMs) do położenia OFF. Wyłączyć to urządzenie.

<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Zresetowanie urządzenia do domyślnych ustawień fabrycznych
> [!IMPORTANT]
> Jeśli potrzebujesz zresetować urządzenie do domyślnych ustawień fabrycznych, skontaktuj się z Microsoft Support. W procedurze opisanej poniżej należy używać tylko w połączeniu z Microsoft Support.
> 
> 

Ta procedura opisuje sposób zresetowania urządzenia Microsoft Azure StorSimple domyślne ustawienia fabryczne przy użyciu programu Windows PowerShell dla urządzenia StorSimple.
Zresetowanie urządzenia usuwa wszystkie dane i ustawienia z całego klastra domyślnie.

Wykonaj poniższe kroki, aby zresetować do domyślnych ustawień fabrycznych urządzenia Microsoft Azure StorSimple:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Aby zresetować urządzenie do domyślnych ustawień w programie Windows PowerShell dla urządzenia StorSimple
1. Dostęp do urządzenia za pomocą jego konsoli szeregowej. Sprawdź komunikat transparentu, aby upewnić się, że masz połączenie z aktywnym kontrolerem.
2. W menu konsoli szeregowej wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**.
3. W wierszu polecenia wpisz następujące polecenie, aby zresetować całego klastra, usuwając wszystkie ustawienia danych, metadane i kontrolera:
   
    `Reset-HcsFactoryDefault`
   
    Aby zamiast tego należy zresetować jednego kontrolera, należy użyć [HcsFactoryDefault resetowania](http://technet.microsoft.com/library/dn688132.aspx) polecenia cmdlet z `-scope` parametru.)
   
    System zostanie ponownie uruchomiony wiele razy. Podczas resetowania zostało zakończone pomyślnie, otrzymasz powiadomienie. 45 do 60 minut na urządzeniu 8100 i 60 90 minut 8600 zakończyć ten proces może potrwać w zależności od modelu systemu.
   
   > [!TIP]
   > * Jeśli używasz 1.2 aktualizacji lub wcześniej używane `–SkipFirmwareVersionCheck` parametr, aby pominąć sprawdzanie wersji oprogramowania układowego (w przeciwnym razie zostanie wyświetlony błąd niezgodności oprogramowania układowego: resetowanie do ustawień fabrycznych nie może kontynuować pracy z powodu niezgodności wersji oprogramowania układowego. ).
   > * Procedury resetowania fabryki może zakończyć się niepowodzeniem dla urządzenia StorSimple, które są uruchomione w portalu dla instytucji rządowych Update 1 lub 1.1 i wykonano pomyślnie kontrolera pojedyncza lub Podwójna zastępczy (z kontrolerami zastąpienia, które zostały wysłane z oprogramowaniem przed aktualizacją 1). Dzieje się tak podczas resetowania fabryka się, że obraz jest weryfikowane pod kątem obecności pliku SHA1 na kontrolerze, który nie istnieje dla oprogramowania 1 przed aktualizacją. Jeśli zostanie wyświetlony błąd resetowania tej fabryce, skontaktuj się z Microsoft Support pomocne w następnych krokach. Ten problem nie występuje z kontrolerami zastąpienia, które zostały wysłane z fabryki Update 1 lub nowszym oprogramowania.
   > 
   > 

## <a name="questions-and-answers-about-managing-device-controllers"></a>Pytania i odpowiedzi dotyczące zarządzania kontrolerami urządzenia
W tej sekcji możemy utworzono podsumowanie niektóre często zadawane pytania dotyczące zarządzania kontrolery urządzeń StorSimple.

**PYTANIE** Co się stanie w przypadku obu kontrolerów w urządzeniu są w dobrej kondycji i został włączony na i I ponowne uruchamianie lub zamykanie aktywnym kontrolerze?

**ODPOWIEDŹ** Jeśli kontrolery na urządzeniu są w dobrej kondycji i został włączony, zostanie wyświetlony monit o potwierdzenie. Można wybrać opcję:

* **Uruchom ponownie kontroler active** — użytkownik zostanie poinformowany, czy ponowne uruchomienie kontrolera active spowoduje, że urządzenia do trybu failover pasywnym kontrolera. Kontroler zostanie uruchomiony ponownie.
* **Zamknij aktywny kontroler** — użytkownik zostanie powiadomiony, że zamykanie active kontrolera spowoduje Przestój. Należy również naciśnij przycisk zasilania na urządzeniu, aby włączyć na kontrolerze.

**PYTANIE** Co się stanie, jeśli pasywnym kontrolera w urządzeniu jest niedostępny lub włączenia wyłączone i I ponowne uruchamianie lub zamykanie aktywnym kontrolerze?

**ODPOWIEDŹ** Jeśli kontroler pasywnym na urządzeniu jest niedostępna albo jest wyłączona funkcja wyłączona i użytkownik wybierze:

* **Uruchom ponownie kontroler active** — użytkownik zostanie powiadomiony, że kontynuowanie tej operacji spowoduje tymczasowe przerw w działaniu usługi, i zostanie wyświetlony monit o potwierdzenie.
* **Zamknij aktywny kontroler** — użytkownik zostanie powiadomiony, że kontynuowanie tej operacji spowoduje Przestój i że konieczne będzie naciśnij przycisk zasilania na jeden lub oba kontrolerów, aby włączyć w urządzeniu. Pojawi się monit o potwierdzenie.

**PYTANIE** Gdy nie kontrolera ponownego uruchomienia lub zamknięcia nie powiedzie się postęp?

**ODPOWIEDŹ** Ponowne uruchamianie lub zamykanie kontroler może się niepowodzeniem, jeśli:

* Trwa aktualizacja urządzenia.
* Ponowne uruchomienie kontrolera jest już w toku.
* Zamknięcie kontrolera jest już w toku.

**PYTANIE** Jak może Ci zorientować się, jeśli kontroler został uruchomiony ponownie lub zamknąć?

**ODPOWIEDŹ** Można sprawdzić stan kontrolera na stronie konserwacji. Wskazuje stan kontrolera czy kontroler został ponownie uruchomiony lub zamknięty. Ponadto strony alertów będzie zawierać alert informacyjny, jeśli kontroler został ponownie uruchomiony lub zamknięty. Operacji zamknięcia i ponownego uruchomienia kontrolera są także rejestrowane w dziennikach operacji. Aby uzyskać więcej informacji na temat dzienniki operacji, przejdź do [Sprawdź dzienniki operacji](storsimple-service-dashboard.md#view-the-operations-logs).

**PYTANIE** Jest ich wpływ na operacje We/Wy wyniku kontrolera trybu failover?

**ODPOWIEDŹ** Połączenia TCP między inicjatorów i aktywnym kontrolerze zostanie zresetowany w wyniku kontrolera pracy awaryjnej, ale będzie można ponownie ustanowić podczas operacji zakłada pasywnym kontrolera. W trakcie tej operacji może być tymczasowe (mniej niż 30 sekund) wstrzymaniu operacje wejścia/wyjścia między inicjatorów i urządzenia.

**PYTANIE** Jak zwraca Moje kontrolera do obsługi po został zamknięty i usunięty?

**ODPOWIEDŹ** Aby przywrócić kontrolera usługi, należy wstawić go do obudowy zgodnie z opisem w [zastąpić modułu kontroler na urządzeniu StorSimple](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Następne kroki
* Jeśli wystąpią problemy z kontrolerów urządzenia StorSimple, których nie można rozpoznać przy użyciu procedur zamieszczonych w tym samouczku [skontaktuj się z Microsoft Support](storsimple-contact-microsoft-support.md).
* Aby dowiedzieć się więcej o korzystaniu z usługi Menedżer StorSimple, przejdź do [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

