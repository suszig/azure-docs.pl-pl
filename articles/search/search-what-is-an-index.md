---
title: "Tworzenie indeksu usługi Azure Search | Microsoft Azure | Hostowana usługa wyszukiwania w chmurze"
description: "Czym jest indeks w usłudze Azure Search i jak się z niego korzysta?"
services: search
documentationcenter: 
author: ashmaka
ms.assetid: a395e166-bf2e-4fca-8bfc-116a46c5f7b1
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3f2a2b6c82c6c931261036ae1fba733b46a074dc


---
# <a name="create-an-azure-search-index"></a>Tworzenie indeksu usługi Azure Search
> [!div class="op_single_selector"]
> * [Omówienie](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

## <a name="what-is-an-index"></a>Co to jest indeks?
*Indeks* jest trwałym magazynem *dokumentów* i innych konstrukcji używanych przez usługę Azure Search. Dokument jest pojedynczą jednostką danych, które można wyszukiwać w indeksie. Na przykład sklep internetowy może mieć dokument dla każdego sprzedawanego produktu, a organizacja medialna — dla każdego artykułu itp. W przełożeniu na lepiej znane pojęcia bazodanowe: *indeks* jest podobny do *tabeli*, a *dokumenty* są w przybliżeniu równe *wierszom* w tabeli.

Podczas dodawania/przekazywania dokumentów i przesyłania zapytań wyszukiwań do usługi Azure Search przesyłasz żądania do konkretnego indeksu w swojej usłudze wyszukiwania.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Typy i atrybuty pól w indeksie usługi Azure Search
W trakcie definiowania schematu musisz określić nazwę, typ i atrybuty każdego pola w indeksie. Typ pola klasyfikuje dane, które są w nim przechowywane. Atrybuty są ustawiane dla poszczególnych pól, aby określić sposób użycia pola. W poniższych tabelach zostały wyszczególnione typy i atrybuty, które możesz określić.

### <a name="field-types"></a>Typy pól
| Typ | Opis |
| --- | --- |
| *Edm.String* |Tekst, który opcjonalnie można podzielić na tokeny na potrzeby wyszukiwania pełnotekstowego (dzielenie wyrazów, analiza słowotwórcza itp.). |
| *Collection(Edm.String)* |Lista ciągów, które opcjonalnie można podzielić na tokeny na potrzeby wyszukiwania pełnotekstowego. Nie ma teoretycznej górnej granicy liczby elementów w kolekcji, ale rozmiar ładunku w kolekcjach jest ograniczony do 16 MB. |
| *Edm.Boolean* |Zawiera wartości prawda/fałsz. |
| *Edm.Int32* |32-bitowe wartości całkowite. |
| *Edm.Int64* |64-bitowe wartości całkowite. |
| *Edm.Double* |Dane liczbowe o podwójnej precyzji. |
| *Edm.DateTimeOffset* |Wartości daty i godziny przedstawione w formacie OData 4 (np. w formacie `yyyy-MM-ddTHH:mm:ss.fffZ` lub `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |Punkt przedstawiający lokalizację geograficzną na świecie. |

Dowiedz się więcej na temat [typów danych obsługiwanych przez usługę Azure Search w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx).

### <a name="field-attributes"></a>Atrybuty pól
| Atrybut | Opis |
| --- | --- |
| *Klucz* |Ciąg udostępniający unikatowy identyfikator każdego dokumentu, używany do wyszukiwania dokumentu. Każdy indeks musi mieć jeden klucz. Tylko jedno pole może być kluczem i musi ono mieć typ Edm.String. |
| *Pobieranie* |Określa, czy pole może być zwracane w wynikach wyszukiwania. |
| *Filtrowanie* |Umożliwia używanie pola w zapytaniach filtrów. |
| *Sortowanie* |Umożliwia zapytaniom sortowanie wyników wyszukiwania za pomocą tego pola. |
| *Tworzenie aspektów* |Umożliwia używanie pola w strukturze [nawigacji aspektowej](search-faceted-navigation.md) podczas samodzielnego filtrowania przez użytkownika. Zwykle jako aspekty najlepiej sprawdzają się pola zawierające powtarzające się wartości, które umożliwiają grupowanie wielu dokumentów (na przykład wiele dokumentów podlegających pod jedną markę lub kategorię usługi). |
| *Wyszukiwanie* |Oznacza pole jako podlegające wyszukiwaniu pełnotekstowemu. |

Dowiedz się więcej na temat [atrybutów indeksów obsługiwanych przez usługę Azure Search w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx).

## <a name="guidance-for-defining-an-index-schema"></a>Wskazówki dotyczące definiowania schematu indeksu
W fazie planowania projektu indeksu poświęć odpowiednio dużo czasu na przemyślenie każdej decyzji. Podczas projektowania indeksu należy pamiętać o środowisku wyszukiwania użytkownika oraz o potrzebach biznesowych, ponieważ do każdego pola należy przypisać [odpowiednie atrybuty](https://msdn.microsoft.com/library/azure/dn798941.aspx). Wprowadzanie zmian do indeksu po jego wdrożeniu obejmuje jego ponowne utworzenie i ponownie załadowanie danych.

Jeśli wymagania dotyczące magazynu danych ulegną zmianom, możesz zwiększyć lub zmniejszyć pojemność przez dodanie lub usunięcie partycji. Aby uzyskać szczegółowe informacje, zobacz [Manage your Search service in Azure](search-manage.md) (Zarządzanie usługą wyszukiwania na platformie Azure) lub [Service Limits](search-limits-quotas-capacity.md) (Ograniczenia usługi).




<!--HONumber=Nov16_HO2-->


