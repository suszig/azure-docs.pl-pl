---
title: Konfigurowanie konta Uruchom jako platformy Azure | Microsoft Docs
description: "Ten samouczek przeprowadza Cię przez proces tworzenia, testowania i przykładowego użycia uwierzytelniania podmiotu zabezpieczeń w usłudze Azure Automation."
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
ms.date: 04/06/2017
ms.author: magoedte
ROBOTS: NOINDEX
redirect_url: /azure/automation/
redirect_document_id: TRUE
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 90570886b3a1ae0c48683691cb438b5a61195d76
ms.contentlocale: pl-pl
ms.lasthandoff: 04/27/2017


---

# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a>Uwierzytelnianie elementów runbook przy użyciu konta Uruchom jako platformy Azure
W tym artykule przedstawiono sposób konfigurowania konta usługi Azure Automation w witrynie Azure Portal. W tym celu można użyć funkcji konta Uruchom jako do uwierzytelniania elementów runbook zarządzających zasobami w usłudze Azure Resource Manager lub Azure Service Management.

Po utworzeniu konta usługi Automation w witrynie Azure Portal automatycznie utworzysz dwa konta:

* Konto Uruchom jako. To konto służy do tworzenia nazwy głównej usługi w usłudze Azure Active Directory (Azure AD) oraz certyfikatu. Umożliwia ono również przypisywanie kontroli dostępu opartej na rolach (RBAC) typu Współautor, która zarządza zasobami usługi Resource Manager przy użyciu elementów runbook.
* Klasyczne konto Uruchom jako. To konto służy do przekazywania certyfikatu zarządzania, który jest używany do zarządzania usługą Service Management lub klasycznymi zasobami przy użyciu elementów runbook.

Utworzenie konta usługi Automation upraszcza proces i pomaga szybko rozpocząć tworzenie i wdrażanie elementów runbook dla różnych potrzeb automatyzacji.

Używając konta Uruchom jako i klasycznego konta Uruchom jako, możesz:

* Zapewnić standardowy sposób uwierzytelniania na platformie Azure podczas zarządzania zasobami usługi Resource Manager lub Service Management z poziomu elementów runbook w witrynie Azure Portal.
* Zautomatyzować użycie globalnych elementów runbook skonfigurowanych w alertach platformy Azure.

> [!NOTE]
> [Funkcja integracji alertu platformy Azure](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) z globalnymi elementami runbook usługi Automation wymaga konta usługi Automation ze skonfigurowanym kontem Uruchom jako i klasycznym kontem Uruchom jako. Można wybrać konto usługi Automation, które ma już zdefiniowane konto Uruchom jako i klasyczne konto Uruchom jako, lub wybrać opcję utworzenia nowego konta usługi Automation.
>  

W tym artykule pokazano, jak utworzyć konto usługi Automation z witryny Azure Portal, zaktualizować konto usługi Automation przy użyciu programu Azure PowerShell, zarządzać konfiguracją konta oraz uwierzytelnić się w elementach runbook.

Przed rozpoczęciem tworzenia konta usługi Automation warto poznać i rozważyć następujące kwestie:

* Utworzenie konta usługi Automation nie wpływa na istniejące konta usługi Automation, które zostały już utworzone w modelu klasycznym lub modelu wdrażania usługi Resource Manager.
* Proces działa tylko w przypadku kont usługi Automation tworzonych w witrynie Azure Portal. Próba utworzenia konta za pomocą klasycznej witryny Azure Portal nie powoduje replikacji konfiguracji konta Uruchom jako.
* Jeśli masz już elementy runbook i zasoby (na przykład harmonogramy lub zmienne) przeznaczone do zarządzania zasobami klasycznymi i chcesz uwierzytelniać elementy runbook przy użyciu nowego klasycznego konta Uruchom jako, wykonaj jedną z następujących czynności:

  * Aby utworzyć klasyczne konto Uruchom jako, postępuj zgodnie z instrukcjami w sekcji „Zarządzanie kontem Uruchom jako”. 
  * Aby zaktualizować istniejące konto, użyj skryptu programu PowerShell z sekcji „Aktualizowanie konta usługi Automation przy użyciu programu PowerShell”.
