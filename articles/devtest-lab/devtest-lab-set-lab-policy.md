---
title: "Zarządzanie zasadami laboratorium w usłudze Azure DevTest Labs | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zdefiniować zasad laboratorium, na przykład rozmiary maszyn wirtualnych, maksymalna maszyn wirtualnych dla użytkowników i zamykania automatyzacji."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 328a4d893637d7150807855e118b485a2c3bbfc5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Zarządzanie wszystkimi zasadami dla laboratorium w usłudze Azure DevTest Labs

Azure DevTest Labs umożliwia kontrolowanie kosztów i zminimalizować odpady w Twojej labs przez Zarządzanie zasadami (ustawienia) dla każdego laboratorium. W tym artykule opisano szczegółowo krok po kroku, jak ustawić każdej zasady.  

## <a name="set-allowed-virtual-machine-sizes"></a>Zestaw dozwolone rozmiary maszyn wirtualnych
Zasady ustalania dozwolone rozmiary maszyn wirtualnych pozwala zminimalizować odpady laboratorium, umożliwiając określenie rozmiarów maszyn wirtualnych, które są dozwolone w środowisku laboratoryjnym. Jeśli zasada ta jest aktywna, tylko rozmiary maszyn wirtualnych z tej listy mogą służyć do tworzenia maszyn wirtualnych.

