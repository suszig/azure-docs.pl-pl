---
title: "Interfejs API REST: operacje systemu plików w usłudze Azure Data Lake Store | Microsoft Docs"
description: "Korzystanie z interfejsów API REST WebHDFS w celu wykonywania operacji systemu plików w usłudze Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: a850b3fdff956abe41ac9a4af10a96dc119a75f4
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-rest-api"></a>Operacje systemu plików w usłudze Azure Data Lake Store przy użyciu interfejsu API REST
> [!div class="op_single_selector"]
> * [Zestaw SDK platformy .NET](data-lake-store-data-operations-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Z tego artykułu dowiesz się, jak używać interfejsów API REST WebHDFS i interfejsów API REST usługi Data Lake Store w celu wykonywania operacji systemu plików w usłudze Azure Data Lake Store. Aby uzyskać instrukcje na temat sposobu wykonywania operacji zarządzania kontem w usłudze Data Lake Store przy użyciu interfejsu API REST, zobacz [Account management operations on Data Lake Store using REST API (Operacje zarządzania kontem w usłudze Data Lake Store przy użyciu interfejsu API REST)](data-lake-store-get-started-rest-api.md).

## <a name="prerequisites"></a>Wymagania wstępne
* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Konto usługi Azure Data Lake Store**. Postępuj zgodnie z instrukcjami w temacie [Rozpoczynanie pracy z usługą Azure Data Lake Store za pomocą witryny Azure Portal](data-lake-store-get-started-portal.md).

* **[Adres cURL](http://curl.haxx.se/)**. W tym artykule jest używane narzędzie cURL w celu zademonstrowania sposobu wykonywania wywołań interfejsu API REST względem konta usługi Data Lake Store.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>W jaki sposób uwierzytelniać za pomocą usługi Azure Active Directory?
Dostępne są dwa podejścia do uwierzytelniania za pomocą usługi Azure Active Directory.

* Aby dowiedzieć się więcej o uwierzytelnianiu użytkowników końcowych w aplikacji (interaktywnym), zobacz [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-rest-api.md) (Uwierzytelnianie użytkowników końcowych w usłudze Data Lake Store za pomocą zestawu SDK platformy .NET).
* Aby dowiedzieć się więcej o uwierzytelnianiu „usługa-usługa” (nieinteraktywnym), zobacz [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md) (Uwierzytelnianie „usługa-usługa” w usłudze Data Lake Store za pomocą zestawu SDK platformy .NET).


## <a name="create-folders"></a>Tworzenie folderów
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

W poprzednim poleceniu zastąp ciąg \<`REDACTED`\> tokenem autoryzacji pobranym wcześniej. To polecenie tworzy katalog o nazwie **mytempdir** w folderze głównym Twojego konta usługi Data Lake Store.

Jeśli operacja zakończy się pomyślnie, powinna pojawić się odpowiedź podobna do poniższego fragmentu kodu:

    {"boolean":true}

## <a name="list-folders"></a>Wyświetlanie listy folderów
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

W poprzednim poleceniu zastąp ciąg \<`REDACTED`\> tokenem autoryzacji pobranym wcześniej.

Jeśli operacja zakończy się pomyślnie, powinna pojawić się odpowiedź podobna do poniższego fragmentu kodu:

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
            "owner": "<GUID>",
            "group": "<GUID>"
        }]
    }
    }

## <a name="upload-data"></a>Przekazywanie danych
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

W poprzedniej składni parametr **-T** wskazuje na lokalizację przekazywanego pliku.

Dane wyjściowe będą podobne do następującego fragmentu kodu:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data"></a>Odczyt danych
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Odczytywanie danych z konta usługi Data Lake Store jest procesem dwuetapowym.

* Najpierw prześlij żądanie GET względem punktu końcowego `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. To wywołanie zwraca lokalizację, do której należy przesłać kolejne żądanie GET.
* Następnie prześlij żądanie GET względem punktu końcowego `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. To wywołanie powoduje wyświetlenie zawartości pliku.

Jednak ponieważ nie ma żadnej różnicy w parametrach wejściowych kroku pierwszego i drugiego, możesz użyć parametru `-L` w celu przesłania pierwszego żądania. `-L` — ta opcja w praktyce łączy dwa żądania w jedno i powoduje, że narzędzie cURL ponawia żądanie dla nowej lokalizacji. Na koniec zostaną wyświetlone dane wyjściowe z wszystkich wywołań żądań, tak jak pokazano w poniższym fragmencie kodu. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Powinny pojawić się dane wyjściowe podobne do następującego fragmentu kodu:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file"></a>Zmienianie nazwy pliku
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Użyj następującego polecenia cURL, aby zmienić nazwę pliku. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Powinny pojawić się dane wyjściowe podobne do następującego fragmentu kodu:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file"></a>Usuwanie pliku
Ta operacja jest oparta na wywołaniu interfejsu API REST WebHDFS zdefiniowanym [tutaj](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Użyj następującego polecenia cURL, aby usunąć plik. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Powinny pojawić się dane wyjściowe podobne do następujących:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="next-steps"></a>Następne kroki
* [Operacje zarządzania kontem w usłudze Data Lake Store przy użyciu interfejsu API REST](data-lake-store-get-started-rest-api.md).

## <a name="see-also"></a>Zobacz też
* [Dokumentacja interfejsu API REST usługi Azure Data Lake Store](https://docs.microsoft.com/rest/api/datalakestore/)
* [Open Source Big Data applications compatible with Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md) (Aplikacje danych big data typu open source zgodne z usługą Azure Data Lake Store)

