---
title: Dostosuj ustawienia migracji Azure oceny | Dokumentacja firmy Microsoft
description: "Opisuje sposób ustawiania i uruchamiania ocenę do migrowania maszyn wirtualnych VMware do platformy Azure przy użyciu usługi Azure planowania migracji"
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: ce47790f6214864afdba33eb5cbe3a9e49b81cd5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="customize-an-assessment"></a>Dostosowywanie oceny

[Azure migracji](migrate-overview.md) tworzy oceny z ustawieniami domyślnymi. Po utworzeniu ocenę, można zmodyfikować te ustawienia domyślne, korzystając z instrukcji w tym artykule.


## <a name="edit-assessment-values"></a>Edytuj wartości oceny

1. W projekcie migracji Azure **oceny** , wybierz ocenę i kliknij przycisk **Edytuj właściwości**.
2. Zmodyfikuj ustawienia zgodnie z poniższą tabelą.

    **Ustawienie** | **Szczegóły** | **Domyślne**
    --- | --- | ---
    **Lokalizacja docelowa** | Lokalizacja platformy Azure, do której chcesz przeprowadzić migrację. |  Zachodnie stany USA 2 jest domyślną lokalizacją.
    **Nadmiarowość magazynu** | Typ magazynu, który będzie używany przez maszyny wirtualne platformy Azure po zakończeniu migracji. | Tylko [magazyn lokalnie nadmiarowy (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) replikacji jest obecnie obsługiwany.
    **Współczynnik komfortu** | Współczynnik komfort jest buforu, który jest używany podczas oceny. Umożliwia ona konta dla elementów, takich jak użycie okresach, krótka historia wydajności, może się zwiększyć w przyszłości użycia. | Ustawienie domyślne to 1.3 x.
    **Historia wydajności** | Czas używany podczas obliczania Historia wydajności. | Domyślny to jeden miesiąc.
    **Procentowy wykorzystania** | Wartość percentylu wziąć pod uwagę historii wydajności. | Domyślnie jest to 95%.
    **Warstwa cenowa** | Można określić [warstwy cenowej](https://azure.microsoft.com/blog/basic-tier-virtual-machines-2/) dla maszyny Wirtualnej.  | Domyślnie [standardowe](../virtual-machines/windows/sizes-general.md) warstwy jest używany.
    **Oferta** | [Azure oferuje](https://azure.microsoft.com/support/legal/offer-details/) , stosowane. | [Płatność za rzeczywiste użycie](https://azure.microsoft.com/offers/ms-azr-0003p/) jest ustawieniem domyślnym.
    **Waluty** | Waluta rozliczeniowa. | Domyślnie jest USD.
    **Rabat (%)** | Rabatów specyficzne dla subskrypcji jest wyświetlany u góry wszystkie oferty. | Ustawieniem domyślnym jest 0%.
    **Korzyści Użyj hybrydowe platformy Azure** | Wskazuje, czy jest zarejestrowany w [Azure hybrydowego Użyj korzyści](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Jeśli ustawiona tak, ceny z systemem innym niż Windows Azure są należy wziąć pod uwagę dla maszyn wirtualnych systemu Windows. | Wartość domyślna to Yes.

3. Kliknij przycisk **zapisać** zaktualizować oceny.


## <a name="next-steps"></a>Następne kroki

[Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
