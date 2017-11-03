---
title: Interfejs programu Data Manager platformy Microsoft Azure StorSimple | Dokumentacja firmy Microsoft
description: "Informacje dotyczące używania usługi Menedżer StorSimple danych interfejsu użytkownika (podglądzie prywatnym)"
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
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a>Zarządzanie przy użyciu usługi Menedżer StorSimple danych interfejsu użytkownika (podglądzie prywatnym)

W tym artykule opisano sposób korzystania interfejsu użytkownika Menedżera danych StorSimple przekształcenie danych na danych znajdujących się w przypadku urządzeń z serii StorSimple 8000. Przekształcone dane mogą być następnie używane przez innych usług Azure, takich jak usługi Azure Media Services, Azure HDInsight uczenie maszynowe Azure i usługi Azure Search. 


## <a name="use-storsimple-data-transformation"></a>Za pomocą przekształcania danych StorSimple

Menedżer StorSimple danych jest zasobów, w którym można wdrożyć transformacji danych. Usługi Data Transformation umożliwia przenoszenie danych z lokalnego urządzenia StorSimple do obiektów blob w magazynie Azure. W związku z tym w przepływie pracy należy określić szczegóły dotyczące urządzenia StorSimple i potrzebne dane, które chcesz przenieść do konta magazynu.

### <a name="create-a-storsimple-data-manager-service"></a>Utwórz usługę Menedżer StorSimple danych

Wykonaj poniższe kroki, aby utworzyć usługę Menedżer StorSimple danych.

