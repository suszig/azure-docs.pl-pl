---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu języka Python | Microsoft Docs"
description: "Dowiedz się, jak utworzyć konto usługi Data Lake Store i przesyłać zadania za pomocą języka Python. "
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 5618650671badfc54860c3ad8af5d1e727d3d8c9
ms.openlocfilehash: 40ccfc59cccd86a7634ec89656571b3cd23566b4
ms.lasthandoff: 11/23/2016


---


# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-python"></a>Samouczek: Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu języka Python
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Dowiedz się, jak używać języka Python do tworzenia kont usługi Azure Data Lake Analytics, definiowania zadań usługi Data Lake Analytics w języku [U-SQL](data-lake-analytics-u-sql-get-started.md) i przesyłania zadań do kont usługi Data Lake Analytics. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).

W ramach tego samouczka utworzysz zadanie, które odczytuje zawartość pliku z wartościami rozdzielanymi tabulatorami (TSV) i konwertuje ją na plik z wartościami rozdzielanymi przecinkami (CSV). Aby wykonać kroki opisane w tym samouczku, korzystając z innych obsługiwanych narzędzi, kliknij odpowiednią kartę w górnej części tej sekcji.

##<a name="prerequisites"></a>Wymagania wstępne

Przed przystąpieniem do wykonywania kroków opisanych w tym samouczku musisz mieć poniższe:

- **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Aplikacja usługi Azure Active Directory**. Za pomocą aplikacji usługi Azure AD można uwierzytelnić aplikację usługi Data Lake Store w usłudze Azure AD. Istnieją różne metody uwierzytelniania w usłudze Azure AD: uwierzytelnianie użytkowników końcowych i uwierzytelnianie między usługami. Instrukcje i dodatkowe informacje na temat uwierzytelniania można znaleźć w artykule [Authenticate with Data Lake Store using Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md) (Uwierzytelnianie w usłudze Data Lake Store przy użyciu usługi Azure Active Directory).
- **[Python](https://www.python.org/downloads/)**. Należy użyć wersji 64-bitowej. W tym artykule używany jest język Python wersji 3.5.2.


## <a name="install-azure-python-sdk"></a>Instalowanie zestawu Azure Python SDK

Aby móc pracować z usługą Data Lake Store przy użyciu języka Python, musisz zainstalować trzy moduły.

Moduł azure-mgmt-datalake-store obejmuje operacje zarządzania kontem usługi Azure Data Lake Store. Moduł azure-mgmt-resource obejmuje inne moduły platformy Azure dla usługi Active Directory itd. Moduł azure-datalake-store obejmuje operacje systemu plików usługi Azure Data Lake Store. Moduł azure-datalake-analytics obejmuje operacje usługi Azure Data Lake Analytics. Użyj następujących poleceń, aby zainstalować moduły.

    pip install azure-mgmt-resource
    pip install azure-mgmt-datalake-store
    pip install azure-mgmt-datalake-analytics
    pip install azure-datalake-store

## <a name="create-a-python-application"></a>Tworzenie aplikacji w języku Python

1. Użyj dowolnego środowiska IDE, aby utworzyć nową aplikację w języku Python, na przykład mysample.py.

2. Dodaj następujące wiersze, aby zaimportować wymagane moduły

        ## Use this only for Azure AD service-to-service authentication
        from azure.common.credentials import ServicePrincipalCredentials

        ## Use this only for Azure AD end-user authentication
        from azure.common.credentials import UserPassCredentials

        ## Required for Azure Resource Manager
        from azure.mgmt.resource.resources import ResourceManagementClient
        from azure.mgmt.resource.resources.models import ResourceGroup

        ## Required for Azure Data Lake Store account management
        from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
        from azure.mgmt.datalake.store.models import DataLakeStoreAccount

        ## Required for Azure Data Lake Store filesystem management
        from azure.datalake.store import core, lib, multithread

        ## Required for Azure Data Lake Analytics account management
        from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
        from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

        ## Required for Azure Data Lake Analytics job management
        from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
        from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

        ## Required for Azure Data Lake Analytics catalog management
        from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

        ## Use these as needed for your application
        import logging, getpass, pprint, uuid, time

3. Zapisz zmiany w pliku aplikacji Python.

## <a name="authentication"></a>Authentication

Użyj jednej z następujących metod uwierzytelniania:

### <a name="end-user-authentication-for-account-management"></a>Uwierzytelnianie użytkowników końcowych w celu zarządzania kontami

Użyj tej metody, aby uwierzytelnić się za pomocą usługi Azure AD w celu wykonywania operacji zarządzania kontem (tworzenie/usuwanie konta usługi Data Lake Store itp.). Należy podać nazwę i hasło użytkownika usługi Azure AD. Nie można skonfigurować konta użytkownika pod kątem uwierzytelniania wieloskładnikowego i nie może to być konto Microsoft/Live ID, na przykład @outlook.com, ani @live.com.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Uwierzytelnianie między usługami z kluczem tajnym klienta w celu zarządzania kontami

Użyj tej metody, aby uwierzytelnić się za pomocą usługi Azure AD w celu wykonywania operacji zarządzania kontem (tworzenie/usuwanie konta usługi Data Lake Store itp.). Poniższego fragmentu kodu można użyć do uwierzytelniania aplikacji w sposób nieinterakcyjny za pomocą klucza tajnego klienta aplikacji / nazwy głównej usługi. Użyj tej metody wraz z istniejącą aplikacją sieci Web usługi Azure AD.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

## <a name="create-resource-management-group"></a>Tworzenie grupy zarządzania zasobami

Aby utworzyć grupę zasobów platformy Azure, użyj poniższego fragmentu kodu:

    ## Declare variables
    subscriptionId= '<Azure Subscription ID>'
    resourceGroup = '<Azure Resource Group Name>'
    location = '<Location>' # i.e. 'eastus2'

    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )

    ## Create an Azure Resource Group
    armGroupResult = resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )


