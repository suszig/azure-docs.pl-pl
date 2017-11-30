---
title: Tworzenie obrazu niestandardowego Azure DevTest Labs na podstawie pliku VHD | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć obraz niestandardowy w usłudze Azure DevTest Labs z pliku VHD za pomocą portalu Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: a421327ab8794315005327833b873dc5ebd57e9e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Tworzenie niestandardowego obrazu z pliku VHD

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Instrukcje krok po kroku

W poniższych krokach objaśniono przez proces tworzenia niestandardowego obrazu z pliku VHD za pomocą portalu Azure:

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Wybierz pozycję **Więcej usług**, a następnie z listy wybierz pozycję **DevTest Labs**.

1. Z listy labs wybierz żądany laboratorium.  

1. W bloku laboratorium, wybierz **konfiguracji**. 

1. W środowisku laboratoryjnym **konfiguracji** bloku, wybierz opcję **niestandardowych obrazów (VHD)**.

1. Na **niestandardowych obrazów** bloku, wybierz opcję **+ Dodaj**.

    ![Dodaj niestandardowy obraz](./media/devtest-lab-create-template/add-custom-image.png)

1. Wprowadź nazwę niestandardowego obrazu. Ta nazwa będzie wyświetlana na liście podstawowej obrazów, podczas tworzenia maszyny Wirtualnej.

1. Wprowadź opis niestandardowego obrazu. Ten opis jest wyświetlany na liście obrazów podstawowej podczas tworzenia maszyny Wirtualnej.

1. Wybierz **wirtualnego dysku twardego**.

1. Z **wirtualnego dysku twardego** bloku, wybierz żądany plik wirtualnego dysku twardego.

1. Wybierz **OK** zamknąć **wirtualnego dysku twardego** bloku.

1. Wybierz **Konfiguracja systemu operacyjnego**.

1. Na **Konfiguracja systemu operacyjnego** , a następnie wybierz opcję **Windows** lub **Linux**.

1. Jeśli **Windows** jest zaznaczone, określ za pomocą pola wyboru czy *Sysprep* zostało uruchomione na tym komputerze. 

1. Wybierz **OK** zamknąć **Konfiguracja systemu operacyjnego** bloku.

1. Wybierz **OK** można utworzyć niestandardowego obrazu.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Wpisy na blogu pokrewne

- [Niestandardowe obrazy lub formuł?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiowanie obrazów niestandardowych między Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Następne kroki

- [Dodaj Maszynę wirtualną do laboratorium](./devtest-lab-add-vm.md)
