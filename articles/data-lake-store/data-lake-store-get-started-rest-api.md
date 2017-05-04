---
title: "Korzystanie z interfejsu API REST przy rozpoczynaniu pracy z usługą Data Lake Store | Microsoft Docs"
description: "Korzystanie z interfejsów API REST WebHDFS w celu wykonywania operacji w usłudze Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: de04bf367f9f9f92756202cf6c1571f811a0f1f7
ms.lasthandoff: 04/22/2017


---
# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Rozpoczynanie pracy z usługą Azure Data Lake Store z użyciem interfejsów API REST
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Z tego artykułu dowiesz się, jak używać interfejsów API REST WebHDFS i interfejsów API REST usługi Data Lake Store w celu zarządzania kontem oraz wykonywania operacji systemu plików w usłudze Azure Data Lake Store. Usługa Azure Data Lake Store ujawnia swoje interfejsy API REST w celu wykonywania operacji zarządzania kontem. Jednak usługa Data Lake Store jest zgodna z systemem plików HDFS i ekosystemem Hadoop, dlatego umożliwia korzystanie z interfejsów API REST WebHDFS w celu wykonywania operacji systemu plików.

> [!NOTE]
> Aby uzyskać szczegółowe informacje na temat obsługi interfejsu API REST przez usługę Data Lake Store, zobacz [Azure Data Lake Store REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx) (Dokumentacja interfejsu API REST usługi Azure Data Lake Store).
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Utworzenie aplikacji usługi Azure Active Directory**. Za pomocą aplikacji usługi Azure AD można uwierzytelnić aplikację usługi Data Lake Store w usłudze Azure AD. Istnieją różne metody uwierzytelniania w usłudze Azure AD: **uwierzytelnianie użytkowników końcowych** i **uwierzytelnianie między usługami**. Instrukcje i dodatkowe informacje na temat uwierzytelniania można znaleźć w artykule [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) (Uwierzytelnianie w usłudze Data Lake Store przy użyciu usługi Azure Active Directory).
* [cURL](http://curl.haxx.se/). W tym artykule jest używane narzędzie cURL w celu zademonstrowania sposobu wykonywania wywołań interfejsu API REST względem konta usługi Data Lake Store.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>W jaki sposób uwierzytelniać za pomocą usługi Azure Active Directory?
Dostępne są dwa podejścia do uwierzytelniania za pomocą usługi Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Uwierzytelnianie użytkowników końcowych (interakcyjne)
W tym scenariuszu aplikacja wyświetla monit o zalogowanie się i wówczas wszystkie operacje są wykonywane w kontekście zalogowanego użytkownika. Wykonaj następujące kroki, aby przeprowadzić uwierzytelnianie interakcyjne.

1. Za pomocą aplikacji przekieruj użytkownika pod następujący adres URL:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > Identyfikator \<REDIRECT-URI> musi być zakodowany na potrzeby adresu URL. Dlatego w celu zapisania adresu https://localhost użyj ciągu `https%3A%2F%2Flocalhost`
   > 
   > 
   
    Na potrzeby tego samouczka możesz zastąpić symbole zastępcze w powyższym adresie URL i wkleić go w pasku adresu przeglądarki sieci web. Nastąpi przekierowanie w celu uwierzytelniania przy użyciu identyfikatora logowania do platformy Azure. Gdy pomyślnie się zalogujesz, odpowiedź zostanie wyświetlona na pasku adresu przeglądarki. Odpowiedź będzie miała następujący format:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Przechwyć kod autoryzacji z odpowiedzi. W ramach niniejszego samouczka możesz skopiować kod autoryzacji z paska adresu przeglądarki sieci web i przekazać go w żądaniu POST do punktu końcowego tokenu w sposób przedstawiony poniżej:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > W takim przypadku identyfikatora \<REDIRECT-URI> nie trzeba kodować.
   > 
   > 
3. Odpowiedzią jest obiekt JSON, który zawiera token dostępu (np. `"access_token": "<ACCESS_TOKEN>"`) oraz token odświeżania (np. `"refresh_token": "<REFRESH_TOKEN>"`). Aplikacja używa tokenu dostępu podczas uzyskiwania dostępu do usługi Azure Data Lake Store i tokenu odświeżania, aby uzyskać inny token dostępu w przypadku wygaśnięcia tokenu dostępu.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Po wygaśnięciu tokenu dostępu możesz zażądać nowego tokenu dostępu, używając tokenu odświeżania w sposób przedstawiony poniżej:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Więcej informacji na temat interakcyjnego uwierzytelniania użytkownika zawiera temat [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Przepływ udzielania kodu autoryzacji).

### <a name="service-to-service-authentication-non-interactive"></a>Uwierzytelnianie między usługami (nieinterakcyjne)
W tym scenariuszu aplikacja udostępnia własne poświadczenia, aby wykonywać operacje. W tym celu należy wygenerować żądanie POST, tak jak pokazano poniżej. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Dane wyjściowe tego żądania będą zawierać token autoryzacji (oznaczony jako `access-token` w danych wyjściowych poniżej), który następnie będzie przekazywany w wywołaniach interfejsu API REST. Zapisz ten token uwierzytelniania w pliku tekstowym. Będzie on potrzebny w dalszej części tego artykułu.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

W tym artykule wykorzystano podejście **nieinterakcyjne**. Więcej informacji na temat podejścia nieinterakcyjnego (wywołań między usługami) zawiera temat [Service to service calls using credentials](https://msdn.microsoft.com/library/azure/dn645543.aspx) (Wywołania między usługami przy użyciu poświadczeń).

## <a name="create-a-data-lake-store-account"></a>Tworzenie konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST zdefiniowanym [tutaj](https://msdn.microsoft.com/library/mt694078.aspx).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

W poleceniu powyżej zastąp ciąg \<`REDACTED`\> tokenem autoryzacji pobranym wcześniej. Ładunek żądania dla tego polecenia jest zawarty w pliku **input.json**, który jest udostępniany dla parametru `-d` powyżej. Zawartość pliku input.json powinna wyglądać w następujący sposób:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="create-folders-in-a-data-lake-store-account"></a>Tworzenie folderów w ramach konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

W poleceniu powyżej zastąp ciąg \<`REDACTED`\> tokenem autoryzacji pobranym wcześniej. To polecenie tworzy katalog o nazwie **mytempdir** w folderze głównym Twojego konta usługi Data Lake Store.

Jeśli operacja zakończy się pomyślnie, powinna pojawić się odpowiedź podobna do poniższej:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Wyświetlanie listy folderów w ramach konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

W poleceniu powyżej zastąp ciąg \<`REDACTED`\> tokenem autoryzacji pobranym wcześniej.

Jeśli operacja zakończy się pomyślnie, powinna pojawić się odpowiedź podobna do poniższej:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Przekazywanie danych do konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

W powyższej składni parametr **-T** wskazuje na lokalizację przekazywanego pliku.

Dane wyjściowe będą podobne do następujących:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data-from-a-data-lake-store-account"></a>Odczytywanie danych z konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Odczytywanie danych z konta usługi Data Lake Store jest procesem dwuetapowym.

* Najpierw prześlij żądanie GET względem punktu końcowego `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. To spowoduje zwrócenie lokalizacji, do której należy przesłać kolejne żądanie GET.
* Następnie prześlij żądanie GET względem punktu końcowego `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Zostanie wyświetlona zawartość pliku.

Jednak ponieważ nie ma żadnej różnicy w parametrach wejściowych kroku pierwszego i drugiego, możesz użyć parametru `-L` w celu przesłania pierwszego żądania. `-L` — ta opcja w praktyce łączy dwa żądania w jedno i powoduje, że narzędzie cURL ponowi żądanie dla nowej lokalizacji. Na koniec zostaną wyświetlone dane wyjściowe z wszystkich wywołań żądań, tak jak pokazano poniżej. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Powinny pojawić się dane wyjściowe podobne do następujących:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Zmiana nazwy pliku w ramach konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Użyj następującego polecenia cURL, aby zmienić nazwę pliku. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Powinny pojawić się dane wyjściowe podobne do następujących:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Usuwanie pliku z konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Użyj następującego polecenia cURL, aby usunąć plik. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Powinny pojawić się dane wyjściowe podobne do następujących:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Usuwanie konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST zdefiniowanym [tutaj](https://msdn.microsoft.com/library/mt694075.aspx).

Użyj poniższego polecenia cURL, aby usunąć konto usługi Data Lake Store. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Powinny pojawić się dane wyjściowe podobne do następujących:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Zobacz też
* [Open Source Big Data applications compatible with Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md) (Aplikacje danych big data typu open source zgodne z usługą Azure Data Lake Store)


