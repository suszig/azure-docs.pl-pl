---
title: "Tworzenie konta użytkownika usługi Azure AD | Microsoft Docs"
description: "W tym artykule opisano sposób tworzenia poświadczeń dla konta użytkownika programu Azure AD dla elementów runbook w automatyzacji Azure do uwierzytelniania na platformie Azure."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: "konto użytkownika usługi Azure Active Directory, azure service management, konto użytkownika usługi Azure AD"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: f0a9664898cd27529daf73d130dd25fd296a9b48
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Uwierzytelnianie elementów Runbook za pomocą klasycznego wdrożenia platformy Azure i usługi Resource Manager
W tym artykule opisano kroki, które należy wykonać, aby skonfigurować konto użytkownika usługi Azure AD dla elementów Runbook usługi Azure Automation działających w odniesieniu do klasycznego modelu wdrażania platformy Azure lub zasobów usługi Azure Resource Manager.  Gdy to nadal być tożsamością obsługiwanych uwierzytelniania dla usługi Azure Resource Manager, na podstawie elementy runbook, zalecaną metodą jest użycie konta Uruchom jako platformy Azure.       

## <a name="create-a-new-azure-active-directory-user"></a>Tworzenie nowego użytkownika usługi Azure Active Directory
1. Zaloguj się do portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz **usługi Azure Active Directory** > **użytkowników i grup** > **wszyscy użytkownicy** > **nowego użytkownika**.
3. Wprowadź szczegóły użytkownika, takie jak **nazwa** i **nazwy użytkownika**.  
4. Zapamiętaj pełną nazwę użytkownika i hasło tymczasowe.
5. Wybierz **roli katalogu**.
6. Przypisz rolę globalny lub Administrator ograniczone.
7. Wyloguj się z platformy Azure, a następnie zaloguj się ponownie przy użyciu właśnie utworzonego konta. Zostanie wyświetlony monit informujący o konieczności zmiany hasła użytkownika.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Tworzenie konta usługi Automation w witrynie Azure Portal
W tej części należy wykonać poniższe kroki, aby utworzyć konto usługi Azure Automation w witrynie Azure Portal. Będzie ono używane z elementami Runbook zarządzającymi zasobami w trybie usługi Azure Resource Manager.  

1. Zaloguj się do portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz opcję **Konta automatyzacji**.
3. Wybierz pozycję **Dodaj**.<br><br>![Dodawanie konta usługi Automation](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. W bloku **Dodaj konto automatyzacji** w polu **Nazwa** wpisz nazwę nowego konta usługi Automation.
5. Jeśli masz więcej niż jedną subskrypcję, jedną z nich określ dla nowego konta, podaj także nową lub istniejącą **grupę zasobów** i **lokalizację** centrum danych Azure.
6. Wybierz wartość **Tak** dla opcji **Utwórz konto Uruchom jako platformy Azure**, a następnie kliknij przycisk **Utwórz**.  
   
    > [!NOTE]
    > Jeśli wybrano opcję **Nie**, aby nie tworzyć konta Uruchom jako, w bloku **Dodaj konto automatyzacji** zostanie wyświetlony komunikat ostrzegawczy.  Gdy konto zostanie utworzone i przypisane do roli **Współautor** w subskrypcji, nie będzie ono miało odpowiedniej tożsamości uwierzytelniania w usłudze katalogowej subskrypcji i dlatego nie będzie miało dostępu do zasobów w ramach subskrypcji.  Pozwoli to zapobiec sytuacji, w której wszelkie elementy Runbook odwołujące się do tego konta miałyby możliwość uwierzytelniania i wykonywania zadań w odniesieniu do zasobów usługi Azure Resource Manager.
    > 
    >

    <br>![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. W trakcie tworzenia konta usługi Automation na platformie Azure postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

Po zakończeniu tworzenia poświadczenia musisz utworzyć zasób poświadczenia, aby skojarzyć konto usługi Automation z utworzonym wcześniej kontem użytkownika usługi AD.  Należy pamiętać, że utworzone zostało tylko konto usługi Automation i nie jest ono skojarzone z tożsamością uwierzytelniania.  Wykonaj czynności opisane w artykule [Credential assets in Azure Automation](automation-credentials.md#creating-a-new-credential-asset) (Zasoby poświadczeń w usłudze Azure Automation) i wprowadź wartość dla zmiennej **username** w formacie **domena\użytkownik**.

## <a name="use-the-credential-in-a-runbook"></a>Użycie poświadczeń w elemencie Runbook
Można pobrać poświadczenie w elemencie Runbook za pomocą działania [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx), a następnie można go użyć za pomocą działania [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) w celu nawiązania połączenia z subskrypcją platformy Azure. Jeśli poświadczenie odnosi się do administratora wielu subskrypcji Azure, należy też użyć polecenia [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx), aby wskazać odpowiednią subskrypcję. Zostało to pokazane w poniższym przykładzie kodu programu Windows PowerShell, który występuje na początku większości elementów Runbook usługi Azure Automation.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Te wiersze należy powtórzyć po każdym [punkcie kontrolnym](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) w elemencie Runbook. Jeśli element Runbook został zawieszony, a następnie jego działanie zostało wznowione w ramach innego procesu roboczego, konieczne jest ponowienie uwierzytelniania.

## <a name="next-steps"></a>Następne kroki
* Informacje o różnych typach elementów Runbook i krokach prowadzących do tworzenia własnych elementów Runbook można znaleźć w artykule [Azure Automation runbook types](automation-runbook-types.md) (Typy elementów Runbook usługi Azure Automation).

