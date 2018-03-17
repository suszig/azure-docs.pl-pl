---
title: "Z tego samouczka, dowiesz się jak zaktualizować oferty Azure stosu i planów | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób wyświetlania i modyfikowania istniejących oferty Azure stosu i planów."
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
ms.openlocfilehash: 3b5d2ab5e924f578f5838d11b0d2058aacf67dec
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-update-offers-and-plans"></a>Samouczek: aktualizowanie oferty i planów
Jako operator stosu Azure możesz utworzyć plany, które zawierają żądane usług i odpowiednie limity przydziału dla użytkowników, aby subskrybować. Te *podstawowa planów* zawiera podstawowe usługi przeznaczonej dla użytkowników i może mieć tylko jeden plan bazowy na ofertę. Jeśli trzeba zmodyfikować ofertę, możesz użyć *planów dodatek* umożliwiają modyfikowanie planu rozszerzenie komputera, magazynu, albo sieci przydziały początkowo oferowany z planu podstawowego. 

Mimo że łączenie wszystko w ramach jednego planu mogą być optymalne w niektórych przypadkach, możesz mieć podstawy plan i oferują dodatkowe usługi przy użyciu planów dodatek. Na przykład można zdecydować, że oferty usług IaaS jako część planu podstawowego, ze wszystkimi usługami PaaS traktowane jako dodatek do planów. Plany mogą służyć do kontrolowania zużycie zasobów w środowisku ASDK ograniczone. Na przykład jeśli chcesz, aby użytkownicy w trosce o ich użycia zasobów, może mieć stosunkowo mały planu podstawowego (w zależności od usług wymaganych) i jak użytkownicy dostępu pojemności, będzie otrzymywać alerty już zajmowany alokacji zasobów na podstawie ich przypisanej planu. Z tego miejsca użytkownicy mogą wybierz plan dostępne dodatkowe dodatkowych zasobów. 

> [!NOTE]
> Gdy użytkownik dodaje plan dodatek do istniejącej subskrypcji oferty, dodatkowych zasobów może potrwać do godziny pojawią się. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie planu dodatku 
> * Subskrypcja do planu dodatku

## <a name="create-an-add-on-plan"></a>Tworzenie planu dodatku
**Dodatek planów** są tworzone przez zmodyfikowanie istniejącej oferty.

1. Zaloguj się do [portal Azure stosu](https://adminportal.local.azurestack.external) jako administrator chmury.
2. Wykonaj te same czynności, które wcześniej używane do [utworzyć plan bazowy](asdk-offer-services.md) do tworzenia nowego planu oferty usług, które nie zostały wcześniej oferowane. W tym przykładzie usługi Key Vault (Microsoft.KeyVault) będą uwzględniane w planie.
3. W portalu administratora, kliknij przycisk **oferuje** , a następnie wybierz ofertę zaktualizowania z planem dodatek.

   ![](media/asdk-update-offers/1.PNG)

4.  Przewiń w dół właściwości oferty i wybierz **planów dodatek**. Kliknij pozycję **Add** (Dodaj).
   
    ![](media/asdk-update-offers/2.PNG)

5. Wybierz plan do dodania. W tym przykładzie nosi nazwę planu **klucza magazynu planu**, a następnie kliknij przycisk **wybierz** dodać planu do oferty. Otrzymasz powiadomienie, że plan zostało pomyślnie dodane do oferty.
   
    ![](media/asdk-update-offers/3.PNG)

6. Zapoznaj się z listą dodatek, który na liście planów dołączone do oferty, aby sprawdzić, czy dodatek nowy plan.
   
    ![](media/asdk-update-offers/4.PNG)

## <a name="subscribe-to-the-add-on-plan"></a>Subskrypcja do planu dodatku
Musisz zalogować się do portalu użytkowników stosu Azure rozszerzyć istniejącą subskrypcję Azure stosu z planem dodatek.

Następujące kroki umożliwiają odnajdywania dodatkowych zasobów, które są oferowane przez podmiot stosu Azure i Dodaj do oferty, które zostały wcześniej subskrybujesz plan dodatek.

1. Zaloguj się do [portal użytkowników](https://portal.local.azurestack.external).
2. Aby znaleźć subskrypcji rozszerzenie z planem dodatek, kliknij przycisk **więcej usług**, kliknij przycisk **subskrypcje**, następnie wybierz subskrypcję.
   
    ![](media/asdk-update-offers/5.PNG)

3.  W obszarze Przegląd subskrypcji, kliknij przycisk **planu Dodaj**.
   
    ![](media/asdk-update-offers/6.PNG)

4. W bloku planu Dodaj wybierz plan dodatek można dodać do subskrypcji. W tym przykładzie **klucza magazynu planu** jest zaznaczone. Otrzymasz powiadomienie, że plan dodatku został pomyślnie nabyte i trzeba odświeżyć portal zaktualizowane subskrypcji dostęp do.
   
    ![](media/asdk-update-offers/7.PNG)

5. Na koniec przegląd planów dodatek skojarzone z subskrypcji, aby upewnić się, czy dodatek plan został pomyślnie dodany.
   
    ![](media/asdk-update-offers/8.PNG)


## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie planu dodatku 
> * Subskrypcja do planu dodatku

> [!div class="nextstepaction"]
> [Utwórz maszynę wirtualną z szablonu](asdk-create-vm-template.md)

