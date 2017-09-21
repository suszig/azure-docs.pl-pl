---
title: "Analiza zagrożeń w usłudze Azure Security Center | Microsoft Docs"
description: "Ten dokument pomaga wykrywać potencjalne zagrożenia na maszynach wirtualnych i komputerach przy użyciu funkcji analizy zagrożeń w usłudze Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: c492662aa3ee444347c55d9771790573ad969150
ms.contentlocale: pl-pl
ms.lasthandoff: 09/14/2017

---
# <a name="threat-intelligence-in-azure-security-center"></a>Analiza zagrożeń w usłudze Azure Security Center
Ten dokument ułatwia korzystanie z analizy zagrożeń usługi Azure Security Center w celu rozwiązywania problemów związanych z zabezpieczeniami.

## <a name="what-is-threat-intelligence"></a>Czym jest analiza zagrożeń?
Używając opcji Analiza zagrożeń dostępnej w usłudze Security Center, administratorzy IT mogą identyfikować zagrożenia dla bezpieczeństwa środowiska, na przykład wykrywać, czy dany komputer jest częścią botnetu. Komputery mogą stać się węzłami w botnecie, gdy osoby atakujące bezprawnie zainstalują złośliwe oprogramowanie, które potajemnie łączy ten komputer z centrum sterowania. Może także identyfikować potencjalne zagrożenia pochodzące z tajnych kanałów komunikacji, takich jak darknet.

Aby umożliwić używanie analizy zagrożeń, usługa Security Center korzysta z danych pochodzących z wielu źródeł firmy Microsoft. Dane te służą do wykrywania potencjalnych zagrożeń dla środowiska. Okienko analizy zagrożeń zawiera trzy główne opcje:

- Typy wykrytych zagrożeń
- Pochodzenie zagrożeń
- Mapa analizy zagrożeń


## <a name="when-should-i-use-threat-intelligence"></a>Kiedy należy używać analizy zagrożeń?
Jeden z etapów [procesu reagowania na incydenty związane z zabezpieczeniami](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) polega na określeniu dotkliwości danego incydentu dla systemów. Etap ten powinien obejmować następujące działania:

- Określenie natury ataku
- Określenie źródła ataku
- Określenie celu ataku Czy atak miał charakter przypadkowy czy był jawnie skierowany przeciwko Twojej organizacji w celu uzyskania konkretnych informacji?
- Zidentyfikowanie systemów, których zabezpieczenia zostały naruszone
- Ustalenie, do których plików uzyskano dostęp oraz jak poufna jest zawartość tych plików. Informacje pochodzące z analizy zagrożeń w usłudze Security Center mogą ułatwić wykonanie tych zadań. 

## <a name="how-to-access-the-threat-intelligence"></a>Jak uzyskać dostęp do analizy zagrożeń?
Aby wyświetlić bieżącą analizę zagrożeń dla środowiska, musisz najpierw wybrać obszar roboczy, w którym są przechowywane informacje. Jeśli masz tylko jeden obszar roboczy, selektor obszarów roboczych nie zostanie wyświetlony i od razu pojawi się pulpit nawigacyjny **Analiza zagrożeń**. Aby uzyskać dostęp do pulpitu nawigacyjnego analizy zagrożeń, wykonaj poniższe kroki:

1.  Otwórz pulpit nawigacyjny usługi **Security Center**.
2.  W lewym okienku w obszarze **Wykrywanie** kliknij pozycję **Analiza zagrożeń**. Zostanie wyświetlony pulpit nawigacyjny **Analiza zagrożeń**.

    ![Analiza zagrożeń](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > Obecność komunikatu **UAKTUALNIJ PLAN** w ostatniej kolumnie oznacza, że dany obszar roboczy używa bezpłatnej subskrypcji. Aby korzystać z tej funkcji, musisz przeprowadzić uaktualnienie do warstwy Standardowa. Jeśli jest widoczny komunikat WYMAGANA AKTUALIZACJA, oznacza to, że aby korzystać z tej funkcji, musisz zaktualizować usługę [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Więcej informacji na temat planu cenowego zawiera cennik usługi Azure Security Center. 
    > 
3. Jeśli trzeba sprawdzić więcej niż jeden obszar roboczy, można ustalić priorytety analiz na podstawie kolumny **ZŁOŚLIWY ADRES IP**, która zawiera aktualną liczbę złośliwych adresów IP w danym obszarze roboczym. Po wybraniu odpowiedniego obszaru roboczego pojawi się pulpit nawigacyjny **Analiza zagrożeń**.

    ![Analiza zagrożeń](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

4. Ten pulpit nawigacyjny zawiera cztery kafelki:
    * **Typy zagrożeń**: zawiera podsumowanie typów zagrożeń, które zostały wykryte w wybranym obszarze roboczym.
    * **Kraj pochodzenia**: zawiera zagregowane informacje o ruchu sieciowym pogrupowane według lokalizacji źródłowej.
    * **Lokalizacja zagrożeń**: ułatwia ustalenie, z których miejsc na świecie są wysyłane komunikaty do środowiska. Kolor strzałek wyświetlanych na mapie odpowiada kierunkowi ruchu sieciowego: pomarańczowy oznacza ruch przychodzący, a czerwony — ruch wychodzący. Kliknięcie strzałki spowoduje wyświetlenie typu zagrożenia i kierunku ruchu.
    * **Szczegóły zagrożenia**: zawiera więcej informacji na temat zagrożenia zaznaczonego na mapie.

Niezależnie od tego, który kafelek jest wybrany, wyświetlany pulpit nawigacyjny korzysta z zapytania funkcji [Wyszukiwanie w dziennikach](https://docs.microsoft.com/azure/security-center/security-center-search), a różnice dotyczą tylko typu zapytania i wyniku.

### <a name="threat-types"></a>Typy zagrożeń
Po kliknięciu kafelka **Typy zagrożeń** pojawia się pulpit nawigacyjny **Wyszukiwanie w dziennikach**. Opcje filtrów są dostępne w jego lewej części, a wyniki zapytania — w prawej.

![Wyszukiwanie w funkcji analizy zagrożeń](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

Wynik zapytania zawiera zagregowane wartości dotyczące zagrożeń, pogrupowane według ich nazw. W lewym okienku możesz wybrać atrybut, na podstawie którego chcesz filtrować. Aby na przykład wyświetlić tylko zagrożenia z aktywnym połączeniem z maszynami, wybierz pozycję **Połączony** w obszarze **STAN SESJI** i kliknij przycisk **Zastosuj**.

![stan sesji](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

W przypadku maszyn wirtualnych platformy Azure pulpit nawigacyjny analizy zagrożeń zawiera tylko dane sieciowe, które przechodzą przez agenta. Funkcja analizy zagrożeń korzysta również z następujących typów danych:

- Dane CEF (typ = CommonSecurityLog)
- Dane WireData (typ = WireData)
- Dzienniki usług IIS (typ = W3CIISLog)
- Zapora systemu Windows (typ = WindowsFirewall)
- Zdarzenia DNS (typ = DnsEvents)


## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono sposób korzystania z analizy zagrożeń w usłudze Security Center, która ułatwia wykrywanie podejrzanej aktywności. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Dowiedz się, jak zarządzać alertami i reagować na zdarzenia związane z bezpieczeństwem w usłudze Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Informacje o alertach zabezpieczeń w usłudze Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Poznaj różne typy alertów zabezpieczeń.
* [Przewodnik rozwiązywania problemów z usługą Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Dowiedz się, jak rozwiązywać typowe problemy z usługą Security Center. 
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.


