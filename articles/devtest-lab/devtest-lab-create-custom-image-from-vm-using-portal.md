---
title: Tworzenie obrazu niestandardowego Azure DevTest Labs na podstawie maszyny Wirtualnej | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć obraz niestandardowy w usłudze Azure DevTest Labs elastycznie maszyny wirtualnej przy użyciu portalu Azure"
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
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 9d2dcf7164985508d691e8a0c123efaf3b8aa19a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-from-a-vm"></a>Tworzenie niestandardowego obrazu z maszyny Wirtualnej

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Instrukcje krok po kroku

Można utworzyć niestandardowy obraz z elastycznie maszyny Wirtualnej, a później użyć niestandardowego obrazu do utworzenia identycznych maszyn wirtualnych. Poniższe kroki pokazano, jak utworzyć obraz niestandardowy z maszyny Wirtualnej:

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Wybierz pozycję **Więcej usług**, a następnie z listy wybierz pozycję **DevTest Labs**.

1. Z listy labs wybierz żądany laboratorium.  

1. W bloku laboratorium, wybierz **Moje maszyny wirtualne**.
 
1. Na **Moje maszyny wirtualne** bloku, wybierz maszynę Wirtualną, z którego chcesz utworzyć niestandardowy obraz.

1. W bloku maszyny Wirtualnej, wybierz **Tworzenie niestandardowego obrazu (VHD)**.

    ![Tworzenie niestandardowego obrazu elementu menu](./media/devtest-lab-create-template/create-custom-image.png)

1. Na **Utwórz obraz** bloku, wprowadź nazwę i opis dla niestandardowego obrazu. Te informacje są wyświetlane na liście klas podstawowych, podczas tworzenia maszyny Wirtualnej.

    ![Tworzenie niestandardowego obrazu bloku](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Określ, czy uruchomiono program sysprep na maszynie Wirtualnej. Jeśli nie uruchomiono programu sysprep na maszynie Wirtualnej, określ, czy uruchamiany po utworzeniu maszyny Wirtualnej z tego obrazu niestandardowego narzędzia sysprep.

1. Wybierz **OK** po zakończeniu można utworzyć niestandardowego obrazu.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Wpisy na blogu pokrewne

- [Niestandardowe obrazy lub formuł?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiowanie obrazów niestandardowych między Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Następne kroki

- [Dodaj Maszynę wirtualną do laboratorium](./devtest-lab-add-vm-with-artifacts.md)
