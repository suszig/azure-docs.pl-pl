---
ms.assetid: 
title: "Klucze konta magazynu usługi Azure Key Vault"
description: "Klucze konta magazynu przedstawić seemless integrację usługi Azure Key Vault i klucza dostępu na podstawie konta magazynu Azure."
ms.topic: article
services: key-vault
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: 6ebac5fc90e259b19e0a4103a732754384232a44
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-storage-account-keys"></a>Klucze konta magazynu usługi Azure Key Vault

Przed magazynu kluczy konta Azure klucz magazynu deweloperzy mają zarządzać własne klucze konta magazynu Azure (ASA) i Obróć je ręcznie lub za pomocą narzędzie automatyzacji. Teraz, klucze konta magazynu kluczy w magazynie są zaimplementowane jako [kluczy tajnych Key Vault](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) do uwierzytelniania przy użyciu konta magazynu Azure. 

Funkcja klucza konta magazynu Azure (ASA) zarządza tajny obrotu. Usuwa również potrzebę bezpośredniego kontaktu z kluczem ASA oferując dostępu sygnatur dostępu Współdzielonego jako metody. 

Aby uzyskać więcej ogólnych informacji o kontach magazynu Azure, zobacz [kont magazynu Azure o](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Obsługa interfejsów

Pełna lista i łącza do naszych interfejsów programowania i obsługi skryptów w znajdziesz [klucza magazynu przewodnik dewelopera](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>Zarządza Magazyn kluczy

Magazyn kluczy wykonuje wiele funkcji zarządzania wewnętrznego w Twoim imieniu używania zarządzanych kluczy konta magazynu.

- Usługa Azure Key Vault zarządza kluczy dla konta magazynu Azure (ASA).
    - Wewnętrznie usługi Azure Key Vault można wyświetlić listę kluczy (synchronizacja) z kontem magazynu platformy Azure.  
    - Usługa Azure Key Vault generuje (obraca) okresowo kluczy. 
    - Wartości klucza nigdy nie są zwracane w odpowiedzi do wywołującego. 
    - Usługa Azure Key Vault zarządza klucze konta magazynu i klasycznych kont magazynu. 
- Usługa Azure Key Vault pozwala, właściciel magazynu/obiektu, do tworzenia definicji SAS (konta lub sygnatury dostępu Współdzielonego usługi).
    - Wartość SAS, utworzone za pomocą definicji SAS, jest zwracana jako klucza tajnego za pośrednictwem ścieżka identyfikatora URI REST. Aby uzyskać więcej informacji, zobacz [operacje konta magazynu usługi Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

## <a name="naming-guidance"></a>Wskazówki dotyczące nazewnictwa

- Nazwy kont usługi Magazyn muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery.  
- Nazwa definicji SAS musi być 1 102 znaków długości i zawierać tylko 0-9, a-z, A-Z.

## <a name="developer-experience"></a>Środowisko dewelopera

### <a name="before-azure-key-vault-storage-keys"></a>Przed usługi Azure Key Vault magazynu kluczy 

Deweloperzy umożliwia należy wykonać następujące rozwiązania kluczem konta magazynu można uzyskać dostępu do magazynu Azure. 
 
```powershell
//create an Azure Storage Account using a connection string containing an account name and a storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Po usługi Azure Key Vault magazynu kluczy 

```powershell
//Make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Get secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  

-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get a SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If your SAS token is about to expire, Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Wskazówki dla deweloperów

- Zezwalaj tylko na Key Vault w celu zarządzanie kluczami ASA. Nie należy próbować Zarządzanie samodzielnie, będzie zakłócać procesów magazynu kluczy. 
- Nie zezwalaj ASA klucze mają być zarządzane przez więcej niż jeden obiekt magazynu kluczy. 
- Jeśli musisz ręcznie wygenerować ponownie klucze ASA, zaleca się Generuj je za pomocą usługi Key Vault. 

## <a name="getting-started"></a>Wprowadzenie

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Ustawienia dla uprawnień kontroli dostępu opartej na rolach

Tożsamość aplikacji usługi Azure Key Vault wymaga uprawnień do *listy* i *ponownie wygenerować* kluczy dla konta magazynu. Skonfiguruj te uprawnienia, wykonując następujące czynności:

- Pobrania ObjectId tożsamości usługi Azure Key Vault: 

    `Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093`

- Przypisz roli operatora klucza magazynu Azure klucza magazynu tożsamości: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Dla typu konta klasycznego, ustaw dla parametru roli *"Classic magazynu konta klucza usługi roli operatora."*

## <a name="working-example"></a>Przykład pracy

W poniższym przykładzie pokazano tworzenie Key Vault zarządzanych konto magazynu Azure i powiązane definicje dostępu sygnatury dostępu Współdzielonego.

### <a name="assumptions"></a>Wartości domyślne

Poniższe instrukcje są givens w ramach tego przykładu pracy.

- Zasobu magazynu znajduje się pod adresem: */subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1*

- Nazwa magazynu kluczy: *yourtest1*

### <a name="get-a-service-principal"></a>Pobierz nazwy głównej usługi

```powershell
$yourKeyVaultServicePrincipalId = (Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Dane wyjściowe poprzedniego polecenia będzie zawierać użytkownika ServicePrincipal, który Zadzwonimy *yourKeyVaultServicePrincipalId*. 

### <a name="set-permissions"></a>Ustaw uprawnienia

Upewnij się, że masz uprawnienia magazynu ustawioną *wszystkich*. Możesz pobrać youruserPrincipalId i ustalanie uprawnień dla magazynu za pomocą następujących poleceń.

```powershell
$youruserPrincipalId = (Get-AzureRmADUser -SearchString "your user principal name").Id
```
Teraz wyszukiwania dla nazwy i pobrania ObjectId powiązane, który będzie używany podczas ustawiania uprawnień w magazynie.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourtest1' -ObjectId $youruserPrincipalId -PermissionsToStorage all
```

### <a name="allow-access"></a>Zezwól na dostęp

Należy zapewnić dostęp do usługi Key Vault do kont magazynu, przed utworzeniem konta magazynu zarządzanego i definicje sygnatury dostępu Współdzielonego.

```powershell
New-AzureRmRoleAssignment -ObjectId $yourKeyVaultServicePrincipalId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '/subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1'
```

### <a name="create-storage-account"></a>Tworzenie konta magazynu

Teraz utworzyć konta magazynu zarządzanego i dwie definicje sygnatury dostępu Współdzielonego. Konto sygnatury dostępu Współdzielonego zapewnia dostęp do usługi blob z innymi uprawnieniami.

```powershell
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="regeneration"></a>Ponowne generowanie

Ustawienie okresu ponowne generowanie za pomocą następujących poleceń.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Definicje SAS zestawu

Ustaw definicje sygnatury dostępu Współdzielonego w magazynie kluczy konta magazynu zarządzanego.

```powershell
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourtest1  -AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName yourtest1  -AccountName msak01 -Name blobsas2 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List,Write
```

### <a name="get-token"></a>Pobierz token

Pobierz odpowiedni tokeny sygnatury dostępu Współdzielonego i wykonywania wywołań do magazynu.

```powershell
$sasToken1 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas1).SecretValueText
$sasToken2 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas2).SecretValueText
```

### <a name="create-storage"></a>Tworzenie magazynu

Zwróć uwagę, że próbujesz uzyskać dostęp z *$sastoken1* nie powiedzie się, ale ten można uzyskać dostęp z *$sastoken2*. 

```powershell
$context1 = New-AzureStorageContext -SasToken $sasToken1 -StorageAccountName yourtest1
$context2 = New-AzureStorageContext -SasToken $sasToken2 -StorageAccountName yourtest1
Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt"  -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt"  -Context $context2
```

### <a name="example-summary"></a>Przykład podsumowania

Jesteś stanie dostępu do obiektu blob magazynu zawartości z tokenu sygnatury dostępu Współdzielonego, który ma dostęp do zapisu.

### <a name="relevant-powershell-cmdlets"></a>Odpowiednich poleceń cmdlet programu Powershell

- [Get-AzureKeyVaultManagedStorageAccount ](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount?view=azurermps-4.3.1)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)

## <a name="storage-account-onboarding"></a>Dołączanie konta magazynu 

Przykład: jako właściciel obiektu usługi Key Vault dodawany obiekt konta magazynu do użytkownika usługa Azure Key Vault dołączeniu konto magazynu.

Podczas dołączania, Key Vault musi sprawdzić, czy tożsamość konta dołączania ma uprawnienia do *listy* i *ponownie wygenerować* magazynu kluczy. Aby można było zweryfikować te uprawnienia, Key Vault pobiera OBO (imieniu z) tokenu z usługi uwierzytelniania, odbiorców, ustaw do usługi Azure Resource Manager i sprawia, że *listy* klucza wywołanie usługi Magazyn Azure. Jeśli *listy* wywołanie zakończy się niepowodzeniem, Key Vault Tworzenie obiektu zakończy się niepowodzeniem z kodem stanu HTTP *zabroniony*. Klucze wymienione w ten sposób są buforowane z magazynem jednostek magazynu kluczy. 

Magazyn kluczy należy sprawdzić, czy tożsamość *ponownie wygenerować* uprawnienia przed jego przejąć prawo własności ponowne generowanie kluczy. Aby sprawdzić, czy tożsamość, za pomocą tokenu OBO, jak również tożsamości firm pierwszej usługi Key Vault ma tych uprawnień:

- Magazyn kluczy wymieniono uprawnienia RBAC zasobu konta magazynu.
- Magazyn kluczy sprawdza poprawność odpowiedzi, używając wyrażenia regularnego dopasowanie działania i z systemem innym niż działania. 

Znajdź przykłady obsługi na [magazyn kluczy - zarządzane przykłady klucze konta magazynu](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Jeśli nie ma tożsamość *ponownie wygenerować* uprawnienia lub jeśli nie ma usługi Key Vault pierwszej strony tożsamości *listy* lub *ponownie wygenerować* uprawnienia, a następnie procesu dołączania żądanie nie powiedzie się, zwracając kod błędu odpowiednie i wiadomości. 

OBO token działa tylko, gdy używasz firmy, aplikacje klienckie natywnego programu PowerShell lub interfejsu wiersza polecenia.

## <a name="other-applications"></a>Inne aplikacje

- Tokeny sygnatury dostępu Współdzielonego, tworzony przy użyciu kluczy konta magazynu usługi Key Vault, zapewniają bardziej kontroli dostępu do konta magazynu platformy Azure. Aby uzyskać więcej informacji, zobacz [używanie sygnatury dostępu współdzielonego](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Zobacz także

- [Informacje o kluczach, wpisach tajnych i certyfikatach](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog zespołu magazyn kluczy](https://blogs.technet.microsoft.com/kv/)
