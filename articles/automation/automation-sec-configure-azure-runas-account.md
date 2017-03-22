---
title: Konfigurowanie konta Uruchom jako platformy Azure | Microsoft Docs
description: "Samouczek przeprowadzający przez proces tworzenia, testowania i przykładowego użycia uwierzytelniania podmiotu zabezpieczeń w usłudze Automatyzacji Azure."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: "nazwa główna usługi, setspn, uwierzytelnianie azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/10/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 15cbf897f3f67b9d1bee0845b4d287fdabe63ba8
ms.lasthandoff: 03/11/2017


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Uwierzytelnianie elementów Runbook przy użyciu konta Uruchom jako platformy Azure
W tym temacie opisano, w jaki sposób można skonfigurować konto usługi Automation w witrynie Azure Portal za pomocą funkcji konta Uruchom jako, aby uwierzytelniać elementy runbook zarządzające zasobami w usłudze Azure Resource Manager lub Azure Service Management.

Po utworzeniu konta usługi Automation witryna Azure Portal automatycznie tworzy:

* Konto Uruchom jako, które tworzy nazwę główną usługi w usłudze Azure Active Directory i certyfikat oraz przypisuje rolę Współautor kontroli dostępu opartej na rolach (RBAC), która są używane do zarządzania zasobami usługi Resource Manager za pomocą elementów runbook.   
* Klasyczne konto Uruchom jako przez przekazanie certyfikatu zarządzania, które jest używane do zarządzania usługą Azure Service Management lub klasycznymi zasobami przy użyciu elementów runbook.  

Upraszcza to proces i pomaga szybko rozpocząć tworzenie i wdrażanie elementów Runbook dla różnych potrzeb automatyzacji.      

Używając konta Uruchom jako i klasycznego konta Uruchom jako, możesz:

* Zapewnić standardowy sposób uwierzytelniania w systemie Azure podczas zarządzania zasobami usługi Azure Resource Manager lub Azure Service Management z poziomu elementów Runbook w portalu Azure.  
* Zautomatyzować użycie globalnych elementów Runbook skonfigurowanych w alertach Azure.

> [!NOTE]
> [Funkcja integracji alertu](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) platformy Azure z globalnymi elementami Runbook automatyzacji wymaga konta usługi Automation ze skonfigurowanym kontem Uruchom jako lub klasycznym kontem Uruchom jako. Można wybrać konto usługi Automation, które ma już zdefiniowane konto Uruchom jako i klasyczne konto Uruchom jako, lub wybrać opcję utworzenia nowego.
>  

Pokazujemy, jak utworzyć konto usługi Automation z witryny Azure Portal, zaktualizować konto usługi Automation przy użyciu programu PowerShell, zarządzać konfiguracją konta oraz uwierzytelnić się w elementach runbook.

Jednak wcześniej musisz zrozumieć i wziąć pod uwagę kilka rzeczy.

