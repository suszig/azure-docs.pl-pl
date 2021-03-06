---
title: Instalowanie aktualizacji na tablicy wirtualnego StorSimple | Dokumentacja firmy Microsoft
description: "Informacje dotyczące używania interfejsu użytkownika sieci web tablicy wirtualnego StorSimple do stosowania aktualizacji za pomocą metody Azure portal i poprawki"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/07/2017
ms.author: alkohli
ms.openlocfilehash: 3fb246b1515e7a637e6cff6499bf324c3f80dd45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="install-update-04-on-your-storsimple-virtual-array"></a>Zainstaluj aktualizację 0,4 na tablica wirtualnego StorSimple

## <a name="overview"></a>Omówienie

W tym artykule opisano kroki wymagane do zainstalowania aktualizacji 0,4 tablica wirtualnego StorSimple, za pośrednictwem lokalnego interfejsu użytkownika sieci web i portalu Azure. Należy zastosować aktualizacje oprogramowania lub poprawek, aby zapewnić aktualność tablica wirtualne StorSimple. 

Należy pamiętać, polegające na zainstalowanie aktualizacji lub poprawki ponownym uruchomieniu urządzenia. Biorąc pod uwagę, że tablica wirtualne StorSimple jest urządzeniem jeden węzeł, wszystkie operacje We/Wy w toku jest zakłócona i urządzenia napotyka przestoju. 

Przed zainstalowaniem aktualizacji zalecamy wykonanie woluminy lub udziały w trybie offline na hoście pierwszy, a następnie urządzenia. Pozwala to zmniejszyć możliwości uszkodzenie danych.

> [!IMPORTANT]
> Jeśli używasz Update 0.1 lub GA wersje oprogramowania, należy użyć metody poprawek za pomocą lokalnego interfejsu użytkownika sieci web do zainstalowania update 0.3. Jeśli używasz wersji Update 0.2 lub później, zaleca się, że musisz zainstalować aktualizacje za pomocą portalu Azure.
 

## <a name="use-the-local-web-ui"></a>Użyj lokalnego interfejsu użytkownika sieci web

Istnieją dwa kroki, korzystając z lokalnego interfejsu użytkownika sieci web:

* Pobieranie aktualizacji lub poprawek
* Zainstalowanie aktualizacji lub poprawek

### <a name="download-the-update-or-the-hotfix"></a>Pobieranie aktualizacji lub poprawek

Wykonaj następujące kroki, aby pobrać aktualizację oprogramowania z Wykazu usługi Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Aby pobrać aktualizacji lub poprawek

1. Uruchom program Internet Explorer i przejdź pod adres [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Jeśli po raz pierwszy używasz Wykazu usługi Microsoft Update na danym komputerze, po wyświetleniu monitu o zainstalowanie dodatku Wykazu usługi Microsoft Update kliknij pozycję **Zainstaluj**.

3. W polu wyszukiwania z wykazu usługi Microsoft Update wprowadź numer bazy wiedzy Knowledge Base (KB) poprawki, którą chcesz pobrać. Wprowadź **3216577** aktualizacja 0,4, a następnie kliknij przycisk **wyszukiwania**.
   
    Poprawka zostanie wyświetlona na liście, na przykład **aktualizacji tablicy wirtualne StorSimple 0,4**.
   
    ![Przeszukiwanie wykazu](./media/storsimple-virtual-array-install-update-04/download1.png)

4. Kliknij pozycję **Dodaj**. Aktualizacja zostanie dodana do koszyka.

5. Kliknij pozycję **Wyświetl koszyk**.

6. Kliknij pozycję **Pobierz**. Określ lokalizację lokalną, do której mają trafiać pobrane pliki, albo **przejdź** do takiej lokalizacji. Aktualizacje zostaną pobrane do wskazanej lokalizacji i umieszczone w podfolderze o nazwie takiej samej jak aktualizacja. Folder można też skopiować do udziału sieciowego osiągalnego z urządzenia.

7. Otwórz skopiowany folder, plik pakietu autonomicznego aktualizacji firmy Microsoft powinna zostać wyświetlona `WindowsTH-KB3011067-x64`. Ten plik służy do instalowania aktualizacji lub poprawek.

### <a name="install-the-update-or-the-hotfix"></a>Zainstalowanie aktualizacji lub poprawek

Przed instalacją aktualizacji lub poprawki upewnij się, że aktualizacja lub poprawka pobierane lokalnie na hoście lub dostępny za pośrednictwem udziału sieciowego. 

Ta metoda umożliwia instalowanie aktualizacji na urządzeniu z systemem GA lub zaktualizuj 0,1 wersje oprogramowania. Ta procedura może zająć mniej niż 2 minut. Wykonaj poniższe kroki, aby zainstalować aktualizacja lub poprawka.

#### <a name="to-install-the-update-or-the-hotfix"></a>Do zainstalowania aktualizacji lub poprawek

1. W lokalnej sieci web interfejsu użytkownika, przejdź do **konserwacji** > **aktualizacji oprogramowania**.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update/update1m.png)

