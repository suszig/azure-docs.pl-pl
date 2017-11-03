---
title: "Często zadawane pytania dotyczące wykorzystania interfejsu API | Dokumentacja firmy Microsoft"
description: "Lista liczników Azure stosu, porównanie Azure użycia interfejsu API, czas użytkowania i zgłoszony czas kody błędów."
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: alfredop
ms.openlocfilehash: 166147c8cb4949be1b23e0a06868e66c8a5844f4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Często zadawane pytania w stosie Azure użycia interfejsu API 
Ten artykuł zawiera odpowiedzi na niektóre często zadawane pytania dotyczące interfejsu API Azure stosu użycia.

## <a name="what-meter-ids-can-i-see"></a>Jakie miernika identyfikatorów może wyświetlać?
Użycie jest raportowanych do sieci, magazynu i dostawców zasobów obliczeniowych.

| **Dostawca zasobów** | **Identyfikator miernika** | **Nazwa licznika** | **Jednostki** | **Informacje dodatkowe** |
| --- | --- | --- | --- | --- | 
| **Sieć** |F271A8A388C44D93956A063E1D2FA80B |Użycie adresu statycznego adresu IP |Adresy IP|Liczba IP ruch używane | 
| |9E2739BA86744796B465F64674B822BA |Użycie adresu dynamicznego adresu IP |Adresy IP|Liczba IP ruch używane | 
| **Storage** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*godziny |Łączna pojemność używane przez tabel |
| | B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*godziny |Łączna pojemność używane przez stronicowych obiektów blob |
| | B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*godziny |Łączna pojemność używane przez kolejki |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*godziny |Łączna pojemność używane przez blokowych obiektów blob |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |Liczba żądań w 10 000 |Żądania obsługi tabeli (w 10 000) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Transfer danych przychodzących danych w GB |Tabela usługi danych wejściowych w GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Outgress w GB |Wyjście danych usługi tabeli w GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |Liczba żądań w 10 000 |Żądania obsługi obiektów blob (w 10 000) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Transfer danych przychodzących danych w GB |Obiekt blob usługi danych wejściowych w GB |
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Outgress w GB |Wyjście danych usługi obiektów blob w GB |
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |Liczba żądań w 10 000 |Żądania obsługi kolejki (w 10 000) |
| | E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Transfer danych przychodzących danych w GB |Kolejki usługi danych wejściowych w GB | 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Outgress w GB |Wyjście danych usługi kolejki w GB |
| **Obliczanie** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Godziny rozmiar podstawowej maszyny Wirtualnej |Wirtualne podstawowe minut | Vcores liczba minut, przez które uruchomienia maszyny Wirtualnej |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Godziny rozmiar maszyny Wirtualnej systemu Windows |Wirtualne podstawowe minut | Vcores liczba minut, przez które uruchomienia maszyny Wirtualnej |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |Godziny rozmiar maszyny Wirtualnej |Liczba godzin korzystania z maszyny Wirtualnej |Przechwytuje zarówno podstawowy, jak i systemu Windows maszyny Wirtualnej. Nie dostosowywać vcores |
| **Usługa Key Vault** | EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Transakcje magazynu kluczy | Liczba żądań w 10000s| Liczba odebranych przez płaszczyzna danych usługi Key Vault żądań interfejsu API REST |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Jak wykorzystanie stosu Azure, interfejsy API porównania [użycia usługi Azure API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (obecnie w publicznej wersji zapoznawczej)?
* Interfejs API użycia dzierżawcy jest zgodna z interfejsu API platformy Azure, z jednym wyjątkiem: *showDetails* flagi obecnie nie jest obsługiwana w stosie Azure.
* Interfejs API użycia dostawcy dotyczy tylko stosu Azure.
* Obecnie [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) czyli dostępnej na platformie Azure nie jest dostępna w stosie Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Jaka jest różnica między czasem użycia i zgłoszony czas?
Raporty danych użycia są dwie wartości czasu głównego:

* **Zgłoszony czas**. Czasu wprowadzenia systemu użycia zdarzenia użycia
* **Czas użytkowania**. Gdy zasobów Azure stos został wykorzystany czas

Można napotkać rozbieżności w wartości do użycia czas i czas zgłoszone zdarzenia użycia określonego. Opóźnienie może być tak długo, jak wiele godzin, w każdym środowisku.

Obecnie można zbadać *jedynie przez czas zgłoszone*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Co oznaczają one użycia interfejsu API?
| **Kod stanu HTTP** | **Kod błędu:** | **Opis** |
| --- | --- | --- |
| 400/Niewłaściwe żądanie |*NoApiVersion* |*Wersja interfejsu api* brakuje parametru zapytania. |
| 400/Niewłaściwe żądanie |*InvalidProperty* |Właściwość nie istnieje lub ma nieprawidłową wartość. Komunikat w kodzie błędu w treści odpowiedzi identyfikuje brakuje właściwości. |
| 400/Niewłaściwe żądanie |*RequestEndTimeIsInFuture* |Wartość *ReportedEndTime* przypada w przyszłości. Wartości w przyszłości nie są dozwolone dla tego argumentu. |
| 400/Niewłaściwe żądanie |*SubscriberIdIsNotDirectTenant* |Wywołanie interfejsu API dostawcy używany identyfikator subskrypcji, która nie jest prawidłową dzierżawy obiektu wywołującego. |
| 400/Niewłaściwe żądanie |*SubscriptionIdMissingInRequest* |Brak Identyfikatora subskrypcji obiektu wywołującego. |
| 400/Niewłaściwe żądanie |*InvalidAggregationGranularity* |Zażądano szczegółowości nieprawidłowy agregacji. Prawidłowe wartości to dzienne i co godzinę. |
| 503 |*ServiceUnavailable* |Wystąpił błąd powtarzający operację, ponieważ usługa jest zajęta lub wywołanie jest ograniczane. |

## <a name="next-steps"></a>Następne kroki
[Odbiorcy rozliczeń i obciążenia zwrotnego w stosie Azure](azure-stack-billing-and-chargeback.md)

[Użycie zasobów dostawcy interfejsu API](azure-stack-provider-resource-api.md)

[Interfejs API użycia zasobów dzierżawcy](azure-stack-tenant-resource-usage-api.md)

