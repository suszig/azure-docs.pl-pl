---
title: Dostosuj ustawienia migracji Azure oceny | Dokumentacja firmy Microsoft
description: "Opisuje sposób ustawiania i uruchamiania ocenę do migrowania maszyn wirtualnych VMware do platformy Azure przy użyciu usługi Azure planowania migracji"
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a068b9c7-5f87-4fe1-90b9-3be48d91aa3f
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 6db4ff050dbf8d4e08415226931e71f964525068
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2017
---
# <a name="customize-an-assessment"></a>Dostosowywanie oceny

[Azure migracji](migrate-overview.md) tworzy oceny z ustawieniami domyślnymi. Po utworzeniu ocenę, można zmodyfikować te ustawienia domyślne, korzystając z instrukcji w tym artykule.


## <a name="edit-assessment-values"></a>Edytuj wartości oceny

1. W projekcie migracji Azure **oceny** , wybierz ocenę i kliknij przycisk **Edytuj właściwości**.
2. Zmodyfikuj ustawienia zgodnie z poniższą tabelą.

    **Ustawienie** | **Szczegóły** | **Domyślne**
    --- | --- | ---
    **Lokalizacja docelowa** | Lokalizacja platformy Azure, do którego chcesz przeprowadzić migrację. |  Tylko wschodnie stany USA jest obecnie obsługiwane.
    **Nadmiarowość magazynu** | Typ magazynu, który będzie używany przez maszyny wirtualne Azure po zakończeniu migracji. | Tylko [magazyn lokalnie nadmiarowy (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) replikacji jest obecnie obsługiwany.
    **Współczynnik komfort** | Współczynnik komfort jest buforu, który jest używany podczas oceny. Umożliwia ona konta dla elementów, takich jak użycie okresach, krótka historia wydajności, może się zwiększyć w przyszłości użycia. | Ustawienie domyślne to 1.3 x.
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
