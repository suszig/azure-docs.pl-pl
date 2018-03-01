---
title: "Zestaw SDK platformy .NET: operacje systemu plików w usłudze Azure Data Lake Store | Microsoft Docs"
description: "Użyj zestawu SDK platformy .NET usługi Azure Data Lake Store w celu wykonania takich operacji systemu plików w usłudze Data Lake Store, jak tworzenie folderów itp."
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
ms.date: 01/30/2018
ms.author: nitinme
ms.openlocfilehash: 1094a5ea3c000707aa7736d22d4df0558da32b5e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-net-sdk"></a>Operacje systemu plików w usłudze Azure Data Lake Store przy użyciu zestawu SDK platformy .NET
> [!div class="op_single_selector"]
> * [Zestaw SDK platformy .NET](data-lake-store-data-operations-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
>

Z tego artykułu dowiesz się, jak wykonywać operacje systemu plików w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET. Operacje systemu plików obejmują tworzenie folderów w ramach konta usługi Data Lake Store, przekazywanie plików, pobieranie plików itp.

Aby uzyskać instrukcje na temat sposobu wykonywania operacji zarządzania kontem w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET, zobacz [Account management operations on Data Lake Store using .NET SDK (Operacje zarządzania kontem w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-get-started-net-sdk.md).

## <a name="prerequisites"></a>Wymagania wstępne
* **Program Visual Studio w wersji 2013, 2015 lub 2017**. Poniższe instrukcje korzystają z programu Visual Studio 2017.

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Konto usługi Azure Data Lake Store**. Aby uzyskać instrukcje dotyczące tworzenia konta, zobacz artykuł [Rozpoczynanie pracy z usługą Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-a-net-application"></a>Tworzenie aplikacji .NET
Przykładowy kod dostępny [w usłudze GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted) przeprowadzi Cię przez proces tworzenia plików w magazynie, łączenia plików, pobierania pliku i usuwania niektórych plików z magazynu. Ta część artykułu przeprowadzi Cię przez najważniejsze fragmenty kodu.

1. Otwórz program Visual Studio i utwórz aplikację konsolową.
2. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.
3. W obszarze **Nowy projekt** wpisz lub wybierz następujące wartości:

   | Właściwość | Wartość |
   | --- | --- |
   | Kategoria |Szablony/Visual C#/Windows |
   | Szablon |Aplikacja konsolowa |
   | Name (Nazwa) |CreateADLApplication |

4. Kliknij przycisk **OK**, aby utworzyć projekt.

5. Dodaj pakiety NuGet do swojego projektu.

   1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i kliknij polecenie **Zarządzaj pakietami NuGet**.
   2. Na karcie **Menedżer pakietów NuGet** upewnij się, że **Źródło pakietów** jest ustawione na wartość **nuget.org** i że zaznaczone jest pole wyboru **Uwzględnij wersję wstępną**.
   3. Wyszukaj i zainstaluj następujące pakiety NuGet:

      * `Microsoft.Azure.DataLake.Store` — w tym samouczku jest używana wersja 1.0.0.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` — w tym samouczku jest używana wersja 2.3.1.
    
    Zamknij **Menedżera pakietów NuGet**.

6. Otwórz plik **Program.cs**, usuń istniejący kod, a następnie dołącz poniższe instrukcje, aby dodać odwołania do przestrzeni nazw.

        using System;
        using System.IO;using System.Threading;
        using System.Linq;
        using System.Text;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.DataLake.Store;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

7. Zadeklaruj zmienne w pokazany poniżej sposób i podaj wartości dla symboli zastępczych. Upewnij się również, że podana tutaj ścieżka lokalna i nazwa pliku istnieją na komputerze.

        namespace SdkSample
        {
            class Program
            {
                private static string _adlsAccountName = "<DATA-LAKE-STORE-NAME>.azuredatalakestore.net";        
            }
        }

Z pozostałych akapitów tego artykułu możesz dowiedzieć się, w jaki sposób używać dostępnych metod .NET w celu wykonywania operacji, takich jak uwierzytelnianie, przekazywanie plików itp.

## <a name="authentication"></a>Authentication

* Aby dowiedzieć się więcej o uwierzytelnianiu użytkowników końcowych w aplikacji, zobacz [End-user authentication with Data Lake Store using .NET SDK (Uwierzytelnianie użytkowników końcowych w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-end-user-authenticate-net-sdk.md).
* Aby dowiedzieć się więcej o uwierzytelnianiu „usługa-usługa” w aplikacji, zobacz [Service-to-service authentication with Data Lake Store using .NET SDK (Uwierzytelnianie „usługa-usługa” w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-service-to-service-authenticate-net-sdk.md).


## <a name="create-client-object"></a>Tworzenie obiektu klienta
Poniższy fragment kodu tworzy obiekt klienta systemu plików usługi Data Lake Store, który jest używany do wysyłania żądań do usługi.

    // Create client objects
    AdlsClient client = AdlsClient.CreateClient(_adlsAccountName, adlCreds);

## <a name="create-a-file-and-directory"></a>Tworzenie plików i katalogów
Dodaj poniższy fragment kodu do aplikacji. Ten fragment kodu dodaje plik, a także dowolny katalog nadrzędny, który nie istnieje.

    // Create a file - automatically creates any parent directories that don't exist
    // The AdlsOuputStream preserves record boundaries - it does not break records while writing to the store
    using (var stream = client.CreateFile(fileName, IfExists.Overwrite))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is test data to write.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);

        textByteArray = Encoding.UTF8.GetBytes("This is the second line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="append-to-a-file"></a>Dołączanie do pliku
Poniższy fragment kodu dołącza dane do istniejącego pliku na koncie usługi Data Lake Store.

    // Append to existing file
    using (var stream = client.GetAppendStream(fileName))
    {
        byte[] textByteArray = Encoding.UTF8.GetBytes("This is the added line.\r\n");
        stream.Write(textByteArray, 0, textByteArray.Length);
    }

## <a name="read-a-file"></a>Odczytywanie pliku
Poniższy fragment kodu odczytuje zawartość z pliku w usłudze Data Lake Store.

    //Read file contents
    using (var readStream = new StreamReader(client.GetReadStream(fileName)))
    {
        string line;
        while ((line = readStream.ReadLine()) != null)
        {
            Console.WriteLine(line);
        }
    }

## <a name="get-file-properties"></a>Pobieranie właściwości pliku
Poniższy fragment kodu zwraca właściwości skojarzone z plikiem lub katalogiem.

    // Get file properties
    var directoryEntry = client.GetDirectoryEntry(fileName);
    PrintDirectoryEntry(directoryEntry);

Definicja metody `PrintDirectoryEntry` jest dostępna w ramach przykładu [w serwisie GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted). 

## <a name="rename-a-file"></a>Zmienianie nazwy pliku
Poniższy fragment kodu zmienia nazwę istniejącego pliku na koncie usługi Data Lake Store.

    // Rename a file
    string destFilePath = "/Test/testRenameDest3.txt";
    client.Rename(fileName, destFilePath, true);

## <a name="enumerate-a-directory"></a>Wyliczanie katalogu
Poniższy fragment kodu wylicza katalogi na koncie usługi Data Lake Store.

    // Enumerate directory
    foreach (var entry in client.EnumerateDirectory("/Test"))
    {
        PrintDirectoryEntry(entry);
    }

Definicja metody `PrintDirectoryEntry` jest dostępna w ramach przykładu [w serwisie GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted).

## <a name="delete-directories-recursively"></a>Rekursywne usuwanie katalogów
Poniższy fragment kodu rekursywnie usuwa katalog i wszystkie jego podkatalogi.

    // Delete a directory and all it's subdirectories and files
    client.DeleteRecursive("/Test");

## <a name="samples"></a>Przykłady
Oto kilka przykładów na temat korzystania z zestawu SDK systemu plików Data Lake Store.
* [Podstawowy przykład w witrynie GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-get-started/tree/master/AdlsSDKGettingStarted)
* [Zaawansowany przykład w witrynie GitHub](https://github.com/Azure-Samples/data-lake-store-adls-dot-net-samples)

## <a name="see-also"></a>Zobacz też
* [Account management operations on Data Lake Store using .NET SDK (Operacje zarządzania kontem w usłudze Data Lake Store przy użyciu zestawu SDK platformy .NET)](data-lake-store-get-started-net-sdk.md)
* [Dokumentacja zestawu SDK .NET usługi Data Lake Store](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Następne kroki
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
