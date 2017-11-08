---
title: "Zarządzanie kosztami przy użyciu usługi Azure Management koszt | Dokumentacja firmy Microsoft"
description: "Zarządzanie kosztami przy użyciu koszt alokacji i ogólnej analizy kosztów i raportów obciążenia zwrotnego."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/06/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 83ddc0cb4227235069b0027a24a52f4d8e818126
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="manage-costs-by-using-azure-cost-management"></a>Zarządzanie kosztami przy użyciu usługi Azure Management koszt

Zarządzanie kosztami i tworzenia raportów ogólnej analizy kosztów w Azure kosztów zarządzania przez Cloudyn Przydzielając koszty tagów. Proces alokacji kosztów przypisuje kosztów zasobów w chmurze wykorzystane. Koszty pełni są przydzielane, gdy wszystkie zasoby są podzielone przy użyciu tagów. Po koszty są przydzielone, można udostępnić ogólnej analizy kosztów lub obciążenia zwrotnego użytkownikom pulpity nawigacyjne i raporty. Jednak wiele zasobów może być nieoznakowanego lub untaggable użyciem kosztów zarządzania.

Na przykład możesz pobrać zwracane engineering kosztów. Musisz mieć możliwość Pokaż zespołu inżynieryjnego wymagającym określoną kwotę, na podstawie kosztów zasobów. Można je wyświetlić raport dla wszystkich zasobów wykorzystanych, które są oznaczone *engineering*.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Skorzystaj z znaczniki niestandardowe, aby przydzielić kosztów.
> * Tworzenie raportów ogólnej analizy kosztów i obciążeń zwrotnych.

## <a name="use-custom-tags-to-allocate-costs"></a>Użyj niestandardowe znaczniki, aby przydzielić koszty

Po uruchomieniu alokacji kosztów w pierwszej kolejności należy wykonać jest zdefiniować zakres przy użyciu modelu kosztów. Model koszt nie zmienia kosztów, rozpowszechnia je. Podczas tworzenia modelu koszt jest segmentu danych koszt jednostki, konta lub subskrypcji, a także wiele tagów. Typowe przykład tagi mogą zawierać kod rozliczeń, Centrum kosztu lub nazwa grupy. Tagi też pomóc Ci wykonać ogólne analizy kosztów lub obciążenia zwrotnego do innych części organizacji.

Aby utworzyć niestandardowy koszt modelu alokacji, wybierz **koszt** &gt; **kosztów zarządzania** &gt; **alokacji kosztów 360°** menu raportu.

![Wybór 360 alokacji kosztu](./media/tutorial-manage-costs/cost-allocation-360.png)

Na **360 alokacji koszt** wybierz pozycję **Dodaj** , a następnie wprowadź nazwę i opis dla modelu kosztów. Wybierz wszystkie konta lub indywidualnych kont. Jeśli chcesz użyć indywidualnych kont, można wybrać wiele kont od wielu dostawców usługi w chmurze. Następnie kliknij przycisk **kategoryzacji** wybrać odnalezionych tagi, które kategoryzowanie danych kosztów. Wybierz tagi (kategorie), które chcesz dołączyć do modelu. W poniższym przykładzie **jednostki** znacznik jest zaznaczony.

![Przykład koszt modelu kategoryzacji](./media/tutorial-manage-costs/cost-model01.png)



W przykładzie pokazano, że 14,444 $ jest bez kategorii (bez tagów).

Następnie wybierz pozycję **bez kategorii zasobów** i wybierz usługi, które miał nieprzydzielone kosztów. Następnie należy zdefiniować reguły można przydzielić koszty.

Na przykład możesz pobrać koszty magazynu Azure i dystrybuować koszty jednakowo na maszynach wirtualnych platformy Azure (VM). Aby to zrobić, wybierz **magazynowania Azure** usługi, wybierz opcję **proporcjonalny do według kategorii**, a następnie wybierz **maszyny Wirtualnej i Azure**. Następnie wybierz opcję **Utwórz**.

![Reguła alokacji modelu przykład kosztów, rozkładając](./media/tutorial-manage-costs/cost-model02.png)



W przykładzie różnych możesz przydzielić koszty sieci platformy Azure do określonej jednostki biznesowej w organizacji. Aby to zrobić, wybierz **Azure i sieci** usługi, a następnie wybierz **rozkładu jawną**. Następnie ustaw procent dystrybucji do 100 i wybierz jednostki biznesowej —**G&amp;A** na poniższej ilustracji:

![Przykład koszt modelu alokacji reguły dla określonej jednostki biznesowej](./media/tutorial-manage-costs/cost-model03.png)



Dla wszystkich pozostałych zasobów bez kategorii należy utworzyć dodatkowe reguły.

