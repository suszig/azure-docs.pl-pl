---
title: "Interfejs API REST: operacje zarządzania kontem w usłudze Azure Data Lake Store | Microsoft Docs"
description: "Użyj usługi Azure Data Lake Store i interfejsu API REST WebHDFS do wykonywania operacji zarządzania kontem w usłudze Data Lake Store"
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
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 6c43f2b341280731707e486ba6f22f11560102c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="account-management-operations-on-azure-data-lake-store-using-rest-api"></a>Operacje zarządzania kontem w usłudze Azure Data Lake Store przy użyciu interfejsu API REST
> [!div class="op_single_selector"]
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Z tego artykułu dowiesz się, jak wykonywać operacje zarządzania kontem w usłudze Data Lake Store za pomocą interfejsu API REST. Operacje zarządzania kontem obejmują tworzenie konta usługi Data Lake Store, usuwanie konta usługi Data Lake Store itp. Aby uzyskać instrukcje na temat sposobu wykonywania operacji systemu plików w usłudze Data Lake Store przy użyciu interfejsu API REST, zobacz [Operacje systemu plików w usłudze Data Lake Store za pomocą interfejsu API REST](data-lake-store-data-operations-rest-api.md).

## <a name="prerequisites"></a>Wymagania wstępne
* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* **[Adres cURL](http://curl.haxx.se/)**. W tym artykule jest używane narzędzie cURL w celu zademonstrowania sposobu wykonywania wywołań interfejsu API REST względem konta usługi Data Lake Store.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>W jaki sposób uwierzytelniać za pomocą usługi Azure Active Directory?
Dostępne są dwa podejścia do uwierzytelniania za pomocą usługi Azure Active Directory.

* Aby dowiedzieć się więcej o uwierzytelnianiu użytkowników końcowych w aplikacji (interaktywnym), zobacz [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-rest-api.md) (Uwierzytelnianie użytkowników końcowych w usłudze Data Lake Store za pomocą zestawu SDK platformy .NET).
* Aby dowiedzieć się więcej o uwierzytelnianiu „usługa-usługa” (nieinteraktywnym), zobacz [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md) (Uwierzytelnianie „usługa-usługa” w usłudze Data Lake Store za pomocą zestawu SDK platformy .NET).


## <a name="create-a-data-lake-store-account"></a>Tworzenie konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST zdefiniowanym [tutaj](https://msdn.microsoft.com/library/mt694078.aspx).

Użyj następującego polecenia cURL. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

W poleceniu powyżej zastąp ciąg \<`REDACTED`\> tokenem autoryzacji pobranym wcześniej. Ładunek żądania dla tego polecenia jest zawarty w pliku **input.json**, który jest udostępniany dla parametru `-d` powyżej. Zawartość pliku input.json przypomina następujący fragment kodu:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-store-account"></a>Usuwanie konta usługi Data Lake Store
Ta operacja jest oparta na wywołaniu interfejsu API REST zdefiniowanym [tutaj](https://msdn.microsoft.com/library/mt694075.aspx).

Użyj poniższego polecenia cURL, aby usunąć konto usługi Data Lake Store. Zastąp ciąg **\<yourstorename>** nazwą Twojej usługi Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Powinny pojawić się dane wyjściowe podobne do następującego fragmentu kodu:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Następne kroki
* [Operacje systemu plików w usłudze Data Lake Store za pomocą interfejsu API REST](data-lake-store-data-operations-rest-api.md).

## <a name="see-also"></a>Zobacz też
* [Dokumentacja interfejsu API REST usługi Azure Data Lake Store](https://docs.microsoft.com/rest/api/datalakestore/)
* [Open Source Big Data applications compatible with Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md) (Aplikacje danych big data typu open source zgodne z usługą Azure Data Lake Store)

