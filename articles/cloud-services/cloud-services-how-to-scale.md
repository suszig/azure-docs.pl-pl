---
title: "Automatyczne skalowanie usługi w chmurze w portalu klasycznym | Dokumentacja firmy Microsoft"
description: "(klasyczne) Dowiedz się, jak skonfigurować regułę automatycznego skalowania dla roli sieci web usługi w chmurze lub roli proces roboczy na platformie Azure przy użyciu klasycznego portalu."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 90d55bbac6e113d6add848ace67cf0749e26342b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-classic-portal"></a>Jak skonfigurować automatyczne skalowanie usługi w chmurze w portalu klasycznym
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-how-to-scale-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-how-to-scale.md)

Na stronie skali w klasycznym portalu Azure można skonfigurować ustawienia skalowania automatycznego dla roli sieci web lub roli proces roboczy. Alternatywnie można skonfigurować, ręczne skalowanie zamiast opartych na regułach Skalowanie automatyczne.

> [!NOTE]
> Ten artykuł dotyczy usługi w chmurze role sieci web i proces roboczy. Po utworzeniu maszyny wirtualnej (klasyczne) bezpośrednio, znajduje się w usłudze w chmurze. Niektóre z tych informacji ma zastosowanie do tych typów maszyn wirtualnych. Skalowanie zestawu dostępności maszyn wirtualnych jest po prostu wyłączając je włączane i wyłączane na podstawie reguł skalowania, które można skonfigurować. Aby uzyskać więcej informacji o maszynach wirtualnych i zestawów dostępności, zobacz [Zarządzanie dostępność maszyn wirtualnych](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Przed skonfigurowaniem skalowania dla aplikacji, należy rozważyć następujące informacje:

* Skalowanie jest zagrożony użycia rdzeni.

    Większe wystąpień roli za pomocą większej liczby rdzeni. Można skalować aplikacji tylko w ramach limitu rdzeni dla Twojej subskrypcji. Na przykład załóżmy, że w subskrypcji ma limit liczby rdzeni (20). Po uruchomieniu aplikacji z dwie średnich chmury usługi (łącznie 4 rdzenie) można tylko zwiększać innych wdrożeń usługi w chmurze w ramach subskrypcji przez pozostałe 16 rdzeni. Aby uzyskać więcej informacji na temat rozmiarów, zobacz [rozmiary usługi w chmurze](cloud-services-sizes-specs.md).

* Należy utworzyć kolejkę i skojarzyć go z roli, zanim można skalować aplikacji opartej na próg komunikatu. Aby uzyskać więcej informacji, zobacz [sposobu korzystania z usługi magazynowania kolejki](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Możesz skalować zasoby, które są połączone z usługi w chmurze. Aby uzyskać więcej informacji o łączeniu zasobów, zobacz [porady: łączenie zasobu usługi w chmurze](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

* Aby włączyć wysoką dostępność aplikacji, należy upewnić się, że jest wdrażany z dwóch lub więcej wystąpień roli. Aby uzyskać więcej informacji, zobacz [umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).

## <a name="schedule-scaling"></a>Skalowanie harmonogramu
Domyślnie wszystkie role nie postępuj zgodnie z określonym harmonogramem. W związku z tym zmienić ustawienia dotyczą wszystkie dni i godziny wszystkich przez cały rok. Jeśli chcesz, możesz skonfigurować ręczne lub automatyczne skalowanie dla jednego z następujących trybów:

* Dni tygodnia
* Weekendy
* Nocy tygodnia
* Mornings tygodnia
* Konkretne daty
* Zakresy określonej daty

Ustawienia harmonogramu jest skonfigurowany w [klasycznego portalu Azure](https://manage.windowsazure.com/) na  
**Usługi w chmurze** > **\[usługi w chmurze\]** > **skali** > **\[środowisko produkcyjne lub tymczasowe\]**  strony.

Kliknij przycisk **Konfigurowanie uruchomień** przycisk dla każdej roli, które chcesz zmienić.

![Automatyczne skalowanie usługi chmury zgodnie z harmonogramem][scale_schedules]

## <a name="manual-scale"></a>Ręczne skali
Na **skali** strony, można ręcznie zwiększyć lub zmniejszyć liczbę uruchomionych wystąpień w usłudze w chmurze. To ustawienie jest skonfigurowane do każdej harmonogram, który został utworzony lub cały czas, jeśli nie utworzono harmonogramu.

1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**, a następnie kliknij nazwę usługi w chmurze, aby otworzyć pulpit nawigacyjny.
   
   > [!TIP]
   > Jeśli nie widzisz usługi w chmurze, może być konieczna zmiana z **produkcji** do **przemieszczania** lub na odwrót.

2. Kliknij przycisk **skali**.
3. Wybierz harmonogram, aby zmienić opcje skalowania. *Nie określonych godzinach* jest wartością domyślną, jeśli masz Brak zdefiniowanych harmonogramów.
4. Znajdź **skali według metryki** a następnie wybierz opcję **Brak**. To ustawienie jest ustawieniem domyślnym dla wszystkich ról.
5. Każda rola w usłudze w chmurze ma suwak do zmiany liczby wystąpień do użycia.
   
    ![Ręcznie skalować roli usługi chmury][manual_scale]
   
    Jeśli potrzebujesz więcej wystąpień, może być konieczna zmiana [rozmiar maszyny wirtualnej usługi w chmurze](cloud-services-sizes-specs.md).
6. Kliknij pozycję **Zapisz**.  
   Wystąpienia roli są dodawane lub usuwane w oparciu o wybrane opcje.

> [!TIP]
> Zawsze, gdy zostanie wyświetlony ![][tip_icon] Przesuń wskaźnik myszy do niego i można uzyskać pomoc dotyczącą ustawienie określone.

## <a name="automatic-scale---cpu"></a>Automatyczne skalowanie - Procesora
Ten tryb skaluje, jeśli nie jest średnią wartość procentową wykorzystania Procesora powyżej lub poniżej określonej wartości progowe. Wystąpienia roli są tworzone lub usuwane w takim przypadku.

1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**, a następnie kliknij nazwę usługi w chmurze, aby otworzyć pulpit nawigacyjny.
   
   > [!TIP]
   > Jeśli nie widzisz usługi w chmurze, może być konieczna zmiana z **produkcji** do **przemieszczania** lub na odwrót.

2. Kliknij przycisk **skali**.
3. Wybierz harmonogram, aby zmienić opcje skalowania. *Nie określonych godzinach* jest wartością domyślną, jeśli masz Brak zdefiniowanych harmonogramów.
4. Znajdź **skali według metryki** a następnie wybierz opcję **Procesora**.
5. Teraz można skonfigurować minimalną i maksymalną zakresu wystąpień ról, docelowego użycia procesora CPU (w celu wyzwolenia skali w górę) oraz liczbę wystąpień skalowanie w górę i w dół przez.

![Skalowanie roli usługi chmury, obciążenie procesora cpu][cpu_scale]

> [!TIP]
> Zawsze, gdy zostanie wyświetlony ![][tip_icon] Przesuń wskaźnik myszy do niego i można uzyskać pomoc dotyczącą ustawienie określone.

## <a name="automatic-scale---queue"></a>Automatyczne skalowanie - kolejki
Ten tryb jest automatycznie skalowany Jeśli liczba wiadomości w kolejce powyżej lub poniżej określonego progu. Wystąpienia roli są tworzone lub usuwane w takim przypadku.

1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**, a następnie kliknij nazwę usługi w chmurze, aby otworzyć pulpit nawigacyjny.
   
   > [!TIP]
   > Jeśli nie widzisz usługi w chmurze, może być konieczna zmiana z **produkcji** do **przemieszczania** lub na odwrót.

2. Kliknij przycisk **skali**.
3. Znajdź **skali według metryki** a następnie wybierz opcję **kolejki**.
4. Teraz można skonfigurować minimalną i maksymalną zakresu wystąpień ról, kolejki i liczbę wiadomości w kolejce do przetworzenia dla każdego wystąpienia i skalowanie w górę i w dół, jak wiele wystąpień.

![Skalowanie roli usługi chmury, kolejki komunikatów][queue_scale]

> [!TIP]
> Zawsze, gdy zostanie wyświetlony ![][tip_icon] Przesuń wskaźnik myszy do niego i można uzyskać pomoc dotyczącą ustawienie określone.

## <a name="scale-linked-resources"></a>Skalowanie połączonych zasobów
Często zdarza się skalowanie roli jest korzystne można skalować bazy danych, który także używa aplikacji. Jeśli bazy danych do usługi w chmurze, aby dostęp do ustawień dla tego zasobu, kliknąć odpowiednie łącze.

1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), kliknij przycisk **usługi w chmurze**, a następnie kliknij nazwę usługi w chmurze, aby otworzyć pulpit nawigacyjny.
   
   > [!TIP]
   > Jeśli nie widzisz usługi w chmurze, może być konieczna zmiana z **produkcji** do **przemieszczania** lub na odwrót.

2. Kliknij przycisk **skali**.
3. Znajdź **połączone zasobów** sekcji, a następnie kliknij przycisk **Zarządzaj skalowania dla tej bazy danych**.
   
   > [!NOTE]
   > Jeśli nie widzisz **połączone zasobów** sekcji, prawdopodobnie nie masz żadnych połączonych zasobów.

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
