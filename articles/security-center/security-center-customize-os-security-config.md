---
title: "Dostosowywanie systemu operacyjnego konfiguracjach zabezpieczeń w Centrum zabezpieczeń Azure [Podgląd] | Dokumentacja firmy Microsoft"
description: "W tym artykule jest przedstawienie sposobu dostosowywania oceny Centrum zabezpieczeń"
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: terrylan
ms.openlocfilehash: 3af59e1b38e70494dd9dc17e2682d31cf7b7d361
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="customizing-os-security-configurations-in-azure-security-center-preview"></a>Dostosowywanie systemu operacyjnego konfiguracjach zabezpieczeń w Centrum zabezpieczeń Azure [Podgląd]

Dowiedz się, jak dostosować oceny systemu operacyjnego konfiguracji zabezpieczeń w Centrum zabezpieczeń Azure za pomocą tego przewodnika.

## <a name="what-are-os-security-configurations"></a>Co to są konfiguracji zabezpieczeń systemu operacyjnego?

Konfiguracje zabezpieczeń monitorów Centrum zabezpieczeń Azure przy użyciu zestawu reguł zalecanych ponad 150 w celu ograniczenia funkcjonalności systemu operacyjnego, tym zasady dotyczące zapór, inspekcji, zasad haseł i innych. Jeśli maszyna zostanie znaleziony narażone konfiguracji, to generowany jest zalecana ze względów bezpieczeństwa.

Dostosowanie zasad może ułatwić organizacjom kontroli, które opcje konfiguracji są bardziej odpowiednie dla swojego środowiska. Ta funkcja umożliwia użytkownikom ustawić zasady niestandardowe oceny i zastosować je na wszystkich komputerach stosowane w ramach subskrypcji.

> [!NOTE]
> - Dostosowywanie konfiguracji zabezpieczeń systemu operacyjnego jest obecnie dostępne dla systemu Windows Server 2008, 2008 R2, 2012 i 2012 R2 w systemach operacyjnych.
- Konfiguracja stosowana do wszystkich maszyn wirtualnych i komputery podłączone do wszystkich obszarów roboczych w ramach wybranej subskrypcji.
- Dostosowywanie konfiguracji zabezpieczeń systemu operacyjnego jest dostępna tylko w warstwie standardowa, Centrum zabezpieczeń.
>
>

Jak dostosowywać reguły konfiguracji zabezpieczeń systemu operacyjnego?

Reguły konfiguracji zabezpieczeń systemu operacyjnego można dostosować, włączanie i wyłączanie określonej reguły, zmiana odpowiednie ustawienie do istniejącej reguły i dodawanie nowej reguły na podstawie typów reguł obsługiwanych (rejestru, zasady inspekcji i zasady zabezpieczeń). Obecnie odpowiednie ustawienie musi być dokładną wartość.

Nowe zasady muszą być ten sam format i struktury jako innych istniejących reguł tego samego typu.

> [!NOTE]
> Aby dostosować konfiguracji zabezpieczeń systemu operacyjnego, musi mieć przypisaną rolę właściciela subskrypcji, współautor subskrypcji lub administratora zabezpieczeń.
>
>

## <a name="customize-security-configuration"></a>Dostosowywanie konfiguracji zabezpieczeń

Aby dostosować domyślna konfiguracja zabezpieczeń systemu operacyjnego w Centrum zabezpieczeń:

1.  Otwórz pulpit nawigacyjny usługi **Security Center**.

2.  W menu głównym Centrum zabezpieczeń, wybierz **zasady zabezpieczeń**.  **Centrum zabezpieczeń — zasady zabezpieczeń** otwiera.

3.  Wybierz subskrypcję, którą chcesz wykonać dostosowania dla.

    ![](media/security-center-customize-os-security-config/open-security-policy.png)

4. W obszarze **składniki zasad**, wybierz pozycję **Edycja konfiguracji zabezpieczeń**.

