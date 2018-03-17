---
title: "Z tego samouczka, dowiesz się subskrybowanie oferty Azure stosu | Dokumentacja firmy Microsoft"
description: "Ten samouczek pokazuje, jak utworzyć nową subskrypcję do usług Azure stosu i przetestować oferty przez utworzenie testowej maszyny wirtualnej."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: d2adda5613b9a10bc3314045b9d68b0f3196f19a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-create-and-test-a-subscription"></a>Samouczek: tworzenie i testowanie subskrypcji
W tym samouczku przedstawiono sposób tworzenia subskrypcji, zawierające oferty, a następnie przetestujemy go. Dla testu, będą logowania do [portal użytkowników](https://portal.local.azurestack.external) administrator chmury subskrybować oferty, a następnie utwórz maszynę wirtualną.

> [!TIP]
> Bardziej zaawansowanych wersji ewaluacyjnej środowisko pracy, możesz [Utwórz subskrypcję dla określonego użytkownika](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) , a następnie zaloguj się jako użytkownik w portalu użytkowników. 

W tym samouczku przedstawiono sposób subskrybowania oferta utworzone w [samouczek poprzedniej](asdk-offer-services.md).

Co dowiesz się:

> [!div class="checklist"]
> * Subskrybowanie oferty 
> * Testowanie oferty

## <a name="subscribe-to-an-offer"></a>Subskrybowanie oferty
Aby subskrybować ofertę jako użytkownik, musisz zalogować się do portalu użytkowników stosu Azure do odnajdywania usługi, które są oferowane przez podmiot stosu Azure.

1. Zaloguj się do [portal użytkowników](https://portal.local.azurestack.external) i kliknij przycisk **uzyskania subskrypcji**.

   ![Uzyskaj subskrypcję](media/asdk-subscribe-services/get-subscription.png)

2. W polu **Nazwa wyświetlana** wpisz nazwę subskrypcji. Następnie kliknij przycisk **oferują** można wybrać jedną z dostępnych ofert w **wybierz ofertę** sekcji, a następnie kliknij przycisk **Utwórz**.

   ![Tworzenie oferty](media/asdk-subscribe-services/create-subscription.png)

   > [!TIP]
   > Teraz należy odświeżyć portal użytkowników, aby rozpocząć korzystanie z subskrypcji.

3. Aby wyświetlić utworzoną subskrypcję, kliknij przycisk **więcej usług**, kliknij przycisk **subskrypcje**, następnie kliknij przycisk nowej subskrypcji. Po zasubskrybowaniu ofertę odświeżyć portalu, aby zobaczyć, czy nowe usługi zostały zawarte w ramach nowej subskrypcji. W tym przykładzie **maszyn wirtualnych** został dodany.

   ![Widok subskrypcji](media/asdk-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Testowanie oferty
Po zalogowaniu do [portal użytkowników](https://portal.local.azurestack.external), możesz przetestować oferty obsługi maszyny wirtualnej przy użyciu nowych funkcji subskrypcji. 

> [!NOTE]
> Ten test wymaga obrazu maszyny Wirtualnej systemu Windows Server 2016 centrum danych, który został dodany do portalu stosu Azure marketplace w [samouczek poprzedniej](asdk-marketplace-item.md). 

1. Zaloguj się do [portal użytkowników](https://portal.local.azurestack.external).

2. W portalu użytkowników, kliknij przycisk **maszyn wirtualnych** > **Dodaj** > **systemu Windows Server Datacenter 2016**, a następnie kliknij przycisk **Utwórz** .

3. W **podstawy** wpisz **nazwa**, **nazwy użytkownika**, i **hasło**, wybierz **subskrypcji**, Utwórz **grupy zasobów** (lub wybrać istniejącą), a następnie kliknij przycisk **OK**.

4. W **wybierz rozmiar** kliknij **A1 standardowe**, a następnie kliknij przycisk **wybierz**.  

5. W bloku ustawienia zaakceptuj wartości domyślne, a następnie kliknij przycisk **OK**.

6. W **Podsumowanie** kliknij **OK** można utworzyć maszyny wirtualnej.  

7. Aby zapoznać się z nowej maszyny wirtualnej, kliknij przycisk **maszyn wirtualnych**, następnie wyszukaj nowej maszyny wirtualnej i kliknij jego nazwę.

    ![Wszystkie zasoby](media/asdk-subscribe-services/view-vm.png)

> [!NOTE]
> Wdrożenie maszyny wirtualnej potrwa kilka minut.


## <a name="next-steps"></a>Kolejne kroki

Przedstawiono w tym samouczku:

> [!div class="checklist"]
> * Subskrybowanie oferty 
> * Testowanie oferty

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak utworzyć Maszynę wirtualną przy użyciu szablonu.

> [!div class="nextstepaction"]
> [Utwórz maszynę wirtualną z szablonu](asdk-create-vm-template.md)