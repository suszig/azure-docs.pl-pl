---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu zestawu SDK języka Python | Microsoft Docs"
description: "Dowiedz się, jak używać zestawu SDK języka Python do pracy z kontami usługi Data Lake Store i systemem plików."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 375a603360ac249fc1b08923a94c85652390a3fc
ms.contentlocale: pl-pl
ms.lasthandoff: 07/01/2017


---

<a id="get-started-with-azure-data-lake-store-using-python" class="xliff"></a>

# Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu języka Python

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Dowiedz się, jak używać zestawu SDK języka Python dla usługi Azure Data Lake Store, aby wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych itp. Aby uzyskać więcej informacji o usłudze Data Lake, zobacz temat [Usługa Azure Data Lake Store](data-lake-store-overview.md).

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

* **Python**. Możesz pobrać środowisko Python [tutaj](https://www.python.org/downloads/). W tym artykule używany jest język Python 3.5.2.

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Utworzenie aplikacji usługi Azure Active Directory**. Za pomocą aplikacji usługi Azure AD można uwierzytelnić aplikację usługi Data Lake Store w usłudze Azure AD. Istnieją różne metody uwierzytelniania w usłudze Azure AD: **uwierzytelnianie użytkowników końcowych** i **uwierzytelnianie między usługami**. Aby uzyskać instrukcje i więcej informacji na temat uwierzytelniania, zobacz [Uwierzytelnianie użytkowników końcowych](data-lake-store-end-user-authenticate-using-active-directory.md) lub [Uwierzytelnianie między usługami](data-lake-store-authenticate-using-active-directory.md).

<a id="install-the-modules" class="xliff"></a>

## Instalacja modułów

Aby móc pracować z usługą Data Lake Store przy użyciu języka Python, musisz zainstalować trzy moduły.

* Moduł `azure-mgmt-resource`. Obejmuje to moduły platformy Azure dla usługi Active Directory itp.
* Moduł `azure-mgmt-datalake-store`. Obejmuje to operacje zarządzania kontem usługi Azure Data Lake Store. Aby uzyskać więcej informacji na temat tego modułu, zobacz [dokumentację modułu do zarządzania dla usługi Azure Data Lake Store](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).
* Moduł `azure-datalake-store`. Obejmuje to operacje na systemie plików usługi Azure Data Lake Store. Aby uzyskać więcej informacji na temat tego modułu, zobacz [dokumentację modułu systemu plików dla usługi Azure Data Lake Store](http://azure-datalake-store.readthedocs.io/en/latest/).

Użyj następujących poleceń, aby zainstalować moduły.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

<a id="create-a-new-python-application" class="xliff"></a>

## Tworzenie nowej aplikacji w języku Python

1. W wybranym środowisku IDE utwórz nową aplikację w języku Python, na przykład **mysample.py**.

2. Dodaj następujące wiersze, aby zaimportować wymagane moduły

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Zapisz zmiany w aplikacji mysample.py.

<a id="authentication" class="xliff"></a>

## Uwierzytelnianie

W tej sekcji omówione zostaną różne sposoby uwierzytelniania w usłudze Azure AD. Dostępne opcje:

* Uwierzytelnianie użytkowników końcowych
* Uwierzytelnianie między usługami
* Uwierzytelnianie wieloskładnikowe

Te opcje uwierzytelnianie muszą być używane dla modułów zarządzania kontami i zarządzania systemem plików.

<a id="end-user-authentication-for-account-management" class="xliff"></a>

### Uwierzytelnianie użytkowników końcowych w celu zarządzania kontami

Użyj tej metody, aby uwierzytelnić się za pomocą usługi Azure AD w celu wykonywania operacji zarządzania kontem (tworzenie/usuwanie konta usługi Data Lake Store itp.). Należy podać nazwę i hasło użytkownika usługi Azure AD. Należy pamiętać, że użytkownik nie powinien być skonfigurowany do uwierzytelniania wieloskładnikowego.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

<a id="end-user-authentication-for-filesystem-operations" class="xliff"></a>

### Uwierzytelnianie użytkowników końcowych dla operacji systemu plików

Użyj tej metody, aby uwierzytelniać przy użyciu usługi Azure AD w przypadku operacji systemu plików (tworzenie folderu, przekazywanie pliku itp.). Użyj tej metody wraz z istniejącą natywną aplikacją **kliencką** usługi Azure AD. Użytkownik usługi Azure AD, dla którego podajesz poświadczenia, nie powinien być skonfigurowany do uwierzytelniania wieloskładnikowego.

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

<a id="service-to-service-authentication-with-client-secret-for-account-management" class="xliff"></a>

### Uwierzytelnianie między usługami z kluczem tajnym klienta w celu zarządzania kontami

Użyj tej metody, aby uwierzytelnić się za pomocą usługi Azure AD w celu wykonywania operacji zarządzania kontem (tworzenie/usuwanie konta usługi Data Lake Store itp.). Poniższego fragmentu kodu można użyć do uwierzytelniania aplikacji w sposób nieinterakcyjny za pomocą klucza tajnego klienta aplikacji / nazwy głównej usługi. Użyj tej metody wraz z istniejącą aplikacją sieci Web usługi Azure AD.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

<a id="service-to-service-authentication-with-client-secret-for-filesystem-operations" class="xliff"></a>

### Uwierzytelnianie między usługami z wpisem tajnym klienta dla operacji systemu plików

Użyj tej metody, aby uwierzytelniać przy użyciu usługi Azure AD w przypadku operacji systemu plików (tworzenie folderu, przekazywanie pliku itp.). Poniższego fragmentu kodu można użyć do uwierzytelniania aplikacji w sposób nieinterakcyjny za pomocą klucza tajnego klienta aplikacji / nazwy głównej usługi. Użyj tej metody wraz z istniejącą aplikacją sieci Web usługi Azure AD.

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

<a id="multi-factor-authentication-for-account-management" class="xliff"></a>

### Uwierzytelnianie wieloskładnikowe w celu zarządzania kontami

Użyj tej metody, aby uwierzytelnić się za pomocą usługi Azure AD w celu wykonywania operacji zarządzania kontem (tworzenie/usuwanie konta usługi Data Lake Store itp.). Poniższego fragmentu kodu można użyć do uwierzytelniania aplikacji za pomocą uwierzytelniania wieloskładnikowego. Użyj tej metody wraz z istniejącą aplikacją sieci Web usługi Azure AD.

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    credentials = AADTokenCredentials(mgmt_token, client_id)

<a id="multi-factor-authentication-for-filesystem-management" class="xliff"></a>

### Uwierzytelnianie wieloskładnikowe dla zarządzania systemem plików

Użyj tej metody, aby uwierzytelniać przy użyciu usługi Azure AD w przypadku operacji systemu plików (tworzenie folderu, przekazywanie pliku itp.). Poniższego fragmentu kodu można użyć do uwierzytelniania aplikacji za pomocą uwierzytelniania wieloskładnikowego. Użyj tej metody wraz z istniejącą aplikacją sieci Web usługi Azure AD.

    token = lib.auth(tenant_id='FILL-IN-HERE')

<a id="create-an-azure-resource-group" class="xliff"></a>

## Tworzenie grupy zasobów platformy Azure

Aby utworzyć grupę zasobów platformy Azure, użyj poniższego fragmentu kodu:

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

<a id="create-clients-and-data-lake-store-account" class="xliff"></a>

## Tworzenie klientów i konta usługi Data Lake Store

Poniższy fragment kodu najpierw tworzy klienta konta usługi Data Lake Store. Używa on obiektu klienta do utworzenia konta usługi Data Lake Store. Na koniec fragment kodu tworzy obiekt klienta systemu plików.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

<a id="list-the-data-lake-store-accounts" class="xliff"></a>

## Wyświetlanie listy kont usługi Data Lake Store

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

<a id="create-a-directory" class="xliff"></a>

## Tworzenie katalogu

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

<a id="upload-a-file" class="xliff"></a>

## Przekazywanie pliku


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


<a id="download-a-file" class="xliff"></a>

## Pobieranie pliku

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

<a id="delete-a-directory" class="xliff"></a>

## Usuwanie katalogu

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

<a id="see-also" class="xliff"></a>

## Zobacz też

- [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
- [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Dokumentacja zestawu SDK .NET usługi Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx)
- [Dokumentacja interfejsu REST usługi Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx)