* Aby uwierzytelniać się przy użyciu nowego konta Uruchom jako i klasycznego konta Uruchom jako usługi Automation, musisz zmodyfikować istniejące elementy runbook za pomocą przykładowego kodu podanego w sekcji [Przykłady kodu uwierzytelniania](#authentication-code-examples).

    >[!NOTE]
    >Konto Uruchom jako jest przeznaczone do uwierzytelniania względem zasobów usługi Resource Manager za pomocą nazwy głównej usługi opartej na certyfikacie. Klasyczne konto Uruchom jako jest przeznaczone do uwierzytelniania względem zasobów usługi Service Management przy użyciu certyfikatu zarządzania.

## <a name="create-an-automation-account-from-the-azure-portal"></a>Tworzenie konta usługi Automation w witrynie Azure Portal
W tej sekcji utworzysz konto usługi Azure Automation w witrynie Azure Portal, co z kolei spowoduje utworzenie konta Uruchom jako i klasycznego konta Uruchom jako.

>[!NOTE]
>Aby utworzyć konto usługi Automation, musisz być członkiem roli Administratorzy usługi lub współadministratorem subskrypcji, która udziela praw dostępu do subskrypcji. Musisz być również użytkownikiem dodanym do domyślnego wystąpienia usługi Active Directory tej subskrypcji. Konto nie musi mieć przypisanej roli uprzywilejowanej.
>
>Jeśli nie jesteś członkiem wystąpienia usługi Active Directory subskrypcji przed dodaniem do roli współadministratora subskrypcji, dodamy Cię do usługi Active Directory jako gościa. W tym wystąpieniu zobaczysz ostrzeżenie „Nie masz uprawnień do utworzenia...” w bloku **Dodawanie konta usługi Automation**.
>
>Użytkownicy dodani do roli współadministratora najpierw mogą zostać usunięci z wystąpienia usługi Active Directory dla subskrypcji i ponownie dodani, aby zostali pełnymi użytkownikami w usłudze Active Directory. Aby zweryfikować tę sytuację w okienku **Azure Active Directory** w witrynie Azure Portal wybierz kolejno pozycje **Użytkownicy i grupy** i **Wszyscy użytkownicy**, a po wybraniu określonego użytkownika wybierz pozycję **Profil**. Wartość atrybutu **Typ użytkownika** na liście profilów użytkowników nie powinna być równa **Gość**.
>

1. Zaloguj się do witryny Azure Portal przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.

2. Wybierz opcję **Konta automatyzacji**.

3. W bloku **Konta usługi Automation** kliknij pozycję **Dodaj**.
Zostanie otwarty blok **Dodawanie konta usługi Automation**.

 ![Blok „Dodawanie konta usługi Automation”](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > Jeśli konto nie jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji, w bloku **Dodawanie konta usługi Automation** zobaczysz poniższe ostrzeżenie:
   >
   >![Ostrzeżenie podczas dodawania konta usługi Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. W bloku **Dodawanie konta usługi Automation** w polu **Nazwa** wpisz nazwę nowego konta usługi Automation.

5. Jeśli masz więcej niż jedną subskrypcję, wykonaj następujące czynności:

    a. W obszarze **Subskrypcja** określ subskrypcję dla nowego konta.

    b. W obszarze **Grupa zasobów** kliknij pozycję **Utwórz nową** lub **Użyj istniejącej**.

    c. W obszarze **Lokalizacja** określ centrum danych platformy Azure.

6. W obszarze **Tworzenie konta Uruchom jako platformy Azure** wybierz pozycję **Tak**, a następnie kliknij pozycję **Utwórz**.

   > [!NOTE]
   > Jeśli zrezygnujesz z tworzenia konta Uruchom jako, wybierając pozycję **Nie**, w bloku **Dodawanie konta usługi Automation** zostanie wyświetlony komunikat ostrzegawczy. Mimo że konto zostanie utworzone w witrynie Azure Portal, nie będzie ono miało odpowiedniej tożsamości uwierzytelniania w usłudze katalogowej subskrypcji klasycznej lub subskrypcji usługi Resource Manager. Dlatego konto nie będzie miało dostępu do zasobów w ramach subskrypcji. Ten scenariusz zapobiega sytuacji, w której wszelkie elementy runbook odwołujące się do tego konta miałyby możliwość uwierzytelniania i wykonywania zadań w odniesieniu do zasobów w tych modelach wdrożenia.
   >
   > ![Komunikat ostrzegawczy w bloku „Dodawanie konta usługi Automation”](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > Ponadto — ponieważ nie została utworzona nazwa główna usługi — nie można przypisać roli Współautor.
   >

7. W trakcie tworzenia konta usługi Automation na platformie Azure postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

### <a name="resources"></a>Zasoby
Po pomyślnym utworzeniu konta usługi Automation automatycznie zostanie utworzonych kilka zasobów. Zasoby są podsumowywane w dwóch następujących tabelach:

#### <a name="run-as-account-resources"></a>Zasoby konta Uruchom jako

| Zasób | Opis |
| --- | --- |
| AzureAutomationTutorial Runbook | Przykładowy graficzny element runbook, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako i pobiera wszystkie zasoby usługi Resource Manager. |
| AzureAutomationTutorialScript Runbook | Przykładowy element runbook programu PowerShell, który demonstruje sposób uwierzytelniania przy użyciu konta Uruchom jako i pobiera wszystkie zasoby usługi Resource Manager. |
| AzureRunAsCertificate | Zasób certyfikatu tworzony automatycznie podczas tworzenia konta usługi Automation lub używania poniższego skryptu programu PowerShell dla istniejącego konta. Certyfikat umożliwia uwierzytelnianie za pomocą platformy Azure, aby można było zarządzać zasobami usługi Azure Resource Manager z poziomu elementów runbook. Certyfikat ma roczny okres obowiązywania. |
| AzureRunAsConnection | Zasób połączenia tworzony automatycznie podczas tworzenia konta usługi Automation lub używania skryptu programu PowerShell dla istniejącego konta. |

#### <a name="classic-run-as-account-resources"></a>Zasoby klasycznego konta Uruchom jako

| Zasób | Opis |
| --- | --- |
| AzureClassicAutomationTutorial Runbook | Przykładowy graficzny element runbook, który pobiera wszystkie maszyny wirtualne tworzone za pomocą klasycznego modelu wdrożenia w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikatu), a następnie zapisuje nazwę i stan maszyny wirtualnej. |
| AzureClassicAutomationTutorial Script Runbook | Przykładowy element runbook programu PowerShell, który pobiera wszystkie klasyczne maszyny wirtualne w subskrypcji przy użyciu klasycznego konta Uruchom jako (certyfikatu), a następnie zapisuje nazwę i stan maszyny wirtualnej. |
| AzureClassicRunAsCertificate | Automatycznie utworzony zasób certyfikatu, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów runbook. Certyfikat ma roczny okres obowiązywania. |
| AzureClassicRunAsConnection | Automatycznie utworzony zasób połączenia, który służy do uwierzytelniania za pomocą platformy Azure, aby można było zarządzać klasycznymi zasobami platformy Azure z poziomu elementów runbook. |

## <a name="verify-run-as-authentication"></a>Sprawdzanie uwierzytelniania Uruchom jako
Wykonaj mały test, aby potwierdzić, że możesz pomyślnie przeprowadzić uwierzytelnienie przy użyciu nowego konta Uruchom jako.

1. Otwórz utworzone wcześniej konto usługi Automation w witrynie Azure Portal.

2. Kliknij kafelek **Elementy runbook**, aby otworzyć listę elementów runbook.

3. Wybierz element runbook **AzureAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić. Wystąpią następujące zdarzenia:
 * Nastąpi utworzenie [zadania elementu runbook](automation-runbook-execution.md), wyświetlenie bloku **Zadanie** oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.
 * Zadanie będzie miało początkowy stan **W kolejce** oznaczający, że trwa oczekiwanie na udostępnienie procesu roboczego elementu runbook w chmurze.
 * Stan zmieni się na **Uruchamianie**, gdy proces roboczy pobierze zadanie.
 * Stan zmieni się na **Uruchomiono**, gdy element runbook zacznie działać.
 * Gdy zadanie elementu runbook zakończy działanie, powinniśmy zobaczyć stan **Ukończono**.

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. Aby wyświetlić szczegółowe wyniki elementu runbook, kliknij kafelek **Dane wyjściowe**.  
Zostanie wyświetlony blok **Dane wyjściowe** z informacją o tym, że element runbook został pomyślnie uwierzytelniony, oraz zostanie zwrócona lista wszystkich zasobów dostępnych w tej grupie zasobów.

5. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.

6. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu runbook **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Sprawdzanie klasycznego uwierzytelniania Uruchom jako
Wykonaj podobny mały test, aby potwierdzić, że możesz pomyślnie przeprowadzić uwierzytelnienie przy użyciu nowego klasycznego konta Uruchom jako.

1. Otwórz utworzone wcześniej konto usługi Automation w witrynie Azure Portal.

2. Kliknij kafelek **Elementy runbook**, aby otworzyć listę elementów runbook.

3. Wybierz element runbook **AzureClassicAutomationTutorialScript**, a następnie kliknij pozycję **Start**, aby go uruchomić. Wystąpią następujące zdarzenia:

 * Nastąpi utworzenie [zadania elementu runbook](automation-runbook-execution.md), wyświetlenie bloku **Zadanie** oraz wyświetlenie stanu zadania w kafelku **Podsumowanie zadania**.
 * Zadanie będzie miało początkowy stan **W kolejce** oznaczający, że trwa oczekiwanie na udostępnienie procesu roboczego elementu runbook w chmurze.
 * Stan zmieni się na **Uruchamianie**, gdy proces roboczy pobierze zadanie.
 * Stan zmieni się na **Uruchomiono**, gdy element runbook zacznie działać.
 * Gdy zadanie elementu runbook zakończy działanie, powinniśmy zobaczyć stan **Ukończono**.

    ![Test elementu Runbook zabezpieczeń](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. Aby wyświetlić szczegółowe wyniki elementu runbook, kliknij kafelek **Dane wyjściowe**.  
Zostanie wyświetlony blok **Dane wyjściowe** z informacją o tym, że element został pomyślnie uwierzytelniony, oraz zostanie zwrócona lista wszystkich klasycznych maszyn wirtualnych w subskrypcji.

5. Zamknij blok **Dane wyjściowe**, aby wrócić do bloku **Podsumowanie zadania**.

6. Zamknij blok **Podsumowanie zadania** i odpowiedni blok elementu runbook **AzureAutomationTutorialScript**.

## <a name="managing-your-run-as-account"></a>Zarządzanie kontem Uruchom jako
W pewnym momencie przed wygaśnięciem ważności konta usługi Automation należy odnowić certyfikat. Jeśli uważasz, że bezpieczeństwo konta Uruchom jako zostało naruszone, możesz je usunąć i utworzyć ponownie. W tej sekcji opisano kroki umożliwiające wykonanie tych operacji.

### <a name="self-signed-certificate-renewal"></a>Odnawianie certyfikatu z podpisem własnym
Certyfikat z podpisem własnym utworzony dla konta Uruchom jako wygasa rok od daty utworzenia. Można go odnowić w dowolnym momencie przed wygaśnięciem jego ważności. Po odnowieniu bieżący ważny certyfikat zostanie zachowany w celu zapewnienia, że ta zmiana nie wpłynie negatywnie na żadne umieszczone w kolejce lub aktywnie działające elementy runbook, które uwierzytelniają się przy użyciu konta Uruchom jako. Certyfikat pozostanie ważny aż do daty wygaśnięcia.

> [!NOTE]
> Jeśli konto Uruchom jako usługi Automation skonfigurowano do użycia certyfikatu wystawionego przez urząd certyfikacji przedsiębiorstwa, w przypadku użycia tej opcji certyfikat przedsiębiorstwa zostanie zastąpiony certyfikatem z podpisem własnym.

Aby odnowić certyfikat, wykonaj następujące czynności:

1. W witrynie Azure Portal otwórz konto usługi Automation.

2. W bloku **Konto usługi Automation** w okienku **Właściwości konta** w obszarze **Ustawienia konta** wybierz pozycję **Konta Uruchom jako**.

    ![Okienko właściwości konta usługi Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. W bloku właściwości **Konta Uruchom jako** wybierz konto Uruchom jako albo klasyczne konto Uruchom jako, dla którego chcesz odnowić certyfikat.

4. W bloku **Właściwości** wybranego konta kliknij pozycję **Odnów certyfikat**.

    ![Odnawianie certyfikatu konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. W trakcie odnawiania certyfikatu postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

### <a name="delete-a-run-as-or-classic-run-as-account"></a>Usuwanie konta Uruchom jako lub klasycznego konta Uruchom jako
W tej sekcji opisano sposób usuwania i ponownego tworzenia konta Uruchom jako lub klasycznego konta Uruchom jako. Konto usługi Automation jest zachowywane podczas wykonywania tej akcji. Po usunięciu konta Uruchom jako lub klasycznego konta Uruchom jako możesz je ponownie utworzyć w witrynie Azure Portal.

1. W witrynie Azure Portal otwórz konto usługi Automation.

2. W bloku **Konto usługi Automation** w okienku właściwości konta wybierz pozycję **Konta Uruchom jako**.

3. W bloku właściwości **Konta Uruchom jako** wybierz konto Uruchom jako albo klasyczne konto Uruchom jako, które chcesz usunąć. Następnie w bloku **Właściwości** wybranego konta kliknij pozycję **Usuń**.

 ![Usuwanie konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. W trakcie usuwania konta postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

5. Po usunięciu konta możesz je ponownie utworzyć w bloku właściwości **Konta Uruchom jako**, wybierając opcję tworzenia **Konto Uruchom jako platformy Azure**.

 ![Ponowne tworzenie konta Uruchom jako usługi Automation](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a>Błąd konfiguracji
Niektóre elementy konfiguracji niezbędne do poprawnego działania konta Uruchom jako lub klasycznego konta Uruchom jako zostały usunięte lub nie zostały utworzone prawidłowo podczas początkowej konfiguracji. Te elementy to na przykład:

* Zasób certyfikatu
* Zasób połączenia
* Konto Uruchom jako zostało usunięte z roli współautora
* Nazwa główna usługi lub aplikacji w usłudze Azure AD

W poprzednim i innych przypadkach błędnej konfiguracji konto usługi Automation wykrywa te zmiany i wyświetla stan *Niekompletne* w bloku właściwości **Konta Uruchom jako** dla konta.

![Stan Niekompletne dla konfiguracji konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

Po wybraniu konta Uruchom jako w okienku **Właściwości** konta zostanie wyświetlony następujący komunikat o błędzie:

![Komunikat ostrzegawczy Niekompletne dla konfiguracji konta Uruchom jako](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).

Te problemy związane z kontem Uruchom jako można szybko rozwiązać, usuwając konto i tworząc je ponownie.

## <a name="update-your-automation-account-by-using-powershell"></a>Aktualizacja konta usługi Automation przy użyciu programu PowerShell
Istniejące konto usługi Automation można zaktualizować przy użyciu programu PowerShell, gdy:

* Użytkownik utworzył konto usługi Automation, ale zrezygnował z tworzenia konta Uruchom jako.
* Użytkownik używa już konta usługi Automation do zarządzania zasobami usługi Resource Manager i chce je zaktualizować, aby umożliwiało uwierzytelnianie elementu runbook za pomocą konta Uruchom jako.
* Użytkownik używa już konta usługi Automation do zarządzania zasobami klasycznymi i chce je zaktualizować, aby używać klasycznego konta Uruchom jako, zamiast tworzenia nowego konta i migrowania do niego elementów runbook i zasobów.   
* Użytkownik chce utworzyć konto Uruchom jako i klasyczne konto Uruchom jako przy użyciu certyfikatu wystawionego przez urząd certyfikacji przedsiębiorstwa.

Skrypt ma następujące wymagania wstępne:

* Skrypt jest obsługiwany tylko w systemach Windows 10 i Windows Server 2016 z modułami usługi Azure Resource Manager w wersji 2.01 lub nowszej. Nie jest obsługiwany przez wcześniejsze wersje systemu Windows.
* Program Azure PowerShell 1.0 lub nowszy. Informacje o wersji PowerShell 1.0 można znaleźć w temacie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Jak zainstalować i skonfigurować program Azure PowerShell).
* Konto usługi Automation, które jest przywoływane jako wartość parametrów *–AutomationAccountName* i *-ApplicationDisplayName* w poniższym skrypcie programu PowerShell.

Aby uzyskać wartości parametrów *SubscriptionID*, *ResourceGroup* i *AutomationAccountName*, które są wymagane w przypadku skryptów, wykonaj następujące czynności:
1. W witrynie Azure Portal wybierz konto usługi Automation w bloku **Konto usługi Automation**, a następnie wybierz pozycję **Wszystkie ustawienia**. 
2. W bloku **Wszystkie ustawienia** w obszarze **Ustawienia konta** wybierz pozycję **Właściwości**. 
3. Zwróć uwagę na wartości w bloku **Właściwości**.

![Okienko „Właściwości” konta usługi Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a>Tworzenie skryptu programu PowerShell konta Uruchom jako
Ten skrypt programu PowerShell obsługuje następujące konfiguracje:

* Tworzenie konta Uruchom jako przy użyciu certyfikatu z podpisem własnym.
* Tworzenie konta Uruchom jako i klasycznego konta Uruchom jako przy użyciu certyfikatu z podpisem własnym.
* Tworzenie konta Uruchom jako i klasycznego konta Uruchom jako przy użyciu certyfikatu przedsiębiorstwa.
* Tworzenie konta Uruchom jako i klasycznego konta Uruchom jako przy użyciu certyfikatu z podpisem własnym w chmurze usługi Azure Government.

W zależności od wybranej opcji konfiguracji skrypt tworzy poniższe elementy.

**W przypadku kont Uruchom jako:**

* Tworzy aplikację Azure AD, która będzie eksportowana przy użyciu klucza publicznego certyfikatu z podpisem własnym lub certyfikatu przedsiębiorstwa, tworzy konto dla nazwy głównej usługi dla aplikacji w usłudze Azure AD i przypisuje rolę Współautor dla konta w bieżącej subskrypcji. To ustawienie możesz zmienić na rolę Właściciel lub inną. Aby uzyskać więcej informacji, zobacz [Kontrola dostępu oparta na rolach w usłudze Azure Automation](automation-role-based-access-control.md).
* Tworzy zasób certyfikatu usługi Automation o nazwie *AzureRunAsCertificate* na określonym koncie usługi Automation. Zasób certyfikatu zawiera klucz prywatny certyfikatu używany przez aplikację Azure AD.
* Tworzy zasób połączenia usługi Automation o nazwie *AzureRunAsConnection* na określonym koncie usługi Automation. Zasób połączenia zawiera identyfikator aplikacji, identyfikator dzierżawy, identyfikator subskrypcji i odcisk palca certyfikatu.

**W przypadku klasycznych kont Uruchom jako:**

* Tworzy zasób certyfikatu usługi Automation o nazwie *AzureClassicRunAsCertificate* na określonym koncie usługi Automation. Zasób certyfikatu zawiera klucz prywatny certyfikatu używany przez certyfikat zarządzania.
* Tworzy zasób połączenia usługi Automation o nazwie *AzureClassicRunAsConnection* na określonym koncie usługi Automation. Zasób połączenia zawiera nazwę subskrypcji, identyfikator subskrypcji i nazwę zasobu certyfikatu.

>[!NOTE]
> Jeśli wybierzesz dowolną opcję tworzenia klasycznego konta Uruchom jako, po wykonaniu skryptu przekaż certyfikat publiczny (z rozszerzeniem nazwy pliku CER) do magazynu zarządzania dla subskrypcji, w ramach której zostało utworzone konto usługi Automation.
> 

Aby wykonać skrypt i przekazać certyfikat, wykonaj następujące czynności:

1. Zapisz na komputerze poniższy skrypt. W tym przykładzie zapisz plik pod nazwą *New-RunAsAccount.ps1*.

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
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
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

        # Create a Run As account by using a service principal
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

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Na komputerze kliknij przycisk **Start**, a następnie uruchom program **Windows PowerShell** z podwyższonym poziomem uprawnień użytkownika.

3. Z powłoki wiersza polecenia programu PowerShell z podwyższonym poziomem uprawnień użytkownika przejdź do folderu zawierającego skrypt utworzony w kroku 1.

4. Wykonaj skrypt, używając wartości parametrów wymaganej konfiguracji.

    **Tworzenie konta Uruchom jako przy użyciu certyfikatu z podpisem własnym**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Tworzenie konta Uruchom jako i klasycznego konta Uruchom jako przy użyciu certyfikatu z podpisem własnym**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Tworzenie konta Uruchom jako i klasycznego konta Uruchom jako przy użyciu certyfikatu przedsiębiorstwa**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Tworzenie konta Uruchom jako i klasycznego konta Uruchom jako przy użyciu certyfikatu z podpisem własnym w chmurze usługi Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Po wykonaniu skryptu pojawi się monit o uwierzytelnienie na platformie Azure. Zaloguj się przy użyciu konta, które jest członkiem roli Administratorzy subskrypcji i współadministratorem subskrypcji.
    >
    >

Po pomyślnym wykonaniu skryptu pamiętaj o następujących kwestiach:
* Jeśli zostało utworzone klasyczne konto Uruchom jako z certyfikatem publicznym z podpisem własnym (plik CER), skrypt tworzy i zapisuje je w folderze plików tymczasowych na komputerze w ramach profilu użytkownika *%USERPROFILE%\AppData\Local\Temp* użytego do uruchomienia sesji programu PowerShell.
* Jeśli zostało utworzone klasyczne konto Uruchom jako z publicznym certyfikatem przedsiębiorstwa (plik CER), użyj tego certyfikatu. Postępuj zgodnie z instrukcjami [przekazywania certyfikatu interfejsu API zarządzania do klasycznej witryny Azure Portal](../azure-api-management-certs.md), a następnie zweryfikuj konfigurację poświadczeń za pomocą zasobów usługi Service Management, używając [przykładowego kodu do uwierzytelniania za pośrednictwem zasobów usługi Service Management](#sample-code-to-authenticate-with-service-management-resources). 
* Jeśli klasyczne konto Uruchom jako *nie* zostało utworzone, uwierzytelnij się za pomocą zasobów usługi Resource Manager i zweryfikuj konfigurację poświadczeń, używając [przykładowego kodu do uwierzytelniania za pośrednictwem zasobów usługi Service Management](#sample-code-to-authenticate-with-resource-manager-resources).

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Przykładowy kod służący do uwierzytelniania przy użyciu Menedżera zasobów
Możesz korzystać z następującego zaktualizowanego przykładowego kodu, pobranego z przykładowego elementu runbook *AzureAutomationTutorialScript*, w celu uwierzytelniania za pomocą konta Uruchom jako do zarządzania zasobami usługi Resource Manager przy użyciu elementów runbook.

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

Aby ułatwić pracę z wieloma subskrypcjami, skrypt zawiera dwa dodatkowe wiersze kodu odwołujące się do kontekstu subskrypcji. Zasób zmiennej o nazwie *SubscriptionId* zawiera identyfikator subskrypcji. Po instrukcji polecenia cmdlet `Add-AzureRmAccount` znajduje się polecenie cmdlet [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) z zestawem parametrów *-SubscriptionId*. Jeśli nazwa zmiennej jest zbyt ogólna, można ją skorygować tak, aby dołączyć prefiks lub zastosować inną konwencję nazewnictwa w celu ułatwienia identyfikacji. Alternatywnie można zastosować zestaw parametrów *-SubscriptionName* zamiast -*-SubscriptionId* z odpowiednim zasobem zmiennej.

Polecenie cmdlet służące do uwierzytelniania w elemencie runbook, `Add-AzureRmAccount`, używa zestawu parametrów *ServicePrincipalCertificate*. Uwierzytelnia się ono za pomocą certyfikatu nazwy głównej usługi, a nie poświadczeń użytkownika.

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Przykładowy kod służący do uwierzytelniania przy użyciu zasobów usługi Service Management
Możesz skorzystać z następującego zaktualizowanego przykładowego kodu, pobranego z przykładowego elementu runbook *AzureClassicAutomationTutorialScript*, w celu uwierzytelniania za pomocą klasycznego konta Uruchom jako do zarządzania klasycznymi zasobami przy użyciu elementów runbook.

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
* [Application and service principal objects in Azure AD](../active-directory/active-directory-application-objects.md) (Obiekty aplikacji i nazwy głównej usługi w usłudze Azure AD)
* [Kontrola dostępu oparta na rolach w usłudze Azure Automation](automation-role-based-access-control.md)
* [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md) (Omówienie certyfikatów usług Azure Cloud Services)

