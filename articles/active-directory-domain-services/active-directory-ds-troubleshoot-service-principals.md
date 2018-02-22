---
title: "Azure Active Directory Domain Services: Rozwiązywania problemów z konfiguracją nazwy głównej usługi | Dokumentacja firmy Microsoft"
description: "Rozwiązywania problemów z konfiguracją nazwy głównej usługi dla usług domenowych Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/19/2018
ms.author: ergreenl
ms.openlocfilehash: 7388bb291f665f195355a01d19a82cba9ed453eb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="troubleshoot-invalid-service-principal-configuration-for-your-managed-domain"></a>Rozwiązywanie problemów z nieprawidłową konfigurację nazwy głównej usługi dla domeny zarządzanej

Ten artykuł pomaga w rozwiązywaniu problemów oraz usuwaniu błędów konfiguracji odnoszące się do podmiotu zabezpieczeń usługi, które powoduje następujący komunikat alertu:

## <a name="alert-aadds102-service-principal-not-found"></a>AADDS102 alertu: Nie można odnaleźć nazwy głównej usługi
**Komunikat alertu:** *wymagane dla usług domenowych Azure AD do prawidłowej nazwy głównej usługi A został usunięty z katalogu usługi Azure AD. Ta konfiguracja ma wpływ na zdolność firmy Microsoft do monitorowania, zarządzania i poprawki i zsynchronizować domeny zarządzanej.*

[Usługa podmiotów](../active-directory/develop/active-directory-application-objects.md) aplikacje, które firma Microsoft używa do zarządzania, aktualizacji i obsługa domeny zarządzanej. Jeśli są one usuwane dzieli możliwość domenę usługi firmy Microsoft. 


## <a name="check-for-missing-service-principals"></a>Sprawdź, czy brak nazwy główne usług
Wykonaj następujące kroki, aby ustalić usługi, która podmiotów zabezpieczeń muszą zostać ponownie utworzone:

1. Przejdź do [aplikacje przedsiębiorstwa — wszystkie aplikacje](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) strony w portalu Azure.
2. W **Pokaż** listy rozwijanej wybierz **wszystkie aplikacje** i kliknij przycisk **Zastosuj**.
3. Korzystając z poniższej tabeli, wprowadź wyszukiwania dla każdego Identyfikatora aplikacji wklejania identyfikator w polu wyszukiwania, a następnie naciskając klawisz. Jeśli wyniki wyszukiwania są puste, wykonując kroki opisane w kolumnie "Rozwiązanie" należy ponownie utworzyć nazwy głównej usługi.

| Identyfikator aplikacji | Rozwiązanie |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Utwórz ponownie Brak nazwy głównej usługi przy użyciu programu PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Zarejestruj ponownie do przestrzeni nazw Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Zarejestruj ponownie do przestrzeni nazw Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Nazwy główne usług, które self Popraw](#service-principals-that-self-correct) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>Utwórz ponownie Brak nazwy głównej usługi przy użyciu programu PowerShell
Wykonaj następujące kroki, jeśli nazwy głównej usługi o identyfikatorze ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` nie ma w katalogu usługi Azure AD.

**Środki zaradcze:** należy wykonać te kroki programu Azure AD PowerShell. Aby uzyskać informacje na temat instalowania programu Azure AD PowerShell, zobacz [w tym artykule](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Aby rozwiązać ten problem, wpisz następujące polecenia w oknie programu PowerShell:
1. Zainstaluj moduł Azure AD PowerShell i zaimportuj go.
    
    ```powershell 
    Install-Module AzureAD
    Import-Module AzureAD
    ```
    
2. Sprawdź, czy nazwy głównej usługi wymagane dla usług domenowych Azure AD w katalogu, wykonując następujące polecenie programu PowerShell:
    
    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```
    
3. Tworzenie nazwy głównej usługi, wpisując następujące polecenie programu PowerShell:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```
    
4. Po utworzeniu usługi Brak podmiotu zabezpieczeń, Zaczekaj dwie godziny i Sprawdź kondycję domeny zarządzanej.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Zarejestruj ponownie do przestrzeni nazw AAD firmy Microsoft przy użyciu portalu Azure
Wykonaj następujące kroki, jeśli nazwy głównej usługi o identyfikatorze ```443155a6-77f3-45e3-882b-22b3a8d431fb``` lub ```abba844e-bc0e-44b0-947a-dc74e5d09022``` nie ma w katalogu usługi Azure AD.

**Środki zaradcze:** wykonaj następujące kroki, aby przywrócić usług domenowych w katalogu:

1. Przejdź do [subskrypcje](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) strony w portalu Azure.
2. Wybierz subskrypcję z tabeli, która jest skojarzona z domeny zarządzanej
3. Przy użyciu nawigacji po lewej stronie, wybierz **dostawców zasobów**
4. Wyszukaj "Microsoft.AAD" w tabeli, a następnie kliknij przycisk **ponownie zarejestrować**
5. Aby upewnić się, że alert nie zostanie rozwiązany, Wyświetl stronę kondycji dla domeny zarządzanej w dwóch godzin.


## <a name="service-principals-that-self-correct"></a>Nazwy główne usług, które self Popraw
Wykonaj następujące kroki, jeśli nazwy głównej usługi o identyfikatorze ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` nie ma w katalogu usługi Azure AD.

**Środki zaradcze:** usługi domenowe Azure AD można Wykryj, kiedy tego podmiotu zabezpieczeń określonej usługi jest Brak, niepoprawnie skonfigurowany lub został usunięty. Usługa automatycznie odtworzy tej nazwy głównej usługi. Sprawdź kondycję domeny zarządzanej po dwóch godzinach, aby upewnić się, że nazwy głównej usługi został utworzony ponownie.


## <a name="contact-us"></a>Skontaktuj się z nami
Skontaktuj się z zespołem produktu usługi Azure Active Directory Domain Services, aby [udostępnić opinię lub pomocy technicznej](active-directory-ds-contact-us.md).
