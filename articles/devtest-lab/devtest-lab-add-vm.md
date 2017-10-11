---
title: "Dodaj Maszynę wirtualną w laboratorium w usłudze Azure DevTest Labs | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać maszynę wirtualną w laboratorium w usłudze Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: tarcher
ms.openlocfilehash: 449bffb040dafc8edd0b8b0afd80dbea35cd28ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Dodaj Maszynę wirtualną w laboratorium w usłudze Azure DevTest Labs
Jeśli masz już [pierwszej maszyny Wirtualnej utworzone](devtest-lab-create-first-vm.md), prawdopodobnie zostało to z wstępnie załadowane [obrazu z witryny marketplace](devtest-lab-configure-marketplace-images.md). Teraz, jeśli chcesz dodać kolejnych maszyn wirtualnych do laboratorium można także *podstawowej* czyli albo [niestandardowego obrazu](devtest-lab-create-template.md) lub [formuła](devtest-lab-manage-formulas.md). Ten samouczek przeprowadzi Cię przez dodawanie maszyn wirtualnych do laboratorium w usłudze DevTest Labs przy użyciu portalu Azure.

W tym artykule przedstawiono również sposób zarządzania artefaktów dla maszyn wirtualnych w laboratorium.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Kroki, aby dodać Maszynę wirtualną w laboratorium w usłudze Azure DevTest Labs
1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Wybierz **więcej usług**, a następnie wybierz **DevTest Labs** z listy.
1. Z listy labs wybierz laboratorium, w którym chcesz utworzyć maszynę Wirtualną.  
1. W laboratorium **omówienie** bloku, wybierz opcję **+ Dodaj**.  

    ![Dodawanie przycisku maszyny Wirtualnej](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Na **wybierz podstawowej** bloku, wybierz podstawowy dla maszyny Wirtualnej.
1. Na **maszyny wirtualnej** bloku, wprowadź nazwę dla nowej maszyny wirtualnej w **nazwę maszyny wirtualnej** pola tekstowego.

    ![Bloku maszyny Wirtualnej laboratorium](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Wprowadź **nazwy użytkownika** udzieleniu uprawnień administratora na maszynie wirtualnej.  
1. Jeśli chcesz skorzystać z hasła przechowywane w Twojej [tajny magazynu](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), wybierz pozycję **używać hasła zapisane**i określ wartości klucza, który odpowiada klucz tajny (hasło). W przeciwnym razie wprowadź hasło w polu tekstowym etykietą **wpisz wartość**.
1. **Typu dysku maszyny wirtualnej** Określa, który typ dysku magazynu jest dozwolony dla maszyn wirtualnych w laboratorium.
1. Wybierz **rozmiar maszyny wirtualnej** i wybierz jedną z wstępnie zdefiniowane elementy, które Określ rdzeni procesora, rozmiar pamięci RAM i rozmiar dysku twardego maszyny wirtualnej do utworzenia.
1. Wybierz **artefakty** a — z listy artefakty — wybierz i skonfiguruj artefaktów, które chcesz dodać do obrazu podstawowego.
    **Uwaga:** Jeśli nowych użytkowników programu DevTest Labs lub konfigurowanie artefaktów, zapoznaj się [Dodaj istniejący artefakt do maszyny Wirtualnej](#add-an-existing-artifact-to-a-vm) sekcji, a następnie wróć tutaj po zakończeniu.
1. Wybierz **Zaawansowane ustawienia** Aby skonfigurować opcje wygaśnięcia i Opcje sieci maszyny Wirtualnej. 

   Aby ustawić opcję wygaśnięcia, wybierz ikonę kalendarza, aby określić datę, na której maszyna wirtualna zostanie automatycznie usunięty.  Domyślnie maszyna wirtualna nigdy nie wygasa. 
1. Jeśli chcesz wyświetlić lub skopiuj szablon usługi Azure Resource Manager, zapoznaj się [szablonu Zapisz Azure Resource Manager](#save-azure-resource-manager-template) , a następnie wróć tutaj po zakończeniu.
1. Wybierz **Utwórz** dodać określoną maszynę Wirtualną do laboratorium.
1. Blok laboratorium Wyświetla stan tworzenia maszyny Wirtualnej — najpierw jako **tworzenie**, następnie jako **systemem** po uruchomieniu maszyny Wirtualnej.

> [!NOTE]
> [Dodaj Maszynę wirtualną claimable](devtest-lab-add-claimable-vm.md) przedstawiono sposób tak, aby nie jest dostępny do użycia przez każdego użytkownika w środowisku laboratoryjnym claimable maszyny Wirtualnej.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Dodaj istniejący artefakt do maszyny Wirtualnej
Podczas tworzenia maszyny Wirtualnej, można dodać istniejącego artefaktów. Każdy laboratorium obejmuje artefaktów z publicznego DevTest Labs artefaktu repozytorium, a także artefaktów, które zostały utworzone i dodane do repozytorium artefaktów.

* Azure DevTest Labs *artefakty* umożliwiają określenie *akcje* które są wykonywane, gdy maszyna wirtualna zostanie zainicjowana, takie jak uruchamianie skryptów programu Windows PowerShell, uruchamianie poleceń Bash i instalowania oprogramowania.
* Artefaktu *parametry* umożliwiają dostosowanie artefaktu dla konkretnego scenariusza

Aby dowiedzieć się, jak tworzyć artefaktów, zapoznaj się z artykułem [Dowiedz się, jak tworzyć własne artefakty do użycia z DevTest Labs](devtest-lab-artifact-author.md).

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Wybierz **więcej usług**, a następnie wybierz **DevTest Labs** z listy.
1. Z listy labs wybierz laboratorium zawierającej maszynę Wirtualną, z którą chcesz pracować.  
1. Wybierz **Moje maszyny wirtualne**.
1. Wybierz odpowiednią maszynę Wirtualną.
1. Wybierz **artefakty**. 
1. Wybierz **zastosować artefakty**.
1. Na **zastosować artefakty** bloku, wybierz artefakt do dodania do maszyny Wirtualnej.
1. Na **artefaktu Dodaj** bloku, wprowadź wartości parametrów wymaganych i opcjonalnych parametrów, które są potrzebne.  
1. Wybierz **Dodaj** dodać artefaktu, a następnie wróć do **zastosować artefakty** bloku.
1. Czy kontynuować dodawanie artefakty zgodnie z potrzebami dla maszyny Wirtualnej.
1. Po dodaniu użytkownika artefakty możesz [zmienić kolejność, w którym są uruchamiane artefaktów](#change-the-order-in-which-artifacts-are-run). Możesz również wrócić do [wyświetlać lub modyfikować artefaktu](#view-or-modify-an-artifact).
1. Po zakończeniu dodawania artefakty wybierz **Zastosuj**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Zmień kolejność uruchamiania artefaktów
Domyślnie akcje artefakty są wykonywane w kolejności, w której są dodawane do maszyny Wirtualnej. Następujące kroki ilustrują sposób zmienić kolejność, w którym są uruchamiane artefaktów.

1. W górnej części **zastosować artefakty** bloku, wybierz łącze wskazującą liczbę artefaktów, które zostały dodane do maszyny Wirtualnej.
   
    ![Liczba artefaktów dodane do maszyny Wirtualnej](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Na **wybrane artefakty** bloku, przeciągnij i upuść artefaktów w odpowiedni sposób. **Uwaga:** Jeśli masz problemy z przeciąganiem artefaktu, upewnij się, że przeciąga z lewej strony artefaktu. 
1. Po zakończeniu wybierz przycisk **OK**.  

## <a name="view-or-modify-an-artifact"></a>Wyświetlanie i modyfikowanie artefaktów
Następujące kroki ilustrują sposób wyświetlić lub zmodyfikować parametrów artefaktu:

1. W górnej części **zastosować artefakty** bloku, wybierz łącze wskazującą liczbę artefaktów, które zostały dodane do maszyny Wirtualnej.
   
    ![Liczba artefaktów dodane do maszyny Wirtualnej](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Na **wybrane artefakty** bloku, wybierz artefaktu, który chcesz wyświetlić lub edytować.  
1. Na **artefaktu Dodaj** bloku, utwórz wszelkie wymagane zmiany i wybierz **OK** zamknąć **artefaktu Dodaj** bloku.
1. Wybierz **OK** zamknąć **wybrane artefakty** bloku.

## <a name="save-azure-resource-manager-template"></a>Zapisz szablon usługi Azure Resource Manager
Szablon usługi Azure Resource Manager pozwala na deklaratywne Definiowanie powtarzalnych wdrożeń. Poniższych krokach opisano sposób zapisywania szablonu usługi Azure Resource Manager dla tworzenia maszyny Wirtualnej.
Po zapisaniu, można użyć szablonu usługi Azure Resource Manager w celu [wdrażania nowych maszyn wirtualnych przy użyciu programu Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Na **maszyny wirtualnej** bloku, wybierz opcję **szablon ARM widoku**.
2. Na **szablonu widoku Azure Resource Manager** bloku, zaznacz tekst, szablon.
3. Kopiowanie zaznaczonego tekstu do Schowka.
4. Wybierz **OK** zamknąć **bloku Wyświetl szablon Menedżera zasobów Azure**.
5. Otwórz Edytor tekstu.
6. Wklej tekst szablon ze Schowka.
7. Zapisz plik do późniejszego użycia.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Następne kroki
* Po utworzeniu maszyny Wirtualnej można Połącz się z maszyną Wirtualną, wybierając **Connect** w bloku maszyny Wirtualnej.
* Dowiedz się, jak [Tworzenie niestandardowych artefaktów dla maszyny Wirtualnej DevTest Labs](devtest-lab-artifact-author.md).
* Eksploruj [galerię szablonów DevTest Labs Azure Resource Manager — Szybki Start](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
