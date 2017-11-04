---
title: "Wdrażanie usługi Menedżer StorSimple | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak tworzyć i usuwać usługi Menedżer StorSimple w klasycznym portalu Azure i opisano sposób zarządzania klucz rejestracji usługi."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba3637a3a8b15b45c16bf5a00c1f4225bcfc5af8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-the-storsimple-manager-service-in-the-azure-classic-portal"></a>Wdrażanie usługi Menedżer StorSimple w klasycznym portalu Azure

## <a name="overview"></a>Omówienie
Usługę Menedżer StorSimple działa na platformie Microsoft Azure i łączy się wieloma urządzeniami StorSimple. Po utworzeniu usługi musisz służy do zarządzania urządzeniami przy użyciu klasycznego portalu Microsoft Azure działającego w przeglądarce. Dzięki temu można monitorować urządzenia, które są podłączone do usługi Menedżer StorSimple z jednym centralnym miejscu, w tym samym minimalizując nakładu prac administracyjnych.

Strona docelowa Menedżer StorSimple Wyświetla wszystkie usługi Menedżer StorSimple, które służą do zarządzania urządzeniami magazynu StorSimple. Dla każdej usługi Menedżer StorSimple następujące informacje są wyświetlane na stronie Menedżer StorSimple:

* **Nazwa** — nazwę, która została przypisana do usługi Menedżer StorSimple, jeśli został on utworzony. **Nie można zmienić nazwy usługi, po utworzeniu usługi. Dotyczy to również dla innych jednostek, takich jak urządzenia, kontenery woluminów, woluminów i zasad tworzenia kopii zapasowych, które nie mogą zostać zmienione w klasycznym portalu Azure.**
* **Stan** — stan usługi, która może być **Active**, **tworzenie**, lub **Online**.
* **Lokalizacja** — lokalizacji geograficznej, w którym zostanie wdrożone urządzenie StorSimple.
* **Subskrypcja** — rozliczeń subskrypcji, która jest skojarzona z usługą.

Typowe zadania, które mogą być wykonywane za pośrednictwem strony Menedżer StorSimple są:

* Tworzenie usługi
* Usuwanie usługi
* Pobieranie klucza rejestracji usługi
* Wygeneruj ponownie klucz rejestracji usługi

W tym samouczku opisano sposób wykonywania wszystkich zadań.

## <a name="create-a-service"></a>Tworzenie usługi
Użyj **szybkie tworzenie** opcję, aby utworzyć usługę Menedżer StorSimple, jeśli chcesz wdrożyć urządzenie StorSimple. Aby utworzyć usługę, musi być:

* Subskrypcja z umową Enterprise
* Aktywne konto magazynu Microsoft Azure
* Informacji dotyczących rozliczeń, która służy do zarządzania dostępem

Można także wygenerować domyślne konto magazynu podczas tworzenia usługi.

Jednej usługi można zarządzać wieloma urządzeniami. Jednak urządzenia nie może obejmować wiele usług. Duże przedsiębiorstwa mogą być wiele wystąpień usługi do pracy z różnych subskrypcji, organizacji lub nawet miejsc wdrożenia. Należy pamiętać, że potrzebujesz oddzielnych wystąpień usługi Menedżer StorSimple do zarządzania urządzeń z serii StorSimple 8000 i tablice wirtualne StorSimple.

> [!IMPORTANT] 
> Jeśli przed sierpnia 2016 nieużywanych utworzono usługi (żadnego urządzenia operacje są wykonywane na ten zasób), nie można zarządzać za pomocą portalu Azure lub klasycznego portalu Azure. Zaleca się tworzenie nowej usługi w portalu Azure.

Wykonaj poniższe kroki, aby utworzyć usługę.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Usuwanie usługi
Przed usunięciem usługi, upewnij się, że żadnych podłączonych urządzeń używają go. Jeśli usługa jest w użyciu, Dezaktywuj połączonych urządzeń. Operacja Dezaktywuj sever połączenia między urządzeniem i usługi, ale zachować dane urządzenie w chmurze.

