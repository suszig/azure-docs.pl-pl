---
title: "Konfigurowanie sieci wirtualnej w usłudze Azure DevTest Labs | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować istniejącą sieć wirtualną i podsieć, a następnie używać ich na maszynie wirtualnej z platformy Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 19dbb1625f46f8864413dc538a96b2413bc6eea0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Konfigurowanie sieci wirtualnej w usłudze Azure DevTest Labs
Jak opisano w artykule [dodać maszyny Wirtualnej z artefaktami do laboratorium](devtest-lab-add-vm-with-artifacts.md), podczas tworzenia maszyn wirtualnych w laboratorium, można określić skonfigurowanej sieci wirtualnej. Jeden scenariusz w ten sposób jest konieczne dostęp do zasobów sieci corpnet z maszyn wirtualnych przy użyciu sieci wirtualnej, który został skonfigurowany za pomocą usługi ExpressRoute i sieci VPN typu lokacja lokacja. Poniższe sekcje przedstawiają sposób dodawania istniejącej sieci wirtualnej do ustawień sieci wirtualnej laboratorium, aby była ona dostępna do wyboru podczas tworzenia maszyn wirtualnych.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Skonfiguruj sieć wirtualną dla laboratorium przy użyciu portalu Azure
W poniższych krokach objaśniono przez procedurę dodawania istniejącej sieci wirtualnej (i podsieci) do laboratorium, dzięki czemu można użyć podczas tworzenia maszyny Wirtualnej w tym samym laboratorium. 

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Wybierz **więcej usług**, a następnie wybierz **DevTest Labs** z listy.
3. Z listy labs wybierz żądany laboratorium. 
4. W bloku laboratorium, wybierz **konfiguracji i zasadach**.
5. W laboratorium **konfiguracji i zasadach** bloku, wybierz opcję **sieci wirtualnych**.
6. Na **sieci wirtualnych** bloku, wyświetlić listę sieci wirtualne skonfigurowane dla bieżącego laboratorium, a także domyślne sieci wirtualnej utworzonym dla laboratorium. 
7. Wybierz **+ Dodaj**.
   
    ![Dodawanie istniejącej sieci wirtualnej do laboratorium](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. Na **sieci wirtualnej** bloku, wybierz opcję **[Wybierz sieć wirtualną]**.
   
    ![Wybierz istniejącą sieć wirtualną](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. Na **sieci wirtualnej wybierz** bloku, wybierz odpowiednią sieć wirtualną. Blok zawiera wszystkie sieci wirtualne, które są w tym samym regionie, w ramach subskrypcji jako laboratorium.  
10. Po wybraniu sieci wirtualnej, nastąpi powrót do **sieci wirtualnej** kliknij podsieci na liście w dolnej części bloku.

    ![Lista podsieci](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Blok podsieci laboratorium jest wyświetlany.

    ![Blok podsieci laboratorium](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. Określ **Nazwa podsieci laboratorium**.
12. Aby zezwolić na podsieci do użycia w laboratorium tworzenia maszyny Wirtualnej, wybierz **używany podczas tworzenia maszyny wirtualnej**.
13. Aby włączyć [udostępnionych publicznego adresu IP](devtest-lab-shared-ip.md), wybierz pozycję **Włącz udostępnione publicznego adresu IP**.
14. Aby zezwolić na publiczne adresy IP w podsieci, wybierz **pozwala publicznego adresu IP tworzenie**.
15. W **maksymalna maszyn wirtualnych dla użytkownika** pola, określ maksymalną maszyn wirtualnych dla poszczególnych użytkowników, dla każdej podsieci. Jeśli chcesz nieograniczoną liczbę maszyn wirtualnych, pozostaw to pole puste.
16. Wybierz **OK** zamknąć blok podsieci laboratorium.
17. Wybierz **zapisać** zamknąć bloku sieci wirtualnej.
18. Teraz, gdy skonfigurowano sieci wirtualnej, można wybrać podczas tworzenia maszyny Wirtualnej. 
    Aby sprawdzić, jak utworzyć Maszynę wirtualną i określ sieć wirtualną, zapoznaj się z artykułem [dodać maszyny Wirtualnej z artefaktami do laboratorium](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Następne kroki
Po dodaniu żądanej sieci wirtualnej do laboratorium, następnym krokiem jest [Dodaj Maszynę wirtualną do laboratorium](devtest-lab-add-vm-with-artifacts.md).

