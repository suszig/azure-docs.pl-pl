<properties
    pageTitle="Tworzenie laboratorium w usłudze Azure DevTest Labs | Microsoft Azure"
    description="Tworzenie laboratorium w usłudze Azure DevTest Labs na potrzeby maszyn wirtualnych"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/12/2016"
    ms.author="tarcher"/>


# Tworzenie laboratorium w usłudze Azure DevTest Labs

## Wymagania wstępne

Aby utworzyć laboratorium, potrzebne są:

- Subskrypcja platformy Azure. Aby zapoznać się z opcjami zakupu platformy Azure, zobacz [Jak kupić system Azure](https://azure.microsoft.com/pricing/purchase-options/) lub [Bezpłatna miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/). Aby utworzyć laboratorium, trzeba być właścicielem subskrypcji.

## Procedura tworzenia laboratorium w usłudze Azure DevTest Labs

Następujące kroki ilustrują tworzenie laboratorium w usłudze Azure DevTest Labs przy użyciu witryny Azure Portal. 

1. Zaloguj się w witrynie [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Wybierz pozycję **Więcej usług**, a następnie z listy wybierz pozycję **DevTest Labs**.

1. W bloku **DevTest Labs** wybierz pozycję **Dodaj**.

    ![Dodawanie laboratorium](./media/devtest-lab-create-lab/add-lab-button.png)

1. W bloku **Utwórz laboratorium DevTest Lab**:

    1. W polu **Nazwa laboratorium** wprowadź nazwę dla nowego laboratorium.
    
    1. W pozycji **Subskrypcja** wybierz subskrypcję do skojarzenia z laboratorium.
    
    1. W pozycji **Lokalizacja** wybierz lokalizację do przechowywania laboratorium.
    
    1. Wybierz pozycję **Automatyczne zamykanie**, aby włączyć opcję i określić parametry automatycznego zamykania wszystkich maszyn wirtualnych laboratorium.
    
    1. Wybierz opcję **Typ magazynu**, aby wskazać typ dysku magazynu dla maszyn wirtualnych laboratorium. 
    
    1. Wybierz pozycję **Utwórz**.

    ![Blok tworzenia laboratorium](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Następne kroki

Po utworzeniu laboratorium warto rozważyć poniższe kroki:

- [Secure access to a lab](devtest-lab-add-devtest-user.md) (Zabezpieczanie dostępu do laboratorium).

- [Set lab policies](devtest-lab-set-lab-policy.md) (Ustawianie zasad laboratorium).

- [Create a lab template](devtest-lab-create-template.md) (Tworzenie szablonu laboratorium).

- [Create custom artifacts for your VMs](devtest-lab-artifact-author.md) (Tworzenie niestandardowych artefaktów dla maszyn wirtualnych).

- [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md) (Dodawanie maszyny wirtualnej z artefaktami do laboratorium).


<!--HONumber=Sep16_HO3-->