> [!IMPORTANT] 
> Po usunięciu usługi nie można cofnąć operacji. Dowolne urządzenie, które zostało przy użyciu usługi będzie trzeba resetowanie, zanim będzie można go używać z inną usługą do ustawień fabrycznych. W tym scenariuszu dane lokalne na urządzeniu, a także konfiguracji, zostaną utracone.

Wykonaj poniższe kroki, aby usunąć usługę.

### <a name="to-delete-a-service"></a>Aby usunąć usługę
1. Na **usługi Menedżer StorSimple** wybierz usługi, który chcesz usunąć.
2. Kliknij przycisk **usunąć** w dolnej części strony.
3. Kliknij przycisk **tak** powiadomienia o potwierdzenie. Może upłynąć kilka minut, aż usługi do usunięcia.

## <a name="get-the-service-registration-key"></a>Pobieranie klucza rejestracji usługi
Po pomyślnym utworzeniu usługi musisz zarejestrować urządzenie StorSimple w usłudze. Aby zarejestrować pierwszego urządzenia StorSimple, konieczne będzie klucz rejestracji usługi. Aby zarejestrować dodatkowych urządzeń z istniejącej usługi StorSimple, konieczne będzie zarówno klucz rejestracji i klucza szyfrowania danych usługi (który jest generowany na pierwszym urządzeniem podczas rejestracji). Aby uzyskać więcej informacji na temat klucza szyfrowania danych usługi, zobacz [zabezpieczenia usługi StorSimple](storsimple-security.md). Możesz pobrać klucz rejestracji po zalogowaniu się do **klucz rejestracji** na **usług** strony.

Wykonaj poniższe kroki, aby pobrać klucz rejestracji usługi.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Klucz rejestracji usługi należy przechowywać w bezpiecznym miejscu. Należy tego klucza, a także klucz szyfrowania danych usługi, aby zarejestrować dodatkowych urządzeń z tą usługą. Po uzyskaniu klucza rejestracji usługi, należy skonfigurować urządzenie za pomocą programu Windows PowerShell dla StorSimple interfejsu.

Aby uzyskać więcej informacji na temat sposobu użycia tego klucza rejestracji, zobacz [krok 3: Konfigurowanie i rejestrowanie urządzenia za pomocą środowiska Windows PowerShell dla StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Wygeneruj ponownie klucz rejestracji usługi
Należy ponownie wygenerować klucz rejestracji usługi, jeśli jest wymagane do wykonania obrotu klucza lub zmiana listy administratorów usługi. Podczas ponownego generowania klucza, nowy klucz służy tylko do celów rejestracji kolejnych urządzeń. Przez ten proces nie dotyczy to urządzeń, które zostały już zarejestrowane.

Wykonaj poniższe kroki, aby wygenerować ponownie klucz rejestracji usługi.

### <a name="to-regenerate-the-service-registration-key"></a>Można ponownie wygenerować klucz rejestracji usługi
1. Na **usługi Menedżer StorSimple** kliknij przycisk **klucz rejestracji**.
2. W **klucz rejestracji usługi** okno dialogowe, kliknij przycisk **ponownie wygenerować**.
3. Zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **OK** kontynuować ponowne generowanie.
4. Pojawi się nowy klucz rejestracji usługi.
5. Skopiuj ten klucz i zapisać go do rejestracji żadnych nowych urządzeń z tą usługą.
6. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-service/HCS_CheckIcon.png) Aby zamknąć to okno dialogowe.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [procesu wdrażania StorSimple](storsimple-deployment-walkthrough-u2.md).
* Dowiedz się więcej o [Zarządzanie kontem magazynu StorSimple](storsimple-manage-storage-accounts.md).
* Dowiedz się więcej o sposobie [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).
