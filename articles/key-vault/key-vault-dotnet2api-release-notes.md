---
title: Klucz magazynu .NET 2.x API informacje o wersji | Dokumentacja firmy Microsoft
description: ".NET umożliwia deweloperom ten interfejs API do kodu dla usługi Azure Key Vault"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.openlocfilehash: c5b5fd7f16faf17d16ecc82269fb1264adf4dd06
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Usługi Azure Key Vault .NET 2.0 — przewodnik migracji i informacje o wersji
Poniższe informacje i wskazówki są dla deweloperów korzystających z platformy .NET magazynu kluczy Azure / biblioteki C#. W ramach tego przejścia z wersji 1.0 w wersji 2.0, liczba aktualizacji wprowadzono ten będzie wymagać pracy migracji w kodzie, aby korzystać z ulepszeń funkcjonalności i funkcji dodatków, takich jak **certyfikaty usługi Key Vault** obsługuje.

## <a name="key-vault-certificates"></a>Magazyn kluczy certyfikatów

Obsługa certyfikatów usługi Key Vault umożliwia zarządzanie x509 Twojego certyfikaty i następujące zachowania:  

* Umożliwia właścicielowi certyfikatu utworzyć certyfikat za pośrednictwem procesu tworzenia usługi Key Vault lub importowania istniejącego certyfikatu. Dotyczy to zarówno z podpisem własnym, a urząd certyfikacji generowane certyfikatów.
* Umożliwia właścicielowi certyfikatu usługi Key Vault, do implementowania bezpiecznego magazynu i zarządzania X509 certyfikatów bez interakcji z materiału klucza prywatnego.  
* Umożliwia właścicielowi certyfikatu utworzyć zasadę, która określa, że magazyn kluczy, aby zarządzać cyklu życia certyfikatu.  
* Umożliwia właścicieli certyfikatu, podaj informacje kontaktowe dla powiadomień dotyczących zdarzeń cyklu życia wygaśnięcia i odnawiania certyfikatów.  
* Obsługuje automatycznego odnawiania z wystawców wybranych - Key Vault partnera X509 certyfikatu dostawców / urzędy certyfikacji.
  
  * Uwaga —-współpracę dostawców/urzędów również są dozwolone, ale nie będzie obsługiwać funkcję automatycznego odnawiania.

## <a name="net-support"></a>Obsługa .NET

* **.NET 4.0** nie jest obsługiwana w wersji 2.0 programu .NET usługi Azure Key Vault / biblioteki C#
* **Oprogramowanie .NET core** jest obsługiwana w wersji 2.0 programu .NET usługi Azure Key Vault / biblioteki C#

## <a name="namespaces"></a>Przestrzenie nazw

* W obszarze nazw **modele** została zmieniona z **Microsoft.Azure.KeyVault** do **Microsoft.Azure.KeyVault.Models**.
* **Microsoft.Azure.KeyVault.Internal** przestrzeni nazw jest porzucany.
* Przestrzeń nazw zależności zestawu Azure SDK nie zostaną zmienione **Hyak.Common** i **Hyak.Common.Internals** do **Microsoft.Rest** i **Microsoft.Rest.Serialization**

## <a name="type-changes"></a>Zmiana typu

* *Klucz tajny* zmieniony na *SecretBundle*
* *Słownik* zmieniony na *IDictionary*
* *Lista<T>, string []* zmieniony na *IList<T>*
* *NextList* zmieniony na *NextPageLink*

## <a name="return-types"></a>Zwracane typy

* **KeyList** i **SecretList** zwróci *IPage<T>*  zamiast *ListKeysResponseMessage*
* Wygenerowany **BackupKeyAsync** zwróci *BackupKeyResult* zawierającą *wartość* (kopie zapasowe obiektów blob). Przed metoda została opakowana i zwracanie tylko wartości.

## <a name="exceptions"></a>Wyjątki

* *KeyVaultClientException* jest zmieniana na *KeyVaultErrorException*
* Błąd usługi została zmieniona z *wyjątku. Błąd* do *wyjątku. Body.Error.Message*.
* Usunięte informacje dodatkowe z komunikat o błędzie dla **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktory

* Zamiast akceptowanie *HttpClient* jako argument konstruktora akceptuje tylko konstruktora *HttpClientHandler* lub *DelegatingHandler []*.

## <a name="downloaded-packages"></a>Pobranych pakietów

Gdy klient przetwarza zależności usługi Key Vault poniżej zostały pobrane

### <a name="previous-package-list"></a>Poprzedniej liście pakietu

* Wersja pakietu id="Hyak.Common" = "1.0.2" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Azure.Common" = "2.0.4" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Azure.Common.Dependencies" = "1.0.0" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Azure.KeyVault" = "1.0.0" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Bcl" = "1.1.9" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Bcl.Async" = "1.0.168" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Bcl.Build" = "1.0.14" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Net.Http" = "2.2.22" targetFramework = "net45"

### <a name="current-package-list"></a>Bieżąca lista pakietów

* Wersja pakietu id="Microsoft.Azure.KeyVault" = "2.0.0-preview" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Rest.ClientRuntime" = "2.2.0" targetFramework = "net45"
* Wersja pakietu id="Microsoft.Rest.ClientRuntime.Azure" = "3.2.0" targetFramework = "net45"

## <a name="class-changes"></a>Klasa zmiany

* **UnixEpoch** klasy został usunięty.
* **Base64UrlConverter** klasy jest zmieniana na **Base64UrlJsonConverter**

## <a name="other-changes"></a>Inne zmiany

* Dodano obsługę dla konfiguracji zasady ponawiania operacji KV na przejściowych błędów do tej wersji interfejsu API.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Dla operacji, które zwrócił *magazynu*, zwracany typ został klasę, która zawiera właściwość magazynu. Typ zwracany jest teraz *magazynu*.
* *PermissionsToKeys* i *PermissionsToSecrets* są teraz *Permissions.Keys* i *Permissions.Secrets*
* Niektóre typy zwracane zmiany dotyczą płaszczyzny sterowania również.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Pakiet został przerwany do **Microsoft.Azure.KeyVault.Extensions** i **Microsoft.Azure.KeyVault.Cryptography** dla operacji kryptograficznych.

