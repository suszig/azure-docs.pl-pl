---
title: Konfigurowanie konta użytkownika usługi Azure AD | Microsoft Docs
description: W tym artykule opisano sposób konfigurowania poświadczeń dla konta użytkownika usługi Azure AD dla elementów Runbook w usłudze Azure Automation do uwierzytelniania w usługach ARM i ASM.
services: automation
documentationcenter: ''
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: konto użytkownika usługi Azure Active Directory, azure service management, konto użytkownika usługi Azure AD

ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte

---
# Uwierzytelnianie elementów Runbook w usłudze Azure Service Management i Resource Manager
W tym artykule opisano kroki, które należy wykonać, aby skonfigurować konto użytkownika usługi Azure AD dla elementów Runbook usługi Azure Automation działających w odniesieniu do zasobów usługi Azure Service Management (ASM) lub Azure Resource Manager (ARM).  To konto nadal jest obsługiwaną tożsamością uwierzytelniania dla elementów Runbook w ramach usługi ARM, jednak zalecane jest używanie nowego konta Uruchom jako platformy Azure.       

## Tworzenie nowego użytkownika usługi Azure Active Directory
1. Zaloguj się do klasycznego portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
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

## Tworzenie konta usługi Automation w klasycznym portalu Azure
W tej części należy wykonać poniższe kroki, aby utworzyć nowe konto usługi Azure Automation w portalu Azure, które będzie używane z elementami Runbook zarządzającymi zasobami w trybie ASM i ARM.  

> [!NOTE]
> Kontami usługi Automation utworzonymi za pomocą klasycznego portalu Azure można zarządzać zarówno za pomocą klasycznego portalu Azure, jak i portalu Azure oraz zestawu poleceń cmdlet. Po utworzeniu konta nie ma znaczenia, jak są tworzone zasoby w ramach konta ani jak się nimi zarządza. Jeśli planujesz dalsze używanie klasycznego portalu Azure, użyj go zamiast portalu Azure do tworzenia kont usługi Automation.
> 
> 

1. Zaloguj się do klasycznego portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
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

## Tworzenie konta usługi Automation w portalu Azure
W tej części należy wykonać poniższe kroki, aby utworzyć nowe konto usługi Azure Automation w portalu Azure, które będzie używane w odniesieniu do elementów Runbook zarządzających zasobami w trybie ARM.  

1. Zaloguj się do portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz opcję **Konta automatyzacji**.
3. W bloku Konta automatyzacji kliknij przycisk **Dodaj**.<br>![Dodawanie konta usługi Automation](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. W bloku **Dodaj konto automatyzacji** w polu **Nazwa** wpisz nazwę nowego konta usługi Automation.
5. Jeśli masz więcej niż jedną subskrypcję, jedną z nich określ dla nowego konta, podaj także nową lub istniejącą **grupę zasobów** i **lokalizację** centrum danych Azure.
6. Wybierz wartość **Nie** dla opcji **Utwórz konto Uruchom jako platformy Azure**, a następnie kliknij przycisk **Utwórz**.  
   
   > [!NOTE]
   > Jeśli wybrano opcję **Nie**, aby nie tworzyć konta Uruchom jako, w bloku **Dodaj konto automatyzacji** zostanie wyświetlony komunikat ostrzegawczy.  Gdy konto zostanie utworzone i przypisane do roli **Współautor** w subskrypcji, nie będzie miało odpowiedniej tożsamości uwierzytelniania w usłudze katalogowej subskrypcji i dlatego nie będzie miało dostępu do zasobów w ramach subskrypcji.  Pozwoli to zapobiec sytuacji, w której wszelkie elementy Runbook odwołujące się do tego konta miałyby możliwość uwierzytelniania i wykonywania zadań w odniesieniu do zasobów usługi ARM.
   > 
   > 
   
    ![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)
7. W trakcie tworzenia konta usługi Automation na platformie Azure postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

Po zakończeniu tworzenia poświadczenia konieczne będzie utworzenie zasobu poświadczenia, aby skojarzyć konto usługi Automation z utworzonym wcześniej kontem użytkownika usługi AD.  Należy pamiętać, że utworzone zostało tylko konto usługi Automation i nie jest ono skojarzone z tożsamością uwierzytelniania.  Wykonaj czynności opisane w artykule [Credential assets in Azure Automation](automation-credentials.md#creating-a-new-credential) (Zasoby poświadczeń w usłudze Azure Automation) i wprowadź wartość dla zmiennej **username** w formacie **domena\użytkownik**.

## Użycie poświadczeń w elemencie Runbook
Można pobrać poświadczenie w elemencie Runbook za pomocą działania [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx), a następnie można go użyć za pomocą działania [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) w celu nawiązania połączenia z subskrypcją platformy Azure. Jeśli poświadczenie odnosi się do administratora wielu subskrypcji Azure, należy też użyć polecenia [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx), aby wskazać odpowiednią subskrypcję. Zostało to pokazane w poniższym przykładzie kodu programu Windows PowerShell, który występuje na początku większości elementów Runbook usługi Azure Automation.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Te wiersze należy powtórzyć po każdym [punkcie kontrolnym](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) w elemencie Runbook. Jeśli element Runbook został zawieszony, a następnie jego działanie zostało wznowione w ramach innego procesu roboczego, konieczne jest ponowienie uwierzytelniania.

## Następne kroki
* Informacje o różnych typach elementów Runbook i krokach prowadzących do tworzenia własnych elementów Runbook można znaleźć w artykule [Azure Automation runbook types](automation-runbook-types.md) (Typy elementów Runbook usługi Azure Automation).

<!--HONumber=Sep16_HO3-->


