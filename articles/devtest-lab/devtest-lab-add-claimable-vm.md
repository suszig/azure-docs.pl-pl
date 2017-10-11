---
title: "Dodaj claimable maszyny Wirtualnej do laboratorium w usłudze Azure DevTest Labs | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać claimable maszyny wirtualnej do laboratorium w usłudze Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: tarcher
ms.openlocfilehash: 98950d72e90b0e178bae2fffa7644fd824a25eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Dodaj claimable maszyny Wirtualnej do laboratorium w usłudze Azure DevTest Labs
Dodaj Maszynę wirtualną claimable laboratorium w podobny sposób jak możesz [dodać standardowe maszyny Wirtualnej](devtest-lab-add-vm.md) — z *podstawowej* czyli albo [niestandardowego obrazu](devtest-lab-create-template.md), [formuła](devtest-lab-manage-formulas.md) , lub [obrazu z witryny Marketplace](devtest-lab-configure-marketplace-images.md). Ten samouczek przeprowadzi Cię przez dodawanie claimable maszyny Wirtualnej do laboratorium w usłudze DevTest Labs przy użyciu portalu Azure i przedstawiono proces się, że użytkownik wykonuje przejąć maszyny Wirtualnej.

> [!NOTE]
> W przypadku wdrożenia maszyn wirtualnych laboratorium za pośrednictwem [szablonów usługi Azure Resource Manager](devtest-lab-create-environment-from-arm.md), można utworzyć claimable maszyn wirtualnych, ustawiając **allowClaim** właściwości na wartość true w sekcji właściwości.
>
>

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Kroki, aby dodać claimable maszyny Wirtualnej do laboratorium w usłudze Azure DevTest Labs
1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Wybierz **więcej usług**, a następnie wybierz **DevTest Labs** z listy.
1. Z listy labs wybierz laboratorium, w którym chcesz utworzyć claimable maszyny Wirtualnej.  
1. W laboratorium **omówienie** bloku, wybierz opcję **+ Dodaj**.  

    ![Dodawanie przycisku maszyny Wirtualnej](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Na **wybierz podstawowej** bloku, wybierz podstawowy dla maszyny Wirtualnej.
1. Na **maszyny wirtualnej** bloku, wprowadź nazwę dla nowej maszyny wirtualnej w **nazwę maszyny wirtualnej** pola tekstowego.

    ![Bloku maszyny Wirtualnej laboratorium](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Wprowadź **nazwy użytkownika** udzieleniu uprawnień administratora na maszynie wirtualnej.  
1. Jeśli chcesz skorzystać z hasła przechowywane w Twojej [tajny magazynu](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), wybierz pozycję **używać hasła zapisane**i określ wartości klucza, który odpowiada klucz tajny (hasło). W przeciwnym razie wprowadź hasło w polu tekstowym etykietą **wpisz wartość**.
1. **Typu dysku maszyny wirtualnej** Określa, który typ dysku magazynu jest dozwolony dla maszyn wirtualnych w laboratorium.
1. Wybierz **rozmiar maszyny wirtualnej** i wybierz jedną z wstępnie zdefiniowane elementy, które Określ rdzeni procesora, rozmiar pamięci RAM i rozmiar dysku twardego maszyny wirtualnej do utworzenia.
1. Wybierz **artefakty** i na liście artefaktów, wybierz i skonfiguruj artefaktów, które chcesz dodać do obrazu podstawowego. Jeśli nowych użytkowników programu DevTest Labs lub konfigurowanie artefaktów, zapoznaj się [Dodaj istniejący artefakt do maszyny Wirtualnej](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) sekcji, a następnie wróć tutaj po zakończeniu.
1. Wybierz **Zaawansowane ustawienia** Aby skonfigurować opcje wygaśnięcia i Opcje sieci maszyny Wirtualnej. W obszarze **oświadczeń opcje**, wybierz **tak** dokonanie claimable komputera.

  ![Wybierz claimable maszyny Wirtualnej.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Jeśli chcesz wyświetlić lub skopiuj szablon usługi Azure Resource Manager, zapoznaj się [szablonu Zapisz Azure Resource Manager](devtest-lab-add-vm.md#save-azure-resource-manager-template) , a następnie wróć tutaj po zakończeniu.
1. Wybierz **Utwórz** dodać określoną maszynę Wirtualną do laboratorium.
1. Blok laboratorium Wyświetla stan tworzenia maszyny Wirtualnej — najpierw jako **tworzenie**, następnie jako **systemem** po uruchomieniu maszyny Wirtualnej.


## <a name="using-a-claimable-vm"></a>Przy użyciu claimable maszyny Wirtualnej

Użytkownik może oświadczeń żadnej maszyny Wirtualnej z listy "Claimable maszyny wirtualne", wykonując jedną z następujących czynności:

* Z listy "Claimable maszyny wirtualnej" w dolnej części bloku omówienie laboratorium należy utworzyć, kliknij prawym przyciskiem myszy na jednym z maszyn wirtualnych na liście, a następnie wybierz pozycję **maszyny oświadczeń**.

 ![Żądanie claimable określonej maszyny Wirtualnej.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* W górnej części **omówienie** bloku, wybierz **uzyskania**. Losowe maszyny wirtualnej jest przypisane z listy claimable maszyn wirtualnych.

 ![Żądanie żadnej claimable maszyny Wirtualnej.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Po użytkownika oświadczeń maszyny Wirtualnej, przenieść w górę do listy "Moje maszyny wirtualne" i nie jest już claimable przez innego użytkownika.

## <a name="next-steps"></a>Następne kroki
* Po utworzeniu maszyny Wirtualnej można Połącz się z maszyną Wirtualną, wybierając **Connect** w bloku maszyny Wirtualnej.
* Eksploruj [galerię szablonów DevTest Labs Azure Resource Manager — Szybki Start](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates)
