---
title: "Tworzenie laboratorium w usłudze Azure DevTest Labs | Microsoft Docs"
description: "Tworzenie laboratorium w usłudze Azure DevTest Labs na potrzeby maszyn wirtualnych"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 29c4c2a2818468a2fa8360eebd4b653bdcbbde19
ms.openlocfilehash: 0749d371466226343227c79db544a8e3dca0cca8


---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Tworzenie laboratorium w usłudze Azure DevTest Labs
## <a name="prerequisites"></a>Wymagania wstępne
Aby utworzyć laboratorium, potrzebne są:

* Subskrypcja platformy Azure. Aby zapoznać się z opcjami zakupu platformy Azure, zobacz [Jak kupić system Azure](https://azure.microsoft.com/pricing/purchase-options/) lub [Bezpłatna miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/). Aby utworzyć laboratorium, trzeba być właścicielem subskrypcji.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Procedura tworzenia laboratorium w usłudze Azure DevTest Labs
Następujące kroki ilustrują tworzenie laboratorium w usłudze Azure DevTest Labs przy użyciu witryny Azure Portal. 

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Wybierz pozycję **Więcej usług**, a następnie z listy wybierz pozycję **DevTest Labs**.
3. W bloku **DevTest Labs** wybierz pozycję **Dodaj**.
   
    ![Dodawanie laboratorium](./media/devtest-lab-create-lab/add-lab-button.png)
4. W bloku **Utwórz laboratorium DevTest Lab**:
   
   1. W polu **Nazwa laboratorium** wprowadź nazwę dla nowego laboratorium.
   2. W pozycji **Subskrypcja** wybierz subskrypcję do skojarzenia z laboratorium.
   3. W pozycji **Lokalizacja** wybierz lokalizację do przechowywania laboratorium.
   4. Wybierz pozycję **Automatyczne zamykanie**, aby włączyć opcję i określić parametry automatycznego zamykania wszystkich maszyn wirtualnych laboratorium. 
   5. Wybierz pozycję **Utwórz**.
      
      ![Blok tworzenia laboratorium](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Następne kroki
Po utworzeniu laboratorium warto rozważyć poniższe kroki:

* [Secure access to a lab](devtest-lab-add-devtest-user.md) (Zabezpieczanie dostępu do laboratorium).
* [Set lab policies](devtest-lab-set-lab-policy.md) (Ustawianie zasad laboratorium).
* [Create a lab template](devtest-lab-create-template.md) (Tworzenie szablonu laboratorium).
* [Create custom artifacts for your VMs](devtest-lab-artifact-author.md) (Tworzenie niestandardowych artefaktów dla maszyn wirtualnych).
* [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md) (Dodawanie maszyny wirtualnej z artefaktami do laboratorium).




<!--HONumber=Dec16_HO3-->