1. Nie wpływa to na istniejące konta usługi Automation, które zostały już utworzone w modelu klasycznym lub modelu wdrażania usługi Resource Manager.  
2. Ma to zastosowanie tylko w przypadku kont usługi Automation utworzonych za pośrednictwem portalu Azure.  Próba utworzenia konta za pomocą portalu klasycznego nie spowoduje replikacji konfiguracji konta Uruchom jako.
3. Jeśli masz już elementy runbook i zasoby (tj. harmonogramy, zmienne itp.) utworzone wcześniej w celu zarządzania zasobami klasycznymi i chcesz, aby były one uwierzytelniane przy użyciu nowego klasycznego konta Uruchom jako, musisz utworzyć konto klasyczne Uruchom jako za pomocą funkcji zarządzania kontem Uruchom jako lub zaktualizować istniejące konto za pomocą poniższego skryptu programu PowerShell.  
4. Aby uwierzytelniać się przy użyciu nowego konta Uruchom jako i klasycznego konta Uruchom jako usługi Automation, musisz zmodyfikować istniejące elementy runbook za pomocą przykładowego kodu podanego w sekcji [Przykłady kodu uwierzytelniania](#authentication-code-examples).  
   
    >[!NOTE] 
    >Konto Uruchom jako służy do uwierzytelniania względem zasobów usługi Resource Manager za pomocą nazwy głównej usługi opartej na certyfikatach, natomiast klasyczne konto Uruchom jako służy do uwierzytelniania w odniesieniu do zasobów usługi Service Management za pomocą certyfikatu zarządzania.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Tworzenie nowego konta usługi Automation w witrynie Azure Portal
W tej części wykonasz poniższe kroki, aby utworzyć nowe konto usługi Azure Automation z portalu Azure.  Spowoduje to utworzenie zarówno konta Uruchom jako, jak i klasycznego konta Uruchom jako.  

> [!NOTE]
> Użytkownik wykonujący te czynności musi być członkiem roli Administratorzy usługi i współadministratorem subskrypcji, która udziela użytkownikowi dostępu do subskrypcji. Użytkownik musi być również dodany jako użytkownik do subskrypcji domyślnych usługi Active Directory; konto nie musi być przypisane do roli uprzywilejowanej. Użytkownicy, którzy nie są członkami usługi Active Directory subskrypcji, przed dodaniem do roli Współadministrator subskrypcji zostaną dodani do usługi Active Directory jako Gość i zobaczą ostrzeżenie „Nie masz uprawnień do utworzenia...” w bloku **Dodaj konto automatyzacji**. Użytkownicy dodani do roli współadministratora najpierw mogą zostać usunięci z subskrypcji usługi Active Directory i ponownie dodani, aby zostali pełnymi użytkownikami w usłudze Active Directory. Ta sytuacja może zostać zweryfikowana w okienku **Azure Active Directory** w witrynie Azure Portal. Po wybraniu pozycji **Użytkownicy i grupy** wybierz pozycję **Wszyscy użytkownicy**, a po wybraniu określonego użytkownika wybierz pozycję **Profil**.  Wartość atrybutu **Typ użytkownika** na liście profilów użytkowników nie powinna być równa **Gość**.  
> 

1. Zaloguj się do witryny Azure Portal przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.
2. Wybierz opcję **Konta automatyzacji**.
3. W bloku Konta automatyzacji kliknij przycisk **Dodaj**.<br>![Dodawanie konta usługi Automation](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Jeśli w bloku **Dodaj konto automatyzacji** widzisz poniższe ostrzeżenie, jest to spowodowane tym, że Twoje konto nie jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.<br>![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. W bloku **Dodaj konto automatyzacji** w polu **Nazwa** wpisz nazwę nowego konta usługi Automation.
5. Jeśli masz więcej niż jedną subskrypcję, określ jedną z nich dla nowego konta, podaj także nową lub istniejącą **grupę zasobów** i **lokalizację** centrum danych Azure.
6. Sprawdź, czy została wybrana wartość **Tak** dla opcji **Utwórz konto Azure Uruchom jako**, a następnie kliknij przycisk **Utwórz**.  
   
   > [!NOTE]
   > Jeśli wybrano opcję **Nie**, aby nie tworzyć konta Uruchom jako, w bloku **Dodaj konto automatyzacji** zostanie wyświetlony komunikat ostrzegawczy.  Gdy konto zostanie utworzone w portalu Azure, nie będzie ono miało odpowiedniej tożsamości uwierzytelniania w usłudze katalogowej subskrypcji klasycznej lub subskrypcji usługi Resource Manager i dlatego nie będzie miało dostępu do zasobów w ramach subskrypcji.  Pozwoli to zapobiec sytuacji, w której wszelkie elementy Runbook odwołujące się do tego konta miałyby możliwość uwierzytelniania i wykonywania zadań w odniesieniu do zasobów w tych modelach wdrożenia.
   > 
   > ![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   > Gdy nie została utworzona nazwa główna usługi, nie można przypisać roli Współautor.
   > 

7. W trakcie tworzenia konta usługi Automation na platformie Azure postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

### <a name="resources-included"></a>Zasoby dołączone
Po pomyślnym utworzeniu konta usługi Automation automatycznie zostanie utworzonych kilka zasobów.  Poniższa tabela zawiera podsumowanie zasobów dla konta Uruchom jako.<br>

| Zasób | Opis |
| --- | --- |
| AzureAutomationTutorial Runbook |Przykładowy graficzny element runbook, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako, i pobiera wszystkie zasoby usługi Resource Manager. |
| AzureAutomationTutorialScript Runbook |Przykładowy element Runbook programu PowerShell, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako, i pobiera wszystkie zasoby usługi Resource Manager. |
| AzureRunAsCertificate |Zasób Certyfikat utworzony automatycznie podczas tworzenia konta usługi Automation lub skorzystania z poniższego skryptu programu PowerShell dla istniejącego konta.  Umożliwia uwierzytelnianie za pomocą platformy Azure, aby można było zarządzać zasobami usługi Azure Resource Manager z poziomu elementów Runbook.  Ten certyfikat ma roczny okres obowiązywania. |
| AzureRunAsConnection |Zasób Połączenie utworzony automatycznie podczas tworzenia konta automatyzacji lub skorzystania z poniższego skryptu programu PowerShell dla istniejącego konta. |

Poniższa tabela zawiera podsumowanie zasobów dla klasycznego konta Uruchom jako.<br>

| Zasób | Opis |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Przykładowy graficzny element runbook, który pobiera wszystkie klasyczne maszyny wirtualne w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikat), a następnie generuje nazwę i stan maszyny wirtualnej. |
| AzureClassicAutomationTutorial Script Runbook |Przykładowy element runbook programu PowerShell, który pobiera wszystkie klasyczne maszyny wirtualne w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikat), a następnie generuje nazwę i stan maszyny wirtualnej. |
| AzureClassicRunAsCertificate |Automatycznie utworzony zasób Certyfikat, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów Runbook.  Ten certyfikat ma roczny okres obowiązywania. |
| AzureClassicRunAsConnection |Automatycznie utworzony zasób Połączenie, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów Runbook. |

## <a name="verify-run-as-authentication"></a>Sprawdzanie uwierzytelniania Uruchom jako
W następnej kolejności wykonamy mały test, aby potwierdzić, że możesz pomyślnie przeprowadzić uwierzytelnienie przy użyciu nowego konta Uruchom jako.     

1. Otwórz utworzone wcześniej konto usługi Automation w witrynie Azure Portal.  
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Wybierz element Runbook **AzureAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić.  Zostanie wyświetlony monit sprawdzający, czy chcesz uruchomić element Runbook.
4. Nastąpi utworzenie [zadania elementu Runbook](automation-runbook-execution.md), wyświetlenie bloku Zadanie oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.  
5. Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a następnie *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
6. Po zakończeniu zadania elementu Runbook powinniśmy zobaczyć stan **Ukończono**.<br> ![Test elementu Runbook zabezpieczeń](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Aby wyświetlić szczegółowe wyniki elementu Runbook, kliknij kafelek **Dane wyjściowe**.
8. W bloku **Dane wyjściowe** powinna być wyświetlana informacja, że element został pomyślnie uwierzytelniony, oraz zwrócona lista wszystkich zasobów dostępnych w tej grupie zasobów.
9. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.
10. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu Runbook **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Sprawdzanie klasycznego uwierzytelniania Uruchom jako
W następnej kolejności wykonamy mały test, aby potwierdzić, że możesz pomyślnie przeprowadzić uwierzytelnienie przy użyciu nowego klasycznego konta Uruchom jako.     

1. Otwórz utworzone wcześniej konto usługi Automation w witrynie Azure Portal.  
2. Kliknij kafelek **Elementy Runbook**, aby otworzyć listę elementów Runbook.
3. Wybierz element Runbook **AzureClassicAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić.  Zostanie wyświetlony monit sprawdzający, czy chcesz uruchomić element Runbook.
4. Nastąpi utworzenie [zadania elementu Runbook](automation-runbook-execution.md), wyświetlenie bloku Zadanie oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.  
5. Zadanie będzie miało początkowy stan *W kolejce*, wskazujący, że trwa oczekiwanie na udostępnienie procesu roboczego elementu Runbook w chmurze. Następnym stanem będzie *Uruchamianie*, gdy proces roboczy wywołuje zadanie, a następnie *Uruchomiono*, gdy element Runbook faktycznie zacznie działać.  
6. Po zakończeniu zadania elementu Runbook powinniśmy zobaczyć stan **Ukończono**.<br> ![Test elementu Runbook zabezpieczeń](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Aby wyświetlić szczegółowe wyniki elementu Runbook, kliknij kafelek **Dane wyjściowe**.
8. W bloku **Dane wyjściowe** powinna być wyświetlana informacja, że element został pomyślnie uwierzytelniony, oraz zwrócona lista wszystkich klasycznych maszyn wirtualnych w subskrypcji.
9. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.
10. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu runbook **AzureClassicAutomationTutorialScript**.

## <a name="managing-azure-run-as-account"></a>Zarządzanie kontem Uruchom jako platformy Azure
W okresie istnienia konta usługi Automation konieczne będzie odnawianie certyfikatu przed jego wygaśnięciem lub, jeśli uważasz, że bezpieczeństwo konta zostało naruszone, możesz usunąć konto Uruchom jako i ponownie je utworzyć.  W tej sekcji znajdują się kroki umożliwiające wykonanie tych operacji.  

### <a name="self-signed-certificate-renewal"></a>Odnawianie certyfikatu z podpisem własnym
Certyfikat z podpisem własnym utworzony dla konta Uruchom jako platformy Azure można odnawiać w dowolnym momencie do czasu jego wygaśnięcia, czyli jednego roku od daty utworzenia.  Po odnowieniu poprzedni ważny certyfikat zostanie zachowany, aby ta zmiana nie miała wpływu na żadne umieszczone w kolejce lub aktywnie działające elementy runbook, które uwierzytelniają się przy użyciu konta Uruchom jako.  Certyfikat będzie nadal istniał aż do wygaśnięcia.    

> [!NOTE]
> Jeśli konto Uruchom jako usługi Automation skonfigurowano do użycia certyfikatu wystawionego przez urząd certyfikacji przedsiębiorstwa, to w przypadku użycia tej opcji certyfikat ten zostanie zastąpiony certyfikatem z podpisem własnym.  

1. W witrynie Azure Portal otwórz konto usługi Automation.  
2. W bloku konta usługi Automation, w okienku właściwości konta wybierz pozycję **Konta Uruchom jako** w sekcji **Ustawienia konta**.<br><br> ![Okienko właściwości konta usługi Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)<br><br>
3. W bloku właściwości **Konta Uruchom jako** wybierz konto Uruchom jako albo klasyczne konto Uruchom jako, dla którego chcesz odnowić certyfikat, i w bloku właściwości wybranego konta kliknij przycisk **Odnów certyfikat**.<br><br> ![Odnawianie certyfikatu konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)<br><br> Zostanie wyświetlony monit sprawdzający, czy chcesz kontynuować.  
4. W trakcie odnawiania certyfikatu postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

### <a name="delete-run-as-account"></a>Usuwanie konta Uruchom jako
Poniżej opisano sposób usunięcia i ponownego utworzenia konta Uruchom jako platformy Azure lub klasycznego konta Uruchom jako.  Konto usługi Automation jest zachowywane podczas wykonywania tej akcji.  Po usunięciu konta Uruchom jako lub klasycznego konta Uruchom jako możesz je ponownie utworzyć w portalu.  

1. W witrynie Azure Portal otwórz konto usługi Automation.  
2. W bloku konta usługi Automation, w okienku właściwości konta wybierz pozycję **Konta Uruchom jako** w sekcji **Ustawienia konta**.
3. W bloku właściwości **Konta Uruchom jako** wybierz konto Uruchom jako albo klasyczne konto Uruchom jako, które chcesz usunąć, i w bloku właściwości wybranego konta kliknij przycisk **Usuń**.<br><br> ![Usuwanie konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)<br><br>  Zostanie wyświetlony monit sprawdzający, czy chcesz kontynuować.
4. W trakcie usuwania konta postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.  Po zakończeniu usuwania możesz ponownie utworzyć konto w bloku właściwości **Konta Uruchom jako**, wybierając opcję tworzenia **Konto Uruchom jako platformy Azure**.<br><br> ![Ponowne tworzenie konta Uruchom jako usługi Automation](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)<br> 

### <a name="misconfiguration"></a>Błąd konfiguracji
Jeśli jakiekolwiek elementy konfiguracji niezbędne do poprawnego działania konta Uruchom jako lub klasycznego konta Uruchom jako zostaną usunięte lub nie zostały utworzone prawidłowo podczas początkowej konfiguracji, na przykład:

* Zasób certyfikatu 
* Zasób połączenia 
* Konto Uruchom jako zostało usunięte z roli współautora
* Nazwa główna usługi lub aplikacji w usłudze Azure AD

usługa Automation wykryje te zmiany i powiadomi o nich przy użyciu stanu **Niekompletne** w bloku właściwości **Konta Uruchom jako** danego konta.<br><br> ![Komunikat o stanie Niekompletne dla konfiguracji konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)<br><br>Po wybraniu konta Uruchom jako w okienku właściwości konta zostanie wyświetlony następujący błąd:<br><br> ![Komunikat ostrzegawczy Niekompletne dla konfiguracji konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).<br>  
Jeśli konto Uruchom jako jest nieprawidłowo skonfigurowane, możesz szybko rozwiązać ten problem przez usunięcie i ponowne utworzenie konta Uruchom jako.   

## <a name="update-an-automation-account-using-powershell"></a>Aktualizacja konta automatyzacji przy użyciu programu PowerShell
W tym miejscu udostępniamy opcję aktualizacji istniejącego konta usługi Automation przy użyciu programu PowerShell, gdy:

1. Użytkownik utworzył konto usługi Automation, ale zrezygnował z tworzenia konta Uruchom jako 
2. Użytkownik już ma konto usługi Automation do zarządzania zasobami usługi Resource Manager i chce zaktualizować je, aby umożliwiało uwierzytelnianie elementu Runbook za pomocą konta Uruchom jako
4. Użytkownik już ma konto usługi Automation do zarządzania zasobami klasycznymi i chce zaktualizować je, aby używać klasycznego konta Uruchom jako, zamiast tworzenia nowego konta i migrowania do niego elementów Runbook i zasobów   
5. Użytkownik chce utworzyć konto Uruchom jako platformy Azure i klasyczne konto Uruchom jako przy użyciu certyfikatu wystawionego przez urząd certyfikacji przedsiębiorstwa

Ten skrypt ma następujące wymagania wstępne:

1. Ten skrypt jest obsługiwany tylko w systemach Windows 10 i Windows Server 2016 z zainstalowanymi modułami usługi Azure Resource Manager w wersji 2.01 lub nowszej.  Nie jest obsługiwany przez wcześniejsze wersje systemu Windows.  
2. Program Azure PowerShell 1.0 lub nowszy. Informacje o tej wersji i sposobie jej instalowania można znaleźć w temacie [Sposób instalowania i konfigurowania programu PowerShell Azure](/powershell/azureps-cmdlets-docs).
3. Utworzono konto automatyzacji.  To konto zostanie użyte jako wartości parametrów -AutomationAccountName i -ApplicationDisplayName w poniższym skrypcie.

Aby uzyskać wartości dla parametrów *SubscriptionID*, *ResourceGroup* i *AutomationAccountName*, które są wymagane w skryptach, w portalu Azure wybierz konto usługi Automation z bloku **Konto usługi Automation**, a następnie wybierz pozycję **Wszystkie ustawienia**.  W bloku **Wszystkie ustawienia** w obszarze **Ustawienia konta** wybierz pozycję **Właściwości**.  W bloku **Właściwości** możesz zauważyć poniższe wartości.<br><br> ![Konto usługi Automation — właściwości](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>Tworzenie skryptu programu PowerShell konta Uruchom jako
Ten skrypt programu PowerShell obsługuje następujące konfiguracje: 

* Tworzenie konta Uruchom jako platformy Azure przy użyciu certyfikatu z podpisem własnym
* Tworzenie konta Uruchom jako platformy Azure i klasycznego konta Uruchom jako platformy Azure przy użyciu certyfikatu z podpisem własnym
* Tworzenie konta Uruchom jako platformy Azure i klasycznego konta Uruchom jako platformy Azure przy użyciu certyfikatu przedsiębiorstwa
* Tworzenie konta Uruchom jako platformy Azure i klasycznego konta Uruchom jako platformy Azure przy użyciu certyfikatu z podpisem własnym w chmurze usługi Azure Government

W zależności od wybranej opcji konfiguracji zostaną utworzone następujące elementy:

* Aplikacja Azure AD, która będzie eksportowana przy użyciu certyfikatu z podpisem własnym lub klucza publicznego certyfikatu przedsiębiorstwa, utworzy konto dla nazwy głównej usługi dla tej aplikacji w usłudze Azure AD i przypisze to konto w bieżącej subskrypcji do roli Współautor (można ją zmienić na rolę Właściciel lub inną).  Więcej informacji można znaleźć w artykule [Kontrola dostępu oparta na rolach w automatyzacji Azure](automation-role-based-access-control.md).
* Zasób certyfikatu usługi Automation w ramach określonego konta automatyzacji o nazwie **AzureRunAsCertificate**, który ma klucz prywatny certyfikatu używany przez aplikację usługi Azure AD.
* Zasób połączenia usługi Automatyzacja w ramach określonego konta automatyzacji o nazwie **AzureRunAsConnection**, który zawiera identyfikator aplikacji, identyfikator dzierżawy, identyfikator subskrypcji i odcisk palca certyfikatu.    

W przypadku klasycznego konta Uruchom jako:

* Zasób certyfikatu usługi Automation w ramach określonego konta automatyzacji o nazwie **AzureClassicRunAsCertificate**, który ma klucz prywatny certyfikatu używany przez certyfikat zarządzania.  
* Zasób połączenia usługi Automation w ramach określonego konta automatyzacji o nazwie **AzureClassicRunAsConnection**, który zawiera nazwę subskrypcji, identyfikator subskrypcji i nazwę zasobu certyfikatu.

Jeśli wybierzesz opcję utworzenia klasycznego konta Uruchom jako, po wykonaniu skryptu musisz przekazać certyfikat publiczny (w formacie cer) do magazynu zarządzania dla subskrypcji, w ramach której zostało utworzone konto usługi Automation. Poniższa procedura przeprowadzi Cię przez proces wykonywania skryptu i przekazywania certyfikatu.    

1. Zapisz na komputerze poniższy skrypt.  W tym przykładzie zapisz plik pod nazwą **New-RunAsAccount.ps1**.  
   
         #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,
 
        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword, 
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose 
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid 

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use Key credentials and create AAD Application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id
   
        # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues 
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }
 
        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create Run As Account using Service Principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"
 
        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired 
        }

        # Create Service Principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId} 

        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create Run As Account using Service Principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#" 
 
             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired 
        }

        # Create the automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Na komputerze uruchom program **Windows PowerShell** z ekranu **Start** z podwyższonym poziomem uprawnień użytkownika.
3. Z poziomu powłoki wiersza polecenia programu PowerShell z podwyższonym poziomem uprawnień przejdź do folderu, który zawiera skrypt utworzony w kroku 1 i uruchom skrypt z odpowiednimi wartościami parametrów w zależności od wymaganej konfiguracji.  

    **Tworzenie konta Uruchom jako platformy Azure przy użyciu certyfikatu z podpisem własnym**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false` 

    **Tworzenie konta Uruchom jako platformy Azure i klasycznego konta Uruchom jako platformy Azure przy użyciu certyfikatu z podpisem własnym**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Tworzenie konta Uruchom jako platformy Azure i klasycznego konta Uruchom jako platformy Azure przy użyciu certyfikatu przedsiębiorstwa**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Tworzenie konta Uruchom jako platformy Azure i klasycznego konta Uruchom jako platformy Azure przy użyciu certyfikatu z podpisem własnym w chmurze usługi Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`
 
    > [!NOTE]
    > Po wykonaniu skryptu otrzymasz monit do uwierzytelnienia w usłudze Azure. Musisz zalogować się przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.
    > 
    > 

Po pomyślnym ukończeniu działania skryptu, jeśli zostało utworzone klasyczne konto Uruchom jako, wykonaj kroki [przekazywania certyfikatu interfejsu API zarządzania ](../azure-api-management-certs.md) do klasycznej witryny Azure Portal.  Jeśli zostało utworzone klasyczne konto Uruchom jako z certyfikatem publicznym z podpisem własnym (w formacie cer), kopię utworzonego certyfikatu możesz znaleźć w folderze plików tymczasowych na komputerze w ramach profilu użytkownika użytego do uruchomienia sesji programu PowerShell — *%USERPROFILE%\AppData\Local\Temp*.  W przeciwnym razie, jeśli klasyczne konto Uruchom jako zostało skonfigurowane do użycia certyfikatu wygenerowanego przez urząd certyfikacji przedsiębiorstwa (w formacie cer), trzeba będzie użyć tego certyfikatu.  Po przekazaniu tego certyfikatu zapoznaj się z [przykładowym kodem](#sample-code-to-authenticate-with-service-management-resources) do weryfikacji konfiguracji poświadczeń za pomocą zasobów usługi Service Management.  

Jeśli nie zostało utworzone klasyczne konto Uruchom jako, zapoznaj się z [przykładowym kodem](#sample-code-to-authenticate-with-resource-manager-resources) poniżej do uwierzytelniania się za pomocą zasobów usługi Resource Manager i weryfikacji konfiguracji poświadczeń.   

##  <a name="authentication-code-examples"></a>Przykłady kodu uwierzytelniania

Poniższe przykłady pokazują, jak uwierzytelniać elementy runbook względem zasobów usługi Resource Manager lub zasobów klasycznych przy użyciu konta Uruchom jako.

### <a name="authenticate-with-resource-manager-resources"></a>Uwierzytelnianie przy użyciu zasobów usługi Resource Manager
Możesz skorzystać z podanego poniżej przykładowego kodu, pobranego z przykładowego elementu Runbook **AzureAutomationTutorialScript**, do uwierzytelniania za pomocą konta Uruchom jako do zarządzania zasobami Menedżera zasobów przy użyciu Twoich elementów Runbook.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }


Skrypt zawiera dwa dodatkowe wiersze kodu odwołujące się do kontekstu subskrypcji, dzięki czemu można łatwo pracować z wieloma subskrypcjami. Zasób zmiennej o nazwie SubscriptionId zawiera identyfikator subskrypcji i po wykonaniu polecenia cmdlet Add-AzureRmAccount, [polecenie cmdlet Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) jest określane za pomocą parametru *-SubscriptionId*. Jeśli nazwa zmiennej jest zbyt ogólna, można ją skorygować w taki sposób, aby dołączyć prefiks lub zastosować inną konwencję nazewnictwa w celu ułatwienia identyfikacji do własnych celów. Alternatywnie można zastosować parametr -SubscriptionName zamiast -SubscriptionId z odpowiednim zasobem zmiennej.    

Zwróć uwagę, że polecenie cmdlet służące do uwierzytelniania w elemencie Runbook — **Add-AzureRmAccount** — używa zestawu parametrów *ServicePrincipalCertificate*.  Uwierzytelnia się ono za pomocą certyfikatu nazwy głównej usługi, a nie poświadczeń.  

### <a name="authenticate-with-service-management-resources"></a>Uwierzytelnianie przy użyciu zasobów usługi Service Management
Możesz skorzystać z podanego poniżej zaktualizowanego przykładowego kodu, pobranego z przykładowego elementu Runbook **AzureClassicAutomationTutorialScript**, do uwierzytelniania za pomocą klasycznego konta Uruchom jako do zarządzania klasycznymi zasobami przy użyciu Twoich elementów Runbook.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a>Następne kroki
* Więcej informacji na temat nazw głównych usługi można znaleźć w temacie [Obiekty aplikacji i obiekty nazwę głównej usługi](../active-directory/active-directory-application-objects.md).
* Więcej informacji na temat kontroli dostępu opartej na rolach w usłudze Automatyzacja platformy Azure można znaleźć w temacie [Kontrola dostępu oparta na rolach w usłudze Automatyzacja platformy Azure](automation-role-based-access-control.md).
* Aby uzyskać więcej informacji na temat certyfikatów i usług Azure, zobacz [Omówienie certyfikatów usług w chmurze platformy Azure](../cloud-services/cloud-services-certs-create.md)


