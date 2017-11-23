---
title: "Lista kontrolna zabezpieczeń bazy danych platformy Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera zestaw Lista kontrolna zabezpieczeń bazy danych platformy Azure."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: 11fa0d01463e398ae4106e1a70dc4c51b8698ff4
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="azure-database-security-checklist"></a>Lista kontrolna zabezpieczeń bazy danych platformy Azure

Aby zwiększyć bezpieczeństwo, bazy danych Azure zawiera szereg wbudowanych zabezpieczeń kontrolki, których można użyć do ograniczenia i kontroli dostępu.

Należą do nich:

-   Zaporą, która umożliwia tworzenie [reguły zapory](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) ograniczenie łączności za pomocą adresu IP
-   Zapory poziomu serwera dostępne w portalu Azure
-   Reguły zapory poziomu bazy danych dostępne w programie SSMS
-   Bezpieczne połączenie z bazą danych przy użyciu bezpiecznego połączenia ciągów
-   Użyj zarządzania dostępem
-   Szyfrowanie danych
-   Inspekcja bazy danych SQL
-   Wykrywanie zagrożeń bazy danych SQL

## <a name="introduction"></a>Wprowadzenie
Chmura obliczeniowa wymaga nowej wzorcami zabezpieczeń, które jest nieznane na wiele aplikacji użytkowników, administratorów bazy danych i programistów. W związku z tym niektóre organizacje są wątpliwości do implementowania infrastruktury chmury w celu zarządzania danymi z powodu możliwego zagrożenia bezpieczeństwa. Jednak większość tego problemu mogą być złagodzone za pośrednictwem lepiej zrozumieć funkcje zabezpieczeń wbudowanych w Microsoft Azure i bazy danych SQL Azure firmy Microsoft.

## <a name="checklist"></a>Lista kontrolna
Zalecamy przeczytanie [najlepsze rozwiązania zabezpieczeń bazy danych Azure](https://docs.microsoft.com/en-us/azure/security/azure-database-security-best-practices) artykuł przed recenzowania tej liście kontrolnej. Można na pełne wykorzystanie ta lista kontrolna po zidentyfikowaniu najlepszych rozwiązań. Ta lista kontrolna umożliwia następnie upewnij się, że uwzględniono istotne problemy w zabezpieczeń bazy danych Azure.


|Lista kontrolna kategorii| Opis|
| ------------ | -------- |
|**Ochrona danych**||
| <br> Szyfrowanie podczas przesyłania/ruchu| <ul><li>[Transport Layer Security](https://docs.microsoft.com/en-us/windows-server/security/tls/transport-layer-security-protocol), do szyfrowania danych, gdy dane są przenoszone do sieci.</li><li>Baza danych wymaga zapewnienia bezpiecznej komunikacji z klientami, na podstawie [TDS (strumienia danych tabelarycznych)](https://msdn.microsoft.com/en-in/library/dd357628.aspx) na protokole TLS (Transport Layer Security).</li></ul> |
|<br>Szyfrowanie w spoczynku| <ul><li>[Funkcja przezroczystego szyfrowania danych](http://go.microsoft.com/fwlink/?LinkId=526242), gdy nieaktywne dane są przechowywane w fizycznie w jakimkolwiek formularzu cyfrowego.</li></ul>|
|**Kontrola dostępu**||  
|<br> Dostęp do bazy danych | <ul><li>[Uwierzytelnianie](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) (Azure Active Directory Authentication) AD uwierzytelniania przy użyciu tożsamości zarządzanych przez usługę Azure Active Directory.</li><li>[Autoryzacji](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-control-access) zezwolenie użytkownikom na co najmniej niezbędne uprawnienia.</li></ul> |
|<br>Dostęp do aplikacji| <ul><li>[Poziom zabezpieczeń wiersz](https://msdn.microsoft.com/library/dn765131) (przy użyciu zasad zabezpieczeń, w tym samym czasie, ograniczanie dostępu na poziomie wiersza na podstawie kontekstu użytkownika tożsamości, roli lub wykonywania).</li><li>[Dynamiczne maskowanie danych](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-dynamic-data-masking-get-started) (przy użyciu uprawnień i zasady, limity ujawnienie danych poufnych przez maskowania go użytkownikom bez uprawnień)</li></ul>|
|**Aktywne monitorowanie**||  
| <br>Śledzenie & wykrywania| <ul><li>[Inspekcja](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing) śledzi zdarzenia bazy danych i zapisuje je w dzienniku inspekcji / log działania sieci [konta magazynu Azure](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account).</li><li>Przy użyciu kondycji bazy danych Azure Śledź [Dzienniki aktywności monitora Azure](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</li><li>[Wykrywanie zagrożeń](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection) wykrywa nietypowe działania bazy danych wskazują możliwe zagrożenia bezpieczeństwa w bazie danych. </li></ul> |
|<br>Azure Security Center| <ul><li>[Monitorowanie danych](https://docs.microsoft.com/en-us/azure/security-center/security-center-enable-auditing-on-sql-databases) korzystanie z Centrum zabezpieczeń Azure jako scentralizowanych zabezpieczeń rozwiązanie monitorujące, SQL i innymi usługami Azure.</li></ul>|     

## <a name="conclusion"></a>Podsumowanie
Bazy danych platformy Azure to platforma niezawodne bazy danych, z pełnym zakresem funkcji zabezpieczeń, które spełniają wymagania organizacyjne i przepisami zgodności wiele. Można łatwo chronić dane przez kontrolowanie fizyczny dostęp do danych i za pomocą różnych opcji zabezpieczeń danych w pliku, kolumny lub poziomie wiersza niewidocznego szyfrowania danych, szyfrowanie na poziomie komórki lub zabezpieczenia na poziomie wiersza. Zawsze zaszyfrowane umożliwia również operacji względem zaszyfrowane dane, uproszczeniu procesu aktualizacji aplikacji. Z kolei dostęp do inspekcji Dzienniki aktywności bazy danych SQL zawiera informacje, które są potrzebne, co należy wiedzieć, jak i kiedy jest uzyskiwany dostęp do danych.

## <a name="next-steps"></a>Następne kroki
Można zwiększyć ochrony bazy danych przed złośliwymi użytkownikami lub nieautoryzowanego dostępu, wystarczy kilka prostych kroków. W tym samouczku dowiesz się:

- Konfigurowanie [reguły zapory](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) serwera i lub bazy danych.
- Ochrona danych za pomocą [szyfrowania](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/sql-server-encryption).
- Włącz [inspekcji bazy danych SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing).

