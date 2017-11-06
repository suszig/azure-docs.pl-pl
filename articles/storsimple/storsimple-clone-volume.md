---
title: Klonowanie woluminu StorSimple | Dokumentacja firmy Microsoft
description: "Opisano w klonowania różnych typów i ich użycie i opisano sposób korzystania z zestawu sklonować poszczególnych woluminów kopii zapasowych."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: cf11a369549b887f79a81c19780048d31e56beae
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>Klonowanie woluminu przy użyciu usługi Menedżer StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Omówienie
Usługę Menedżer StorSimple **katalogu kopii zapasowej** na stronie są wyświetlane wszystkie zestawy kopii zapasowych, które są tworzone podczas ręczne lub automatyczne kopie zapasowe są pobierane. Ta strona służy do listę wszystkich kopii zapasowych dla zasad tworzenia kopii zapasowej lub woluminem, zaznacz lub usuń kopii zapasowych lub użyj kopii zapasowej do przywrócenia lub sklonować woluminu.

![Strona katalogu kopii zapasowej](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Ten przewodnik opisuje sposób korzystania z zestawu sklonować poszczególnych woluminów kopii zapasowych. Objaśniono także różnice między *przejściowa* i *stałe* klonów. 

## <a name="create-a-clone-of-a-volume"></a>Tworzenie własnego klonu woluminu
Klon na tym samym urządzeniu, innego urządzenia lub nawet maszynę wirtualną można utworzyć przy użyciu lokalnych lub migawka w chmurze.

#### <a name="to-clone-a-volume"></a>Klonowanie woluminów
1. Na stronie usługi Menedżer StorSimple, kliknij przycisk **katalog kopii zapasowej** i wybierz zestaw kopii zapasowych.
2. Rozwiń zestaw, aby wyświetlić skojarzone woluminy kopii zapasowych. Kliknij i wybierz wolumin z zestawu kopii zapasowych.
   
     ![Klonowanie woluminu](./media/storsimple-clone-volume/HCS_Clone.png) 
3. Kliknij przycisk **klonowania** aby rozpocząć klonowanie wybranego woluminu.
4. W Kreatorze klonowania woluminu w obszarze **Określ nazwę i lokalizację**:
   
   1. Określ urządzenia docelowego. Jest to lokalizacja, w której zostanie utworzona klonu. Można wybrać tego samego urządzenia lub Określ innego urządzenia. Jeśli wybierzesz woluminu skojarzony z innym dostawcom usług w chmurze (nie Azure), listy rozwijanej dla urządzenia będzie pokazywać tylko fizyczne urządzenia. Nie można sklonować woluminu skojarzony z innym dostawcom usług w chmurze na urządzeniu wirtualnym.
      
      > [!NOTE]
      > Upewnij się, że wydajność wymaganą dla klonu jest starsza niż pojemność dostępna na urządzeniu docelowym.
      > 
      > 
   2. Określ nazwę unikatową woluminu z klonu. Nazwa musi zawierać od 3 do 127 znaków.
   3. Kliknij ikonę strzałki, ![ikona strzałki](./media/storsimple-clone-volume/HCS_ArrowIcon.png) Aby przejść do następnej strony.
5. W obszarze **określić hosty, na których można korzystać z tego woluminu**:
   
   1. Określ rekord kontroli dostępu (ACR) dla klonu. Można dodawać nowe ACR lub wybierz z listy.
   2. Kliknij ikonę znacznika wyboru ![ikona znacznika wyboru](./media/storsimple-clone-volume/HCS_CheckIcon.png)do ukończenia tej operacji.
6. Zadanie klonowania zostanie rozpoczęte i użytkownik zostanie powiadomiony, gdy klonu został utworzony pomyślnie. Kliknij przycisk **zadania** do monitorowania zadania sklonowany na **zadania** strony.
7. Po zakończeniu zadania klonu:
   
   1. Przejdź do **urządzeń** i wybrać opcję **kontenery woluminów** kartę. 
   2. Wybierz kontener woluminu, który jest skojarzony z woluminu źródłowego, który można sklonować. Na liście woluminów powinna zostać wyświetlona klonowania, która została właśnie utworzona.

> [!NOTE]
> Monitorowanie i domyślnego tworzenia kopii zapasowej są automatycznie wyłączane na sklonowanym woluminie.
> 
> 

Sklonowany, która jest tworzona w ten sposób jest przejściowy klonowania. Aby uzyskać więcej informacji na temat typów w klonowania, zobacz [przejściowej a stałe klony](#transient-vs-permanent-clones).

Tego klonu jest teraz regularne woluminu, a wszelkie operacje dostępnego na woluminie będą dostępne dla sklonowanego. Należy skonfigurować ten wolumin do wszelkich kopii zapasowych.

## <a name="transient-vs-permanent-clones"></a>Przejściowy a klony stałych
Klony przejściowych i stałe są tworzone tylko wtedy, gdy są klonowania na innym urządzeniu. Można sklonować określonego woluminu z kopii zapasowej ustawiona na innym urządzeniu. Klon utworzone w ten sposób jest *przejściowa* klonowania. Przejściowa klonowania zostanie odwołania do oryginalnego woluminu i użyje tego woluminu do odczytu podczas zapisywania lokalnie. 

Po wykonaniu migawkę chmury w klonowania przejściowej klonu wynikowy będzie *stałe* klonowania. Stałe klonowania jest niezależna i nie zawiera żadnych odwołań do oryginalnego woluminu, który został sklonowany z.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenariusze dotyczące klony przejściowych i stałe
W poniższych sekcjach opisano przykład sytuacji, w których można użyć klony przejściowych i stałe.

### <a name="item-level-recovery-with-a-transient-clone"></a>Odzyskiwanie na poziomie elementu przejściowej klonu
Należy odzyskać plik prezentacji programu Microsoft PowerPoint co roku tygodniowych. IT administrator identyfikuje kopii zapasowej w tym czasie, a następnie filtruje woluminu. Następnie administrator klonuje wolumin, lokalizuje plik, którego szukasz i przekazuje go do użytkownika. W tym scenariuszu jest używana przejściowej klonowania. 

![Zobacz film](./media/storsimple-clone-volume/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób użycia klonu i przywrócić funkcji w programie StorSimple, aby odzyskać usunięte pliki, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testowanie w środowisku produkcyjnym klonu stałych
Należy sprawdzić usterkę testowania w środowisku produkcyjnym. Możesz Tworzenie własnego klonu woluminu w środowisku produkcyjnym, wykonując migawkę chmury dla tego klonu. Sklonowany woluminu jest teraz niezależne. W tym scenariuszu stały klonowania jest używany.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [przywracania woluminu StorSimple z zestawu kopii zapasowych](storsimple-restore-from-backup-set.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

