---
title: "Wyświetl trend miesięczny koszt szacowany laboratorium w usłudze Azure DevTest Labs | Dokumentacja firmy Microsoft"
description: "Więcej informacji o usłudze Azure DevTest Labs miesięczne szacowany koszt wykresu trendu."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: b3ad1ead522908d4b41b7cca98d20ac91664998e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Wyświetl trend miesięczny koszt szacowany laboratorium w usłudze Azure DevTest Labs
Funkcja zarządzania koszt DevTest Labs pomaga śledzić koszt laboratorium. W tym artykule przedstawiono sposób użycia **miesięczny Trend szacowany koszt** wykresu, aby wyświetlić bieżącego miesiąca kalendarzowego szacowany koszt do daty i szacowany koszt koniec miesiąca dla bieżącego miesiąca kalendarzowego. W tym artykule Dowiedz się jak wyświetlić miesięczne wykresu trendu szacowany koszt w portalu Azure.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Wyświetlanie wykresu miesięczny Trend szacowany koszt
Aby wyświetlić na wykresie miesięczny Trend szacowany koszt, wykonaj następujące kroki: 

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Wybierz **więcej usług**, a następnie wybierz **DevTest Labs** z listy.
3. Z listy labs wybierz żądany laboratorium.   
4. W bloku laboratorium, wybierz **koszt ustawienia**.
5. W laboratorium **koszt ustawienia** bloku, wybierz opcję **trend koszt laboratorium**.
6. Poniższy zrzut ekranu przedstawia przykład wykresu kosztów. 
   
    ![Wykres kosztu](./media/devtest-lab-configure-cost-management/graph.png)

**Szacowany koszt** wartość jest bieżącego miesiąca kalendarzowego szacowany koszt do daty. **Koszt planowany** jest szacowany koszt dla całego bieżącego miesiąca kalendarzowego obliczane przy użyciu koszt laboratorium dla poprzedniego pięć dni.

Kwoty kosztów są zaokrąglona w górę do najbliższej liczby całkowitej. Na przykład: 

* 5.01 Zaokrągla do 6 
* 5.50 Zaokrągla do 6
* 5.99 Zaokrągla do 6

W trakcie jego stany powyżej wykresu, są koszty Zobacz na wykresie *szacowany* koszty przy użyciu [płatność za rzeczywiste użycie](https://azure.microsoft.com/offers/ms-azr-0003p/) stawek oferty.
Ponadto są następujące *nie* uwzględnionych podczas obliczania kosztów:

* Dostawca usług Kryptograficznych i Dreamspark subskrypcje obecnie nie są obsługiwane jako używa Azure DevTest Labs [Azure API rozliczeń](../billing/billing-usage-rate-card-overview.md) do obliczania kosztów, który nie obsługuje dostawcy usług Kryptograficznych lub Dreamspark subskrypcji laboratorium.
* Twoje stawek oferty. Obecnie nie możemy do użycia z stawek oferty (wyświetlany w obszarze subskrypcji) czy musisz mieć negocjowane z Microsoft lub partnerów. Użyto płatność za rzeczywiste użycie stawki.
* Twoje podatki
* Twoje zniżki
* Waluta. Obecnie koszt laboratorium jest wyświetlany tylko w walucie USD.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Wpisy na blogu pokrewne
* [Więcej opcji Zachowaj koszt zgodnie z planem w usłudze DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Dlaczego koszt progi?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Następne kroki
Poniżej przedstawiono niektóre czynności, aby spróbować obok:

* [Definiowanie zasad laboratorium](devtest-lab-set-lab-policy.md) — Dowiedz się, jak skonfigurowanie różnych zasad używane do sterowania korzystania laboratorium i jego maszyn wirtualnych. 
* [Tworzenie niestandardowego obrazu](devtest-lab-create-template.md) — podczas tworzenia maszyny Wirtualnej, należy określić podstawowy, który może być niestandardowy obraz lub obrazu z witryny Marketplace. W tym artykule przedstawiono sposób tworzenia niestandardowego obrazu z pliku VHD.
* [Konfigurowanie portalu Marketplace obrazów](devtest-lab-configure-marketplace-images.md) — DevTest Labs obsługuje tworzenie maszyn wirtualnych, oparte na obrazach portalu Azure Marketplace. W tym artykule przedstawiono sposób określić, które, portalu Azure Marketplace obrazy mogą być używane podczas tworzenia maszyn wirtualnych w laboratorium.
* [Utwórz maszynę Wirtualną w laboratorium](devtest-lab-add-vm-with-artifacts.md) -ilustruje sposób tworzenia maszyny Wirtualnej z obrazu podstawowego (albo niestandardowe lub Marketplace) oraz sposób pracy z artefaktami w maszynie Wirtualnej.

