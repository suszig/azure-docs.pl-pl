---
title: "Funkcje adaptacyjnego sterowania aplikacjami w usłudze Azure Security Center | Microsoft Docs"
description: "Ten dokument ułatwia korzystanie z funkcji adaptacyjnego sterowania aplikacjami w usłudze Azure Security Center w celu tworzenia listy dozwolonych aplikacji uruchamianych na maszynach wirtualnych platformy Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 890acae2aebf7684e567b9b49377ca7b6da95245
ms.openlocfilehash: 9c3a9a7255bbbdab8f4c356eb07022d7f1d242d7
ms.contentlocale: pl-pl
ms.lasthandoff: 09/20/2017

---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Funkcje adaptacyjnego sterowania aplikacjami w usłudze Azure Security Center (wersja zapoznawcza)
Z tego przewodnika dowiesz się, jak skonfigurować funkcje sterowania aplikacjami w usłudze Azure Security Center.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Co to są funkcje adaptacyjnego sterowania aplikacjami w usłudze Security Center?
Funkcje adaptacyjnego sterowania aplikacjami ułatwiają sterowanie tym, które aplikacje mogą być uruchamiane na maszynach wirtualnych na platformie Azure, co pozwala między innymi wzmocnić ochronę tych maszyn przed złośliwym oprogramowaniem. Usługa Security Center analizuje procesy uruchomione na maszynie wirtualnej przy użyciu uczenia maszynowego i za pomocą tej analizy ułatwia zastosowanie listy reguł elementów dozwolonych. Ta funkcja znacząco upraszcza proces konfigurowania i konserwowania list dozwolonych aplikacji, co pozwala na:

- Blokowanie lub ostrzeganie w przypadku prób uruchomienia złośliwych aplikacji, w tym takich, które w przeciwnym razie mogłyby zostać niezauważone przez rozwiązania chroniące przed złośliwym oprogramowaniem.
- Zachowanie zgodności z zasadami zabezpieczeń organizacji, które nakazują korzystanie wyłącznie z licencjonowanego oprogramowania.
- Unikanie używania w danym środowisku niechcianego oprogramowania.
- Unikanie uruchamiania starych i nieobsługiwanych aplikacji.
- Zapobieganie używaniu określonych programów narzędziowych, które nie są dozwolone w organizacji.
- Umożliwianie informatykom sterowania dostępem do poufnych danych za pomocą aplikacji.

> [!NOTE]
> Funkcje adaptacyjnego sterowania aplikacjami są dostępne dla klientów z usługą Azure Security Center w wersji Standard w postaci ograniczonej publicznej wersji zapoznawczej. Wyślij [nam](mailto:ASC_appcontrol@microsoft.com) wiadomość e-mail ze swoim identyfikatorem subskrypcji, aby dołączyć do tej wersji zapoznawczej.

## <a name="how-to-enable-adaptive-application-controls"></a>Jak włączyć funkcje adaptacyjnego sterowania aplikacjami?
Funkcje adaptacyjnego sterowania aplikacjami ułatwiają zdefiniowanie zestawu aplikacji, które mogą być uruchamiane w skonfigurowanych grupach zasobów. Ta funkcja jest dostępna tylko dla komputerów z systemem Windows (we wszystkich wersjach, klasycznych lub usługi Azure Resource Manager). Wykonaj poniższe kroki, aby skonfigurować listę dozwolonych aplikacji w usłudze Security Center:

1.  Otwórz pulpit nawigacyjny **Centrum zabezpieczeń**, a następnie kliknij przycisk **Przegląd**.
2.  W obszarze **Zaawansowana ochrona w chmurze** na kafelku **Funkcje adaptacyjnego sterowania aplikacjami** wskazana jest liczba maszyn wirtualnych, do których są obecnie zastosowane te funkcje w porównaniu do liczby wszystkich maszyn wirtualnych. Podana jest też liczba problemów znalezionych w ostatnim tygodniu: 

    ![Funkcje adaptacyjnego sterowania aplikacjami](./media/security-center-adaptive-application\security-center-adaptive-application-fig1.png)

