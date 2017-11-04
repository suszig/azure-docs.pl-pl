---
title: "Omówienie programu Microsoft Azure StorSimple Data | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie usługi Menedżer StorSimple danych (w podglądzie prywatnym)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: aedb44610fe57055851538b9dbdb810e66e58d73
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a>Omówienie Menedżera danych StorSimple (podglądzie prywatnym)

## <a name="overview"></a>Omówienie

Microsoft Azure StorSimple jest rozwiązania magazynu hybrydowego chmury, którego dotyczy złożoności danych niestrukturalnych zwykle powiązanych ze udziałów plików. StorSimple używa magazynu w chmurze jako rozszerzenia rozwiązania lokalnego, i automatycznie warstwy danych w lokalnej pamięci masowej i magazynu w chmurze. Zintegrowana ochrony danych z lokalnego i w chmurze migawek, eliminuje potrzebę stosowania sprawling infrastruktury magazynu. Archiwalne i odzyskiwanie po awarii jest również bezproblemowe z chmurą, działając jako lokalizacji poza siedzibą firmy.

Usługa przekształcania danych, które udostępniono w tym dokumencie umożliwia uzyskiwanie dostępu do danych StorSimple w chmurze. Ta usługa zawiera interfejsy API do wyodrębniania danych z StorSimple i prezentować innymi usługami platformy Azure w formatach które mogą być łatwo używane. W tej wersji zapoznawczej obsługiwane formaty są obiekty BLOB platformy Azure i zasoby usługi Azure Media Services. Ta transformacja umożliwia łatwe połączenie usług, takich jak usługi Azure Media Services, Azure HDInsight uczenie maszynowe Azure i usługi Azure Search do działania na urządzeniu lokalnym serii StorSimple 8000.

Poniżej przedstawiono diagram blokowy wysokiego poziomu pokazujący to.

![Diagram ogólny](./media//storsimple-data-manager-overview/high-level-diagram.png)

W tym dokumencie opisano, jak można założyć prywatnej wersji zapoznawczej tej usługi. Wyjaśniono również, jak można użyć tej usługi do pisania aplikacji, które używają danych StorSimple i innych usług Azure w chmurze.

## <a name="sign-up-for-data-manager-preview"></a>Załóż Data Manager w wersji zapoznawczej
Przed zarejestrowaniem się usługi Menedżera danych, sprawdź następujące wymagania wstępne.

### <a name="prerequisites"></a>Wymagania wstępne

Tym ćwiczeniu przyjęto założenie, że masz
* Aktywną subskrypcją platformy Azure.
* dostęp do zarejestrowanych StorSimple 8000 serii urządzenia
* wszystkie klucze skojarzone z danym urządzeniem z serii StorSimple 8000.

### <a name="sign-up"></a>Rejestrowanie

Menedżer StorSimple danych jest w prywatnej wersji zapoznawczej. Wykonaj poniższe kroki, aby utworzyć konto w prywatnej wersji zapoznawczej tej usługi:

1.  Zaloguj się do portalu Azure z rozszerzeniem StorSimple Data Manager w: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Twoje poświadczenia platformy Azure można używać do logowania.

2.  Kliknij przycisk  **+**  ikonę, aby utworzyć usługę. Kliknij przycisk **magazynu** , a następnie kliknij przycisk **Zobacz wszystkie** w otwartym bloku.

    ![Ikona danych Menedżera StorSimple wyszukiwania](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. Pojawi się ikona Menedżer StorSimple danych.

    ![Wybierz ikonę Menedżera StorSimple danych](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. Kliknij ikonę Menedżer StorSimple danych, a następnie kliknij przycisk **Utwórz**. Wybierz subskrypcję, którą chcesz włączyć w prywatnej wersji zapoznawczej, a następnie kliknij przycisk **Zapisz się!**

    ![Zapisz się](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Spowoduje to wysłanie żądania można dołączyć użytkownik. Firma Microsoft będzie dołączyć należy jak najszybciej. Po włączeniu subskrypcji można utworzyć usługi Menedżer StorSimple danych.

6. Aby łatwo uzyskiwać dostęp do usługi Menedżer StorSimple danych, kliknij ikonę gwiazdki, aby przypiąć go do ulubionych.

    ![Dostęp do danych StorSimple menedżerów](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Następne kroki

[Użyj Menedżera danych StorSimple interfejsu użytkownika Służącego do przekształcenia danych](storsimple-data-manager-ui.md).