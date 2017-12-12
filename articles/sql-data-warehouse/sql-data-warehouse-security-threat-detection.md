---
title: "Rozpoczynanie pracy z wykrywanie zagrożeń magazynu danych SQL"
description: "Jak rozpocząć pracę z wykrywanie zagrożeń"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: c9073dd9-6c62-4735-8457-dfb9f859c900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 7f5dab6936e8cac10ac7a4a7dc4c3be116de5ad5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-threat-detection"></a>Rozpoczynanie pracy z wykrywanie zagrożeń
> [!div class="op_single_selector"]
> * [Inspekcja](sql-data-warehouse-auditing-overview.md)
> * [Wykrywanie zagrożeń](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a>Omówienie
Wykrywanie zagrożeń wykrywa nietypowe działania bazy danych wskazują możliwe zagrożenia bezpieczeństwa w bazie danych. Wykrywanie zagrożeń jest w wersji zapoznawczej i jest obsługiwana dla usługi SQL Data Warehouse.

Wykrywanie zagrożeń udostępnia nową warstwę zabezpieczeń, co umożliwia klientom wykrywanie i odpowiadanie na potencjalne zagrożenia w miarę ich występowania, zapewniając alerty zabezpieczeń w nietypowych działań. Użytkownicy można eksplorować podejrzane zdarzenia przy użyciu [inspekcję magazynu danych SQL Azure](sql-data-warehouse-auditing-overview.md) ustalenie, jeśli są one wynikiem próby dostępu, naruszenia lub wykorzystania danych w magazynie danych.
Wykrywanie zagrożeń upraszcza potencjalne zagrożenia w magazynie danych, bez konieczności ekspertów zabezpieczeń lub zarządzać zabezpieczeniami zaawansowanymi, monitorowanie systemów.

Na przykład wykrywanie zagrożeń wykrywa niektóre działania nietypowych bazy danych, które wskazują potencjalne prób iniekcji kodu SQL. Iniekcja kodu SQL jest jednym z typowych problemów zabezpieczeń aplikacji sieci Web w Internecie, używane do ataków opartych na danych aplikacji. Osoby atakujące wykorzystać luki w zabezpieczeniach aplikacji do dodania złośliwego instrukcje SQL do pola wejścia aplikacji, naruszenia i modyfikacji danych w bazie danych.

## <a name="set-up-threat-detection-for-your-database"></a>Skonfiguruj wykrywanie zagrożeń dla bazy danych
1. Uruchamianie portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).
2. Przejdź do bloku konfiguracji usługi SQL Data Warehouse, który chcesz monitorować. W bloku ustawienia, wybierz **Inspekcja i wykrywanie zagrożeń**.
   
    ![Okienko nawigacji][1]
3. W **Inspekcja i wykrywanie zagrożeń** Włącz bloku konfiguracji **ON** inspekcji, które będą wyświetlane ustawienia wykrywania zagrożeń.
   
    ![Okienko nawigacji][2]
4. Włącz **ON** wykrywanie zagrożeń.
5. Skonfiguruj listę wiadomości e-mail, które będą wysyłane alerty zabezpieczeń po wykryciu nietypowych danych magazynu działań.
6. Kliknij przycisk **zapisać** w **Inspekcja i wykrywanie zagrożeń** bloku konfiguracji, aby zapisać nowe lub zaktualizowane inspekcji i zasady wykrywania zagrożeń.
   
    ![Okienko nawigacji][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Eksplorowanie danych nietypowych działań magazynu po wykryciu podejrzanych zdarzeń
1. Otrzymasz wiadomość e-mail z powiadomieniem po wykryciu nietypowe działania bazy danych. <br/>
   Wiadomość e-mail będzie zawierał informacje o zdarzeniu podejrzane zabezpieczeń w tym rodzaju nietypowych działań, nazwa bazy danych, nazwę serwera i czas trwania zdarzenia. Ponadto zawierają informacje dotyczące możliwe przyczyny i zalecane akcje do sprawdzania i ograniczyć potencjalne zagrożenie dla bazy danych.<br/>
   
    ![Okienko nawigacji][4]
2. W wiadomości e-mail, kliknij polecenie **dziennik inspekcji SQL Azure** łącza, co spowoduje uruchomienie portalu Azure i wyświetlić odpowiednie rekordy inspekcji w czasie zbliżonym do podejrzane zdarzenia.
   
    ![Okienko nawigacji][5]
3. Polecenie rekordów inspekcji, aby wyświetlić więcej szczegółów na działania podejrzane bazy danych, takie jak instrukcji SQL, Niepowodzenie IP Przyczyna i klienta.
   
    ![Okienko nawigacji][6]
4. W bloku rekordów inspekcji kliknij **Otwórz w programie Excel** otworzyć wstępnie skonfigurowany szablon programu excel Aby zaimportować i uruchomić dokładniejszej analizy w czasie zbliżonym do podejrzane zdarzenia dziennika inspekcji.<br/>
   **Uwaga:** programu Excel 2010 lub nowszej, zasilanie kwerendy i **szybkie łączenie** ustawienie jest wymagane
   
    ![Okienko nawigacji][7]
5. Aby skonfigurować **szybkie łączenie** ustawienie — w **dodatku POWER QUERY** karty wstążki, wybierz opcję **opcje** do wyświetlenia okna dialogowego Opcje. Wybierz sekcję prywatności i wybierz opcję drugi — "Ignorowanie poziomów prywatności i potencjalne poprawianie wydajności":
   
    ![Okienko nawigacji][8]
6. Aby załadować dzienników inspekcji SQL, upewnij się, że parametry w ustawieniach kartę są poprawnie ustawione, a następnie wybierz wstążki "Dane" i kliknij przycisk "Odśwież wszystko".
   
    ![Okienko nawigacji][9]
7. Wyniki są wyświetlane w **dzienników inspekcji SQL** arkusza, co pozwala na przeprowadzanie analizy głębiej nietypowych działań, które zostały wykryte i ograniczyć wpływ zdarzeń zabezpieczeń w aplikacji.

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