6.  **Edytuj konfiguracje zabezpieczeń** otwiera. Wykonaj kroki wyróżniane na ekranie, aby pobrać, edytować i przekazać zmodyfikowany plik.

    ![](media/security-center-customize-os-security-config/blade.png)

  > [!NOTE]
  > Domyślnie pobrany plik konfiguracji jest w *json* format. Aby uzyskać instrukcje na temat sposobu modyfikowania tego pliku, przejdź do [dostosować plik konfiguracji](#customize-the-configuration-file).
  >

7. Po pomyślnym zapisaniu pliku, konfiguracja zostanie zastosowana do wszystkich maszyn wirtualnych i komputery podłączone do wszystkich obszarów roboczych w ramach wybranej subskrypcji. Ten proces może potrwać pewien czas, zwykle za kilka minut, ale można go może trwać dłużej, ponieważ zależy od rozmiaru infrastruktury. Wybierz **zapisać** dokonać zmiany, w przeciwnym razie wartość zasad nie są przechowywane.

    ![](media/security-center-customize-os-security-config/save-successfully.png)

W dowolnym momencie można zresetować bieżącej konfiguracji zasad do wartości domyślnych zasad, wybierając **zresetować** opcji **reguły Edycja konfiguracji zabezpieczeń systemu operacyjnego**. Jeśli wybierzesz tę opcję, pojawi się następujący alert elementu podręcznego. Wybierz **tak** o potwierdzenie.

![](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Dostosowywanie pliku konfiguracji

W pliku dostosowania wszystkie obsługiwane wersje systemu operacyjnego ma zestaw reguł (zestaw reguł). Każdy zestaw reguł ma własną nazwę i unikatowy identyfikator, jak pokazano w poniższym przykładzie:

![](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Ten plik był edytowany przy użyciu programu Visual Studio, ale umożliwia także Notatnik tak długo, jak długo mają zainstalowane wtyczki Podgląd JSON.
>
>

Edytując ten plik, można zmodyfikować jedną regułę lub wszystkie z nich. Każdy zestaw reguł zawiera *reguły* sekcji, który zawiera reguły, podzielić na 3 kategorie reguł: rejestru, zasady inspekcji i zasady zabezpieczeń jako wyświetlane poniżej:

![](media/security-center-customize-os-security-config/rules-section.png)

Każda kategoria ma swój własny zestaw atrybutów. W przypadku istniejących reguł można wprowadzić zmiany w następujących pól:

- expectedValue: typ danych pola tego atrybutu musi odpowiadać obsługiwane wartości dla poszczególnych typów reguł, na przykład:

  - baselineRegistryRules: wartość powinna być zgodna [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) zdefiniowane w tej regule.

  - baselineAuditPolicyRules: wartość powinna być wartości ciągu, jedną z następujących czynności:

    - Sukcesy i niepowodzenia

    - Powodzenie

  - baselineSecurityPolicyRules: wartość powinna być wartości ciągu, jedną z następujących czynności:

    - "Nikt"

    - Lista dozwolonych grup użytkowników, na przykład: "Administratorzy, Operatorzy kopii zapasowych"

-   Stan: ciąg, który może zawierać opcji "Wyłączone" lub "Enabled". W tej wersji prywatnej wersji zapoznawczej ciąg jest rozróżniana wielkość liter.

Są to jedyne pola, które można skonfigurować. Jeśli użytkownik narusza format pliku lub rozmiar, nie można zapisać zmiany. Następujący komunikat o błędzie występuje, gdy nie można przetworzyć pliku:

![](media/security-center-customize-os-security-config/invalid-json.png)

Zobacz [kody błędów](#error-codes) listę potencjalnych błędów.

Poniżej masz kilka przykładów tych zasad. Można zmienić atrybutów "expectedValue" i "stanu":

**Sekcja reguł:** baselineRegistryRules
```
{

    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

}
```

**Sekcja reguł:** baselineAuditPolicyRules
```
{
"auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
"ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
"originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
"cceId": "CCE-11001-5",
"ruleName": "Audit Policy: Account Management: Other Account Management Events",
"ruleType": "AuditPolicy",
"expectedValue": "Success and Failure",
"severity": "Critical",
"analyzeOperation": "Equals",
"source": "Microsoft",
"state": "Enabled"
},
```

**Sekcje reguł:** baselineSecurityPolicyRules
```
{
"sectionName": "Privilege Rights",
"settingName": "SeIncreaseWorkingSetPrivilege",
"ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
"originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
"cceId": "CCE-10548-6",
"ruleName": "Increase a process working set",
"ruleType": "SecurityPolicy",
"expectedValue": "Administrators, Local Service",
"severity": "Warning",
"analyzeOperation": "Equals",
"source": "Microsoft",
"state": "Enabled"
},
```

Brak niektórych reguł, które są duplikowane dla różnych typów systemu operacyjnego. Zduplikowane reguły mają tego samego originalId.

## <a name="adding-a-new-custom-rule"></a>Dodawanie nowej reguły niestandardowej

Można również utworzyć nową regułę. Przed utworzeniem nowej reguły, mieć na uwadze następujące ograniczenia:

-   Wersja schematu *baselineId* i *baselineName* nie można zmienić.

-   Nie można usunąć zestaw reguł.

-   Nie można dodać zestaw reguł.

-   Maksymalna liczba reguł niedozwolonych (w tym reguły domyślne) wynosi 1000 reguły.

Nowe reguły niestandardowe są oznaczone ikoną z nowego źródła niestandardowego (! = "Microsoft"). *RuleId* pole może być zerowa ani pusta. Jeśli jest pusty, Microsoft generuje go. Jeśli nie jest pusta, musi mieć prawidłowy identyfikator GUID Unikatowy we wszystkich reguł (domyślne i niestandardowe). Przejrzyj ograniczenia poniżej dotyczących pola podstawowe:

-   *originalId* — może być zerowa ani pusta. Jeśli *originalId* jest nie jest pusty, powinno być prawidłowym identyfikatorem GUID.

-   *cceId* — może być zerowa ani pusta. Jeśli *cceId* jest nie jest pusty, musi być unikatowa.

-   *Typ* — jeden z: rejestru, AuditPolicy lub SecurityPolicy.

-   *Ważność* — jeden z: nieznany, krytyczne, ostrzeżenie lub komunikat informacyjny.

-   *analyzeOperation* — musi być równa.

-   *auditPolicyId* — musi być prawidłowym identyfikatorem GUID.

-   *regValueType* — musi mieć jedną z: Int, Long, ciągu lub MultipleString.

> [!NOTE]
> Gałąź musi być *LocalMachine*.
>
>

Przykład nową regułę niestandardową:

**Rejestru**:
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule”, "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
   }
```
**Zasady zabezpieczeń**:
```
{

   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Zasady inspekcji:**
```
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Plik operacji przekazywania zakończonych niepowodzeniem

Jeśli plik przesłanych konfiguracji jest nieprawidłowa z powodu błędów w wartości lub formatowania, jest wyświetlany błąd awarii. Można pobrać raportu csv szczegóły błędów można skorygować i napraw błędy przed ponownym przesłaniem plik konfiguracji poprawiony.

![](media/security-center-customize-os-security-config/invalid-configuration.png)

Przykładowy plik błędów:

![](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Kody błędów

Poniższa lista zawiera wszystkie potencjalne błędy:

| **Błąd**                                | **Opis**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | Właściwość "schemaVersion" znaleziono nieprawidłowy lub pusty. Wartość musi być równa "{0}".                                                         |
| BaselineInvalidStringError               | Właściwość "{0}" nie może zawierać "\\n".                                                                                                         |
| BaselineNullRuleError                    | Listę reguł linii bazowej konfiguracji zawiera reguły o wartości "null".                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE-ID "{0}" nie jest unikatowa.                                                                                                                  |
| BaselineRuleEmptyProperty                | Właściwości: "{0}" jest nieprawidłowy lub niedostępny.                                                                                                       |
| BaselineRuleIdNotInDefault               | Reguła ma właściwości source 'Microsoft', ale nie znaleziono zestaw reguł domyślna firmy Microsoft.                                                   |
| BaselineRuleIdNotUniqueError             | Identyfikator reguły nie jest unikatowa.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Właściwość "{0}" jest nieprawidłowy. Wartość nie jest prawidłowym identyfikatorem Guid.                                                                             |
| BaselineRuleInvalidHive                  | Gałąź musi być LocalMachine.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Nazwa reguły nie jest unikatowa.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Zasada nie została odnaleziona w nowej konfiguracji. Nie można usunąć reguły.                                                                     |
| BaselineRuleNotFoundError                | Reguły nie został znaleziony w domyślnym podstawowym zestawem reguł.                                                                                        |
| BaselineRuleNotInPlace                   | Reguła zgodny z regułą domyślnego o typ {0} i wymienionej na liście {1}.                                                                       |
| BaselineRulePropertyTooLong              | Właściwości: "{0}" jest za długa. Maksymalna dozwolona długość: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Wartość oczekiwana "{0}" jest niezgodny typ wartości rejestru, który jest zdefiniowany.                                                              |
| BaselineRulesetAdded                     | Zestaw reguł o identyfikatorze "{0}" nie został znaleziony w konfiguracji domyślnej. Nie można dodać zestawu reguł.                                               |
| BaselineRulesetIdMustBeUnique            | Zestaw reguł jednej linii bazowej "{0}" musi być unikatowa.                                                                                           |
| BaselineRulesetNotFound                  | Ustaw zasady o identyfikatorze "{0}", a nazwa "{1}" nie został znaleziony w danej konfiguracji. Nie można usunąć zestawu reguł.                                |
| BaselineRuleSourceNotMatch               | Reguła o identyfikatorze "{0}" jest już zdefiniowany.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Domyślny typ reguły jest "{0}".                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Rzeczywisty typ reguły to: {0}, ale typ właściwości: {1}.                                                                          |
| BaselineRuleUnpermittedChangesError      | Dozwolone są tylko właściwości 'expectedValue' i 'state' zostanie zmieniony.                                                                       |
| BaselineTooManyRules                     | Maksymalną dozwoloną liczbę reguł niestandardowych jest reguły {0}. Danej konfiguracji zawiera reguły {1}. (reguły domyślne {2} + {3} dostosowywać reguł. |
| ErrorNoConfigurationStatus               | Stan konfiguracji, nie został znaleziony. Proszę podać stan wymaganą konfiguracją — "Default" lub "Custom".                                    |
| ErrorNonEmptyRulesetOnDefault            | Stan konfiguracji jest ustawiona na wartość domyślną. Lista BaselineRulesets musi być zerowa ani pusta.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Stan danej konfiguracji jest "Custom", ale właściwość "baselineRulesets" ma wartość null lub pusty.                                             |
| ErrorParsingBaselineConfig               | Danej konfiguracji jest nieprawidłowy. Co najmniej jedna z wartości zdefiniowanych ma wartość null lub nieprawidłowego typu.                                  |
| ErrorParsingIsDefaultProperty            | Wartość danego configurationStatus "{0}" jest nieprawidłowa. Wartość może być tylko "Default" lub "Custom".                                         |
| InCompatibleViewVersion                  | Wyświetlanie wersji: {0} nie jest obsługiwana w tym typie obszaru roboczego.                                                                                   |
| InvalidBaselineConfigurationGeneralError | Jednej linii bazowej konfiguracji znaleziono z co najmniej jeden błąd sprawdzania poprawności typu.                                                          |
| ViewConversionError                      | Widok jest starsza wersja obsługiwana w tym obszarze roboczym. Wyświetl konwersja nie powiodła się: {0}                                                                 |

Jeśli nie masz wystarczających uprawnień, może wystąpić błąd ogólny błąd:

![](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Kolejne kroki
W tym artykule pokazano, jak dostosować oceny systemu operacyjnego konfiguracji zabezpieczeń w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat zasad konfiguracji i korygowania, zobacz:

- [Wspólne identyfikatory konfiguracji Centrum zabezpieczeń i reguły linii bazowej](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Centrum zabezpieczeń używane CCE (typowych konfiguracji wyliczenie) do przypisywania unikatowych identyfikatorów dla reguły konfiguracji. Odwiedź stronę [CCE](https://nvd.nist.gov/config/cce/index) witryny, aby uzyskać więcej informacji.
- [Skoryguj konfiguracje zabezpieczeń](security-center-remediate-os-vulnerabilities.md) pokazuje, jak rozwiązać luk w zabezpieczeniach, podczas konfiguracji systemu operacyjnego jest niezgodny z reguły konfiguracji zabezpieczeń.
