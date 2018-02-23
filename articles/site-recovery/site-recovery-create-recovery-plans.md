---
title: "Tworzenie i Dostosowywanie planu odzyskiwania w trybie failover i odzyskiwania w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzyć i dostosowywać plany odzyskiwania w usłudze Azure Site Recovery. W tym artykule opisano sposób pracy awaryjnej i odzyskiwania maszyn wirtualnych i serwerów fizycznych."
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
ms.openlocfilehash: 6ad82a8a2f8e16ab794ba90c109904bd45cb6b89
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-recovery-plan-by-using-site-recovery"></a>Tworzenie planu odzyskiwania przy użyciu usługi Site Recovery

W tym artykule opisano sposób tworzenia i dostosowywania planu odzyskiwania w [usługi Azure Site Recovery](site-recovery-overview.md).

Tworzenie planu odzyskiwania, aby wykonać następujące czynności:

* Definiowanie grup komputerów w tryb failover ze sobą i uruchom razem.
* Model zależności między maszynami przez ich grupowanie razem w grupie planu odzyskiwania. Na przykład aby tryb failover i wyświetlić określonej aplikacji, obejmują wszystkich maszyn wirtualnych dla tej aplikacji w tej samej grupie planu odzyskiwania.
* Uruchom tryb failover. Można uruchomić testu, planowane, ani nieplanowane przełączenie awaryjne, w ramach planu odzyskiwania.

## <a name="why-use-a-recovery-plan"></a>Dlaczego warto używać planu odzyskiwania

Plan odzyskiwania może pomóc przygotowania proces systematyczne odzyskiwania przez utworzenie małych jednostek niezależne, którymi można zarządzać. Jednostki zwykle odpowiadają aplikacji w danym środowisku. Plan odzyskiwania może pomóc zdefiniować sekwencji, w którym start maszyn wirtualnych. Plan odzyskiwania umożliwia również automatyzacji typowych zadań podczas odzyskiwania.

> [!TIP]
> Jednym ze sposobów sprawdzenia, czy są przygotowywane do chmury odzyskiwania migracji lub po awarii jest zapewnienie, że poszczególnych aplikacji jest częścią planu odzyskiwania. Ponadto upewnij się, że każdy plan odzyskiwania jest sprawdzane pod kątem odzyskiwania na platformie Azure. Z tego przygotowania można bezpiecznie migracji lub pracy awaryjnej pełną centrum danych Azure.
 
Plan odzyskiwania ma trzy kluczowe wartościowe propozycje:

* Model aplikacji do przechwytywania zależności.
* Zautomatyzować większość zadań odzyskiwania, aby zmniejszyć RTO.
* Testowanie trybu failover będzie gotowa do awarii.

### <a name="model-an-application-to-capture-dependencies"></a>Model aplikacji do przechwytywania zależności

Plan odzyskiwania to grupa maszyny wirtualne wchodzące w skład zazwyczaj aplikacji i który przełączyć się ze sobą. Konstruuje przy użyciu planu odzyskiwania, można zwiększyć, grupę planu odzyskiwania, aby przechwycić właściwości specyficzne dla aplikacji. 

W tym artykule używamy Typowa trójwarstwowa aplikacja, której może występować SQL kopii punkt końcowy, oprogramowanie pośredniczące i frontonu sieci web. Można dostosować do zapewnienia, że maszyna wirtualna jest uruchamiana we właściwej kolejności po przejściu w tryb failover planu odzyskiwania. Zaplecza SQL powinna uruchomić się jako pierwszy, oprogramowanie pośredniczące należy uruchomić następny i frontonu sieci web powinna być uruchamiana ostatniego. To zamówienie gwarantuje, że aplikacja działa w czasie uruchamiania Ostatnia maszyna wirtualna. 