1. W laboratorium **konfiguracji i zasadach** bloku, wybierz opcję **dozwolone rozmiary maszyn wirtualnych**.
   
    ![Rozmiary maszyn wirtualnych dozwolonych](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Wybierz **na** Aby włączyć tę zasadę, a **poza** je wyłączyć.

1. Po włączeniu tych zasad, wybierz rozmiarów maszyn wirtualnych, które mogą zostać utworzone w laboratorium.

1. Wybierz pozycję **Zapisz**.

## <a name="set-virtual-machines-per-user"></a>Zestaw maszyn wirtualnych dla użytkownika
Zasady dla **maszyn wirtualnych dla użytkownika** można określić maksymalną liczbę maszyn wirtualnych, które mogą zostać utworzone przez użytkownika. Jeśli użytkownik próbuje utworzyć lub oświadczeń maszyny Wirtualnej, gdy limit użytkowników zostały spełnione, komunikat o błędzie wskazuje, że maszyna wirtualna nie może być utworzony przejęte. 

1. W laboratorium **konfiguracji i zasadach** menu, wybierz opcję **maszyn wirtualnych dla użytkownika**.
   
    ![Maszyny wirtualne na użytkownika](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Wybierz **tak** Aby ograniczyć liczbę maszyn wirtualnych dla poszczególnych użytkowników. Jeśli nie chcesz ograniczyć liczbę maszyn wirtualnych dla poszczególnych użytkowników, wybierz **nr**. W przypadku wybrania **tak**, wprowadź wartość liczbową wskazującą maksymalną liczbę maszyn wirtualnych, które można utworzyć ani przejęte przez użytkownika. 

1. Wybierz **tak** Aby ograniczyć liczbę maszyn wirtualnych, które mogą używać dysków SSD (dysk SSD). Jeśli nie chcesz ograniczyć liczbę maszyn wirtualnych, które można używać dysków SSD, wybierz **nr**. W przypadku wybrania **tak**, wprowadź wartość wskazującą maksymalną liczbę maszyn wirtualnych, które mogą zostać utworzone przy użyciu dysków SSD. 

1. Wybierz pozycję **Zapisz**.

## <a name="set-virtual-machines-per-lab"></a>Zestaw maszyn wirtualnych dla laboratorium
Zasady dla **maszyn wirtualnych dla laboratorium** można określić maksymalną liczbę maszyn wirtualnych, które mogą być tworzone dla bieżącego laboratorium. Jeśli użytkownik próbuje utworzyć Maszynę wirtualną, jeśli osiągnięto limit laboratorium, komunikat o błędzie wskazuje, że nie można utworzyć maszyny Wirtualnej. 

1. W laboratorium **konfiguracji i zasadach** menu, wybierz opcję **maszyn wirtualnych dla laboratorium**.
   
    ![Maszyn wirtualnych dla laboratorium](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Wybierz **tak** Aby ograniczyć liczbę maszyn wirtualnych dla laboratorium. Jeśli nie chcesz ograniczyć liczbę maszyn wirtualnych dla laboratorium, wybierz **nr**. W przypadku wybrania **tak**, wprowadź wartość liczbową wskazującą maksymalną liczbę maszyn wirtualnych, które można utworzyć ani przejęte przez użytkownika. 

1. Wybierz **tak** Aby ograniczyć liczbę maszyn wirtualnych, które mogą używać dysków SSD (dysk SSD). Jeśli nie chcesz ograniczyć liczbę maszyn wirtualnych, które można używać dysków SSD, wybierz **nr**. W przypadku wybrania **tak**, wprowadź wartość wskazującą maksymalną liczbę maszyn wirtualnych, które mogą zostać utworzone przy użyciu dysków SSD. 

1. Wybierz pozycję **Zapisz**.

## <a name="set-auto-shutdown"></a>Ustaw automatyczne zamykanie
Zasady automatycznego zamykania pozwala zminimalizować odpady laboratorium, umożliwiając umożliwia określenie czasu zamykania maszyn wirtualnych w tym laboratorium.

1. W laboratorium **konfiguracji i zasadach** bloku, wybierz opcję **automatyczne zamykanie**.
   
    ![Automatyczne zamykanie](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Wybierz **na** Aby włączyć tę zasadę, a **poza** je wyłączyć.

1. Po włączeniu tych zasad, określ czas (i strefy czasowej) do zamykania wszystkich maszyn wirtualnych w bieżącym laboratorium.

1. Określ **tak** lub **nr** opcji wysyłania powiadomień 15 minut przed upływem czasu określonego automatyczne zamykanie. Jeśli określisz **tak**, wprowadź punktu końcowego adresu URL elementu webhook do odbioru powiadomienia. Aby uzyskać więcej informacji na temat elementów webhook, zobacz [tworzenia elementu webhook lub funkcja interfejsu API Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Wybierz pozycję **Zapisz**.

    Domyślnie po włączeniu tych zasad dotyczy wszystkich maszyn wirtualnych w bieżącym laboratorium. Aby usunąć tego ustawienia z określonej maszyny Wirtualnej, otwórz blok maszyny Wirtualnej i zmień jego **automatyczne zamykanie** ustawienie 

## <a name="set-auto-start"></a>Ustaw automatyczne uruchamianie
Zasady automatycznego uruchamiania pozwala określić, kiedy mają być uruchamiane maszyny wirtualne w bieżącym laboratorium.  

1. W laboratorium **konfiguracji i zasadach** bloku, wybierz opcję **Auto-start**.
   
    ![Automatyczne uruchamianie](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Wybierz **na** Aby włączyć tę zasadę, a **poza** je wyłączyć.

3. Po włączeniu tych zasad, określ z zaplanowanym czasem rozpoczęcia, strefa czasowa i dni tygodnia, którego dotyczy czas. 

4. Wybierz pozycję **Zapisz**.

    Po włączeniu tych zasad nie została automatycznie zastosowana do żadnej maszyny wirtualnej w bieżącym laboratorium. Aby zastosować to ustawienie do określonej maszyny Wirtualnej, otwórz blok maszyny Wirtualnej i zmień jego **Auto-start** ustawienie 

## <a name="set-expiration-date"></a>Ustawianie daty wygaśnięcia
Można ustawić wygaśnięcie datę, gdy użytkownik [utworzyć maszynę Wirtualną](devtest-lab-add-vm.md). W **Zaawansowane ustawienia**, wybierz ikonę kalendarza, aby określić datę, na której maszyna wirtualna zostanie automatycznie usunięty.  Domyślnie maszyna wirtualna nigdy nie wygasa.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Następne kroki
Po zdefiniowany i zastosować różne ustawienia zasad maszyn wirtualnych dla laboratorium, poniżej przedstawiono niektóre czynności, aby spróbować obok:

* [Zrozumienie udostępnionego adresy IP](devtest-lab-shared-ip.md) -wyjaśniono, jak udostępnione IP adresy są używane w usłudze DevTest Labs, aby zminimalizować liczbę publicznych adresów IP wymagane do nawiązania laboratorium maszyn wirtualnych.
* [Konfigurowanie zarządzania koszt](devtest-lab-configure-cost-management.md) -ilustruje sposób używania **miesięczny Trend szacowany koszt** wykresu  
  Aby wyświetlić bieżącego miesiąca na szacowane Data kosztu i szacowany koszt koniec miesiąca.
* [Tworzenie niestandardowego obrazu](devtest-lab-create-template.md) — podczas tworzenia maszyny Wirtualnej, należy określić podstawowy, który może być niestandardowy obraz lub obrazu z witryny Marketplace. W tym artykule przedstawiono sposób tworzenia niestandardowego obrazu z pliku VHD.
* [Konfigurowanie portalu Marketplace obrazów](devtest-lab-configure-marketplace-images.md) — Azure DevTest Labs obsługuje tworzenie maszyn wirtualnych, oparte na obrazach portalu Azure Marketplace. W tym artykule przedstawiono sposób określić, które, portalu Azure Marketplace obrazy mogą być używane podczas tworzenia maszyn wirtualnych w laboratorium.
* [Utwórz maszynę Wirtualną w laboratorium](devtest-lab-add-vm-with-artifacts.md) -ilustruje sposób tworzenia maszyny Wirtualnej z obrazu podstawowego (albo niestandardowe lub Marketplace) oraz sposób pracy z artefaktami w maszynie Wirtualnej.

