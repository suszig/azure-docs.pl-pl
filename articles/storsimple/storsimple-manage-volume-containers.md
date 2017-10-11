---
title: "Zarządzanie kontenerów woluminu StorSimple | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak służy strony kontenery woluminów usługi Menedżer StorSimple do dodawania, modyfikowania i usuwania kontenera woluminów."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: bb55a7a4bff0fd4319de6f6ce958686ad8a4142b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a>Usługę Menedżer StorSimple umożliwia zarządzanie kontenery woluminów StorSimple
## <a name="overview"></a>Omówienie
W tym samouczku opisano sposób korzystania z usługi Menedżer StorSimple, tworzenie i zarządzanie nimi kontenery woluminów StorSimple.

Kontener woluminów na urządzeniu Microsoft Azure StorSimple zawiera jeden lub więcej woluminów, które mają konta magazynu, szyfrowania i ustawienia zużycie przepustowości. Urządzenie może mieć wiele kontenerów woluminu dla jego woluminów. 

Kontener woluminów obejmuje następujące atrybuty:

* **Woluminy** — warstwowy lub woluminów StorSimple, które znajdują się w kontenerze wolumin przypięty lokalnie. Kontener woluminów może zawierać maksymalnie 256 woluminów StorSimple.
* **Szyfrowanie** — klucz szyfrowania, które mogą być definiowane dla każdego kontenera woluminów. Ten klucz służy do szyfrowania danych, które są wysyłane z urządzenia StorSimple w chmurze. Z klasy zbrojnych AES 256-bitowym kluczem jest używany z kluczem wprowadzonych przez użytkownika. Aby zabezpieczyć dane, zalecamy zawsze włączone szyfrowanie magazynu w chmurze.
* **Konto magazynu** — konta magazynu, które jest połączone z dostawcą usług magazynu w chmurze. Wszystkie woluminy znajdującej się w kontenerze woluminów udostępnić to konto magazynu. Wybierz konto magazynu z istniejącej listy lub Utwórz nowe konto, podczas tworzenia kontenera woluminu, a następnie określ poświadczenia dostępu dla tego konta.
* **Chmury przepustowości** — przepustowości przez urządzenie po wysłaniu danych z urządzenia do chmury. Można wymusić kontroli przepustowości, określając wartość z zakresu od 1 do 1000 MB/s podczas definiowania tego kontenera. Jeśli chcesz, aby urządzenie, aby korzystać z całej dostępnej przepustowości, ustaw nieograniczone tego pola. Można również utworzyć i zastosować szablon przepustowości przydzielić przepustowość zgodnie z harmonogramem.

![Strona kontenery woluminów](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Ta poniższe procedury dotyczą sposobu używania StorSimple **kontenery woluminów** stronie, aby wykonać następujące operacje wspólne:

* Dodaj kontener woluminów 
* Modyfikowanie kontenera woluminów 
* Usunięcie kontenera woluminów 

## <a name="add-a-volume-container"></a>Dodaj kontener woluminów
Wykonaj poniższe kroki, aby dodać kontener woluminów.

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a>Modyfikowanie kontenera woluminów
Wykonaj poniższe kroki, aby zmodyfikować kontener woluminów.

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a>Usunięcie kontenera woluminów
Kontener woluminów obejmuje woluminy znajdujące się w nim. Można usunąć tylko wtedy, gdy wszystkie woluminy, które są zawarte w niej najpierw zostaną usunięte. Wykonaj poniższe kroki, aby usunąć kontener woluminów.

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [zarządzania woluminami StorSimple](storsimple-manage-volumes.md). 
* Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