Na przykład po uruchomieniu oprogramowanie pośredniczące go próbuje podłączyć się do warstwy SQL. Plan odzyskiwania zapewnia warstwy SQL jest już uruchomiony. Konieczności uruchomienia na serwerze frontonu ostatnio pozwala zagwarantować, że użytkownicy końcowi nie łączyć do adresu URL aplikacji, zanim wszystkie składniki są uruchomione i uruchomiona, a aplikacja jest gotowa do akceptowania żądań. Aby utworzyć te zależności, dostosowywania planu odzyskiwania, aby dodać grupy, a następnie wybierz maszynę wirtualną. Aby przenieść maszyny wirtualnej między grupami, Zmień grupę maszyny wirtualnej.

![Zrzut ekranu przedstawiający przykładowy plan odzyskiwania w usłudze Site Recovery](./media/site-recovery-create-recovery-plans/rp.png)

Po ukończeniu dostosowań można zwizualizować kolejnych kroków odzyskiwania. Poniżej przedstawiono kolejność kroków, które są wykonywane w trybie failover planu odzyskiwania:

1. Krok zamknięcia próbuje wyłączyć maszyny wirtualne lokalną. (Z wyjątkiem testowy tryb failover, w którym lokacji głównej musi nadal działać.)
2. Próba zamknięcia wyzwala trybu failover wszystkich maszyn wirtualnych w planie odzyskiwania równolegle. Krok trybu failover przygotowuje dysków maszyny wirtualnej przy użyciu zreplikowanych danych.
3. Grupy uruchamiania wykonywany w kolejności ich i uruchom maszyny wirtualne w każdej grupie. Najpierw wykonuje grupy 1, a następnie wykonuje grupa 2 i finally, grupa 3 wykonuje. Jeśli istnieje więcej niż jednej maszyny wirtualnej w dowolnej grupie (na przykład równoważeniem obciążenia frontonu sieci web), wszystkie maszyny wirtualne Uruchom równolegle.

> [!TIP]
> Sekwencjonowanie między grupami gwarantuje, że zależności między warstwami aplikacji różnych są honorowane. Równoległość, gdzie jest to odpowiednie do użycia, poprawia RTO odzyskiwania aplikacji.

   > [!NOTE]
   > Komputery, które są częścią pojedynczej grupy w trybie Failover równolegle. Komputery, które należą do różnych grup w trybie Failover według grup. Komputery z grupy 2 start ich pracy awaryjnej tylko wtedy, gdy wszystkie komputery z grupy 1 zostały przełączone do trybu failover i uruchomić.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Większość zadań odzyskiwania, aby zmniejszyć RTO automatyzacji

Odzyskiwanie dużych aplikacji może być złożonym zadaniem. O wiele ręczne do zapamiętania czas chaos jest trudne i sprawia, że proces jest podatne na błędy. Ponadto może być osoby, która nie rozpoznaje mogli dokładnie zapoznać aplikacji, które faktycznie wyzwala pracy awaryjnej. 

Plan odzyskiwania można użyć do automatyzowania wymagane akcje, które należy wykonać w każdym kroku. Wymagane akcje można skonfigurować przy użyciu elementu runbook usługi Automatyzacja Azure. Z elementami runbook można zautomatyzować typowych zadań odzyskiwania, takich jak zadania w poniższych przykładach. Zadania, które nie mogły zostać zautomatyzowane można wstawić działań ręcznych do planów odzyskiwania.

* **Zadania na post-trybu failover maszyny wirtualnej platformy Azure**: te zadania są zwykle wymagane Aby można było połączyć się z maszyną wirtualną. Przykłady:
    * Utwórz publiczny adres IP na post-tryb failover maszyny wirtualnej.
    * Przypisywanie sieciowej grupy zabezpieczeń do karty Sieciowej na maszynie wirtualnej przełączona w tryb failover.
    * Dodaj usługi równoważenia obciążenia do zestawu dostępności.
* **Zadania wewnątrz maszyny wirtualnej po pracy w trybie failover**: te zadania ponownie skonfigurować aplikację tak, aby go w dalszym ciągu działać w nowym środowisku. Przykłady:
    * Zmodyfikuj parametry połączenia bazy danych na maszynie wirtualnej.
    * Zmień konfigurację serwera sieci web lub reguły.

