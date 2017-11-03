---
title: "Analiza strumienia: Obracanie poświadczenia logowania dla danych wejściowych i wyjściowych | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zaktualizować poświadczenia dla usługi analiza strumienia danych wejściowych i wyjściowych."
keywords: "poświadczenia logowania"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: a1a927fa9c34b38e54fdb22782e80fd13bf430c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Obróć poświadczenia logowania dla wejścia i wyjścia w zadania usługi analiza strumienia
## <a name="abstract"></a>Abstrakcyjny
Usługa Azure Stream Analytics obecnie nie zezwala na zastępowanie poświadczeń na wejścia/wyjścia podczas uruchamiania zadania.

Gdy usługi Azure Stream Analytics obsługuje wznawianie zadania z ostatnich danych wyjściowych, możemy udostępniać cały proces minimalizując zwłokę między zatrzymywanie i uruchamianie zadania i obracanie poświadczenia logowania.

## <a name="part-1---prepare-the-new-set-of-credentials"></a>Część 1 — przygotowanie nowego zestawu poświadczeń:
Ta część dotyczy wejścia/wyjścia:

* Blob Storage
* Usługa Event Hubs
* SQL Database
* Table Storage

Dla innych wejść/wyjść przejdź do części 2.

### <a name="blob-storagetable-storage"></a>Magazyn obiektów blob magazynu/tabeli.
1. Przejdź do rozszerzenie magazynu w portalu zarządzania Azure:  
   ![graphic1][graphic1]
2. Zlokalizuj miejsca używanego przez zadanie, a następnie przejdź w niej:  
   ![graphic2][graphic2]
3. Kliknij polecenie Zarządzaj kluczami dostępu:  
   ![graphic3][graphic3]
4. Między podstawowy klucz dostępu i pomocniczy klucz dostępu **pobrania nie jest używany przez zadanie**.
5. Regenerate trafień:  
   ![graphic4][graphic4]
6. Skopiuj nowo wygenerowany klucz:  
   ![graphic5][graphic5]
7. Nadal część 2.

### <a name="event-hubs"></a>Centra zdarzeń
1. Przejdź do rozszerzenia usługi Service Bus w portalu zarządzania Azure:  
   ![graphic6][graphic6]
2. Zlokalizuj Namespace magistrali usług, które są używane przez zadanie, a następnie przejdź w niej:  
   ![graphic7][graphic7]
3. Jeśli zadanie używa zasady dostępu współużytkowanego na Namespace magistrali usługi, należy przejść do kroku 6  
4. Przejdź do karty centra zdarzeń:  
   ![graphic8][graphic8]
5. Zlokalizuj Centrum zdarzeń, używane przez zadania, a następnie przejdź w niej:  
   ![graphic9][graphic9]
6. Przejdź na kartę Konfiguracja:  
   ![graphic10][graphic10]
7. Na rozwijanej nazwę zasady Znajdź zasady dostępu współdzielonego, używane przez zadania:  
   ![graphic11][graphic11]
8. Między klucz podstawowy i klucz pomocniczy **pobrania nie jest używany przez zadanie**.  
9. Regenerate trafień:  
   ![graphic12][graphic12]
10. Skopiuj nowo wygenerowany klucz:  
   ![graphic13][graphic13]
11. Nadal część 2.  

### <a name="sql-database"></a>SQL Database
> [!NOTE]
> Uwaga: należy połączyć się z usługą baza danych SQL. Zamierzamy pokazują, jak to zrobić przy użyciu możliwości zarządzania w portalu zarządzania Azure, ale można używać niektórych po stronie klienta narzędzia, takiego jak SQL Server Management Studio również.
>
> 

1. Przejdź do rozszerzenia bazy danych SQL w portalu zarządzania Azure:  
   ![graphic14][graphic14]
2. Zlokalizuj bazę danych SQL używane przez zadania i **kliknij serwer** łącza w tym samym wierszu:  
   ![graphic15][graphic15]
3. Kliknij polecenie Zarządzaj:  
   ![graphic16][graphic16]
4. Typ główny bazy danych:  
   ![graphic17][graphic17]
5. Wpisz nazwę użytkownika, hasło i kliknij przycisk Zaloguj:  
   ![graphic18][graphic18]
6. Kliknij pozycję Nowa kwerenda:  
   ![graphic19][graphic19]
7. Typ w następującym zapytaniu zamianę < login_name > Twoja nazwa użytkownika i zastępowanie <enterStrongPasswordHere> przy użyciu nowego hasła:  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. Kliknij przycisk Uruchom:  
   ![graphic20][graphic20]
9. Wróć do kroku 2 do tego czasu, kliknij bazy danych:  
   ![graphic21][graphic21]
10. Kliknij polecenie Zarządzaj:  
   ![graphic22][graphic22]
11. Wpisz nazwę użytkownika, hasło i kliknij przycisk logowania:  
   ![graphic23][graphic23]
12. Kliknij pozycję Nowa kwerenda:  
   ![graphic24][graphic24]