1. Aby utworzyć usługę Menedżer StorSimple danych, przejdź do [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Kliknij przycisk  **+**  ikony, jak i wyszukiwania StorSimple Data Manager. Kliknij opcję usługi Menedżer StorSimple danych, a następnie kliknij przycisk **Utwórz**.

3. Włączenie subskrypcji do tworzenia tej usługi, zobaczysz następujący blok.

    ![Utwórz zasób menedżerów danych StorSimple](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Wprowadź dane wejściowe, a następnie kliknij przycisk **Utwórz**. Określona lokalizacja powinna być ten, który zawiera wszystkie konta magazynu i usługi Menedżer StorSimple. Obecnie są obsługiwane tylko w regionach zachodnie stany USA i Europa Zachodnia. W związku z tym usługi Menedżer StorSimple, usługa Menedżera danych i konta magazynu skojarzone wszystkie należy w powyższej obsługiwane regiony. Trwa około minuty można utworzyć usługi.

### <a name="create-a-data-transformation-job-definition"></a>Tworzenie definicji zadania przekształcania danych

W ramach usługi Menedżer StorSimple danych należy utworzyć definicję zadania przekształcania danych. Definicji zadania określa szczegóły danych, które planuje się do konta magazynu w formacie native. 

Wykonaj poniższe kroki, aby utworzyć nową definicję zadania przekształcania danych.

1.  Przejdź do usługi, który został utworzony. Kliknij przycisk **+ definicji zadania**.

    ![Kliknij pozycję + definicji zadania](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. Otwiera nowy blok definicji zadania. Nadaj nazwę definicja zadania, a następnie kliknij przycisk **źródła**. W **Konfigurowanie źródła danych** bloku, określ szczegóły urządzenia StorSimple i potrzebne dane.

    ![Tworzenie definicji zadania](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Ponieważ jest to nowa usługa Data Manager, są skonfigurowane nie repozytoria danych. Aby dodać Menedżera StorSimple jako repozytorium danych, kliknij przycisk **Dodaj nowe** danych repozytorium z listy rozwijanej, a następnie kliknij polecenie **Dodaj repozytorium danych**.

4. Wybierz **serii StorSimple 8000 Menedżera** jako repozytorium typu, a następnie wprowadź właściwości z **Menedżer StorSimple**. Dla **identyfikator zasobu** pola, musisz wprowadzić numer przed **:** w kluczu rejestracji menedżera StorSimple.

    ![Utwórz źródło danych](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Kliknij przycisk **OK** po zakończeniu. Spowoduje to zapisanie danych repozytorium, a ten Menedżer StorSimple mogą być ponownie używane w innych definicje zadań bez konieczności ponownego wprowadzania tych parametrów. Trwa kilka sekund po kliknięciu **OK** dla Menedżera StorSimple wyświetlani na liście rozwijanej.

6.  W **Konfigurowanie źródła danych** bloku, wprowadź nazwę urządzenia i nazwę woluminu, zawierający dane zainteresowań.

7.  W **filtru** podsekcji, wprowadź katalog główny, zawierający dane odsetek (w tym polu powinny rozpoczynać się od `\`). Można również dodać wszystkie filtry plików.

8.  Działania usługi przekształcania danych na dane wypychana do platformy Azure za pomocą migawek. Podczas wykonywania tego zadania, można wybrać utworzyć kopię zapasową za każdym razem, gdy to zadanie jest uruchamiane (do pracy najnowszych danych) lub użyć istniejącego ostatniej kopii zapasowej w chmurze (Jeśli pracujesz nad niektórych danych archiwalnych).

    ![Nowe szczegóły źródła danych](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. Następnie należy skonfigurować ustawienia obiektu docelowego. Istnieją 2 typy elementów docelowych obsługiwanych — kont usługi Azure Storage i Azure Media Services. Wybieranie kont magazynu, aby umieścić pliki do obiektów blob w tym kontem. Wybierz nośnik konto usługi do umieszczać plików zasobów na tym koncie. Ponownie należy dodać repozytorium. Na liście rozwijanej wybierz **Dodaj nowe** , a następnie **skonfigurować ustawienia**.

    ![Utwórz ujścia danych](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. W tym miejscu można wybrać typ repozytorium, które chcesz dodać i inne parametry skojarzone z repozytorium. W obu przypadkach kolejki magazynu jest tworzone po uruchomieniu zadania. Jest ona wypełniana przy użyciu komunikatów dotyczących przekształconych obiektów blob, gdy będą gotowe. Nazwa tej kolejki jest taka sama jak nazwa definicji zadania. W przypadku wybrania **Media Services** jako typ repozytorium, następnie możesz też wprowadzić poświadczenia konta magazynu miejsce tworzenia kolejki.

    ![Nowe szczegóły ujścia danych](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. Po dodaniu repozytorium danych (która zajmuje kilka sekund), można znaleźć na liście rozwijanej w repozytorium **nazwa konta docelowego**.  Wybierz element docelowy, które są potrzebne.

12. Kliknij przycisk **OK** do tworzenia definicji zadania. Definicja zadania jest teraz skonfigurowane. Można użyć tej definicji zadania wiele razy za pośrednictwem interfejsu użytkownika.

    ![Dodawanie nowej definicji zadania](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a>Uruchom definicji zadania

Zawsze, gdy trzeba przenieść dane z StorSimple do konta magazynu, które zostały określone w definicji zadania, należy wywołać go. W odniesieniu do zmiany parametrów, za każdym razem, gdy wywołanie zadania charakteryzuje się pewną elastycznością. Dostępne są następujące kroki:

1. Wybierz usługę Menedżer StorSimple danych i przejdź do **monitorowanie**. Kliknij przycisk **Uruchom teraz**.

    ![Wyzwalacz definicji zadania](./media/storsimple-data-manager-ui/run-now.png)

2. Wybierz definicji zadania, które chcesz uruchomić. Kliknij przycisk **parametrów uruchomieniowych** można zmodyfikować wszystkie ustawienia, które możesz chcieć zmienić wykonywania tego zadania.

    ![Uruchom zadania, ustawienia](./media/storsimple-data-manager-ui/run-settings.png)

3. Kliknij przycisk **OK** , a następnie kliknij przycisk **Uruchom** można uruchomić zadania. Aby monitorować zadanie, przejdź do **zadania** strony w Menedżera StorSimple danych.

    ![Lista zadań i stanu](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. Oprócz monitorowania w **zadania** bloku można również wykrywać w kolejce magazynu, gdy wiadomość zostanie dodany za każdym razem, gdy plik zostanie przeniesiony z StorSimple do konta magazynu.


## <a name="next-steps"></a>Następne kroki

[Użyć zestawu .NET SDK w celu uruchomienia zadania Menedżer StorSimple danych](storsimple-data-manager-dotnet-jobs.md).