## <a name="create-data-lake-store-account"></a>Tworzenie konta usługi Data Lake Store

Każde konto usługi Data Lake Analytics wymaga posiadania konta w usłudze Data Lake Store. Aby uzyskać instrukcje, zobacz [Tworzenie konta usługi Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).



## <a name="create-data-lake-analytics-account"></a>Tworzenie konta usługi Data Lake Analytics

    ## Declare variables
    subscriptionId= '<Azure Subscription ID>'
    resourceGroup = '<Azure Resource Group Name>'
    location = '<Location>' # i.e. 'eastus2'
    adlsAccountName = '<Azure Data Lake Store Account Name>'
    adlaAccountName = '<Azure Data Lake Analytics Account Name>'

    ## Create management client object
    adlaAcctClient = DataLakeAnalyticsAccountManagementClient(
        credentials,
        subscriptionId
    )

    ## Create an Azure Data Lake Analytics account
    adlaAcctResult = adlaAcctClient.account.create(
        resourceGroup,
        adlaAccountName,
        DataLakeAnalyticsAccount(
            location=location,
            default_data_lake_store_account=adlsAccountName,
            data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adlsAccountName)]
        )
    ).wait()


##<a name="submit-data-lake-analytics-jobs"></a>Przesyłanie zadań usługi Data Lake Analytics

Zadania usługi Data Lake Analytics są napisane w języku U-SQL. Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

    ## Declare variables
    adlsAccountName = '<Azure Data Lake Store Account Name>'

    ## Create management client object
    adlaJobClient = DataLakeAnalyticsJobManagementClient(
        credentials,
        'azuredatalakeanalytics.net'
    )

    ## Submit a U-SQL job
    jobId = str(uuid.uuid4())
    jobResult = adlaJobClient.job.create(
        adlaAccountName,
        jobId,
        JobInformation(
            name='Sample Job',
            type='USql',
            properties=USqlJobProperties(
                script='DROP DATABASE IF EXISTS FOO; CREATE DATABASE FOO;'
            )
        )
    )

    ## Print the job result
    while(jobResult.state != JobState.ended):
        print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
        time.sleep(3)
        jobResult = adlaJobClient.job.get(adlaAccountName, jobId)
        
    print ('Job finished with result: ' + jobResult.result.value)

## <a name="next-steps"></a>Następne kroki

- Aby wyświetlić ten samouczek przy użyciu innych narzędzi, kliknij odpowiedni selektor karty w górnej części strony.
- Aby uzyskać informacje na temat bardziej złożonego zapytania, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Aby rozpocząć tworzenie aplikacji w języku U-SQL, zobacz artykuł [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
- Informacje o zadaniach zarządzania znajdziesz w artykule [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md).
- Aby zapoznać się z omówieniem usługi Data Lake Analytics, zobacz [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).


