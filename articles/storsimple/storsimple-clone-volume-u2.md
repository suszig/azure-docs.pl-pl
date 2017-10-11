---
title: "Klonowanie woluminów serii StorSimple 8000 | Dokumentacja firmy Microsoft"
description: "Opisano w klonowania różnych typów i ich użycie i opisano sposób korzystania z zestawu sklonować poszczególnych woluminów kopii zapasowych."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 070ac53e-7388-4c48-b8a5-8ed7f9108b2c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 2b627250df62bbe3299869ef3812947afbb8f29f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Klonowanie woluminów (Update 2) przy użyciu usługi Menedżer StorSimple
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Omówienie
Usługę Menedżer StorSimple **katalogu kopii zapasowej** na stronie są wyświetlane wszystkie zestawy kopii zapasowych, które są tworzone podczas ręczne lub automatyczne kopie zapasowe są pobierane. Ta strona służy do listę wszystkich kopii zapasowych dla zasad tworzenia kopii zapasowej lub woluminem, zaznacz lub usuń kopii zapasowych lub użyj kopii zapasowej do przywrócenia lub sklonować woluminu.

![Strona katalogu kopii zapasowej](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Ten przewodnik opisuje sposób korzystania z zestawu sklonować poszczególnych woluminów kopii zapasowych. Objaśniono także różnice między *przejściowa* i *stałe* klonów.

> [!NOTE]
> Jako wolumin warstwowy zostaną sklonowane woluminu przypiętego lokalnie. Jeśli potrzebujesz sklonowany wolumin przypięty lokalnie, klonu można przekonwertować woluminu przypiętego lokalnie po pomyślnym zakończeniu operacji klonowania. Aby uzyskać informacje o konwertowanie woluminu warstwowego na wolumin przypięty lokalnie, przejdź do [zmienić typ woluminu](storsimple-manage-volumes-u2.md#change-the-volume-type).
> 
> Próba konwersji sklonowany woluminu z warstwy do przypięty lokalnie natychmiast po zakończeniu klonowania (gdy nadal jest przejściowy klonowania), konwersja niepowodzenie następujący komunikat o błędzie:
> 
> `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
> 
> To jest błąd tylko wtedy, gdy są klonowania na innym urządzeniu. Pomyślnie przekonwertować wolumin przypięty lokalnie konwersji przejściowej klonu do stałego klonowania. Aby przekonwertować przejściowej klonu stałe klonowania, należy utworzyć migawkę chmury go.
> 
> 

## <a name="create-a-clone-of-a-volume"></a>Tworzenie własnego klonu woluminu
Można utworzyć klona na tym samym urządzeniu, innego urządzenia lub nawet maszynę wirtualną za pomocą lokalnego lub migawka w chmurze.

#### <a name="to-clone-a-volume"></a>Klonowanie woluminów
1. Na stronie usługi Menedżer StorSimple, kliknij przycisk **katalog kopii zapasowej** i wybierz zestaw kopii zapasowych.
2. Rozwiń zestaw, aby wyświetlić skojarzone woluminy kopii zapasowych. Kliknij i wybierz wolumin z zestawu kopii zapasowych.
   
     ![Klonowanie woluminu](./media/storsimple-clone-volume-u2/CloneVol.png) 
3. Kliknij przycisk **klonowania** aby rozpocząć klonowanie wybranego woluminu.
4. W Kreatorze klonowania woluminu w obszarze **Określ nazwę i lokalizację**:
   
   1. Określ urządzenia docelowego. Jest to lokalizacja, w której zostanie utworzona klonu. Można wybrać tego samego urządzenia lub Określ innego urządzenia. Jeśli wybierzesz woluminu skojarzony z innym dostawcom usług w chmurze (nie Azure), listy rozwijanej dla urządzenia będzie pokazywać tylko fizyczne urządzenia. Nie można sklonować woluminu skojarzony z innym dostawcom usług w chmurze na urządzeniu wirtualnym.
      
      > [!NOTE]
      > Upewnij się, że wydajność wymaganą dla klonu jest starsza niż pojemność dostępna na urządzeniu docelowym.
      > 
      > 
   2. Określ nazwę unikatową woluminu z klonu. Nazwa musi zawierać od 3 do 127 znaków. 
      
      > [!NOTE]
      > **Klonowania woluminu jako** pole będzie **warstwowego** nawet wtedy, gdy są klonowania woluminu przypiętego lokalnie. Nie można zmienić to ustawienie; Jednak jeśli potrzebujesz sklonowany woluminu, który ma być przypięty lokalnie również można przekonwertować klonu do woluminu przypiętego lokalnie po utworzeniu klonu. Aby uzyskać informacje o konwertowanie woluminu warstwowego na wolumin przypięty lokalnie, przejdź do [zmienić typ woluminu](storsimple-manage-volumes-u2.md#change-the-volume-type).
      > 
      > 
      
        ![Kreator klonowania 1](./media/storsimple-clone-volume-u2/clone1.png) 
   3. Kliknij ikonę strzałki, ![ikona strzałki](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) Aby przejść do następnej strony.
5. W obszarze **określić hosty, na których można korzystać z tego woluminu**:
   
   1. Określ rekord kontroli dostępu (ACR) dla klonu. Można dodawać nowe ACR lub wybierz z listy.
      
        ![Kreator klonowania 2](./media/storsimple-clone-volume-u2/clone2.png) 
   2. Kliknij ikonę znacznika wyboru ![ikona znacznika wyboru](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)do ukończenia tej operacji.
6. Zadanie klonowania zostanie rozpoczęte i użytkownik zostanie powiadomiony, gdy klonu został utworzony pomyślnie. Kliknij przycisk **zadania** do monitorowania zadania sklonowany na **zadania** strony. Po zakończeniu zadania sklonowany, zostanie wyświetlony następujący komunikat:
   
    ![Komunikat w klonowania](./media/storsimple-clone-volume-u2/CloneMsg.png) 
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
Przejściowa klony są tworzone tylko wtedy, gdy są klonowania do innego urządzenia. Można sklonować określonego woluminu z kopii zapasowej ustawiona na innym urządzeniu zarządzanym przez Menedżera StorSimple. Przejściowa klonowania zostanie odniesień do danych w ramach oryginalnego woluminu i użyje tych danych do odczytu i zapisu lokalnie na urządzeniu docelowym. 

Po wykonaniu migawkę chmury w klonowania przejściowej klonu wynikowy będzie *stałe* klonowania. W trakcie tego procesu zostanie utworzona kopia danych w chmurze i czas kopiowania tych danych jest zależny od rozmiaru danych i opóźnienia Azure (jest to kopia Azure do platformy Azure). Ten proces może potrwać dni, tygodnie. Przejściowa klonu staje się stałe klonowania w ten sposób i nie zawiera żadnych odwołań do oryginalnych danych woluminu, który został sklonowany z. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenariusze dotyczące klony przejściowych i stałe
W poniższych sekcjach opisano przykład sytuacji, w których można użyć klony przejściowych i stałe.

### <a name="item-level-recovery-with-a-transient-clone"></a>Odzyskiwanie na poziomie elementu przejściowej klonu
Należy odzyskać plik prezentacji programu Microsoft PowerPoint co roku tygodniowych. IT administrator identyfikuje kopii zapasowej w tym czasie, a następnie filtruje woluminu. Następnie administrator klonuje wolumin, lokalizuje plik, którego szukasz i przekazuje go do użytkownika. W tym scenariuszu jest używana przejściowej klonowania. 

![Zobacz film](./media/storsimple-clone-volume-u2/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób użycia klonu i przywrócić funkcji w programie StorSimple, aby odzyskać usunięte pliki, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testowanie w środowisku produkcyjnym klonu stałych
Należy sprawdzić usterkę testowania w środowisku produkcyjnym. Tworzenie własnego klonu woluminu w środowisku produkcyjnym i wykonaj migawkę chmury dla tego klonu. do utworzenia woluminu sklonowany niezależne. W tym scenariuszu stały klonowania jest używany.  

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [przywracania woluminu StorSimple z zestawu kopii zapasowych](storsimple-restore-from-backup-set-u2.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

