---
title: 'Azure Databricks: Typowe pytania i pomoc | Dokumentacja firmy Microsoft'
description: "Odpowiedzi na często zadawane pytania i informacje dotyczące rozwiązywania problemów dotyczących Azure Databricks."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: b6b001087cba5f8550d4fea3e4a2f7c1c865beae
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="azure-databricks-preview-common-questions-and-help"></a>Azure Databricks w wersji zapoznawczej: Typowe pytania i pomoc

W tym artykule wymieniono najczęściej używanych zapytania, które mogą mieć dotyczą Azure Databricks. Zawiera także listę niektórych typowych problemów, które możesz napotkać podczas używania Azure Databricks. Aby uzyskać więcej informacji na Azure Databricks, zobacz [co to jest Azure Databricks?](what-is-azure-databricks.md) 

## <a name="common-questions"></a>Często zadawane pytania

Ta sekcja zawiera często zadawane pytania dotyczące Azure Databricks.

### <a name="can-i-use-my-own-keys-for-local-encryption"></a>Czy można użyć własnych kluczy szyfrowania lokalnego? 
W bieżącej wersji za pomocą własnych kluczy z Azure Keyvault nie jest obsługiwana. 

### <a name="can-i-use-azure-vnets-with-azure-databricks"></a>Z Azure Databricks mogą używać sieci wirtualnych platformy Azure?
Nowej sieci Wirtualnej jest tworzony jako część Azure Databricks inicjowania obsługi administracyjnej. W ramach tej wersji nie można użyć własnych sieci wirtualnej platformy Azure.

### <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Jak dostęp do usługi Azure Data Lake Store z notesu? 

1. W usłudze Azure Active Directory udostępniać nazwy głównej usługi i zarejestrować jego klucza.
2. Przypisz uprawnienia niezbędne do nazwy głównej usługi w usłudze Azure Data Lake Store.
3. Aby uzyskać dostęp do plików w usłudze Azure Data Lake Store, Użyj poświadczeń nazwy głównej usługi w Notatniku.

Aby uzyskać więcej informacji, zobacz [Użyj Data Lake Store z Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

W tej sekcji opisano sposoby rozwiązywania typowych problemów z Azure Databricks.

### <a name="issue-your-account-email-does-not-have-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problem: Twoje konto {e-mail} nie ma roli właściciela lub współautora Databricks zasobu obszaru roboczego w portalu Azure.

**Komunikat o błędzie**

Konto {e-mail} nie ma roli właściciela lub współautora Databricks zasobu obszaru roboczego w portalu Azure. Ten błąd może również wystąpić, jeśli użytkownik-Gość w dzierżawie. Poproś administratora o udzielenie dostępu lub dodanie Ciebie jako użytkownik bezpośrednio w obszarze roboczym Databricks. 

**Rozwiązania**

Inicjowania dzierżawca, musi być zalogowany jako zwykły użytkownik dzierżawy, a nie użytkownika gościa. Rola współautora musi mieć również Databricks zasobu obszaru roboczego. Można udzielić użytkownikowi dostępu z **(IAM) kontroli dostępu** kartę w obszarze roboczym Azure Databricks w portalu Azure.

### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problem: Twoje konto {e-mail} nie została zarejestrowana w Databricks 

**Rozwiązania**

Jeśli nie może utworzyć obszaru roboczego są dodawane jako użytkownik obszaru roboczego, skontaktuj się z osobą, utworzony obszar roboczy, aby dodać użytkownika za pomocą konsoli administracyjnej Databricks Azure. Aby uzyskać instrukcje, zobacz [Dodawanie i zarządzanie użytkownikami](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Jeśli utworzono obszar roboczy i nadal ten błąd, spróbuj ponowne kliknięcie "Zainicjować obszaru roboczego" z portalu Azure.

### <a name="issue-cloud-provider-launch-failure-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problem: Błąd uruchamiania dostawcy chmury: Napotkano błąd dostawcy chmury podczas konfigurowania klastra.

**Komunikat o błędzie**

Błąd uruchamiania dostawcy chmury: Napotkano błąd dostawcy chmury podczas konfigurowania klastra. Zobacz Przewodnik Databricks, aby uzyskać więcej informacji. Kod błędu systemu Azure: PublicIPCountLimitReached. Komunikat o błędzie platformy Azure: nie można utworzyć więcej niż 60 publicznych adresów IP dla tej subskrypcji w tym regionie.

**Rozwiązania**

Azure klastrom Databricks jeden publiczny adres IP na węzeł. Jeśli subskrypcja jest już używana wszystkich swoich publicznych adresów IP, należy [żądania, aby zwiększyć przydział](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request). Wybierz **przydziału** jako **typu problemu**, **sieci ARM** jako **typu przydziału**i poproś o zwiększenie limitu przydziału publicznego adresu IP  **Szczegóły** (na przykład, jeśli limit jest obecnie 60 i ma zostać utworzony 100 węzła klastra, zwiększenia limitu do 160).

## <a name="next-steps"></a>Następne kroki
Aby uzyskać instrukcje krok po kroku można utworzyć fabryki danych w wersji 2 zobacz następujące samouczki:

- [Szybki Start: Rozpoczynanie pracy z Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Co to jest Azure Databricks?](what-is-azure-databricks.md)

