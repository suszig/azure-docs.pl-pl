---
title: "Skalowanie liczby wystąpień ręcznie lub automatycznie skalowana za pomocą portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można skalować usług Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: ancav
ms.openlocfilehash: 7e16926b5a1c28c0c8c80ee8c17e8c2fdb9e55a7
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="scale-instance-count-manually-or-automatically"></a>Skalowanie liczby wystąpień ręcznie lub automatycznie
W [Azure Portal](https://portal.azure.com/), możesz ręcznie ustawić liczbę wystąpień usługi lub można ustawić parametry, aby go automatycznie na podstawie zapotrzebowania skali. To jest zwykle nazywany *skalowanie* lub *skalować w*.

Przed skalowanie na podstawie liczby wystąpień, należy rozważyć, czy skalowania dotyczy **warstwa cenowa** oprócz liczba wystąpień. Różnych warstw cenowych może mieć różne liczby rdzeni i ilości pamięci, a więc będą miały lepszą wydajność w przypadku taką samą liczbę wystąpień (czyli *skalowanie w górę* lub *dół*). W tym artykule szczegółowo omówiono *skalować w* i *limit*.

Możesz skalować w portalu i umożliwia także [interfejsu API REST](https://msdn.microsoft.com/library/azure/dn931953.aspx) lub [zestawu .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) dostosowanie skali ręcznie lub automatycznie.

## <a name="scaling-manually"></a>Skalowanie ręcznie
1. W [Azure Portal](https://portal.azure.com/), kliknij przycisk **Przeglądaj**, a następnie przejdź do zasobu, aby skalować, takich jak **planu usługi aplikacji**.
2. Kliknij przycisk **Ustawienia > skalowania w poziomie (plan usługi App Service).**
3. W górnej części **skali** bloku widać historii akcji skalowania automatycznego usługi.
   
    ![Blok skali](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)
   
   > [!NOTE]
   > Tylko akcje, które są wykonywane przez skalowania automatycznego będą widoczne na tym wykresie. Jeśli ręcznie dopasować liczba wystąpień, zmiany nie zostaną odzwierciedlone na tym wykresie.
   > 
   > 
4. Można ręcznie Dostosuj liczbę **wystąpień** z suwaka.
5. Kliknij przycisk **zapisać** polecenia i będzie można przeskalować do podanej liczby wystąpień niemal natychmiast.

## <a name="scaling-based-on-a-pre-set-metric"></a>Skalowanie w oparciu metryki wstępnie ustawiona
Liczba wystąpień, które ma być dostosowywane automatycznie w oparciu metryki, zaznacz Metryka w **skalowanie przez** listy rozwijanej. Na przykład w przypadku **planu usługi aplikacji** można skalować przez **procent użycia procesora CPU**.

1. Po wybraniu metrykę otrzymasz suwaka lub pola tekstowe do wprowadzania liczba wystąpień, który chcesz skalować między:
   
    ![Blok skali z procent użycia procesora CPU](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)
   
    Funkcja automatycznego skalowania potrwa nigdy nie usługi poniżej lub powyżej granice, które można ustawić, bez względu na obciążenie.
2. Po drugie można wybrać zakres docelowy dla metryki. Na przykład, jeśli została wybrana opcja **procent użycia procesora CPU**, można ustawić elementu docelowego dla średnie wykorzystanie Procesora we wszystkich wystąpień w usłudze. Skalowanie się stanie, gdy średnie wykorzystanie Procesora przekracza maksymalną liczbę zdefiniowanych, podobnie skali w zostanie wykonana przy każdym średnią Procesora spadnie poniżej wartości minimalnej.
3. Kliknij przycisk **zapisać** polecenia. Funkcja automatycznego skalowania będzie sprawdzać co kilka minut, upewnij się, że są w zakresie wystąpienia i docelowego dla Twojego metryki. Gdy usługa odbierze dodatkowy ruch, uzyskasz więcej wystąpień bez żadnego działania.

## <a name="scale-based-on-other-metrics"></a>Oparte na inne metryki skalowania
Można skalować na podstawie metryk inne niż ustawienia, które są widoczne w **skalowanie przez** listy rozwijanej i może nawet ma złożonego zestawu elementów skalowania w poziomie i Skaluj w regułach.

### <a name="adding-or-changing-a-rule"></a>Dodawanie lub zmienianie reguły
1. Wybierz **reguły wydajności i harmonogram** w **skalowanie przez** listy rozwijanej: ![reguły wydajności](./media/insights-how-to-scale/Insights_PerformanceRules.png)
2. Jeśli poprzednio skalowania automatycznego na zobaczysz widok szczegółowe reguły, które były.
3. Skalowanie oparte na innym kliknij metryki **Dodaj regułę** wiersza. Możesz również kliknąć jeden z istniejących wierszy zmiana z metrykę, które wcześniej trzeba było metrykę, które ma być skalowana przez.
   ![Dodaj regułę](./media/insights-how-to-scale/Insights_AddRule.png)
4. Teraz musisz wybrać metryki, które ma być skalowana przez. W przypadku wybrania metrykę, które należy wziąć pod uwagę kilka rzeczy:
   
   * *Zasobów* Metryka pochodzi z. Zazwyczaj są to taka sama jak zasób, do którego są skalowania. Jednak jeśli chcesz skalować przy głębokości kolejki magazynu zasób jest kolejki, do której ma być skalowana przez.
   * *Nazwa metryki* samej siebie.
   * *Czasu agregacji* metryki. Jest to, jak dane są łączenie za pośrednictwem *czas trwania*.
5. Po wybraniu Twoje metryki wybierzesz próg Metryka i operatora. Na przykład można powiedzieć **większe** **80%**.
6. Następnie wybierz akcję, którą chcesz wykonać. Istnieje kilka różnych rodzajów akcje:
   
   * Zwiększanie lub zmniejszanie przez — spowoduje to dodanie lub usunięcie **wartość** liczba wystąpień, należy zdefiniować
   * Zwiększanie lub zmniejszanie procent — spowoduje to zmianę liczby wystąpień przez wartość procentową. Na przykład możesz umieścić 25 w **wartość** pola, a jeśli masz obecnie wystąpień 8, 2 zostanie dodany.
   * Zwiększyć lub zmniejszyć do — ta opcja zostanie ustawiona liczba wystąpień na **wartość** należy zdefiniować.
7. Ponadto można wybrać chłodzenia — jak długo ta reguła ma czekać po poprzedniej akcji skalowania można skalować ponownie.
8. Po skonfigurowaniu reguły trafień **OK**.
9. Po skonfigurowaniu wszystkich reguł ma należy koniecznie trafień **zapisać** polecenia.

### <a name="scaling-with-multiple-steps"></a>Skalowanie za pomocą wielu kroków
Powyższe przykłady są bardzo proste. Jednak jeśli mają być skuteczniejsze temat skalowania w górę (lub w dół), nawet dodać wiele reguł skalowania dla tego samego metryki. Na przykład można zdefiniować dwa reguł skalowania procent użycia procesora CPU:

1. Skalowanie przez wystąpienie 1, jeśli procent użycia procesora CPU przekracza 60%
2. Skalowanie przez 3 wystąpienia, jeśli procent użycia procesora CPU wynosi powyżej 85%

![Wiele reguł skalowania](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

Z tą regułą dodatkowe jeśli obciążenie przekracza 85%, przed akcji skalowania, otrzymasz dwa dodatkowe wystąpienia zamiast jednego.

## <a name="scale-based-on-a-schedule"></a>Skala zgodnie z harmonogramem
Domyślnie podczas tworzenia reguły skalowania zostanie zawsze zastosowana. Można stwierdzić, że po kliknięciu nagłówka profilu:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Można jednak mieć bardziej agresywną skalowanie w ciągu dnia lub tygodnia, niż na weekend. Można nawet zamknąć usługi całkowicie poza godzinami pracy.

1. W tym celu w profilu, należy wybrać **cyklu** zamiast **zawsze,** i wybierz godziny, które ma profil do zastosowania.
2. Na przykład, aby mieć profil, który ma zastosowanie w tygodniu, **dni** Usuń zaznaczenie pola wyboru Lista rozwijana **sobota** i **niedziela**.
3. Aby profil, który ma zastosowanie podczas dziennych, ustaw **godzina rozpoczęcia** do porę dnia, który ma być uruchamiany w.
   
    ![Domyślne cyklu](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)
4. Kliknij przycisk **OK**.
5. Następnie należy dodać profil, który chcesz zastosować w innym czasie. Kliknij przycisk **Dodaj profil** wiersza.
    ![Wolne od pracy](./media/insights-how-to-scale/Insights_ProfileOffWork.png)
6. Nazwa nowej, sekundy, profil, na przykład można go wywołać **wyłączone pracy**.
7. Następnie wybierz **cyklu** ponownie i wybierz zakres liczba wystąpienie ma w tym czasie.
8. Jako profil domyślny, wybierz polecenie **dni** ten profil ma zastosowanie i **godzina rozpoczęcia** w ciągu dnia.
   
   > [!NOTE]
   > Funkcja automatycznego skalowania użyje reguły oszczędność czasu letniego dla zależności **strefy czasowej** wybrania. Jednak podczas czasu letniego przesunięcie UTC wyświetli przesunięcie strefy czasowej podstawowej, nie przesunięcie czasu letniego oszczędność czasu UTC.
   > 
   > 
9. Kliknij przycisk **OK**.
10. Teraz należy dodać niezależnie od zasady, które chcesz zastosować w drugim profilu. Kliknij przycisk **Dodaj regułę**, a następnie można utworzyć tę samą zasadę masz podczas profil domyślny.
    
    ![Dodaj regułę wyłączone pracy](./media/insights-how-to-scale/Insights_RuleOffWork.png)
11. Pamiętaj utworzyć obie reguły do skalowania w poziomie i skali w lub w profilu liczba wystąpień tylko będzie rosnąć (lub zmniejszyć).
12. Na koniec kliknij **zapisać**.

## <a name="next-steps"></a>Następne kroki
* [Monitorowanie metryk usługi](insights-how-to-customize-monitoring.md) się upewnić, że usługa jest dostępna i elastyczny.
* [Włączanie monitorowania i diagnostyki](insights-how-to-use-diagnostics.md) zbierania szczegółowych o dużej częstotliwości metryk usługi.
* [Odbieraj powiadomienia o alertach](insights-receive-alert-notifications.md) zawsze, gdy wystąpią zdarzenia operacyjne lub metryki przekroczą próg.
* [Monitorowanie wydajności aplikacji](../application-insights/app-insights-azure-web-apps.md) Jeśli chcesz zrozumieć, dokładnie tak jak wykonywanie kodu w chmurze.
* [Wyświetl zdarzenia i dziennik aktywności](insights-debugging-with-events.md) Aby dowiedzieć się wszystkie zasoby, które miało miejsce w usłudze.
* [Monitorowanie dostępności i czasu odpowiedzi dowolnej strony sieci web](../application-insights/app-insights-monitor-web-app-availability.md) z usługi Application Insights, można sprawdzić, czy strony nie działa.

