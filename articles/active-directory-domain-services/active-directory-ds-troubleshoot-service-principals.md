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
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: e6144a4018f7fbe7dbf7b4693e3f41884e4a80a2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-service-principal-configuration"></a>Usługi domenowe AD Azure — Rozwiązywanie problemów z nazwy głównej usługi konfiguracji

Usługi zarządzania i Aktualizacja domeny, firma Microsoft korzysta z różnych [nazwy główne usług](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects) do komunikowania się z katalogiem. Jeśli jedna jest nieprawidłowo skonfigurowana lub usunięty, może spowodować zakłócenia w usłudze.

## <a name="aadds102-service-principal-not-found"></a>AADDS102: Nie można odnaleźć nazwy głównej usługi

**Komunikat alertu:**

*Wymagane dla usług domenowych Azure AD do prawidłowej nazwy głównej usługi został usunięty z katalogu usługi Azure AD. Ta konfiguracja ma wpływ na zdolność firmy Microsoft do monitorowania, zarządzania i poprawki i zsynchronizować domeny zarządzanej.*

Nazwy główne usług są aplikacje, które firma Microsoft używa do zarządzania, aktualizacji i obsługa domeny zarządzanej. Jeśli są one usuwane dzieli możliwość domenę usługi firmy Microsoft. Wykonaj następujące kroki, aby ustalić usługi, która podmiotów zabezpieczeń muszą zostać ponownie utworzone.

1. Przejdź do [aplikacje przedsiębiorstwa — wszystkie aplikacje](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) strony w portalu Azure.
2. Za pomocą listy rozwijanej Pokaż, wybierz **wszystkie aplikacje** i kliknij przycisk **Zastosuj**.
3. Korzystając z poniższej tabeli, wprowadź wyszukiwania dla każdego Identyfikatora aplikacji wklejania identyfikator w polu wyszukiwania, a następnie naciskając klawisz. Jeśli wyniki wyszukiwania są puste, wykonując kroki opisane w kolumnie "Rozwiązanie" należy ponownie utworzyć nazwy głównej usługi.

| Identyfikator aplikacji | Rozwiązanie |
| :--- | :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Ponowne tworzenie Brak nazwy głównej usługi przy użyciu programu PowerShell](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Zarejestruj ponownie do przestrzeni nazw Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Zarejestruj ponownie do przestrzeni nazw Microsoft.AAD](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Nazwy główne usług, które Self Popraw](#service-principals-that-self-correct) |

### <a name="recreate-a-missing-service-principal-with-powershell"></a>Utwórz ponownie Brak nazwy głównej usługi przy użyciu programu PowerShell

*Wykonaj następujące kroki dla brakujących identyfikatorów: 2565bd9d-da50-47d4-8b85-4c97f669dc36*

**Środki zaradcze:**

Należy wykonać te kroki programu Azure AD PowerShell. Aby uzyskać informacje na temat instalowania programu Azure AD PowerShell, zobacz [w tym artykule](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.).

Aby rozwiązać ten problem, wpisz następujące polecenia w oknie programu PowerShell:
1. Zainstaluj moduł AzureAD
2. Import-Module AzureAD
3. Sprawdź, czy nazwy głównej usługi wymagane dla usług domenowych Azure AD w katalogu, wykonując następujące polecenie programu PowerShell:
      ```PowerShell
      Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
      ```
4. Tworzenie nazwy głównej usługi, wpisując następujące polecenie programu PowerShell:
     ```PowerShell
     New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
     ```
5. Po utworzeniu Brak nazwy głównej usługi, Zaczekaj dwie godziny i Sprawdź kondycję użytkownika domeny.


### <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Zarejestruj ponownie do przestrzeni nazw AAD firmy Microsoft przy użyciu portalu Azure

*Wykonaj następujące kroki dla brakujących identyfikatorów: 443155a6-77f3-45e3-882b-22b3a8d431fb i abba844e-bc0e-44b0-947a-dc74e5d09022*

**Środki zaradcze:**

Aby przywrócić usług domenowych w katalogu, wykonaj następujące kroki:

1. Przejdź do [subskrypcje](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) strony w portalu Azure.
2. Wybierz subskrypcję z tabeli, która jest skojarzona z domeny zarządzanej
3. Przy użyciu nawigacji po lewej stronie, wybierz **dostawców zasobów**
4. Wyszukaj "Microsoft.AAD" w tabeli, a następnie kliknij przycisk **ponownie zarejestrować**
5. Aby sprawdzić, upewnij się, że alert został rozwiązany, należy wyświetlić stronę alertu health w dwóch godzin.


### <a name="service-principals-that-self-correct"></a>Nazwy główne usług, które self Popraw

*Wykonaj następujące kroki dla brakujących identyfikatorów: d87dcbc6-a371-462e-88e3-28ad15ec4e64*

**Środki zaradcze:**

Microsoft można zdefiniować określonej nazwy główne usług brakuje, niepoprawnie skonfigurowany lub usunięty. Aby szybko rozwiązać usługi, Microsoft odtworzy nazwy główne usług, do samej siebie. Sprawdź kondycję domeny po dwóch godzinach, aby upewnić się, że podmiot zabezpieczeń został utworzony ponownie.

## <a name="contact-us"></a>Skontaktuj się z nami
Skontaktuj się z zespołem produktu usługi Azure Active Directory Domain Services, aby [udostępnić opinię lub pomocy technicznej](active-directory-ds-contact-us.md).
