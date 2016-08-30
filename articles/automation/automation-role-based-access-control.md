<properties 
   pageTitle="Kontrola dostępu oparta na rolach w usłudze Azure Automation | Microsoft Azure"
   description="Funkcja kontroli dostępu opartej na rolach (role-based access control, RBAC) umożliwia zarządzanie dostępem do zasobów platformy Azure. Ten artykuł zawiera opis sposobu konfigurowania funkcji RBAC w usłudze Azure Automation."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn"
   keywords="automation rbac, role based access control, azure rbac" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/10/2016"
   ms.author="magoedte;sngun"/>

# Kontrola dostępu oparta na rolach w usłudze Azure Automation

## Kontrola dostępu oparta na rolach

Funkcja kontroli dostępu opartej na rolach (role-based access control, RBAC) umożliwia zarządzanie dostępem do zasobów platformy Azure. Przy użyciu funkcji [RBAC](../active-directory/role-based-access-control-configure.md) można przeprowadzić segregowanie zadań w ramach zespołu i nadać użytkownikom, grupom i aplikacjom tylko takie uprawnienia dostępu, które są im niezbędne do wykonywania zadań. Prawo dostępu oparte na rolach może zostać przydzielone użytkownikom za pomocą portalu Azure, narzędzia wiersza polecenia platformy Azure lub interfejsów API zarządzania platformy Azure.

## Funkcja RBAC w ramach kont automatyzacji

W usłudze Azure Automation prawo dostępu jest nadawane poprzez przypisywanie użytkownikom, grupom i aplikacjom odpowiednich ról RBAC w zakresie konta usługi. Poniżej przedstawiono wbudowane role obsługiwane przez konto automatyzacji:  