Jeśli masz wystąpienia nieprzydzielonego zastrzeżone Amazon Web usługi (AWS), można przypisać je do oznakowanych kategorii z **zastrzeżone wystąpień**.

Aby wyświetlić informacje o dostępnych opcjach, wprowadzone do przydzielenia kosztów, wybierz **Podsumowanie**. Aby zapisać informacje i kontynuować pracę na dodatkowe reguły później, wybierz **Zapisz jako projekt**. Lub, aby zapisać informacje i Cloudyn rozpoczyna przetwarzanie modelu alokacji kosztów, wybierz **Zapisz i aktywować**.

Na liście modeli koszt przedstawiono nowego modelu koszt za pomocą **przetwarzania stanu**. Może upłynąć pewien czas, zanim aktualizacji bazy danych Cloudyn modelu kosztów. Po zakończeniu przetwarzania stan zostanie zaktualizowany do **Ukończono**. Można wyświetlić danych z modelu koszt raportu analizy kosztów w obszarze **rozszerzone filtry** &gt; **modelu koszt**.

### <a name="category-manager"></a>Menedżer kategorii

Menedżer kategorii jest narzędzie czyszczenia danych, które pomaga scalić wartości wiele kategorii (tagów), aby utworzyć nowe. Jest proste narzędzie opartego na regułach gdzie wybierz kategorię i tworzenie reguł do scalenia istniejące wartości. Na przykład może być istniejące kategorie **R&amp;D** i **deweloperów** którym reprezentują zarówno grupy deweloperskiej.

W portalu Cloudyn kliknij koło zębate symbol w górnym prawym rogu i wybierz pozycję **menedżera kategorii**. Aby utworzyć nową kategorię, wybierz znak plus (**+**). Wprowadź nazwę dla kategorii, a następnie w obszarze **klucze**, wprowadź klucze kategorii, które mają zostać uwzględnione w nowej kategorii.

Podczas definiowania regułę można dodać wiele wartości z warunkiem lub. Można również wykonać pewne podstawowe operacje na ciągach. Dla obu przypadkach kliknij symbol wielokropka (**...** ) z prawej strony **reguły**.

Aby zdefiniować nową regułę w **reguły** obszaru, Utwórz nową regułę. Na przykład wprowadź **deweloperów** w obszarze **reguły** , a następnie wprowadź **R&amp;D** w obszarze **akcje**. Gdy wszystko będzie gotowe, Zapisz nową kategorię.

Na poniższej ilustracji przedstawiono przykład reguł utworzonych dla nową kategorię o nazwie **obciążenia pracą**:

![Przykład kategorii](./media/tutorial-manage-costs/category01.png)



## <a name="create-showback-and-chargeback-reports"></a>Tworzenie raportów ogólnej analizy kosztów i obciążeń zwrotnych

Metoda, która organizacji używać do wykonywania ogólnej analizy kosztów i obciążeń zwrotnych zależy w dużej mierze. Jednak służy pulpity nawigacyjne i raporty w portalu Cloudyn jako podstawa albo celu. Można zapewnić dostęp użytkownika innym osobom w organizacji, tak aby mogli wyświetlać pulpity nawigacyjne i raporty na żądanie. Wszystkie koszty analizy raporty obsługują ogólnej analizy kosztów, ponieważ pokazywały użytkowników zasobów, które są używane. I ich użytkownicy mogą przejść do szczegółów w koszt lub użycia dane, które są specyficzne dla ich grupy w organizacji.

Aby wyświetlić wyniki alokacji kosztów, otwórz raport analizy kosztów i wybierz utworzonego modelu kosztów. Następnie należy dodać grupę przez jedną lub więcej tagów wybrane w modelu kosztów.

![Raport analizy kosztów](./media/tutorial-manage-costs/cost-analysis.png)

Można łatwo tworzyć i zapisywać raporty skupiających się na określonych usług używane przez określonych grup. Na przykład może być działu, który często używa maszynach wirtualnych platformy Azure. Można utworzyć raport, który jest filtrowana na maszynach wirtualnych Azure, aby pokazać zużycia i kosztów.

Należy podać dane migawki do innych zespołów po wyeksportowaniu wszystkich raportów w formacie PDF lub CSV.


## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Skorzystaj z znaczniki niestandardowe, aby przydzielić kosztów.
> * Tworzenie raportów ogólnej analizy kosztów i obciążeń zwrotnych.



Aby dowiedzieć się więcej na temat rozpoczynania pracy z Cloudyn i używania funkcji, przejdź do dokumentacji Cloudyn.

> [!div class="nextstepaction"]
> [Dokumentacja Cloudyn](https://support.cloudyn.com/hc/)