2. W **ścieżka pliku aktualizacji**, wprowadź nazwę pliku dla aktualizacji lub poprawek. Możesz również przejść do lokalizacji pliku instalacyjnego aktualizacja lub poprawka umieszczony w udziale sieciowym. Kliknij przycisk **Zastosuj**.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update/update2m.png)

3. Zostanie wyświetlone ostrzeżenie. Biorąc pod uwagę te to urządzenie jednego węzła, po zastosowaniu aktualizacji, ponownym uruchomieniu urządzenia i brak przestojów. Kliknij ikonę znacznika wyboru.
   
   ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update/update3m.png)

4. Aktualizacja zostanie uruchomiony. Po pomyślnym zaktualizowaniu urządzenia ponownego uruchomienia. Lokalnego interfejsu użytkownika nie jest dostępny w tym przedziale czasu.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update/update5m.png)

5. Po ponownym uruchomieniu komputera, zostają przeniesieni do **Zaloguj** strony. Aby zweryfikować, że oprogramowanie urządzenia został zaktualizowany w lokalnej sieci web interfejsu użytkownika, przejdź do **konserwacji** > **aktualizacji oprogramowania**. Wersja oprogramowania wyświetlane **10.0.0.0.0.10289.0** 0,4 aktualizacji.
   
   > [!NOTE]
   > Wersje oprogramowania Microsoft raport w sposób nieco inne w lokalnym interfejsu użytkownika sieci web i portalu Azure. Na przykład raporty lokalnego interfejsu użytkownika sieci web **10.0.0.0.0.10289** i raporty portalu Azure **10.0.10289.0** dla tej samej wersji.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a>Korzystanie z witryny Azure Portal

Z aktualizacji, 0,2 i nowszymi wersjami, zaleca się zainstalowanie aktualizacji za pośrednictwem portalu Azure. Procedury portalu wymaga od użytkownika skanowania, Pobierz i zainstaluj aktualizacje. Ta procedura trwa około 7 minut do wykonania. Wykonaj poniższe kroki, aby zainstalować aktualizacja lub poprawka.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Po zakończeniu instalacji (co wskazuje stan zadania w skali 100%), przejdź do usługi Menedżer StorSimple urządzenia. Wybierz **urządzeń** , a następnie wybierz i kliknij przycisk urządzenia, które chcesz zaktualizować z listy urządzeń połączonych z tej usługi. W **ustawienia** bloku, przejdź do **Zarządzaj** a następnie wybierz opcję **aktualizacji urządzenia**. Wersja oprogramowania wyświetlane **10.0.10289.0**.


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [administrowanie tablica wirtualnego StorSimple](storsimple-ova-web-ui-admin.md).

