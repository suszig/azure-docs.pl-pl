<properties
    pageTitle="Tworzenie laboratorium w usłudze DevTest Labs | Microsoft Azure"
    description="Tworzenie nowego laboratorium w usłudze DevTest Labs na potrzeby maszyn wirtualnych"
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
    ms.date="06/01/2016"
    ms.author="tarcher"/>

# Tworzenie laboratorium w usłudze Azure DevTest Labs

## Wymagania wstępne

Aby utworzyć laboratorium, będą potrzebne:

- Subskrypcja platformy Azure. Aby zapoznać się z opcjami zakupu platformy Azure, zobacz [Jak kupić system Azure](https://azure.microsoft.com/pricing/purchase-options/) lub [Bezpłatna miesięczna wersja próbna](https://azure.microsoft.com/pricing/free-trial/). Aby utworzyć laboratorium, trzeba być właścicielem subskrypcji.
- Grupa zasobów platformy Azure dla laboratorium. Aby uzyskać więcej informacji, zobacz [Omówienie usługi Azure Resource Manager](../resource-group-overview.md) oraz [Kontrola dostępu oparta na rolach na platformie Azure](../active-directory/role-based-access-control-configure.md).

## Tworzenie laboratorium

1. Zaloguj się do [Portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Wybierz pozycję **Przeglądaj**.

1. Wybierz z listy pozycję **DevTest Labs**.

1. W bloku **DevTest Labs** wybierz pozycję **Dodaj**.

    ![Dodawanie laboratorium](./media/devtest-lab-create-lab/add-lab-button.png)

1. W bloku **Utwórz laboratorium DevTest Lab**:

    1. W polu **Nazwa laboratorium** wprowadź nazwę dla nowego laboratorium.
    1. W pozycji **Subskrypcja** wybierz subskrypcję do skojarzenia z laboratorium.
    1. W pozycji **Lokalizacja** wybierz lokalizację do przechowywania laboratorium.
    1. Wybierz pozycję **Utwórz**.

    ![Blok tworzenia laboratorium](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

## Następne kroki

Po utworzeniu laboratorium warto rozważyć poniższe kroki:

- [Secure access to a lab](devtest-lab-add-devtest-user.md) (Zabezpieczanie dostępu do laboratorium).

- [Set lab policies](devtest-lab-set-lab-policy.md) (Ustawianie zasad laboratorium).

- [Create a lab template](devtest-lab-create-template.md) (Tworzenie szablonu laboratorium).

- [Create custom artifacts for your VMs](devtest-lab-artifact-author.md) (Tworzenie niestandardowych artefaktów dla maszyn wirtualnych).

- [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md) (Dodawanie maszyny wirtualnej z artefaktami do laboratorium).


<!--HONumber=Jun16_HO2-->