13. Wpisz poniższe zapytanie zamienianie < nazwa_użytkownika > o nazwie, przez którą ma tę nazwę logowania w kontekście tej bazy danych (taką samą wartość, nadana < login_name >, na przykład można podać) identyfikację i zastępowanie < login_name > Nowa nazwa użytkownika:  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. Kliknij przycisk Uruchom:  
   ![graphic25][graphic25]
15. Teraz należy podać nowy użytkownik o tej samej role i uprawnienia miał oryginalny użytkownika.
16. Nadal część 2.

## <a name="part-2-stopping-the-stream-analytics-job"></a>Część 2: Zatrzymywanie zadania usługi analiza strumienia
1. Przejdź do rozszerzenia usługi Stream Analytics w portalu zarządzania Azure:  
   ![graphic26][graphic26]
2. Znajdź swoją pracę i przejdź do niej:  
   ![graphic27][graphic27]
3. Przejdź na kartę danych wejściowych lub wyjściowych według tego, czy są obracanie poświadczeń na danych wejściowych lub wyjściowych.  
   ![graphic28][graphic28]
4. Kliknij polecenie zatrzymania i upewnij się, że zadanie zostało zatrzymane:  
   ![graphic29][graphic29] Zaczekaj na zatrzymanie zadania.
5. Znajdź wejścia/wyjścia, który chcesz obrócić poświadczeń i przejdź do niego:  
   ![graphic30][graphic30]
6. Przejdź do części 3.

## <a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Część 3: Edytowanie poświadczeń na zadania usługi analiza strumienia
### <a name="blob-storagetable-storage"></a>Magazyn obiektów blob magazynu/tabeli.
1. Znajdź pole klucz konta magazynu i wkleić klucz wygenerowanym:  
   ![graphic31][graphic31]
2. Kliknij polecenie Zapisz i sprawdź, zapisywania zmian:  
   ![graphic32][graphic32]
3. Test połączenia zostaną automatycznie uruchomione po zapisaniu zmian, upewnij się, to znaczy został pomyślnie przekazany.
4. Przejdź do części 4.

### <a name="event-hubs"></a>Centra zdarzeń
1. Znajdź pole klucza zasad Centrum zdarzeń i wkleić klucz wygenerowanym:  
   ![graphic33][graphic33]
2. Kliknij polecenie Zapisz i sprawdź, zapisywania zmian:  
   ![graphic34][graphic34]
3. Test połączenia zostaną automatycznie uruchomione po zapisaniu zmian, upewnij się, że został przekazany pomyślnie.
4. Przejdź do części 4.

### <a name="power-bi"></a>Power BI
1. Kliknij przycisk Odnów autoryzacji:  

   ![graphic35][graphic35]
2. Otrzymasz po potwierdzeniu:  

   ![graphic36][graphic36]
3. Kliknij polecenie Zapisz i sprawdź, zapisywania zmian:  
   ![graphic37][graphic37]
4. Test połączenia zostaną automatycznie uruchomione po zapisaniu zmian, upewnij się, że jej został pomyślnie przekazany.
5. Przejdź do części 4.

### <a name="sql-database"></a>SQL Database
1. Znajdź pola Nazwa użytkownika i hasło i Wklej nowo utworzonego zestawu poświadczeń do nich:  
   ![graphic38][graphic38]
2. Kliknij polecenie Zapisz i sprawdź, zapisywania zmian:  
   ![graphic39][graphic39]
3. Test połączenia zostaną automatycznie uruchomione po zapisaniu zmian, upewnij się, że został przekazany pomyślnie.  
4. Przejdź do części 4.

## <a name="part-4-starting-your-job-from-last-stopped-time"></a>Część 4: Uruchamianie zadania od ostatniego zatrzymania
1. Opuścić wejścia/wyjścia:  
   ![graphic40][graphic40]
2. Kliknij polecenie Start:  
   ![graphic41][graphic41]
3. Wybierz czas ostatniego zatrzymania, a następnie kliknij przycisk OK:  
   ![graphic42][graphic42]
4. Przejdź do części 5.  

## <a name="part-5-removing-the-old-set-of-credentials"></a>Część 5: Usuwanie starego zestaw poświadczeń
Ta część dotyczy wejścia/wyjścia:

* Blob Storage
* Usługa Event Hubs
* SQL Database
* Table Storage

### <a name="blob-storagetable-storage"></a>Magazyn obiektów blob magazynu/tabeli.
Powtórz część 1 dla klucza dostępu, który był wcześniej używany przez zadanie Aby odnowić teraz nieużywane klucz dostępu.

### <a name="event-hubs"></a>Centra zdarzeń
Powtórz część 1 dla klucza, który był wcześniej używany przez zadanie Aby odnowić klucz teraz nieużywane.

### <a name="sql-database"></a>SQL Database
1. Wróć do okna zapytania z część 1 krok 7 i wpisz następujące zapytanie, zastępując < previous_login_name > Nazwa użytkownika, który był wcześniej używany przez zadania:  
   `DROP LOGIN <previous_login_name>`  
2. Kliknij przycisk Uruchom:  
   ![graphic43][graphic43]  

Należy pobrać następujące potwierdzenia: 

    Command(s) completed successfully.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

