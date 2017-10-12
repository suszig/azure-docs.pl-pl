---
title: "Tworzenie konta użytkownika usługi Azure AD | Microsoft Docs"
description: "W tym artykule opisano sposób tworzenia poświadczeń konta użytkownika usługi Azure AD dla elementów Runbook w usłudze Azure Automation do uwierzytelniania na platformie Azure i klasycznej platformie Azure."
services: automation
documentationcenter: 
author: eslesar
manager: jwhit
editor: tysonn
keywords: "konto użytkownika usługi Azure Active Directory, azure service management, konto użytkownika usługi Azure AD"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 8f24e6e57c2eec5950c8c12d9f4383ce11cf5c11
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Uwierzytelnianie elementów Runbook za pomocą klasycznego wdrożenia platformy Azure i usługi Resource Manager
W tym artykule opisano kroki, które należy wykonać, aby skonfigurować konto użytkownika usługi Azure AD dla elementów Runbook usługi Azure Automation działających w odniesieniu do klasycznego modelu wdrażania platformy Azure lub zasobów usługi Azure Resource Manager.  To konto nadal jest obsługiwaną tożsamością uwierzytelniania dla elementów Runbook w ramach usługi Azure Resource Manager, jednak zalecane jest używanie konta Uruchom jako platformy Azure.       

## <a name="create-a-new-azure-active-directory-user"></a>Tworzenie nowego użytkownika usługi Azure Active Directory
1. Zaloguj się do klasycznej witryny Azure Portal jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz usługę **Active Directory**, a następnie wybierz nazwę katalogu organizacji.
3. Wybierz kartę **Użytkownicy**, a następnie w obszarze poleceń wybierz polecenie **Dodaj użytkownika**.
4. Na stronie **Poinformuj nas o tym użytkowniku** w obszarze **Typ użytkownika** wybierz opcję **Nowy użytkownik w organizacji**.
5. Wprowadź nazwę użytkownika.  
6. Wybierz nazwę katalogu, który jest skojarzony z subskrypcją platformy Azure na stronie usługi Active Directory.
7. Na stronie **profilu użytkownika** podaj imię i nazwisko, nazwę przyjazną dla użytkownika i rolę Użytkownik z listy **Role**.  Nie wybieraj opcji **Włącz uwierzytelnianie wieloskładnikowe**.
8. Zapamiętaj pełną nazwę użytkownika i hasło tymczasowe.
9. Wybierz opcje **Ustawienia > Administratorzy > Dodaj**.
10. Wpisz pełną nazwę utworzonego przed chwilą użytkownika.
11. Wybierz subskrypcję, którą ma zarządzać ten użytkownik.
12. Wyloguj się z platformy Azure, a następnie zaloguj się ponownie przy użyciu właśnie utworzonego konta. Zostanie wyświetlony monit informujący o konieczności zmiany hasła użytkownika.

## <a name="create-an-automation-account-in-azure-classic-portal"></a>Tworzenie konta usługi Automation w klasycznej witrynie Azure Portal
W tej części należy wykonać poniższe kroki, aby utworzyć nowe konto usługi Azure Automation w witrynie Azure Portal. Będzie ono używane z elementami Runbook zarządzającymi zasobami w klasycznym wdrożeniu platformy Azure.  

> [!NOTE]
> Kontami usługi Automation utworzonymi za pomocą klasycznej witryny Azure Portal można zarządzać zarówno za pomocą klasycznej witryny Azure Portal, jak i witryny Azure Portal oraz zestawu poleceń cmdlet. Po utworzeniu konta nie ma znaczenia, jak są tworzone zasoby w ramach konta ani jak się nimi zarządza. Jeśli planujesz dalsze używanie klasycznej witryny Azure Portal, użyj jej zamiast witryny Azure Portal do tworzenia kont usługi Automation.
> 
> 

1. Zaloguj się do klasycznej witryny Azure Portal jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz opcję **Automatyzacja**.
3. Na stronie **Automatyzacja** wybierz opcję **Utwórz konto automatyzacji**.
4. W polu **Utwórz konto automatyzacji** wpisz nazwę nowego konta w usłudze Automation i wybierz opcję **Region** z listy rozwijanej.  
5. Kliknij przycisk **OK**, aby zaakceptować ustawienia i utworzyć konto.
6. Po utworzeniu zostanie ono wyświetlone na stronie **Automatyzacja**.
7. Kliknięcie konta spowoduje wyświetlenie strony Pulpit nawigacyjny.  
8. Na stronie pulpitu nawigacyjnego usługi Automation wybierz opcję **Zasoby**.
9. Na stronie **Zasoby** wybierz opcję **Dodaj ustawienia** znajdującą się na dole strony.
10. Na stronie **Dodawanie ustawień** wybierz opcję **Dodaj poświadczenie**.
11. Na stronie **Definiowanie poświadczenia** wybierz opcję **Poświadczenie programu Windows PowerShell** z listy rozwijanej **Typ poświadczenia** i podaj nazwę dla poświadczenia.
12. Na następnej stronie **Definiowanie poświadczenia** wpisz nazwę użytkownika utworzonego wcześniej konta użytkownika usługi AD w polu **Nazwa użytkownika** oraz hasło w polach **Hasło** i **Potwierdź hasło**. Kliknij przycisk **OK**, aby zapisać zmiany.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Tworzenie konta usługi Automation w witrynie Azure Portal
W tej części należy wykonać poniższe kroki, aby utworzyć konto usługi Azure Automation w witrynie Azure Portal. Będzie ono używane z elementami Runbook zarządzającymi zasobami w trybie usługi Azure Resource Manager.  

1. Zaloguj się do portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz opcję **Konta automatyzacji**.
3. W bloku Konta automatyzacji kliknij przycisk **Dodaj**.<br><br>![Dodawanie konta usługi Automation](media/automation-create-aduser-account/add-automation-acct-properties.png)
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

