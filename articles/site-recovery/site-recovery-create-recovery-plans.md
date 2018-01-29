---
title: "Tworzyć i dostosowywać plany odzyskiwania dla trybu failover i odzyskiwania w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Opisuje, jak tworzyć i dostosowywać plany odzyskiwania w programie Azure Site Recovery w tryb failover i odzyskiwania maszyn wirtualnych i serwerów fizycznych"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/26/2018
ms.author: raynew
ms.openlocfilehash: 9839a989246b28c1a194b8d1f0e99c1bd80ac2e5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="create-recovery-plans"></a>Tworzenie planów odzyskiwania


Ten artykuł zawiera informacje na temat tworzenia i dostosowywania planów odzyskiwania w [usługi Azure Site Recovery](site-recovery-overview.md).

Zamieść wszelkie komentarze lub pytania pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Utworzyć plany odzyskiwania, aby wykonać następujące czynności:

* Definiowanie grup komputerów, które w tryb failover ze sobą, a następnie uruchom ze sobą.
* Model zależności między komputerami, grupując je do odzyskiwania Zaplanuj grupy. Na przykład do pracy awaryjnej i wyświetlić określonej aplikacji, możesz grupy wszystkich maszyn wirtualnych dla tej aplikacji w tej samej grupie planu odzyskiwania.
* Uruchom tryb failover. Można uruchomić testu, planowane, ani nieplanowane przełączenie awaryjne, w ramach planu odzyskiwania.

## <a name="why-use-recovery-plans"></a>Dlaczego warto używać planów odzyskiwania?

Plany odzyskiwania pomocne podczas procesu systematyczne odzyskiwania przez utworzenie małych jednostek niezależne, którymi można zarządzać. Te jednostki zazwyczaj będzie reprezentować aplikacji w danym środowisku. Plan odzyskiwania można nie tylko zdefiniować sekwencji, w którym uruchomić maszyny wirtualne, ale pomaga również automatyzacji typowych zadań podczas odzyskiwania.


**Zasadniczo jednym ze sposobów Sprawdź, czy są przygotowane do migracji w chmurze lub odzyskiwania po awarii jest zapewnienie, że każdy aplikacji jest częścią planu odzyskiwania i każdy z planów odzyskiwania jest sprawdzane pod kątem odzyskiwania Microsoft Azure. Z tym gotowości bez obaw można migrować lub pracy awaryjnej pełną centrum danych do systemu Microsoft Azure.**
 
Poniżej przedstawiono trzy propozycje wartości klucza w planie odzyskiwania:

### <a name="model-an-application-to-capture-dependencies"></a>Model aplikacji do przechwytywania zależności

Plan odzyskiwania jest grupy maszyn wirtualnych zwykle składającej się z aplikacji czy tryb failover razem. Konstruuje przy użyciu planu odzyskiwania, można rozszerzyć tę grupę, aby przechwycić właściwości specyficzne dla aplikacji.
 
Daj nam zająć przykład typowa aplikacja trzy warstwy z

* jeden wewnętrznej bazy danych SQL
* jeden oprogramowania pośredniczącego
* jedną witrynę sieci web frontonu

Plan odzyskiwania można dostosować w taki sposób, aby upewnić się, że maszyny wirtualne znaleziona w odpowiedniej kolejności po przejściu w tryb failover. Wewnętrznej bazy danych SQL powinna pochodzić pierwszy, oprogramowanie pośredniczące powinno znaleziona dalej i frontonu sieci web powinna pochodzić się ostatnio. To zamówienie sprawia, że niektóre, że aplikacja działa przez czas, który ostatnia maszyna wirtualna funkcjonuje. Na przykład gdy oprogramowanie pośredniczące, próbuje nawiązać połączenia z warstwą SQL i planu odzyskiwania ma zapewnić warstwy SQL jest już uruchomiony. Serwerów frontonu powtarzający się ostatnio również zapewnia, że użytkownicy końcowi nie podłączaj do adresu URL aplikacji przez pomyłkę dopóki wszystkie składniki są uruchomione są uruchomione i jest gotowy do akceptowania żądań aplikacji. Aby utworzyć te zależności, można dostosować planu odzyskiwania, aby dodać grupy. Wybierz maszynę wirtualną i zmienić jej grupy, aby przenieść go między grupami.

