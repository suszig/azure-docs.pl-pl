---
title: "Używanie narzędzia Fiddler do oceny i testowania interfejsów API REST usługi Azure Search | Microsoft Docs"
description: "Używanie narzędzia Fiddler bez korzystania z kodu, aby zweryfikować dostępność usługi Azure Search i wypróbować interfejsy API REST."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/17/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 6ff31940f3a4e7557e0caf3d9d3740590be3bc04
ms.openlocfilehash: 12f5a22fa65d84682e5792bcbe88b67986540498


---
# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Używanie narzędzia Fiddler do oceny i testowania interfejsów API REST usługi Azure Search
> [!div class="op_single_selector"]
> * [Omówienie](search-query-overview.md)
> * [Eksplorator wyszukiwania](search-explorer.md)
> * [Fiddler](search-fiddler.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

W tym artykule wyjaśniono, jak używać narzędzia Fiddler do wysyłania żądań HTTP i przeglądania odpowiedzi przy użyciu interfejsu API REST usługi Azure Search, bez konieczności pisania kodu. Narzędzie to można [bezpłatnie pobrać ze strony firmy Telerik](http://www.telerik.com/fiddler). Azure Search jest w pełni zarządzaną, hostowaną usługą wyszukiwania w chmurze na platformie Microsoft Azure, którą można łatwo zaprogramować za pomocą platformy .NET i interfejsów API REST. Interfejsy API REST usługi Azure Search są opisane w witrynie [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

W poniższych krokach utworzysz indeks, przekażesz dokumenty, wykonasz zapytania względem indeksu, a następnie wykonasz zapytania względem systemu pod kątem informacji o usłudze.

Aby móc wykonać te kroki, niezbędna będzie usługa Azure Search i klucz `api-key`. Aby uzyskać instrukcje dotyczące rozpoczęcia pracy, zobacz [Create an Azure Search service in the portal](search-create-service-portal.md) (Tworzenie usługi Azure Search w portalu).

## <a name="create-an-index"></a>Tworzenie indeksu
1. Uruchom narzędzie Fiddler. W menu **File** (Plik) wyłącz opcję **Capture Traffic** (Przechwyć ruch) w celu ukrycia dodatkowej aktywności protokołu HTTP, która nie ma wpływu na bieżące zadanie.
2. Na karcie **Composer** (Kompozytor) sformułuj żądanie takie, jak to przedstawione na poniższym zrzucie ekranu.
   
      ![][1]
3. Wybierz pozycję **PUT**.
4. Wprowadź adres URL, który określa adres URL usługi, atrybuty żądania i wersję interfejsu API. Kilka wskazówek, o których należy pamiętać:
   
   * Używaj prefiksu protokołu HTTPS.
   * Atrybutem żądania jest „/indexes/hotels”. Na tej podstawie usługa wyszukiwania utworzy indeks o nazwie „hotels”.
   * Wersja interfejsu API jest pisana małymi literami i określana jako ciąg „?api-version=2015-02-28”. Wersje interfejsu API są ważne, ponieważ usługa Azure Search regularnie wdraża aktualizacje. W rzadkich przypadkach aktualizacja usługi może wprowadzić do interfejsu API istotną zmianę, która może powodować błędy. Z tego powodu usługa Azure Search wymaga podania parametru api-version dla każdego wysyłanego żądania, aby użytkownik miał pełną kontrolę nad tym, która wersja jest używana.
     
     Pełny adres URL powinien wyglądać podobnie, jak przedstawiono w następującym przykładzie.
     
             https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28
5. Określ nagłówek żądania, zastępując nazwę hosta i klucz api-key wartościami, które są prawidłowe dla Twojej usługi.
   
         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. W obszarze Request Body (Treść żądania) wklej pola, które tworzą definicję indeksu.
   
          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }
7. Kliknij polecenie **Execute** (Wykonaj).

W ciągu kilku sekund na liście sesji powinna zostać wyświetlona odpowiedź 201 protokołu HTTP wskazująca, że indeks został pomyślnie utworzony.

Jeśli otrzymasz odpowiedź 504 protokołu HTTP, sprawdź, czy adres URL określa protokół HTTPS. Jeśli zobaczysz odpowiedź 400 lub 404 protokołu HTTP, sprawdź treść żądania, aby zweryfikować, czy nie było żadnych błędów podczas kopiowania i wklejania. Odpowiedź 403 protokołu HTTP zazwyczaj wskazuje na problem z kluczem api-key (nieprawidłowy klucz lub problem ze składnią klucza api-key).

## <a name="load-documents"></a>Ładowanie dokumentów
Na karcie **Composer** (Kompozytor) Twoje żądanie opublikowania dokumentów będzie wyglądać następująco. Treść żądania zawiera dane wyszukiwania dla 4 hoteli.

   ![][2]

1. Wybierz pozycję **POST**.
2. Wprowadź adres URL, który rozpoczyna się od ciągu HTTPS, po którym następuje adres URL Twojej usługi, a następnie ciąg „/indexes/<nazwa_indeksu>/docs/index?api-version=2015-02-28”. Pełny adres URL powinien wyglądać podobnie, jak przedstawiono w następującym przykładzie.
   
         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
3. Nagłówek żądania powinien być taki jak poprzednio. Pamiętaj, że nazwa hosta i klucz api-key zostały zastąpione wartościami, które są prawidłowe dla Twojej usługi.
   
         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Obszar Request Body (Treść żądania) zawiera cztery dokumenty, które mają zostać dodane do indeksu hotels.
   
         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }
