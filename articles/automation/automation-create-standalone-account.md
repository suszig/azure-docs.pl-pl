---
title: "Tworzenie autonomicznego konta usługi Azure Automation | Microsoft Docs"
description: "Samouczek przeprowadzający przez proces tworzenia, testowania i przykładowego użycia uwierzytelniania podmiotu zabezpieczeń w usłudze Automatyzacji Azure."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: e3c18c7886c8338efc6168464b63a9557909a769
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-standalone-azure-automation-account"></a>Tworzenie autonomicznego konta usługi Azure Automation
W tym temacie przedstawiono sposób tworzenia konta usługi Automation w witrynie Azure Portal, jeśli chcesz ocenić i dowiedzieć się o usłudze Azure Automation bez uwzględniania dodatkowych rozwiązań do zarządzania lub integracji z usługą OMS Log Analytics w celu zapewnienia zaawansowanego monitorowania zadań elementów Runbook.  W dowolnym momencie w przyszłości możesz dodać te rozwiązania do zarządzania lub zintegrować je z usługą Log Analytics.  Konto usługi Automation umożliwia uwierzytelnianie elementów Runbook zarządzających zasobami w usłudze Azure Resource Manager albo w klasycznym wdrożeniu platformy Azure.

Po utworzeniu konta usługi Automation witryna Azure Portal automatycznie tworzy:

* Konto Uruchom jako, które tworzy nową nazwę główną usługi w usłudze Azure Active Directory i certyfikat oraz przypisuje rolę Współautor kontroli dostępu opartej na rolach (RBAC), która jest używana do zarządzania zasobami usługi Resource Manager za pomocą elementów Runbook.   
* Klasyczne konto Uruchom jako przez przekazanie certyfikatu zarządzania, które jest używane do zarządzania klasycznymi zasobami przy użyciu elementów Runbook.  

Upraszcza to proces i pomaga szybko rozpocząć tworzenie i wdrażanie elementów Runbook dla różnych potrzeb automatyzacji.  

## <a name="permissions-required-to-create-automation-account"></a>Uprawnienia wymagane do utworzenia konta usługi Automation
Aby utworzyć lub zaktualizować konto usługi Automation, musisz mieć następujące określone uprawnienia i uprawnienia wymagane do wykonania zadań opisanych w tym temacie.   
 
