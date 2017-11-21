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
ms.date: 11/17/2017
ms.author: nitinme
ms.openlocfilehash: 6bb542537ec713be272f7e58e0b247763214ef4a
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
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

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problem: Ta subskrypcja nie jest zarejestrowany do korzystania z przestrzeni nazw "Microsoft.Databricks"

**Komunikat o błędzie**

Ta subskrypcja nie jest zarejestrowany w celu używania przestrzeni nazw "Microsoft.Databricks". Zobacz https://aka.ms/rps-not-found dotyczące sposobu rejestrowania subskrypcji. (Kod: MissingSubscriptionRegistration)

**Rozwiązania**

1. Przejdź do [portalu Azure](https://portal.azure.com).
2. Kliknij przycisk **subskrypcje**, subskrypcji, a następnie kliknij przycisk **dostawców zasobów**. 
3. Lista dostawców zasobów przed **Microsoft.Databricks**, kliknij przycisk **zarejestrować**. Musi mieć rolę współautora lub właściciela subskrypcji można zarejestrować dostawcy zasobów.


### <a name="issue-your-account-email-does-not-have-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problem: Twoje konto {e-mail} nie ma roli właściciela lub współautora Databricks zasobu obszaru roboczego w portalu Azure.

**Komunikat o błędzie**

Konto {e-mail} nie ma roli właściciela lub współautora Databricks zasobu obszaru roboczego w portalu Azure. Ten błąd może również wystąpić, jeśli użytkownik-Gość w dzierżawie. Poproś administratora o udzielenie dostępu lub dodanie Ciebie jako użytkownik bezpośrednio w obszarze roboczym Databricks. 

**Rozwiązania**

Poniżej przedstawiono kilka rozwiązania tego problemu:

* Inicjowania dzierżawca, musi być zalogowany jako zwykły użytkownik dzierżawy, a nie użytkownika gościa. Rola współautora musi mieć również Databricks zasobu obszaru roboczego. Można udzielić użytkownikowi dostępu z **(IAM) kontroli dostępu** kartę w obszarze roboczym Azure Databricks w portalu Azure.

* Ten błąd może również wystąpić, jeśli nazwy domeny poczty e-mail jest przypisany do wielu usług Active Directory. Aby obejść ten problem, należy utworzyć nowego użytkownika w usłudze Active Directory zawierającą subskrypcji z obszaru roboczego Databricks.

    a. W portalu Azure, przejdź do usługi Azure Active Directory, kliknij przycisk **użytkowników i grup**, kliknij przycisk **dodać użytkownika**.

    b. Dodaj użytkownika z `@<tenant_name>.onmicrosoft.com` e-mail zamiast @< format domena_użytkownika > wiadomości e-mail. Można znaleźć < nazwa_dzierżawy >. onmicrosoft.com skojarzone z usługi Active Directory w **domen niestandardowych** w usłudze Azure Active Directory w portalu Azure.
    
    c. Przyznaj tym nowego użytkownika **współautora** roli Databricks zasobu obszaru roboczego.
    
    d. Zaloguj się do portalu Azure przy użyciu nowego użytkownika i Znajdź obszar roboczy Databricks.
    
    e. Uruchom obszaru roboczego Databricks jako ten użytkownik.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problem: Twoje konto {e-mail} nie została zarejestrowana w Databricks 

**Rozwiązania**

Jeśli nie może utworzyć obszaru roboczego są dodawane jako użytkownik obszaru roboczego, skontaktuj się z osobą, utworzony obszar roboczy, aby dodać użytkownika za pomocą konsoli administracyjnej Databricks Azure. Aby uzyskać instrukcje, zobacz [Dodawanie i zarządzanie użytkownikami](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Jeśli utworzono obszar roboczy i nadal ten błąd, spróbuj ponowne kliknięcie "Zainicjować obszaru roboczego" z portalu Azure.

### <a name="issue-cloud-provider-launch-failure-publicipcountlimitreached-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problem: Chmury uruchamianie błąd dostawcy (PublicIPCountLimitReached): Napotkano błąd dostawcy chmury podczas konfigurowania klastra

**Komunikat o błędzie**

Błąd uruchamiania dostawcy chmury: Napotkano błąd dostawcy chmury podczas konfigurowania klastra. Zobacz Przewodnik Databricks, aby uzyskać więcej informacji. Kod błędu systemu Azure: PublicIPCountLimitReached. Komunikat o błędzie platformy Azure: nie można utworzyć więcej niż 60 publicznych adresów IP dla tej subskrypcji w tym regionie.

**Rozwiązania**

Azure klastrom Databricks jeden publiczny adres IP na węzeł. Jeśli subskrypcja jest już używana wszystkich swoich publicznych adresów IP, należy [żądania, aby zwiększyć przydział](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request). Wybierz **przydziału** jako **typu problemu**, **sieci: ARM** jako **typu przydziału**i poproś o zwiększenie limitu przydziału publicznego adresu IP  **Szczegóły**. Na przykład, jeśli limit jest obecnie 60 i chcesz utworzyć 100 węzła klastra, aby 160 zwiększenia limitu.

### <a name="issue-cloud-provider-launch-failure-missingsubscriptionregistration-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problem: Chmury uruchamianie błąd dostawcy (MissingSubscriptionRegistration): Napotkano błąd dostawcy chmury podczas konfigurowania klastra

**Komunikat o błędzie**

Błąd uruchamiania dostawcy chmury: Napotkano błąd dostawcy chmury podczas konfigurowania klastra. Zobacz Przewodnik Databricks, aby uzyskać więcej informacji.
Kod błędu systemu Azure: komunikat o błędzie platformy MissingSubscriptionRegistration Azure: subskrypcji nie jest zarejestrowany w celu używania przestrzeni nazw "Microsoft.Compute". Zobacz https://aka.ms/rps-not-found dotyczące sposobu rejestrowania subskrypcji

**Rozwiązania**

1. Przejdź do [portalu Azure](https://portal.azure.com).
2. Kliknij przycisk **subskrypcje**, subskrypcji, a następnie kliknij przycisk **dostawców zasobów**. 
3. Lista dostawców zasobów przed **Microsoft.Compute**, kliknij przycisk **zarejestrować**. Musi mieć rolę współautora lub właściciela subskrypcji można zarejestrować dostawcy zasobów.

Zobacz [dostawców zasobów i typów](../azure-resource-manager/resource-manager-supported-services.md) Aby uzyskać szczegółowe instrukcje.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać instrukcje krok po kroku można utworzyć fabryki danych w wersji 2 zobacz następujące samouczki:

- [Szybki Start: Rozpoczynanie pracy z Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Co to jest Azure Databricks?](what-is-azure-databricks.md)