3. Kliknij kafelek **Funkcje adaptacyjnego sterowania aplikacjami**, aby wyświetlić więcej opcji.

    ![funkcje sterowania](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

4. Sekcja Grupy zasobów zawiera trzy karty:
    * **Zalecane**: lista grup zasobów, dla których zaleca się sterowanie aplikacjami. Usługa Security Center przy użyciu uczenia maszynowego identyfikuje maszyny wirtualne odpowiednie do zastosowania funkcji sterowania aplikacjami na podstawie tego, czy na tych maszynach są spójnie uruchamiane te same aplikacje.
    * **Skonfigurowane**: lista grup zasobów zawierających maszyny wirtualne, dla których skonfigurowano sterowanie aplikacjami. 
    * **Brak zaleceń**: lista grup zasobów zawierających maszyny wirtualne bez żadnych zaleceń dotyczących sterowania aplikacjami. Na przykład maszyny wirtualne, na których aplikacje stale się zmieniają i nie osiągają stanu stabilnego.

W kolejnych sekcjach bardziej szczegółowo omówiono poszczególne opcje i sposób korzystania z nich.

### <a name="configure-a-new-application-control-policy"></a>Konfigurowanie nowych zasad sterowania aplikacjami
Kliknij kartę **Zalecane**, aby uzyskać listę grup zasobów z zaleceniami dotyczącymi sterowania aplikacjami:

![Zalecane](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

Lista zawiera:
- **NAZWA**: nazwa subskrypcji i grupy zasobów
- **MASZYNY WIRTUALNE**: liczba maszyn wirtualnych w grupie zasobów
- **STAN**: stan zaleceń (w większości przypadków będzie to stan otwarty)
- **WAŻNOŚĆ**: poziom ważności zaleceń

Wybierz grupę zasobów, aby otworzyć opcję **Utwórz reguły sterowania aplikacjami**:

![Reguły sterowania aplikacjami](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

W obszarze **Wybierz maszyny wirtualne** zapoznaj się z listą zalecanych maszyn wirtualnych i wyczyść zaznaczenie pól wyboru obok tych, do których nie ma być stosowane sterowanie aplikacjami. W obszarze **Wybierz procesy dla reguł umieszczania na białej liście** zapoznaj się z listą zalecanych aplikacji i wyczyść zaznaczenie pól wyboru obok tych, do których nie ma być stosowana ta funkcja. Lista zawiera:

- **NAZWA**: pełna ścieżka aplikacji
- **PROCESY**: ile aplikacji znajduje się w każdej ścieżce
- **WSPÓLNE**: wartość „prawda” wskazuje, że te procesy są wykonywane na większości maszyn wirtualnych w tej grupie zasobów.
- **MOŻLIWE DO WYKORZYSTANIA**: ikona ostrzeżenia wskazuje, czy osoba atakująca może wykorzystać aplikacje do pominięcia listy dozwolonych aplikacji. Zdecydowanie zaleca się przejrzenie tych aplikacji przed ich zatwierdzeniem. 

Po wybraniu opcji kliknij przycisk **Utwórz**. Domyślnie usługa Security Center zawsze uruchamia sterowanie aplikacjami w trybie *inspekcji*. Po upewnieniu się, że lista dozwolonych nie wpływa niekorzystnie na używane obciążenia, można zmienić tryb na tryb *wymuszania*.

> [!NOTE]
> Zgodnie z najlepszymi rozwiązaniami dotyczącymi zabezpieczeń usługa Security Center zawsze próbuje utworzyć regułę wydawcy dla aplikacji, która powinna znaleźć się na liście dozwolonych. Jedynie w przypadku aplikacji bez informacji o wydawcy (bez podpisu) zostanie utworzona reguła ścieżki dla pełnej ścieżki konkretnego pliku EXE.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Edytowanie i monitorowanie grupy, dla której skonfigurowano sterowanie aplikacjami

Aby edytować i monitorować grupę, dla której skonfigurowano sterowanie aplikacjami, kliknij pozycję **SKONFIGUROWANO** w obszarze **Grupy zasobów**:

![Grupy zasobów](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

Lista zawiera:

- **NAZWA**: nazwa subskrypcji i grupy zasobów
- **MASZYNY WIRTUALNE**: liczba maszyn wirtualnych w grupie zasobów
- **TRYB**: w trybie inspekcji będą rejestrowane próby uruchomienia aplikacji spoza listy dozwolonych, a w trybie blokowania uruchamianie takich aplikacji będzie blokowane
- **WAŻNOŚĆ**: poziom ważności zaleceń

Wybierz grupę zasobów, aby wprowadzić zmiany na stronie **Edytuj zasady sterowania aplikacjami**.

![Ochrona](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

W obszarze **Tryb ochrony** do wyboru są następujące opcje:
- **Inspekcja**: w tym trybie rozwiązanie sterowania aplikacjami nie będzie wymuszać reguł, a jedynie prowadzić inspekcję działań na chronionych maszynach wirtualnych. Jest to zalecane w scenariuszach, w których trzeba najpierw przyjrzeć się ogólnym zachowaniom, zanim zablokuje się uruchamianie aplikacji na docelowej maszynie wirtualnej.
- **Wymuszanie**: w tym trybie rozwiązanie sterowania aplikacjami będzie wymuszać reguły i aplikacje, które nie powinny być uruchamiane, będą blokowane. 

Jak wcześniej wspomniano, domyślnie nowe zasady sterowania aplikacjami są zawsze skonfigurowane w trybie *inspekcji*. W obszarze **Rozszerzenie zasad** można dodać własne ścieżki aplikacji, które mają znaleźć się na liście dozwolonych. Po dodaniu tych ścieżek usługa Security Center utworzy odpowiednie reguły dla tych aplikacji i doda je do już wdrożonych reguł. W sekcji **Problemy** są wyświetlane wszelkie bieżące naruszenia.

![Problemy](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

Ta lista zawiera:

- **PROBLEMY**: wszelkie zarejestrowane naruszenia. Mogą to być:
    - **Naruszenia zablokowane (ViolationsBlocked)**: gdy rozwiązanie jest w trybie wymuszania i próbowano uruchomić aplikację spoza listy dozwolonych.
    - **Naruszenia poddane inspekcji (ViolationsAudited)**: gdy rozwiązanie jest w trybie inspekcji i uruchomiono aplikację spoza listy dozwolonych.
    - **Ręczne naruszenie reguł (RulesViolatedManually)**: gdy użytkownik próbował ręcznie skonfigurować reguły na maszynach wirtualnych, a nie za pomocą portalu zarządzania ASC.
- **LICZBA MASZYN WIRTUALNYCH**: liczba maszyn wirtualnych z tym typem problemu.

Kliknięcie dowolnego z tych wierszy spowoduje przekierowanie do strony [dziennika aktywności platformy Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), na której można sprawdzić informacje na temat wszystkich maszyn wirtualnych z tego typu naruszeniem. Kliknięcie trzech kropek znajdujących się na końcu wiersza pozwala usunąć dany wpis. Sekcja **Skonfigurowane maszyny wirtualne** zawiera informacje o maszynach wirtualnych, których dotyczą te reguły. 

![Skonfigurowane maszyny wirtualne](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

Sekcja **Reguły umieszczania na białej liście na podstawie dostawcy** zawiera listę aplikacji, dla których na podstawie informacji z certyfikatów znalezionych dla poszczególnych z nich utworzono reguły wydawcy. Aby uzyskać więcej informacji, zobacz [Opis reguł wydawców w funkcji Applocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker).

![Reguły umieszczania na liście dozwolonych](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

Kliknięcie trzech kropek znajdujących się na końcu wiersza pozwala usunąć daną regułę. Sekcja **Reguły umieszczania na białej liście na podstawie ścieżki** zawiera całą ścieżkę aplikacji (obejmującą plik wykonywalny) dla aplikacji, które nie są podpisane za pomocą certyfikatu cyfrowego, ale są obecne w regułach umieszczania na liście dozwolonych. 

> [!NOTE]
> Domyślnie, zgodnie z najlepszymi rozwiązaniami dotyczącymi zabezpieczeń, usługa Security Center zawsze próbuje utworzyć regułę wydawcy dla pliku EXE, który powinien znaleźć się na liście dozwolonych. Jedynie w przypadku pliku EXE bez informacji o wydawcy (bez podpisu) zostanie utworzona reguła ścieżki dla pełnej ścieżki konkretnego pliku EXE.

![Reguły umieszczania na białej liście na podstawie ścieżki](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

Lista zawiera:
- **NAZWA**: pełna ścieżka pliku wykonywalnego
- **MOŻLIWE DO WYKORZYSTANIA**: wartość „prawda” wskazuje, że osoba atakująca może wykorzystać aplikacje do pominięcia listy dozwolonych aplikacji.  

Kliknięcie trzech kropek znajdujących się na końcu wiersza pozwala usunąć daną regułę. Po wprowadzeniu zmian możesz kliknąć przycisk **Zapisz**. Jeśli nie chcesz stosować zmian, kliknij przycisk **Odrzuć**.

### <a name="not-recommended-list"></a>Lista bez zaleceń

Usługa Security Center zaleci dodanie aplikacji do listy dozwolonych tylko w przypadku maszyn wirtualnych, na których uruchamiane są stabilne zestawy aplikacji. Zalecenia nie zostaną utworzone w przypadku ciągłych zmian aplikacji na skojarzonych maszynach wirtualnych. 

![Zalecenie](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Lista zawiera:
- **NAZWA**: nazwa subskrypcji i grupy zasobów.
- **MASZYNY WIRTUALNE**: liczba maszyn wirtualnych w grupie zasobów.

## <a name="see-also"></a>Zobacz też
W tym dokumencie omówiono korzystanie z funkcji adaptacyjnego sterowania aplikacjami w usłudze Azure Security Center w celu tworzenia listy dozwolonych aplikacji uruchamianych na maszynach wirtualnych platformy Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Dowiedz się, jak zarządzać alertami i reagować na zdarzenia związane z bezpieczeństwem w usłudze Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Informacje o alertach zabezpieczeń w usłudze Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Poznaj różne typy alertów zabezpieczeń.
* [Przewodnik rozwiązywania problemów z usługą Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Dowiedz się, jak rozwiązywać typowe problemy z usługą Security Center. 
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.