![Przykładowy plan odzyskiwania](./media/site-recovery-create-recovery-plans/rp.png)

Po ukończeniu dostosowań można zwizualizować kolejnych kroków odzyskiwania. Poniżej przedstawiono kolejność czynności wykonywane w trybie failover planu odzyskiwania:

* Najpierw jest krokiem zamykania, który próbuje wyłączyć maszyny wirtualne lokalnej (z wyjątkiem testowy tryb failover, gdy lokacja główna musi działać w dalszym ciągu)
* Następnie wyzwala trybu failover wszystkich maszyn wirtualnych w planie odzyskiwania równolegle. Krok trybu failover przygotowuje dysków maszyn wirtualnych z replikowanych danych.
* Na koniec grup uruchamiania wykonywania w ich kolejność, najpierw uruchamianie maszyn wirtualnych w każdej grupie — Grupa 1 następnie grupa 2, a na końcu grupy 3. Jeśli istnieje więcej niż jeden maszyn wirtualnych w dowolnej grupie (na przykład frontonu sieci web z równoważeniem obciążenia) wszystkich z nich są rozruchu się równolegle.

**Sekwencjonowanie między grupami gwarantuje, że są honorowane zależności między warstwami różnych aplikacji i równoległości odpowiednim zwiększa RTO odzyskiwania aplikacji.**

   > [!NOTE]
   > Komputery, które są częścią pojedynczej grupy przejdzie w tryb failover równolegle. Komputery, które należą do różnych grup przejdzie w tryb failover w Odry grup. Tylko wtedy, gdy wszystkie komputery z grupy 1 zostały przełączone do trybu failover i uruchomiony, maszyn z grupy 2 rozpocznie się ich pracy awaryjnej.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Większość zadań odzyskiwania, aby zmniejszyć RTO automatyzacji

Odzyskiwanie dużych aplikacji może być złożonym zadaniem. Jest również trudno należy pamiętać, że kroki dokładne dostosowanie post trybu failover lub migracji. Czasami nie jest użytkownik, ale inną osobę nie rozpoznaje aplikacji mogli dokładnie zapoznać się, kto chce wyzwolić tryb failover. Zapamiętywanie trudno jest zbyt wiele wymagane ręczne wykonanie czynności w czasie chaos i błąd podatnych na błędy. Plan odzyskiwania zawiera sposobem zautomatyzowania wymagane akcje, które należy wykonać na każdym etapie, za pomocą elementów runbook automatyzacji Microsoft Azure. Z elementami runbook można zautomatyzować wspólne zadania odzyskiwania, takie jak przykłady podane poniżej. Te zadania, które nie mogły zostać zautomatyzowane plany odzyskiwania także umożliwić możliwość wstawiania działań ręcznych.

* Zadania na maszynie wirtualnej Azure po pracy awaryjnej — zazwyczaj są to wymagane, dzięki czemu można połączyć z maszyną wirtualną, na przykład:
    * Tworzenie publicznego adresu IP w tryb failover post maszyny wirtualnej
    * Przypisanie grupy NSG do nieudane za pośrednictwem karty Sieciowej maszyny wirtualnej
    * Dodaj usługi równoważenia obciążenia do zestawu dostępności
* Zadania na maszynie wirtualnej po pracy awaryjnej — te zmiany konfiguracji aplikacji, dzięki czemu nadal działają prawidłowo w nowym środowisku, na przykład:
    * Zmodyfikuj parametry połączenia bazy danych na maszynie wirtualnej
    * Zmień reguły konfiguracji serwera sieci web