> [!TIP]
> Osiągnięcia trybu failover z jednym kliknięciem i zoptymalizować RTO przez utworzenie planu odzyskiwania ukończone, który automatyzuje zadania po odzyskiwaniu przy użyciu elementu runbook usługi Automatyzacja.

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testowanie trybu failover będzie gotowa do awarii

Aby wyzwolić tryb failover lub test trybu failover, można użyć planu odzyskiwania. Zawsze ukończyć test trybu failover na aplikacji przed wykonaniem trybu failover. Testowanie przechodzenia w tryb failover pomoc, sprawdź, czy aplikacja pojawia się w lokacji odzyskiwania. Pominięto element w konfiguracji, można łatwo wyzwolić oczyszczania i ponownie przeprowadzić test trybu failover. Wiele razy do momentu sprawnie odzyskuje aplikację do testowania trybu failover.

![Zrzut ekranu przedstawiający przykładowy plan odzyskiwania testowy w usłudze Site Recovery](./media/site-recovery-create-recovery-plans/rptest.png)

> [!TIP]
> Ponieważ każda aplikacja jest unikatowa, należy utworzyć plany odzyskiwania, które są dostosowane dla każdej aplikacji. 
>
> Ponadto w obecnych dynamicznych, fokus centrum danych, aplikacji oraz ich zależności często zmienić. Aby upewnić się, że plan odzyskiwania jest aktualna, należy przetestować aplikacje w trybie failover co kwartał.

## <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

1. Wybierz **planów odzyskiwania** > **utworzenie planu odzyskiwania**.
   Określ nazwę planu odzyskiwania i źródłowym i docelowym. Lokalizacja źródłowa musi mieć maszyn wirtualnych, które są włączone dla trybu failover i odzyskiwania. Wybierz źródło i cel oparte na maszynach wirtualnych, które mają być częścią planu odzyskiwania. 

   |Scenariusz                   |Element źródłowy               |Obiekt docelowy           |
   |---------------------------|---------------------|-----------------|
   |Azure–Azure             |Region platformy Azure         |Region platformy Azure     |
   |Z programu VMware do platformy Azure            |Serwer konfiguracji |Azure            |
   |Program Virtual Machine Manager (VMM) na platformie Azure               |Nazwa wyświetlana programu VMM    |Azure            |
   |Z lokacji funkcji Hyper-V na platformę Azure      |Nazwa lokacji funkcji Hyper-V    |Azure            |
   |Maszyn fizycznych do platformy Azure |Serwer konfiguracji |Azure            |
   |Program VMM do programu VMM                 |Przyjazna nazwa programu VMM    |Nazwa wyświetlana programu VMM|

   > [!NOTE]
   > Plan odzyskiwania może zawierać maszyn wirtualnych, które mają taką samą źródłowego i docelowego. Maszyny wirtualne VMware i System Center Virtual Machine Manager (VMM) nie może być w tym samym planie odzyskiwania. Jednak można dodać maszyny wirtualne VMware i maszyn fizycznych do tego samego planu odzyskiwania. W takim przypadku źródło na obu komputerach jest serwer konfiguracji.

2. W obszarze **wybierz maszyny wirtualne**, wybierz maszyny wirtualne (lub grupa replikacji), który ma zostać dodany do domyślnej grupy (Grupa 1) w planie odzyskiwania. Można wybrać tylko maszyny wirtualne, które były chronione w źródle (jako wybrane w planie odzyskiwania) i które są chronione na elemencie docelowym (jako wybrane w planie odzyskiwania).

## <a name="customize-and-extend-recovery-plans"></a>Dostosowywanie i rozszerzanie planów odzyskiwania

Dostosowywanie i rozszerzanie plany odzyskiwania, przejdź do okienka zasobów planu odzyskiwania usługi Site Recovery. Wybierz **Dostosuj** kartę. Można dostosować i rozszerzyć planów odzyskiwania przy użyciu następujących opcji:

- **Dodaj nowe grupy**: można dodać do siedmiu grup planu odzyskiwania dodatkowe do domyślnej grupy. Następnie można dodać więcej komputerów lub grup replikacji dla grup planu odzyskiwania. Grupy są numerowane w kolejności, w którym dodać. Maszyny wirtualnej lub grupa replikacji może zawierać tylko w jednej grupie planu odzyskiwania.
- **Dodaj akcję ręczną**: można dodać ręczne akcje, które są uruchamiane przed lub po grupie planu odzyskiwania. Po uruchomieniu planu odzyskiwania przestaje w momencie, jaką dodaje akcji ręcznej. Okno dialogowe zostanie wyświetlony monit o określenie, że akcja ręczna została zakończona.
- **Dodawanie skryptu**: istnieje możliwość dodania skryptów uruchamianych przed lub po grupie planu odzyskiwania. Po dodaniu skrypt dodaje nowy zestaw akcje dla grupy. Na przykład zestaw kroków wstępnego dla grupy 1 jest tworzony z nazwą *Grupa 1: kroki przed*. Wszystkie kroki wstępne są wymienione w tym zestawie. Skrypt w lokacji głównej można dodać tylko wtedy, gdy jest wdrożony serwer programu VMM. Aby uzyskać więcej informacji, zobacz [skrypt programu VMM do planu odzyskiwania](site-recovery-how-to-add-vmmscript.md).
- **Dodaj elementy runbook Azure**: planów odzyskiwania można rozszerzyć za pomocą elementów runbook platformy Azure. Na przykład można użyć elementu runbook do automatyzacji zadań lub utworzyć pojedynczy krok odzyskiwania. Aby uzyskać więcej informacji, zobacz [elementu runbook usługi Automatyzacja Azure Dodaj do planów odzyskiwania](site-recovery-runbook-automation.md).


## <a name="add-a-script-runbook-or-manual-action-to-a-plan"></a>Dodawanie skryptu, element runbook lub akcji ręcznej do planu

Po dodaniu grup replikacji lub maszyn wirtualnych do domyślnej grupy planu odzyskiwania skryptu lub akcji ręcznej można dodać do grupy planu odzyskiwania.

1. Otwórz planu odzyskiwania.
2. W **krok** , wybierz element na liście. Następnie wybierz opcję **skryptu** lub **Akcja ręczna**.
3. Określ, czy chcesz dodać skrypt lub akcji przed lub po wybranego elementu. Aby przemieścić skryptu w górę lub w dół, wybierz **Przenieś w górę** lub **Przenieś w dół** przycisków.
4. Jeśli dodasz skryptu programu VMM, wybierz **trybu Failover do skryptu VMM**. W **ścieżka skryptu**, wprowadź ścieżkę względną do udziału. Na przykład **\RPScripts\RPScript.PS1**.
5. Jeśli dodasz element runbook usługi Automatyzacja, należy określić konto automatyzacji, w którym znajduje się elementu runbook. Wybierz skrypt runbook platformy Azure.
6. Aby upewnić się, że skrypt działa zgodnie z oczekiwaniami, czy tryb failover planu odzyskiwania.

Opcje skryptu lub elementu runbook są dostępne tylko w następujących scenariuszach i podczas pracy awaryjnej lub powrotu po awarii. Akcja ręczna będzie dostępna zarówno w trybie failover i powrotu po awarii.


|Scenariusz               |Tryb failover |Powrót po awarii |
|-----------------------|---------|---------|
|Azure–Azure         |Element Runbook |Element Runbook  |
|Z programu VMware do platformy Azure        |Element Runbook |Nie dotyczy       | 
|Z programu VMM na platformę Azure           |Element Runbook |Skrypt   |
|Z lokacji funkcji Hyper-V na platformę Azure  |Element Runbook |Nie dotyczy       |
|Program VMM do programu VMM             |Skrypt   |Skrypt   |


## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się więcej o [uruchamiania trybu failover](site-recovery-failover.md).  
* Aby wyświetlić odzyskiwania planowanie w akcji, zobacz ten film:
    
    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
