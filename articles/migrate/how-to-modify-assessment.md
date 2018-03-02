---
title: Dostosuj ustawienia migracji Azure oceny | Dokumentacja firmy Microsoft
description: "Opisuje sposób ustawiania i uruchamiania ocenę do migrowania maszyn wirtualnych VMware do platformy Azure przy użyciu usługi Azure planowania migracji"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 02/26/2018
ms.author: raynew
ms.openlocfilehash: efb4ad59d25a0c1209e4f0f6cd406c2f0d48159c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="customize-an-assessment"></a>Dostosowywanie oceny

[Azure migracji](migrate-overview.md) tworzy oceny z właściwości domyślnych. Po utworzeniu ocenę, można zmodyfikować właściwości domyślnych, korzystając z instrukcji w tym artykule.


## <a name="edit-assessment-properties"></a>Edytowanie właściwości oceny

1. W **oceny** strony migrację, wybierz ocenę i kliknij przycisk **Edytuj właściwości**.
2. Modyfikowanie właściwości zgodnie z poniższej tabeli:

    **Ustawienie** | **Szczegóły** | **Domyślne**
    --- | --- | ---
    **Lokalizacja docelowa** | Lokalizacja platformy Azure, do której chcesz przeprowadzić migrację.<br/><br/> Migracji Azure obsługuje obecnie 30 regionów, w tym Australia Wschodnia, Południowo-Wschodnia Australia, Brazylia Południowa, Kanada centralnej, Kanada Wschodnia, Indie środkowe, środkowe stany USA, wschodnie Chin, Chin Północna, Azja Wschodnia, wschodnie stany USA, Niemcy centralnej, Niemcy północno-wschodnie, wschodnie stany USA 2, Japonia Wschodnia, Japonia Zachodnia Korei Południowej środkowe, Korea, Północna środkowe stany USA, Europa Północna, południowo środkowe stany USA, Azja południowo-wschodnia, Indie Południowe, Wielka Brytania Południowa, Wielka Brytania Zachodnia, zachodnie środkowe stany USA, Europa Zachodnia, Indie Zachodnie, zachodnie stany USA i US2 zachodnie. |  Zachodnie stany USA 2 jest domyślną lokalizacją.
    **Nadmiarowość magazynu** | Typ nadmiarowość magazynu, który będzie używany przez maszyny wirtualne Azure po zakończeniu migracji. | [Magazyn lokalnie nadmiarowy (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) jest wartością domyślną. Azure obsługuje tylko migracji zarządzane opartej na dyskach: ocen i dysków zarządzanych obsługują tylko LRS, dlatego właściwość obecnie zawiera tylko opcja LRS. 
    **Kryterium zmiany rozmiaru** | Kryterium do użycia przez usługę Azure Migrate w celu określenia odpowiedniego rozmiaru dla maszyn wirtualnych platformy Azure. Należy albo czy *na podstawie wydajności* zmiany rozmiaru lub rozmiar maszyn wirtualnych *jako lokalną*, bez uwzględniania Historia wydajności. | Na podstawie rozmiaru jest domyślną opcją.
    **Historia wydajności** | Czas trwania wziąć pod uwagę do oceny wydajności maszyn wirtualnych. Ta właściwość ma zastosowanie tylko w przypadku zmiany rozmiaru kryterium *wydajności na podstawie rozmiaru*. | Domyślny to jeden dzień.
    **Procentowy wykorzystania** | Wartość próbki wydajność, Ustaw to zostać uwzględnione podczas doboru wielkości. Ta właściwość ma zastosowanie tylko w przypadku zmiany rozmiaru kryterium *wydajności na podstawie rozmiaru*.  | Domyślnie jest 95. percentyl.
    **Warstwa cenowa** | Możesz określić [warstwę cenową (Podstawowa/Standardowa)](../virtual-machines/windows/sizes-general.md) dla docelowych maszyn wirtualnych platformy Azure. Na przykład jeśli planujesz migrację środowiska produkcyjnego, rozważ warstwę Standardowa, która udostępnia maszyny wirtualne o małych opóźnieniach, lecz które mogą kosztować więcej. Z drugiej strony w przypadku środowiska deweloperskiego i testowego rozważ warstwę Podstawowa, która udostępnia maszyny wirtualnych o większych opóźnieniach, lecz przy niższych kosztach. | Domyślnie jest używana warstwa [Standardowa](../virtual-machines/windows/sizes-general.md).
    **Współczynnik komfortu** | Podczas oceny usługa Azure Migrate uwzględnia bufor (współczynnik komfortu). Jest on stosowany do wszystkich danych użycia maszyn wirtualnych (procesora, pamięci, dysku i sieci). Współczynnik komfortu uwzględnia kwestie, takie jak okresowe użycie, krótka historia wydajności i prawdopodobne zwiększenie użycia w przyszłości.<br/><br/> Na przykład 10-rdzeniowa maszyna wirtualna o użyciu na poziomie 20% jest w normalnych warunkach równoważna 2-rdzeniowej maszynie wirtualnej. Jednak wynik zastosowania współczynnika komfortu o wartości 2 daje 4-rdzeniową maszynę wirtualną. | Ustawienie domyślne to 1.3 x.
    **Oferta** | [Oferta platformy Azure](https://azure.microsoft.com/support/legal/offer-details/) , które są zarejestrowane w usłudze. | [Płatność za rzeczywiste użycie](https://azure.microsoft.com/offers/ms-azr-0003p/) jest ustawieniem domyślnym.
    **Waluty** | Waluta rozliczeniowa. | Domyślnie jest USD.
    **Rabat (%)** | Rabatów specyficzne dla subskrypcji jest wyświetlany u góry oferta platformy Azure. | Ustawieniem domyślnym jest 0%.
    **Korzyści hybrydowe platformy Azure** | Określ, jeśli masz pakiet software assurance i kwalifikują się do [korzyści hybrydowego Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Jeśli ustawiona tak, ceny z systemem innym niż Windows Azure są traktowane jako dla maszyn wirtualnych systemu Windows. | Wartość domyślna to Yes.

3. Kliknij przycisk **zapisać** zaktualizować oceny.


## <a name="next-steps"></a>Kolejne kroki

[Dowiedz się więcej](concepts-assessment-calculation.md) na temat sposobu obliczania ocen.