* Przed utworzeniem konta usługi Automation konto użytkownika usługi AD musi zostać dodane do roli z uprawnieniami odpowiadającymi roli właściciela dla zasobów Microsoft.Automation w sposób opisany w artykule [Kontrola dostępu oparta na rolach w usłudze Azure Automation](automation-role-based-access-control.md).  
* Jeśli ustawienie Rejestracje aplikacji ma wartość **Tak**, użytkownicy inni niż administratorzy w dzierżawie usługi Azure AD mogą [rejestrować aplikacje usługi AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Jeśli wartością ustawienia rejestracji aplikacji jest **Nie**, użytkownik wykonujący tę akcję musi być administratorem globalnym w usłudze Azure AD. 

Jeśli nie jesteś członkiem wystąpienia usługi Active Directory subskrypcji, przed dodaniem do roli administratora globalnego/współadministratora subskrypcji, dodamy Cię do usługi Active Directory jako gościa. W tej usłudze zobaczysz ostrzeżenie „Nie masz uprawnień do utworzenia...” w bloku **Dodawanie konta usługi Automation**. Użytkownicy, którzy najpierw zostali dodani do roli administratora globalnego/współadministratora, mogą zostać usunięci z wystąpienia usługi Active Directory dla subskrypcji i ponownie dodani, aby zostać pełnymi użytkownikami w usłudze Active Directory. Aby zweryfikować tę sytuację, w okienku **Azure Active Directory** w witrynie Azure Portal wybierz kolejno pozycje **Użytkownicy i grupy** i **Wszyscy użytkownicy**, a po wybraniu określonego użytkownika wybierz pozycję **Profil**. Wartość atrybutu **Typ użytkownika** na liście profilów użytkowników nie powinna być równa **Gość**.

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Tworzenie nowego konta usługi Automation w witrynie Azure Portal
W tej części wykonaj poniższe kroki, aby utworzyć konto usługi Azure Automation w witrynie Azure Portal.    

1. Zaloguj się do witryny Azure Portal przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.
2. Kliknij przycisk **Nowy**.<br><br> ![Wybieranie opcji Nowy w witrynie Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Wyszukaj termin **Automation**, a następnie w wynikach wyszukiwania wybierz pozycję **Automatyzacja i kontrola***.<br><br> ![Wyszukiwanie i wybieranie usługi Automation w witrynie Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. W bloku Konta automatyzacji kliknij przycisk **Dodaj**.<br><br>![Dodawanie konta usługi Automation](media/automation-create-standalone-account/automation-create-automationacct-properties.png)


   > [!NOTE]
   > Jeśli w bloku **Dodawanie konta usługi Automation** znajduje się poniższe ostrzeżenie, jest to spowodowane tym, że Twoje konto nie jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.<br><br>![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. W bloku **Dodaj konto automatyzacji** w polu **Nazwa** wpisz nazwę nowego konta usługi Automation.
5. Jeśli masz więcej niż jedną subskrypcję, określ jedną z nich dla nowego konta, nowej lub istniejącej **grupy zasobów** i **lokalizacji** centrum danych Azure.
6. Sprawdź, czy została wybrana wartość **Tak** dla opcji **Utwórz konto Azure Uruchom jako**, a następnie kliknij przycisk **Utwórz**.  
   
   > [!NOTE]
   > Jeśli wybrano opcję **Nie**, aby nie tworzyć konta Uruchom jako, w bloku **Dodawanie konta usługi Automation** zostanie wyświetlony komunikat ostrzegawczy.  Gdy konto zostanie utworzone w witrynie Azure Portal, nie ma odpowiedniej tożsamości uwierzytelniania w usłudze katalogowej subskrypcji klasycznej lub subskrypcji usługi Resource Manager i dlatego nie ma dostępu do zasobów w ramach subskrypcji.  Zapobiegnie to sytuacji, w której wszelkie elementy Runbook odwołujące się do tego konta miałyby możliwość uwierzytelniania i wykonywania zadań w odniesieniu do zasobów w tych modelach wdrożenia.
   > 
   > ![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Jeśli nie została utworzona jednostka usługi, rola Współautor nie zostanie przypisana.
   > 

7. W trakcie tworzenia konta usługi Automation na platformie Azure postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

### <a name="resources-included"></a>Zasoby dołączone
Po pomyślnym utworzeniu konta usługi Automation automatycznie zostanie utworzonych kilka zasobów.  Poniższa tabela zawiera podsumowanie zasobów dla konta Uruchom jako.<br>

| Zasób | Opis |
| --- | --- |
| AzureAutomationTutorial Runbook |Przykładowy graficzny element runbook, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako, i pobiera wszystkie zasoby usługi Resource Manager. |
| AzureAutomationTutorialScript Runbook |Przykładowy element Runbook programu PowerShell, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako, i pobiera wszystkie zasoby usługi Resource Manager. |
| Element runbook AzureAutomationTutorialPython2 |Przykładowy element Runbook programu Python, który demonstruje sposób uwierzytelniania za pomocą konta Uruchom jako, a następnie wyświetla listę grup zasobów obecnych w określonej subskrypcji. |
| AzureRunAsCertificate |Zasób Certyfikat utworzony automatycznie podczas tworzenia konta automatyzacji lub skorzystania z poniższego skryptu programu PowerShell dla istniejącego konta.  Umożliwia uwierzytelnianie za pomocą platformy Azure, aby można było zarządzać zasobami usługi Azure Resource Manager z poziomu elementów Runbook.  Ten certyfikat ma roczny okres obowiązywania. |
| AzureRunAsConnection |Zasób Połączenie utworzony automatycznie podczas tworzenia konta automatyzacji lub skorzystania z poniższego skryptu programu PowerShell dla istniejącego konta. |

Poniższa tabela zawiera podsumowanie zasobów dla klasycznego konta Uruchom jako.<br>

| Zasób | Opis |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Przykładowy graficzny element Runbook, który pobiera wszystkie klasyczne maszyny wirtualne w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikat), a następnie generuje nazwę i stan maszyny wirtualnej. |
| AzureClassicAutomationTutorial Script Runbook |Przykładowy element Runbook programu PowerShell, który pobiera wszystkie klasyczne maszyny wirtualne w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikat), a następnie generuje nazwę i stan maszyny wirtualnej. |
| AzureClassicRunAsCertificate |Automatycznie utworzony zasób Certyfikat, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów Runbook.  Ten certyfikat ma roczny okres obowiązywania. |
| AzureClassicRunAsConnection |Automatycznie utworzony zasób Połączenie, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów Runbook. |


## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej na temat tworzenia elementów graficznych, zobacz [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md) (Tworzenie elementów graficznych w usłudze Azure Automation).
* Aby rozpocząć pracę z elementami Runbook programu PowerShell, zobacz artykuł [My first PowerShell runbook](automation-first-runbook-textual-powershell.md) (Mój pierwszy element Runbook programu PowerShell).
* Aby rozpocząć pracę z elementami runbook przepływu pracy programu PowerShell, zobacz [Mój pierwszy element runbook przepływu pracy programu PowerShell](automation-first-runbook-textual.md).
* Aby rozpocząć pracę z elementami Runbook programu Python2, zobacz [Mój pierwszy element Runbook programu Python2](automation-first-runbook-textual-python2.md).
