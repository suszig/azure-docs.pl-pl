---
title: "Opis koszt raportów w programie Azure kosztów zarządzania | Dokumentacja firmy Microsoft"
description: "Ten artykuł pomaga w zrozumieniu Cloudyn raportów podstawowej struktury i funkcji."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 38c1313f42a58403e158cad9c2930b6541da5adc
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="understanding-cost-reports"></a>Omówienie raportów kosztów

Ten artykuł pomaga w zrozumieniu Cloudyn raportów podstawowej struktury i funkcji. Większość raportów Cloudyn są intuicyjne i zapewnia jednolite wyglądu i działania. Po przeczytaniu tego artykułu są gotowe do użycia wszystkich raportów. Wiele funkcji dostępnych w całej różne raporty, umożliwiając Przejdź raporty z łatwością. Raporty są można dostosowywać i można wybrać kilka opcji do obliczania i wyświetlania wyników.

## <a name="report-fields-and-options"></a>Opcje i pola raportu

Poniżej przedstawiono wygląd, na przykład raport koszt w czasie. Większość Cloudyn raporty mają podobny układ.

![przykładowy raport](./media/understanding-cost-reports/sample-report.png)

Poszczególnych numerowane obszar obrazu poprzedniego jest szczegółowo opisane w poniższych informacji:

1. **Zakres dat**

    Użyj listy zakres dat, aby określić przedział czasu raportu za pomocą wstępnie ustawionych lub niestandardowy.
2. **Zapisany filtr**

    Użyj listy zapisany filtr, aby zapisać bieżące grupy i filtry, które są stosowane do raportu. Zapisane filtry są dostępne przez raportów kosztów i wydajności, w tym:

      - Analiza kosztów
      - Alokacja
      - Zarządzanie zasobami
      - Optymalizacja

  Wpisz nazwę filtru, a następnie kliknij pozycję **zapisać**.

3. **Tagi**

    Użyj obszaru tagi do grupy według kategorii tagu. Tagi wyświetlane w menu są Azure dział lub tagi Centrum kosztów lub tagi koszt Cloudyn przez jednostki i subskrypcji. Wybierz tagi, które mają być filtrowane wyniki. Możesz także wpisać nazwę znacznika (— słowo kluczowe) mają być filtrowane wyniki.

    ![Wybierz opcje](./media/understanding-cost-reports/select-options.png)

    Kliknij przycisk **Dodaj** można dodać nowego filtru.

    ![Dodaj filtr](./media/understanding-cost-reports/add-filter.png)

    Tag, grupowanie i filtrowanie nie odnosi się do zasobów platformy Azure lub tagi grupy zasobów.

    Alokacja kosztu tag grupowanie i filtrowanie są dostępne w **grup** opcji menu.

4. **Grupami w raportach**

    Użyj grup w analizy kosztów raportów do wyświetlenia standard wyszczególnione kategorii z rozliczeniami dane w raporcie.  Jednak grup w Pokaż raporty alokacji kosztów wyświetlanie znaczników kategorii. Na podstawie tag kategorie są definiowane w modelu alokacji kosztów i standardowe kategorii dla pozycji z danych dotyczących rozliczeń.

    ![tagi grup](./media/understanding-cost-reports/groups-tags01.png)

    ![tagi grup](./media/understanding-cost-reports/groups-tags02.png)

    W raportach alokacji kosztów grup w kategorie na podstawie tag grupy mogą być następujące:
      - Tagi
      - tagi grupy zasobów
      - Cloudyn koszt tagów jednostki
      - Kategorie tagu subskrypcji dla celów alokacji kosztów

  Przykładami mogą być następujące:
     - Centrum kosztu
     - Dział
     - Aplikacja
     - Środowisko
     - Kod kosztów

5. **Filtry**

    Użyj filtrów pojedynczego lub wielokrotnego wyboru, aby ustawić zakresy wybranych wartości. Aby ustawić filtr, kliknij przycisk **Dodaj** , a następnie wybierz filtr kategorii i wartości.

6. **Model kosztu**

    Koszt modelu umożliwia wybierz wcześniej utworzony za pomocą koszt alokacji 360 model kosztów. Może mieć wielu Cloudyn koszt modeli, w zależności od wymagań alokacji kosztów. Niektóre z zespołów organizacji może kosztów alokacji wymagania, które różnią się od innych użytkowników. Każdy zespół może mieć własne dedykowane koszt modelu.

    Informacji o tworzeniu definicję modelu koszt alokacji, zobacz [użyć znaczniki niestandardowe, aby przydzielić koszty](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **Amortyzacja**

    Użyj amortyzacji w raportach alokacji kosztów, aby zobaczyć użycie z systemem innym niż na podstawie opłaty za usługi lub jednorazowe dostawcami koszty i rozpowszechniania kosztów wraz z upływem czasu równomiernie podczas ich cykl życia. Przykłady jednorazowe opłaty mogą być następujące:
    - Opłaty roczne
    - Opłaty roczne składniki zabezpieczeń
    - Zarezerwowane wystąpień zakupu opłat
    - Niektóre elementy portalu Azure Marketplace.

  W obszarze amortyzacji, wybierz **amortyzowanego koszt** lub **rzeczywisty koszt**.

8. **Rozdzielczość**

    Umożliwia wybór rozwiązania czasu, w ramach zakresu dat zaznaczonego rozpoznawania. Rozdzielczość czasu określa sposób jednostki są wyświetlane w raporcie i może być:
    - Codziennie
    - Co tydzień
    - Miesięczne
    - Co kwartał
    - Roczne

9. **Zasady alokacji**

    Użyj reguł alokacji na stosowanie lub wyłączyć obliczanie kosztu alokacji kosztów. Można włączyć lub wyłączyć Obliczanie alokacji kosztów dla danych dotyczących rozliczeń. Ponowne obliczanie ma zastosowanie do wybranych kategorii w raporcie. Umożliwia ona ocenić wpływ ponowne obliczenie kosztów alokacji względem danych pierwotnych rozliczeń.

10. **Bez kategorii**

    Użyj bez kategorii, aby dołączyć lub wykluczyć bez kategorii kosztów w raporcie.

11. **Pokaż/Ukryj pola**

    Pokaż/Ukryj opcji nie ma żadnego efektu w raportach.

12.   **Format wyświetlania**

    Użyj formatu wyświetlania, aby wybrać różne widoki wykresu lub tabeli.

    ![Format wyświetlania](./media/understanding-cost-reports/display-formats.png)

13. **Kolor wielu**

    Użyj wielu kolorów, aby ustawić kolor wykresy w raporcie.

14. **Akcje**

    Użyj akcje, aby zapisać, wyeksportować lub ustalania harmonogramu raportu.

## <a name="next-steps"></a>Kolejne kroki

- Jeśli nie zostało już ukończone pierwszy samouczek koszt zarządzania, przeczytaj je na [Przejrzyj użycia i koszty](tutorial-review-usage.md).