|**Rola** | **Opis** |
|:--- |:---|
| Właściciel | Rola właściciela umożliwia dostęp do wszystkich zasobów i akcji w ramach konta usługi, w tym zapewnienie innym użytkownikom, grupom i aplikacjom dostępu do zarządzania kontem. |
| Współautor | Rola współautora umożliwia zarządzanie wszystkim, z wyjątkiem modyfikowania uprawnień dostępu innych użytkowników do konta usługi. |
| Czytelnik | Rola czytelnika służy do wyświetlania wszystkich zasobów w ramach konta usługi, ale nie pozwala na wprowadzanie żadnych zmian. |
| Operator usługi | Rola operatora usługi umożliwia wykonywanie zadań operacyjnych, takich jak uruchamianie, zatrzymywanie, wstrzymywanie, wznawianie i planowanie zadań. Ta rola jest przydatna, jeśli zasoby konta usługi Automation, takie jak zasoby poświadczeń i inne elementy Runbook, mają być chronione przed możliwością wyświetlenia lub modyfikowania, ale członkowie organizacji mają mieć możliwość wykonywania tych elementów Runbook. W tabeli [Akcje operatora usługi Automation](../active-directory/role-based-access-built-in-roles.md#automation-operator) znajduje się lista akcji obsługiwanych przez rolę operatora usługi w odniesieniu do konta usługi i jego zasobów. |
| Administrator dostępu użytkownika | Rola administratora dostępu użytkownika umożliwia zarządzanie dostępem użytkowników do kont usługi Azure Automation. |

Ten artykuł przeprowadza przez kolejne kroki sposobu konfigurowania funkcji RBAC w usłudze Azure Automation. 

## Konfigurowanie funkcji RBAC dla konta usługi Automation za pomocą portalu Azure

1.  Zaloguj się do [portalu Azure](https://portal.azure.com/) i otwórz konto usługi Automation z bloku Konta usługi Automation.  

2.  Kliknij przycisk **Dostęp** w prawym górnym rogu. Spowoduje to otwarcie bloku **Użytkownicy**, w którym można dodawać nowych użytkowników, grupy i aplikacje do zarządzania kontem usługi Automation oraz wyświetlić istniejące role, które można skonfigurować dla konta usługi.  

    ![Przycisk Dostęp](media/automation-role-based-access-control/automation-01-access-button.png)  

>[AZURE.NOTE]  Opcja **Administratorzy subskrypcji** już istnieje jako domyślny użytkownik. Grupa Administratorzy subskrypcji usługi Active Directory obejmuje administratorów i współadministratorów usługi dla subskrypcji platformy Azure. Administrator usługi jest właścicielem subskrypcji platformy Azure i jej zasobów i ma rolę właściciela dziedziczoną również dla kont usługi Automation. Oznacza to, że dostęp jest **dziedziczony** dla **administratorów i współadministratorów usługi ** subskrypcji i **przypisany** wszystkim innym użytkownikom. Kliknij opcję **Administratorzy subskrypcji**, aby wyświetlić więcej szczegółów na temat ich uprawnień.  

### Dodawanie nowego użytkownika i przypisywanie roli

1.  W bloku Użytkownicy kliknij przycisk **Dodaj**, aby otworzyć **blok Dodaj dostęp**, w którym można dodać użytkownika, grupę lub aplikację i przypisywać im rolę.  

    ![Dodawanie użytkownika](media/automation-role-based-access-control/automation-02-add-user.png)  

2.  Wybierz rolę z listy dostępnych ról. W artykule została wybrana rola **Czytelnik**, ale można wybrać dowolną z dostępnych wbudowanych ról obsługiwanych przez konto usługi Automation lub dowolną rolę niestandardową, która mogła zostać zdefiniowana.  

    ![Wybór roli](media/automation-role-based-access-control/automation-03-select-role.png)  

3.  Kliknij przycisk **Dodaj użytkowników**, aby otworzyć blok **Dodaj użytkowników**. Po dodaniu wszystkich użytkowników, grup lub aplikacji do zarządzania subskrypcją ich pozycje zostaną umieszczone na liście i można będzie je wybierać, aby dodać dostęp. Jeśli nie ma żadnych użytkowników na liście lub nie znajduje się na niej konkretny użytkownik, kliknij przycisk **Zaproś**, aby otworzyć blok **Zaproś gościa**, w którym można zaprosić użytkownika z poprawnym adresem e-mail konta Microsoft, takim jak Outlook.com, OneDrive lub Xbox Live ID. Po wprowadzeniu adresu e-mail użytkownika kliknij przycisk **Wybierz**, aby dodać użytkownika, a następnie kliknij przycisk **OK**. 

    ![Dodawanie użytkowników](media/automation-role-based-access-control/automation-04-add-users.png)  
 
Użytkownik dodany do bloku **Użytkownicy** powinien zostać wyświetlony z przypisaną rolą **Czytelnik**.  

![Wyświetlanie użytkowników](media/automation-role-based-access-control/automation-05-list-users.png)  

Można także przypisać rolę użytkownikowi w bloku **Role**. Kliknij przycisk **Role** z bloku Użytkownicy, aby otworzyć **blok Role**. W tym bloku można wyświetlić nazwę roli oraz liczbę użytkowników i grup przypisanych do tej roli.

![Przypisywanie roli w bloku użytkowników](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
>[AZURE.NOTE] Funkcję kontroli dostępu opartej na rolach można ustawić tylko na poziomie konta usługi Automation, a nie z poziomu jakiegokolwiek zasobu poniżej konta usługi.

Użytkownikowi, grupie lub aplikacji można przypisać więcej inż jedną rolę. Na przykład jeśli dodamy użytkownikowi rolę **Operator usługi** razem z **rolą Czytelnik**, będzie on mógł wyświetlać wszystkie zasoby automatyzacji, a także wykonywać zadania elementów Runbook. Można rozwinąć listę rozwijaną, aby wyświetlić listę ról przypisanych do użytkownika.  

![Przeglądanie wielu ról](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  
 
### Usuwanie użytkownika

Można usunąć uprawnienia dostępu dla użytkownika, który nie zarządza kontem usługi Automation lub nie pracuje już w organizacji. Poniżej przedstawiono kroki prowadzące do usunięcia użytkownika: 

1.  W bloku **Użytkownicy** wybierz przypisanie roli, które chcesz usunąć.

2.  Kliknij przycisk **Usuń** w bloku szczegółów przypisania.

3.  Kliknij przycisk **Tak**, aby potwierdzić usunięcie. 

    ![Usuwanie użytkowników](media/automation-role-based-access-control/automation-08-remove-users.png)  

## Użytkownik przypisany do roli

Gdy użytkownik przypisany do roli loguje się do swojego konta usługi Automation, widzi konto właściciela umieszczone na liście **Katalogi domyślne**. Aby wyświetlić konto usługi Automation, do którego został dodany, musi przełączyć katalog domyślny na katalog właściciela.  

![Katalog domyślny](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### Środowisko użytkownika dla roli operatora usługi

Gdy użytkownik przypisany do roli operatora usługi przegląda konto usługi Automation, do którego jest przypisany, może wyświetlić jedynie listę elementów Runbook, zadań elementów Runbook i harmonogramów utworzonych w ramach konta usługi Automation, ale nie może wyświetlić ich definicji. Może uruchamiać, zatrzymywać, wstrzymywać, wznawiać lub planować zadania elementu Runbook. Nie ma dostępu do innych zasobów usługi Automation, takich jak konfiguracje, grupy hybrydowych procesów roboczych ani węzły DSC.  

![Brak dostępu do zasobów](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Gdy użytkownik kliknie element Runbook, polecenia służące do wyświetlenia źródła lub edytowania elementu nie są dostępne, ponieważ rola operatora usługi nie pozwala na dostęp do nich.  

![Brak dostępu do funkcji edytowania elementu Runbook](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

Użytkownik ma dostęp do wyświetlania i tworzenia harmonogramów, ale nie ma dostępu do jakichkolwiek innych typów zasobów.  

![Brak dostępu do zasobów](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Użytkownik nie ma także dostępu do funkcji wyświetlania elementów webhook skojarzonych z elementem Runbook

![Brak dostępu do elementów webhook](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## Konfiguracja funkcji RBAC dla konta usługi Automation przy użyciu programu Azure PowerShell

Można również skonfigurować dostęp oparty na rolach do konta usługi Automation za pomocą następujących [poleceń cmdlet programu Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) wyświetla wszystkie role, które są dostępne w funkcji RBAC usługi Azure Active Directory. Można użyć tego polecenia z właściwością **Nazwa**, aby wyświetlić listę wszystkich użytkowników przypisanych do określonej roli.  
    **Przykład:**  
    ![Pobranie definicji roli](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) wyświetla przypisania ról RBAC platformy Azure w podanym zakresie. Bez żadnych parametrów to polecenie zwraca wszystkie przypisania ról dokonane w ramach subskrypcji. Parametr **ExpandPrincipalGroups** powoduje wyświetlenie listy przypisań do określonego użytkownika oraz grup, których członkiem jest użytkownik.  
    **Przykład:** użyj poniższego polecenia, aby wyświetlić listę wszystkich użytkowników i ich ról w ramach konta usługi Automation.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Pobranie przypisania roli](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) powoduje nadanie użytkownikom, grupom i aplikacjom praw dostępu w określonym zakresie.  
    **Przykład:** za pomocą następującego polecenia utwórz nową rolę „Operator usługi” dla użytkownika w zakresie konta usługi Automation.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Przypisanie nowej roli](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) powoduje usunięcie prawa dostępu dla określonego użytkownika, grupy lub aplikacji w określonym zakresie.
    **Przykład:** za pomocą następującego polecenia utwórz nową rolę „Operator usługi” dla użytkownika w zakresie konta usługi Automation.

    Remove-AzureRmRoleAssignment -SignInName "<sign-in Id of a user you wish to remove>" -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

W powyższych poleceniach cmdlet nazwę logowania, identyfikator subskrypcji, nazwę grupy zasobów i nazwę konta automatyzacji zastąp danymi dotyczącymi swojego konta. Wybierz **tak** po wyświetleniu monitu o kontynuowanie usuwania przypisania roli.   


## Następne kroki
-  Więcej informacji dotyczących różnych sposobów konfigurowania funkcji RBAC w usłudze Azure Automation można znaleźć w artykule dotyczącym [zarządzania funkcją RBAC przy użyciu programu Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).
- Szczegółowe informacje dotyczące różnych sposobów uruchamiania elementu Runbook można znaleźć w artykule [Uruchamianie elementu Runbook](automation-starting-a-runbook.md).
- Informacje dotyczące różnych typów można znaleźć w artykule [Azure Automation runbook types](automation-runbook-types.md) (Typy elementów Runbook w usłudze Azure Automation).




<!--HONumber=jun16_HO2-->


