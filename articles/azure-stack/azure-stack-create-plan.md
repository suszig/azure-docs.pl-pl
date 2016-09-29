<properties
    pageTitle="Tworzenie planu w usłudze Azure Stack | Microsoft Azure"
    description="Administrator usługi powinien utworzyć plan, który umożliwi subskrybentom aprowizację maszyn wirtualnych."
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


# Tworzenie planu w usłudze Azure Stack

[Plany](azure-stack-key-features.md#services-plans-offers-and-subscriptions) są grupami obejmującymi co najmniej jedną usługę. Jako dostawca możesz utworzyć plany do zaoferowania dzierżawcom. Z kolei dzierżawcy mogą subskrybować oferty, aby korzystać z planów i objętych nimi usług. W tym przykładzie zostanie utworzony plan obejmujący dostawców zasobów obliczeniowych, sieci i magazynu. Umożliwi to subskrybentom tego planu aprowizację maszyn wirtualnych.

1.  W przeglądarce internetowej przejdź do witryny https://portal.azurestack.local.

2.  [Zaloguj się](azure-stack-connect-azure-stack.md#log-in-as-a-service-administrator) w witrynie Azure Stack Portal jako administrator usługi i wprowadź swoje poświadczenia administratora usługi (to jest konto utworzone w kroku 5 sekcji [Uruchamianie skryptu programu PowerShell](azure-stack-run-powershell-script.md)), a następnie kliknij przycisk **Zaloguj się**.

    Administratorzy usługi mogą tworzyć oferty i plany oraz zarządzać użytkownikami.

3.  Aby utworzyć plan i ofertę, które dzierżawcy mogą subskrybować, kliknij pozycję **Nowy**.

    ![](media/azure-stack-create-plan/image1.png)

4.  W bloku tworzenia kliknij pozycję **Oferty i plany dzierżawy**, a następnie kliknij pozycję **Plan**.

    ![](media/azure-stack-create-plan/image2.png)

5.  Wypełnij pola **Nazwa wyświetlana** i **Nazwa zasobu**. Nazwa wyświetlana jest przyjazną nazwą planu. Nazwa zasobu jest widoczna tylko dla administratora. Jest to nazwa używana przez administratorów do pracy z planem jako zasobem usługi Azure Resource Manager.

    ![](media/azure-stack-create-plan/image3.png)

6.  Wybierz lub utwórz nową **grupę zasobów** jako kontener dla planu. Domyślnie wszystkie plany i oferty będą umieszczane w grupie zasobów o nazwie OffersAndPlans.

7.  Kliknij pozycję **Oferowane usługi**, użyj klawisza Shift, aby wybrać wszystkich trzech dostawców (**dostawcę obliczeń**, **dostawcę magazynu** i **dostawcę sieci**), a następnie kliknij pozycję **Wybierz**.

    ![](media/azure-stack-create-plan/image4.png)

8.  Kliknij pozycję **Microsoft.Compute**, a następnie kliknij pozycję **Wymaga konfiguracji**.

    ![](media/azure-stack-create-plan/image5.png)

9.  W bloku **Ustawianie limitów przydziału** zaakceptuj ustawienia domyślne, kliknij przycisk **OK**, a następnie ponownie kliknij przycisk **OK**.

    ![](media/azure-stack-create-plan/image6.png)

10. Kliknij pozycję **Microsoft.Network**, a następnie kliknij pozycję **Wymaga konfiguracji**.

    ![](media/azure-stack-create-plan/image7.png)

11. W bloku **Ustawianie limitów przydziału** zaznacz wszystkie pola wyboru, kliknij przycisk **OK**, a następnie ponownie kliknij przycisk **OK**.

    ![](media/azure-stack-create-plan/image8.png)

12. Kliknij pozycję **Microsoft.Storage**, kliknij pozycję **Wymaga konfiguracji**, a następnie w bloku **Ustawianie limitów przydziału** zaakceptuj ustawienia domyślne, kliknij przycisk **OK**, kliknij ponownie przycisk **OK**, po czym kliknij przycisk **Utwórz**, aby utworzyć plan.

    ![](media/azure-stack-create-plan/image9.png)

13. Plan może teraz zostać umieszczony w ofercie. Aby wyświetlić powiadomienia, kliknij ikonę dzwonka w prawym górnym rogu.

    ![](media/azure-stack-create-plan/image10.png)

## Następne kroki

[Tworzenie oferty](azure-stack-create-offer.md)



<!--HONumber=Sep16_HO3-->