5. Kliknij polecenie **Execute** (Wykonaj).

W ciągu kilku sekund na liście sesji powinna zostać wyświetlona odpowiedź 200 protokołu HTTP. Oznacza to, że dokumenty zostały pomyślnie utworzone. Jeśli otrzymasz odpowiedź 207, przekazanie co najmniej jednego dokumentu nie powiodło się. Jeśli otrzymasz odpowiedź 404, wystąpił błąd składniowy w nagłówku lub w treści żądania.

## <a name="query-the-index"></a>Wykonywanie zapytań względem indeksu
Teraz, gdy indeks i dokumenty są załadowane, możesz wykonywać zapytania względem nich.  Na karcie **Composer** (Kompozytor) polecenie **GET**, które umożliwia wykonanie zapytania o usługę, będzie wyglądać podobnie jak na poniższym zrzucie ekranu.

   ![][3]

1. Wybierz pozycję **GET**.
2. Wprowadź adres URL, który rozpoczyna się od ciągu HTTPS, po którym następuje adres URL Twojej usługi, następnie ciąg „/indexes/<nazwa_indeksu>/docs?”, a na końcu parametry zapytania. Możesz użyć następującego przykładowego adresu URL, zastępując przykładową nazwę hosta nazwą prawidłową dla Twojej usługi.
   
         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28
   
   To zapytanie wyszukuje wystąpienia terminu „motel” i pobiera kategorie aspektów dla klasyfikacji.
3. Nagłówek żądania powinien być taki jak poprzednio. Pamiętaj, że nazwa hosta i klucz api-key zostały zastąpione wartościami, które są prawidłowe dla Twojej usługi.
   
         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

Powinien zostać wyświetlony kod odpowiedzi 200, a dane wyjściowe odpowiedzi powinny wyglądać podobnie jak na poniższym zrzucie ekranu.

   ![][4]

Poniższe przykładowe zapytanie można znaleźć w [operacji wyszukiwania indeksu (interfejs API usługi Azure Search)](http://msdn.microsoft.com/library/dn798927.aspx) w witrynie MSDN. Wiele przykładowych zapytań w tym temacie zawiera spacje, które nie są dozwolone w narzędziu Fiddler. Przed wklejeniem ciągu zapytania oraz podjęciem próby jego wykonania w narzędziu Fiddler zastąp każdą spację znakiem +.

**Przed zastąpieniem spacji:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Po zastąpieniu spacji znakiem +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Wykonywanie zapytań względem systemu
Zapytania możesz także wykonywać względem systemu, aby uzyskać informacje o liczbie dokumentów i użyciu przestrzeni dyskowej. Na karcie **Composer** (Kompozytor) Twoje żądanie będzie wyglądało podobnie do poniższego, a odpowiedź zwróci liczbę dokumentów i ilość używanej przestrzeni dyskowej.

 ![][5]

1. Wybierz pozycję **GET**.
2. Wprowadź adres URL, który zawiera adres URL usługi, po którym następuje ciąg „/indexes/hotels/stats?api-version=2015-02-28”:
   
         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28
3. Określ nagłówek żądania, zastępując nazwę hosta i klucz api-key wartościami, które są prawidłowe dla Twojej usługi.
   
         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Pozostaw treść żądania pustą.
5. Kliknij polecenie **Execute** (Wykonaj). Na liście sesji powinien zostać wyświetlony kod stanu 200 protokołu HTTP. Wybierz wpis opublikowany dla Twojego polecenia.
6. Kliknij kartę **Inspectors** (Inspektorzy), potem kliknij kartę **Headers** (Nagłówki), a następnie wybierz format JSON. Powinny zostać wyświetlone informacje o liczbie dokumentów i rozmiarze magazynu (w KB).

## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o zarządzaniu i korzystaniu z usługi Azure Search bez konieczności pisania kodu, zobacz [Manage your Search service on Azure](search-manage.md) (Zarządzanie usługą wyszukiwania na platformie Azure).

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png



<!--HONumber=Nov16_HO2-->


