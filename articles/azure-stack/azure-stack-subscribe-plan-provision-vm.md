<properties
    pageTitle="Subskrybowanie oferty, a następnie inicjowanie obsługi administracyjnej maszyny wirtualnej w usłudze Azure Stack (dzierżawca) | Microsoft Azure"
    description="Dowiedz się, jak subskrybować ofertę jako dzierżawca, a następnie zainicjować obsługę administracyjną maszyny wirtualnej w usłudze Azure Stack."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="erikje"/>


# Subskrybowanie oferty

Po [utworzeniu oferty](azure-stack-create-offer.md) należy sprawdzić, czy dzierżawcy mogą utworzyć subskrypcję.

1.  Na komputerze z usługą Azure Stack POC zaloguj się do portalu `https://portal.azurestack.local` jako [dzierżawca](azure-stack-connect-azure-stack.md#log-in-as-a-tenant) i kliknij przycisk **Uzyskaj subskrypcję**.

    ![](media/azure-stack-subscribe-plan-provision-vm/image1.png)

2.  W polu **Nazwa wyświetlana** wpisz nazwę subskrypcji.

    ![](media/azure-stack-subscribe-plan-provision-vm/image2.png)

3.  Kliknij pozycję **Oferta** i sprawdź, czy utworzona oferta znajduje się w bloku **Wybierz ofertę**. Kliknij tę ofertę, kliknij pozycję **Wybierz**, a następnie kliknij przycisk **OK**.  

    ![](media/azure-stack-subscribe-plan-provision-vm/image3.png)

4.  Aby wyświetlić utworzoną subskrypcję, kliknij przycisk **Przeglądaj**, a następnie kliknij pozycję **Subskrypcje**.  

    ![](media/azure-stack-subscribe-plan-provision-vm/image4.png)

Po subskrybowaniu oferty poczekaj na zmianę stanu subskrypcji na InSync. Następnie odśwież portal, aby zobaczyć, które usługi są częścią nowej subskrypcji.




## Następne kroki

[Konta magazynu](azure-stack-provision-storage-account.md)



<!--HONumber=Sep16_HO3-->