**Plan odzyskiwania ukończone, który automatyzuje zadania odzyskiwania post za pomocą elementu runbook usługi Automatyzacja możesz osiągnąć trybu failover z jednym kliknięciem i zoptymalizować Docelowy.**

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testowanie trybu failover będzie gotowa do awarii

Planu odzyskiwania może służyć do wyzwolenia zarówno trybu failover lub test trybu failover. Należy zawsze wykonać test trybu failover na aplikacji przed wykonaniem trybu failover. Testowanie trybu failover pomaga sprawdzić, czy aplikacja zostanie znaleziona w lokacji odzyskiwania.  Jeśli pominięto coś, można łatwo wyzwolić oczyszczania i ponownie przeprowadzić test trybu failover. Do testowania trybu failover wielokrotnie, dopóki nie wiesz, z pewnością, który aplikacja przywraca sprawnie.

![Plan odzyskiwania testu](./media/site-recovery-create-recovery-plans/rptest.png)

**Każda aplikacja jest inne, a następnie należy utworzyć plany odzyskiwania, które są dostosowane do każdego. Ponadto w tym środowisku dynamicznym centrum danych aplikacji oraz ich zależności zachować zmiany. Testowanie trybu failover Sprawdź, czy bieżący plan odzyskiwania aplikacji raz na kwartał.**

## <a name="how-to-create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

1. Kliknij przycisk **planów odzyskiwania** > **utworzenie planu odzyskiwania**.
   Określ nazwę planu odzyskiwania i źródłowym i docelowym. Lokalizacja źródłowa musi mieć maszyn wirtualnych, które są włączone dla trybu failover i odzyskiwania. Wybierz źródło i cel oparte na maszynach wirtualnych, które mają być częścią planu odzyskiwania. 

   |Scenariusz                   |Element źródłowy               |Obiekt docelowy           |
   |---------------------------|---------------------|-----------------|
   |Azure–Azure             |Region platformy Azure         |Region Azure     |
   |VMware do platformy Azure            |Serwer konfiguracji |Azure            |
   |Z programu VMM na platformę Azure               |Przyjazna nazwa programu VMM    |Azure            |
   |Lokacji funkcji Hyper-v na platformę Azure      |Nazwa lokacji funkcji Hyper-v    |Azure            |
   |Maszyn fizycznych do platformy Azure |Serwer konfiguracji |Azure            |
   |Program VMM do programu VMM                 |Przyjazna nazwa programu VMM    |Przyjazna nazwa programu VMM|

   > [!NOTE]
   > Plan odzyskiwania może zawierać maszyn wirtualnych, które mają taką samą źródłowego i docelowego. Maszyny wirtualne VMware i program VMM nie może być częścią tego samego planu odzyskiwania. Maszyny wirtualne VMware i maszyn fizycznych, jednak mogą być dodawane do tego samego planu, jako źródło dla obu z nich jest serwer konfiguracji.

2. W **wybierz maszyny wirtualne**, wybierz maszyny wirtualne (lub grupa replikacji), który ma zostać dodany do domyślnej grupy (Grupa 1) w planie odzyskiwania. Do wyboru mogą być tylko maszyny wirtualne, które były chronione w źródle (jako wybrane w planie odzyskiwania) i są chronione w miejscu docelowym (jako wybrane w planie odzyskiwania).

## <a name="how-to-customize-and-extend-recovery-plans"></a>Sposób dostosowywania i rozszerzania planów odzyskiwania

Można dostosować i rozszerzyć plany odzyskiwania, przechodząc do bloku zasobów planu odzyskiwania usługi Site Recovery i klikając kartę Dostosuj.

Można dostosować i rozszerzyć planów odzyskiwania:

- **Dodaj nowe grupy**— Dodawanie grup planu odzyskiwania dodatkowe (maksymalnie 7) do domyślnej grupy, a następnie dodaj więcej komputerów lub grup replikacji dla grup planu odzyskiwania. Grupy są numerowane w kolejności, w którym dodać. Maszyny wirtualnej lub grupy replikacji tylko mogą zostać włączone w grupie planu odzyskiwania jednego.
- **Dodaj akcję ręczną**— można dodać ręczne akcje, które są uruchamiane przed lub po grupie planu odzyskiwania. Po uruchomieniu planu odzyskiwania przestaje w momencie, jaką dodaje akcji ręcznej. Okno dialogowe zostanie wyświetlony monit o określenie, że akcja ręczna została zakończona.
- **Dodawanie skryptu**— można dodać skryptów uruchamianych przed lub po grupie planu odzyskiwania. Po dodaniu skrypt dodaje nowy zestaw akcje dla grupy. Na przykład, zostanie utworzony zestaw kroków wstępne 1 grupy o nazwie: Grupa 1: wstępne czynności. Wszystkie kroki wstępne są wymienione w tym zestawie. Skrypt można dodać w lokacji głównej, jeśli serwer VMM wdrożone. [Dowiedz się więcej](site-recovery-how-to-add-vmmscript.md).
- **Dodaj elementy runbook Azure**— można rozszerzyć plany odzyskiwania z elementów runbook platformy Azure. Na przykład w celu zautomatyzowania zadań lub utworzyć pojedynczy krok odzyskiwania. [Dowiedz się więcej](site-recovery-runbook-automation.md).


## <a name="how-to-add-a-script-runbook-or-manual-action-to-a-plan"></a>Jak dodać działanie skryptu, element runbook lub ręcznego do planu

Skrypt lub akcji ręcznej można dodać do domyślnej grupy planu odzyskiwania po utworzeniu dodane do niej maszyny wirtualne lub grup replikacji i utworzony plan.

1. Otwórz planu odzyskiwania.
2. Kliknij element **krok** , a następnie kliknij przycisk **skryptu** lub **Akcja ręczna**.
3. Określ, czy chcesz dodać skrypt lub akcji przed lub po wybranego elementu. Aby przenieść pozycji skryptu w górę lub w dół, użyj **Przenieś w górę** i **Przenieś w dół** przycisków.
4. Jeśli dodasz skryptu programu VMM, wybierz **trybu Failover do skryptu VMM**. W **ścieżka skryptu**, wpisz względną ścieżkę do udziału. W poniższym przykładzie programu VMM, określ ścieżkę: **\RPScripts\RPScript.PS1**.
5. Po dodaniu usługi Automatyzacja Azure Uruchom książki Określ konto usługi Automatyzacja Azure, w której znajduje się element runbook, a następnie wybierz skrypt odpowiednich elementów runbook platformy Azure.
6. Aby upewnić się, że skrypt działa zgodnie z oczekiwaniami, czy tryb failover planu odzyskiwania.

Opcje skryptu lub elementu runbook są dostępne tylko w następujących scenariuszach, w trakcie pracy awaryjnej lub powrotu po awarii. Ręczna akcja jest dostępna dla trybu failover i powrotu po awarii.


|Scenariusz               |Tryb failover |Powrót po awarii |
|-----------------------|---------|---------|
|Azure–Azure         |Elementy Runbook |Element Runbook  |
|VMware do platformy Azure        |Elementy Runbook |Nie dotyczy       | 
|Z programu VMM na platformę Azure           |Elementy Runbook |Skrypt   |
|Lokacji funkcji Hyper-v na platformę Azure  |Elementy Runbook |Nie dotyczy       |
|Program VMM do programu VMM             |Skrypt   |Skrypt   |


## <a name="next-steps"></a>Kolejne kroki

[Dowiedz się więcej](site-recovery-failover.md) dotyczące uruchamiania trybu failover.

Obejrzyj ten film, aby zobaczyć odzyskiwania plan działania.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
