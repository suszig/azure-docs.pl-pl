---
title: "Monitorowanie tożsamości i dostępu w usłudze Azure Security Center | Microsoft Docs"
description: "Ten dokument ułatwia korzystanie z funkcji zarządzania tożsamościami i dostępem w usłudze Azure Security Center w celu monitorowania dostępu użytkowników i rozwiązywania problemów związanych z tożsamościami."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9f04e730-4cfa-4078-8eec-905a443133da
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 2e351cd38e7baefc09fa36ceabffec92de22433b
ms.contentlocale: pl-pl
ms.lasthandoff: 09/14/2017

---
# <a name="monitoring-identity-and-access-in-azure-security-center"></a>Monitorowanie tożsamości i dostępu w usłudze Azure Security Center
Ten dokument ułatwia korzystanie z usługi Azure Security Center w celu monitorowania tożsamości i dostępu użytkowników.

## <a name="why-monitor-identity-and-access"></a>Dlaczego warto monitorować tożsamość i dostęp?
Tożsamość powinna być warstwą kontroli w przedsiębiorstwie. Ochrona tożsamości powinna stanowić najwyższy priorytet. W przeszłości organizacja była otoczona granicami pełniącymi rolę podstawowych stref obronnych. Obecnie jednak coraz więcej danych i aplikacji jest przenoszonych do chmury, przez co nową strefą ochronną staje się tożsamość.

Dzięki monitorowaniu działań związanych z tożsamością można podejmować prewencyjne działania przed wystąpieniem zdarzenia i reagować na próby ataku. Pulpit nawigacyjny Tożsamość i dostęp udostępnia przegląd stanu tożsamości, w tym liczbę nieudanych prób logowania, konta użytkowników wykorzystywane podczas tych prób, konta zablokowane, konta, dla których zmieniono lub zresetowano hasło, a także liczbę kont aktualnie zalogowanych użytkowników.

## <a name="how-to-monitor-identity-and-access-activities"></a>Jak monitorować działania związane z tożsamościami i dostępem?
Aby wyświetlić bieżące działania związane z tożsamościami i dostępem, wykonaj poniższe kroki. Wymagany jest dostęp do pulpitu nawigacyjnego **Tożsamość i dostęp**:

1.  Otwórz pulpit nawigacyjny usługi **Security Center**.
2.  W lewym okienku w obszarze **Zapobieganie** kliknij pozycję **Tożsamość i dostęp**. Jeśli masz wiele obszarów roboczych, zostanie wyświetlony selektor obszarów roboczych.

    ![wybór obszaru roboczego](./media/security-center-identity-access\security-center-identity-access-fig1.png)

    > [!NOTE]
    > Obecność komunikatu **UAKTUALNIJ PLAN** w ostatniej kolumnie oznacza, że dany obszar roboczy używa bezpłatnej subskrypcji. Aby korzystać z tej funkcji, musisz przeprowadzić uaktualnienie do warstwy Standardowa. Jeśli jest widoczny komunikat WYMAGANA AKTUALIZACJA, oznacza to, że aby korzystać z tej funkcji, musisz zaktualizować usługę [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Więcej informacji na temat planu cenowego zawiera cennik usługi Azure Security Center. 
    > 
3. Jeśli trzeba sprawdzić więcej niż jeden obszar roboczy, można ustalić priorytety analiz na podstawie kolumny **NIEUDANE OPERACJE LOGOWANIA**, która zawiera aktualną liczbę nieudanych operacji logowania w danym obszarze roboczym. Po wybraniu odpowiedniego obszaru roboczego pojawi się pulpit nawigacyjny **Tożsamość i dostęp**.

    ![Tożsamość i dostęp](./media/security-center-identity-access\security-center-identity-access-fig2.png)

4. Informacje zawarte na tym pulpicie nawigacyjnym mogą bezpośrednio pomóc w zidentyfikowaniu potencjalne podejrzanych działań. Ten pulpit nawigacyjny dzieli się na trzy główne obszary:
    * **Stan tożsamości**: zawiera podsumowanie działań dotyczących tożsamości, które mają miejsce w danym obszarze roboczym.
    * **Nieudane logowania**: pomaga szybko ustalić główną przyczynę nieudanych prób logowania oraz zawiera listę pierwszych dziesięciu kont, w przypadku których liczba nieudanych prób logowania jest największa.
    * **Logowania w czasie**: ułatwia szybkie ustalenie liczby prób logowania w poszczególnych przedziałach czasu oraz zawiera listę kont komputerów, w przypadku których liczba prób logowania jest największa.
    
Niezależnie od tego, który kafelek jest wybrany, wyświetlany pulpit nawigacyjny korzysta z [zapytania funkcji Wyszukiwanie w dziennikach](https://docs.microsoft.com/azure/security-center/security-center-search), a różnice dotyczą tylko typu zapytania i wyniku. Klikając wybrany element, na przykład komputer, można wyświetlić odpowiednie dane. 

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono sposób monitorowania tożsamości i dostępu w usłudze Azure Security Center. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Dowiedz się, jak zarządzać alertami i reagować na zdarzenia związane z bezpieczeństwem w usłudze Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Informacje o alertach zabezpieczeń w usłudze Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Poznaj różne typy alertów zabezpieczeń.
* [Przewodnik rozwiązywania problemów z usługą Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Dowiedz się, jak rozwiązywać typowe problemy z usługą Security Center. 
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.


