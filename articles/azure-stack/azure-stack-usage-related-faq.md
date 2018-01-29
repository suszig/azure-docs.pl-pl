---
title: "Często zadawane pytania dotyczące wykorzystania interfejsu API | Dokumentacja firmy Microsoft"
description: "Lista liczników Azure stosu, porównanie Azure użycia interfejsu API, czas użytkowania i zgłoszony czas kody błędów."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: alfredop
ms.openlocfilehash: 65b9ff0881e46836d9f19a04cf470835679e7b2f
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Często zadawane pytania w stosie Azure użycia interfejsu API
Ten artykuł zawiera odpowiedzi na niektóre często zadawane pytania dotyczące interfejsu API Azure stosu użycia.

## <a name="what-meter-ids-can-i-see"></a>Jakie miernika identyfikatorów może wyświetlać?
Użycie jest zgłaszany w przypadku następujących dostawców zasobów:

| **Dostawca zasobów** | **Identyfikator miernika** | **Nazwa licznika** | **Unit** | **Dodatkowe informacje** |
| --- | --- | --- | --- | --- |
| **Sieć** |F271A8A388C44D93956A063E1D2FA80B |Użycie adresu statycznego adresu IP |Adresy IP| Adresy IP liczba używane |
| |9E2739BA86744796B465F64674B822BA |Użycie adresu dynamicznego adresu IP |Adresy IP| Adresy IP liczba używane |
| **Storage** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*godziny |Łączna pojemność używane przez tabel |
| |B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*godziny |Łączna pojemność używane przez stronicowych obiektów blob |
| |B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*godziny |Łączna pojemność używane przez kolejki |
| |09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*godziny |Łączna pojemność używane przez blokowych obiektów blob |
| |B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |Liczba żądań w 000's 10 |Żądania obsługi tabeli (w 10 000's) |
| |50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Transfer danych przychodzących danych w GB |Tabela usługi danych wejściowych w GB |
| |1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Transfer danych wychodzących w GB |Wyjście danych usługi tabeli w GB |
| |43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |Żądania liczba 10, 000's |Żądania obsługi obiektów blob (w 10 000's) |
| |9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Transfer danych przychodzących danych w GB |Obiekt blob usługi danych wejściowych w GB |
| |3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Transfer danych wychodzących w GB |Wyjście danych usługi obiektów blob w GB |
| |EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |Żądania liczba 10, 000's |Żądania obsługi kolejki (w 10 000's) |
| |E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Transfer danych przychodzących danych w GB |Kolejki usługi danych wejściowych w GB |
| |DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Transfer danych wychodzących w GB |Wyjście danych usługi kolejki w GB |
| **Sql RP**            | CBCFEF9A-B91F-4597-A4D3-01FE334BED82 | DatabaseSizeHourSqlMeter   | MB\*godziny   | Całkowita pojemność baz danych podczas tworzenia zgłosił co godzinę.  |
| **MySql RP**          | E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3 | DatabaseSizeHourMySqlMeter | MB\*godziny    | Całkowita pojemność baz danych podczas tworzenia zgłosił co godzinę. |
| **Obliczanie** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Godziny rozmiar podstawowej maszyny Wirtualnej |Wirtualne podstawowe minut | Rdzenie wirtualnego liczba minut, przez które uruchomienia maszyny Wirtualnej |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Godziny rozmiar maszyny Wirtualnej systemu Windows |Wirtualne podstawowe minut | Rdzenie wirtualnego liczba minut, przez które uruchomienia maszyny Wirtualnej |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |Godziny rozmiar maszyny Wirtualnej |Liczba godzin korzystania z maszyny Wirtualnej |Przechwytuje zarówno podstawowy, jak i systemu Windows maszyny Wirtualnej. Nie dostosowywać rdzeni |
| **Usługa Key Vault** |EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Transakcje magazynu kluczy | Liczba żądań w 000's 10| Liczba odebranych przez płaszczyzna danych usługi Key Vault żądań interfejsu API REST |
| **Usługi aplikacji** |190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  | App Service   | Wirtualne podstawowe godziny  | Liczba rdzeni wirtualnego używane do uruchamiania usługi aplikacji |
|             | 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE | Funkcje — żądań obliczeń      | 10 żądań              | Dotyczy funkcji  |
|             | 957E9F36-2C14-45A1-B6A1-1723EF71A01D | Liczba godzin korzystania z usługi aplikacji — warstwa          | 1 godzina                   |                       |
|             | 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9 | Liczba godzin korzystania z usługi aplikacji — warstwa bezpłatna            | 1 godzina                   |                       |
|             | 88039D51-A206-3A89-E9DE-C5117E2D10A6 | Liczba godzin korzystania z małych standardowe usługi aplikacji  | 1 godzina                   |                       |
|             | 83A2A13E-4788-78DD-5D55-2831B68ED825 | Godziny standardowe usługi aplikacji — średnia | 1 godzina                   |                       |
|             | 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6 | Liczba godzin korzystania z dużych standardowe usługi aplikacji  | 1 godzina                   |                       |
|             | 264ACB47-AD38-47F8-ADD3-47F01DC4F473 | SNI SSL                           | Na powiązanie SNI SSL      | Dotyczy usługi aplikacji |
|             | 60B42D72-DC1C-472C-9895-6C516277EDB4 | IP SSL                            | Według adresu IP na podstawie powiązania SSL | Dotyczy usługi aplikacji |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Jak wykorzystanie stosu Azure, interfejsy API porównania [użycia usługi Azure API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (obecnie w publicznej wersji zapoznawczej)?
* Interfejs API użycia dzierżawcy jest zgodna z interfejsu API platformy Azure, z jednym wyjątkiem: *showDetails* flagi obecnie nie jest obsługiwana w stosie Azure.
* Interfejs API użycia dostawcy dotyczy tylko stosu Azure.
* Obecnie [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) czyli dostępnej na platformie Azure nie jest dostępna w stosie Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Jaka jest różnica między czasem użycia i zgłoszony czas?
Raporty danych użycia są dwie wartości czasu głównego:

* **Zgłoszony czas**. Czasu wprowadzenia systemu użycia zdarzenia użycia
* **Czas użytkowania**. Gdy zasobów Azure stos został wykorzystany czas

Można napotkać rozbieżności w wartości do użycia czas i czas zgłoszone zdarzenia użycia określonego. Opóźnienie może być tak długo, jak wiele godzin, w każdym środowisku.

Obecnie można wysyłać zapytania tylko przez *zgłoszony czas*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Co oznaczają one użycia interfejsu API?
| **Kod stanu HTTP** | **Kod błędu:** | **Opis** |
| --- | --- | --- |
| 400/Niewłaściwe żądanie |*NoApiVersion* |*Wersja interfejsu api* brakuje parametru zapytania. |
| 400/Niewłaściwe żądanie |*InvalidProperty* |Właściwość nie istnieje lub ma nieprawidłową wartość. Komunikat w kodzie błędu w treści odpowiedzi identyfikuje brakuje właściwości. |
| 400/Niewłaściwe żądanie |*RequestEndTimeIsInFuture* |Wartość *ReportedEndTime* przypada w przyszłości. Wartości w przyszłości nie są dozwolone dla tego argumentu. |
| 400/Niewłaściwe żądanie |*SubscriberIdIsNotDirectTenant* |Wywołanie interfejsu API dostawcy został użyty identyfikator subskrypcji, która nie jest prawidłową dzierżawy obiektu wywołującego. |
| 400/Niewłaściwe żądanie |*SubscriptionIdMissingInRequest* |Brak Identyfikatora subskrypcji obiektu wywołującego. |
| 400/Niewłaściwe żądanie |*InvalidAggregationGranularity* |Zażądano szczegółowości nieprawidłowy agregacji. Prawidłowe wartości to dzienne i co godzinę. |
| 503 |*ServiceUnavailable* |Wystąpił błąd powtarzający operację, ponieważ usługa jest zajęta lub wywołanie jest ograniczane. |

## <a name="next-steps"></a>Następne kroki
[Odbiorcy rozliczeń i obciążenia zwrotnego w stosie Azure](azure-stack-billing-and-chargeback.md)

[Użycie zasobów dostawcy interfejsu API](azure-stack-provider-resource-api.md)

[Interfejs API użycia zasobów dzierżawcy](azure-stack-tenant-resource-usage-api.md)
