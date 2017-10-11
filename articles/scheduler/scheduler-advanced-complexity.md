---
title: "Sposób tworzenia harmonogramów złożone i zaawansowane cyklu z harmonogramem Azure"
description: "Sposób tworzenia harmonogramów złożone i zaawansowane cyklu z harmonogramem Azure"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 20c3e3c1cb85308cad47054c2efa87f61cae0f22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Sposób tworzenia harmonogramów złożone i zaawansowane cyklu z harmonogramem Azure
## <a name="overview"></a>Omówienie
Istotą Harmonogram systemu Azure jest zadanie *harmonogram*. Harmonogram Określa, kiedy i jak planista wykonuje zadanie.

Harmonogram systemu Azure można określić różne harmonogramy jednorazowe i cyklicznego dla zadania. *Jednorazowe* harmonogramy wyzwalać raz o określonej godzinie — skutecznie znajdują się one *cyklicznego* harmonogramy, które są wykonywane tylko raz. Harmonogramów cyklicznych wyzwalać na wstępnie określoną częstotliwością.

Z tego rodzaju elastyczności Harmonogram systemu Azure umożliwia obsługuje wiele różnych scenariuszy biznesowych:

* Porządkowanie danych okresowe — np. codziennie, Usuń wszystkie tweetów starsze niż 3 miesiące
* Archiwizacja — np. co miesiąc wypychanie historii faktury do usługi tworzenia kopii zapasowej
* Żądania dotyczące danych zewnętrznych — np. co 15 minut, pobierają nowe ski prognoza pogody z NOAA
* Przetwarzanie — np. każdy dzień tygodnia, poza godzinami szczytu obrazu, użyj przetwarzania do skompresowania obrazy przekazać tego dnia w chmurze

W tym artykule firma Microsoft przeprowadzenie przykład zadań, które można utworzyć z Harmonogram systemu Azure. Firma Microsoft udostępnia dane JSON, który opisano każdy z harmonogramów. Jeśli używasz [interfejsu API REST harmonogramu](https://msdn.microsoft.com/library/mt629143.aspx), można użyć tego samego JSON dla [Tworzenie zadania harmonogramu Azure](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Obsługiwane scenariusze
Wiele przykładów w tym temacie przedstawiono szerokość scenariusze, które obsługuje Harmonogram systemu Azure. Ogólnie te przykłady przedstawiają sposób tworzenia harmonogramów wielu wzorców zachowanie, łącznie z tymi poniżej:

* Uruchom raz od określonej daty i godziny
* Uruchom i powtarzanie wiele razy jawne
* Natychmiastowe uruchomienie i powtarzanie
* Uruchom i powtarzanie co  *n*  minuty, godziny, dni, tygodnie lub miesiące, począwszy od określonego czasu
* Uruchom i powtarzanie częstotliwością co tydzień lub co miesiąc, ale tylko w określone dni, określone dni tygodnia lub określone dni miesiąca
* Uruchom i powtarzanie na wiele razy w okresie — np. ostatni piątek i poniedziałek każdego miesiąca lub 5:15:00 i 17:15:00 każdego dnia

## <a name="dates-and-datetimes"></a>Daty i dat i godzin
Postępuj zgodnie z daty w zadania harmonogramu Azure [specyfikacji ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) i zawierają tylko data.

Postępuj zgodnie z odwołaniami do daty i godziny w Azure harmonogramu zadań [specyfikacji ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) i zawierać części zarówno datę i godzinę. Daty i godziny, która nie określa przesunięcie UTC zakłada się, że czas UTC.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Korzystanie z formatu JSON i interfejsu API REST do tworzenia harmonogramów
Aby utworzyć przy użyciu prostego harmonogramu [interfejsu API REST harmonogramu Azure](https://msdn.microsoft.com/library/mt629143), pierwszy [zarejestrować subskrypcji u dostawcy zasobów](https://msdn.microsoft.com/library/azure/dn790548.aspx) (nazwy dostawcy dla harmonogramu jest *Microsoft.Scheduler* ), następnie [tworzenie kolekcji zadań](https://msdn.microsoft.com/library/mt629159.aspx), a na końcu [utworzyć zadanie](https://msdn.microsoft.com/library/mt629145.aspx). Podczas tworzenia zadania można określić ustawienia planowania i cyklu przy użyciu formatu JSON, tak jak fragmentem książki poniżej:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // optional, how often to fire (default to 1)
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default to recur infinitely)
            "endTime": "2012-11-04",      // optional (default to recur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Omówienie: Zadanie schematu podstawy
Poniższa tabela zawiera omówienie głównych elementów odnoszące się do ponownego i Planowanie zadania:

| **Nazwy JSON** | **Opis** |
|:--- |:--- |
| ***czas rozpoczęcia*** |*wartość startTime* jest daty i godziny. Proste harmonogramów *startTime* jest pierwsze wystąpienie i złożone harmonogramów, zadanie zostanie uruchomione nie wcześniej niż *startTime*. |
| ***cyklu*** |*Cyklu* obiektu określa zasady cyklu zadania i cyklu zadanie będzie wykonywane za pomocą. Obiekt cyklu obsługuje elementy *częstotliwości, interwał, endTime, count,* i *harmonogram*. Jeśli *cyklu* jest zdefiniowany, *częstotliwość* jest wymagana; inne elementy *cyklu* są opcjonalne. |
| ***częstotliwość*** |*Częstotliwość* ciąg reprezentujący jednostkę częstotliwość powtarzania zadania. Obsługiwane wartości to *"min", "Godzina", "day", "tydzień"* lub *"miesiąc".* |
| ***Interwał*** |*Interwał* jest dodatnią liczbą całkowitą i określa interwał *częstotliwość* określający, jak często zadanie zostanie uruchomione. Na przykład jeśli *interwał* 3 i *częstotliwość* jest "tydzień" zadanie wystąpi co 3 tygodni. Harmonogram systemu Azure obsługuje maksymalnie *interwał* 18 miesięcy do miesięcznej częstotliwości 78 tygodniach częstotliwości co tydzień lub 548 dni dzienną częstotliwość. Godziny i minuty częstotliwość obsługiwanych zakres to 1 < = *interwał* < = 1000. |
| ***wartość endTime*** |*EndTime* ciąg Określa przeszłości daty i godziny, które nie mają być wykonywane zadania. Nie jest prawidłową ma *endTime* w przeszłości. Jeśli nie *endTime* lub jest określona liczba, zadanie działa nieograniczonej. Zarówno *endTime* i *liczby* nie może zostać uwzględnione w tym samym zadaniu. |
| ***Liczba*** |<p>*Liczba* jest dodatnią liczbą całkowitą (większe od zera) określa, ile razy to zadanie powinno zostać uruchomione przed zakończeniem.</p><p>*Liczba* reprezentuje liczbę razy, że zadanie będzie uruchamiane przed określane jako ukończone. Na przykład dla zadania, która jest wykonywana codziennie o *liczba* 5 i rozpoczęcie poniedziałek, zadanie zostało ukończone po wykonaniu w piątek. Jeśli data rozpoczęcia przypada w przeszłości, pierwszy wykonywania jest obliczana na podstawie czasu utworzenia.</p><p>Jeśli nie *endTime* lub *liczba* określono, że zadanie będzie uruchamiane nieograniczonej. Zarówno *endTime* i *liczby* nie może zostać uwzględnione w tym samym zadaniu.</p> |
| ***Harmonogram*** |Zadanie o określonej częstotliwości zmienia jego cyklu na podstawie harmonogramu cyklu. A *harmonogram* zawiera zmiany na podstawie minuty, godziny, dni tygodnia, dni miesiąca i numer tygodnia. |

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Omówienie: Zadania wartości domyślne schematu, ograniczenia i przykłady
Po tym omówieniu omówimy każdego z tych elementów szczegółowo opcji.

| **Nazwy JSON** | **Typ wartości** | **Wymagane?** | **Wartość domyślna** | **Prawidłowe wartości** | **Przykład** |
|:--- |:--- |:--- |:--- |:--- |:--- |
| ***czas rozpoczęcia*** |Ciąg |Nie |Brak |ISO 8601 daty i godziny |<code>"startTime" : "2013-01-09T09:30:00-08:00"</code> |
| ***cyklu*** |Obiekt |Nie |Brak |Cyklu |<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code> |
| ***częstotliwość*** |Ciąg |Tak |Brak |"min", "Godzina", "day", "tydzień", "miesiąc" |<code>"frequency" : "hour"</code> |
| ***Interwał*** |Numer |Nie |1 |1 do 1000. |<code>"interval":10</code> |
| ***wartość endTime*** |Ciąg |Nie |Brak |Wartość daty i godziny reprezentującą godzinę w przyszłości |<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
| ***Liczba*** |Numer |Nie |Brak |>= 1 |<code>"count": 5</code> |
| ***Harmonogram*** |Obiekt |Nie |Brak |Obiekt harmonogramu |<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code> |

## <a name="deep-dive-starttime"></a>Szczegółowe informacje na temat: *startTime*
W poniższej tabeli sposobu przechwytywania *startTime* kontroluje sposób uruchamiania zadania.

| **wartość startTime** | **Brak cyklu** | **Cyklu. Nie harmonogramu** | **Cyklu z harmonogramem** |
|:--- |:--- |:--- |:--- |
| **Brak godziny rozpoczęcia** |Uruchom raz natychmiast |Uruchom raz natychmiast. Uruchom podczas kolejnych wykonań kodu oparte na obliczanie od czasu ostatniego wykonania |<p>Uruchom raz natychmiast</p><p>Uruchom podczas kolejnych wykonań kodu na podstawie harmonogramu cyklu</p> |
| **Godzina rozpoczęcia w przeszłości** |Uruchom raz natychmiast |<p>Oblicz pierwszy czas wykonania przyszłych po czasie rozpoczęcia, a następnie uruchom w tym czasie</p><p>Uruchom oncalculating podczas kolejnych wykonań kodu na podstawie od czasu ostatniego wykonania</p><p>Zobacz przykład pod tą tabelą, aby uzyskać więcej informacji</p> |<p>Zadania uruchamiania *nie sooner niż* określonym czasie rozpoczęcia. Pierwsze wystąpienie jest na podstawie harmonogramu obliczana na podstawie czasu rozpoczęcia</p><p>Uruchom podczas kolejnych wykonań kodu na podstawie harmonogramu cyklu</p> |
| **Godzina rozpoczęcia w przyszłości lub obecnie** |Uruchom raz o określonym czasie rozpoczęcia |<p>Uruchom raz o określonym czasie rozpoczęcia</p><p>Uruchom podczas kolejnych wykonań kodu oparte na obliczanie od czasu ostatniego wykonania</p> |<p>Zadania uruchamiania *nie sooner niż* określonym czasie rozpoczęcia. Pierwsze wystąpienie jest na podstawie harmonogramu obliczana na podstawie czasu rozpoczęcia</p><p>Uruchom podczas kolejnych wykonań kodu na podstawie harmonogramu cyklu</p> |

Zobacz przykład co się stanie, gdy *startTime* przypada w przeszłości, z *cyklu* , lecz nie *harmonogram*.  Załóżmy, że bieżący czas jest 2015-04-08 13:00, *startTime* jest 2015-04-07 14:00 i *cyklu* to 2 dni (zdefiniowane z *częstotliwość*: dzień i *interwał*: 2.) Należy pamiętać, że *startTime* przypada w przeszłości i występuje przed bieżącym czasem

W tych warunkach *wykonanie pierwszej* będzie 2015-04-09 14:00\. Aparat harmonogramu oblicza wykonywania wystąpienia od godziny rozpoczęcia.  Wszystkie wystąpienia w przeszłości są odrzucane. Aparat korzysta następne wystąpienie, który występuje w przyszłości.  W tym przypadku *startTime* jest 2015-04-07 godzinie 2:00, więc następne wystąpienie jest 2 dni od tego czasu jest 2015-04-09 2:00 pm.

Należy pamiętać, że pierwszy wykonanie może być tym samym, nawet jeśli startTime 2015-04-05 14:00 lub 14:00\ 2015-04-01. Po wykonaniu pierwszej podczas kolejnych wykonań kodu są obliczane przy użyciu zaplanowanego — tak byłyby w 2015-04-11 godzinie 2:00, następnie 2015-04-13 godzinie 2:00, następnie 2015-04-15 godzinie 2:00, itp.

Na koniec gdy zadanie ma harmonogramu, jeśli godziny i/lub minut nie są ustawione w harmonogramie, one domyślnie godzin i/lub minut pierwszy wykonywania odpowiednio.

## <a name="deep-dive-schedule"></a>Szczegółowe informacje na temat: *harmonogramu*
Z jednej strony *harmonogram* można *limit* Liczba wykonań zadania.  Na przykład, jeśli zadanie o częstotliwości "miesiąc" ma *harmonogram* czy działa tylko dzień 31, zadanie działa tylko miesięcy, które mają 31<sup>st</sup> dnia.

Z drugiej strony *harmonogram* może również *rozwiń* Liczba wykonań zadania. Na przykład, jeśli zadanie o częstotliwości "miesiąc" ma *harmonogram* czy działa na dni miesiąca 1 i 2, zadanie działa na wartość 1<sup>st</sup> i 2<sup>nd</sup> dni miesiąca, a nie tylko raz w miesiącu.

Jeśli określonych jest wiele elementów harmonogramu, kolejność jest od największej do najmniejszej — tygodnia number, miesiąc, dzień, dzień tygodnia, godziny i minuty.

W poniższej tabeli opisano *harmonogram* elementy szczegółowo.

| **Nazwy JSON** | **Opis** | **Prawidłowe wartości** |
|:--- |:--- |:--- |
| **minut** |Minuty, godziny, o której zadanie zostanie uruchomione |<ul><li>Liczba całkowita, lub</li><li>Tablica liczb całkowitych</li></ul> |
| **godziny** |Godziny, dnia, o której zadanie zostanie uruchomione |<ul><li>Liczba całkowita, lub</li><li>Tablica liczb całkowitych</li></ul> |
| **dni tygodnia** |Dni tygodnia zadanie zostanie uruchomione. Można określić tylko z częstotliwością co tydzień. |<ul><li>"Poniedziałek", "Wtorek", "Środa", "Czwartek", "Piątek", "Sobota" i "Niedziela"</li><li>Tablica żadnej z powyższych wartości (tablicy maksymalny rozmiar 7)</li></ul>*Nie* z uwzględnieniem wielkości liter |
| **monthlyOccurrences** |Określa, które dni miesiąca, zadanie zostanie uruchomione. Można określić tylko z częstotliwością miesięcznych. |<ul><li>Tablica obiektów monthlyOccurrence:</li></ul> <pre>{ "day": *day*,<br />  "occurrence": *occurrence*<br />}</pre><p> *dzień* dzień tygodnia zadanie zostanie uruchomione, np. {niedziela} jest niedziela każdego miesiąca. Wymagane.</p><p>Wystąpienie jest *wystąpienie* dzień w miesiącu, np. {niedziela, -1} jest niedzielę ostatniego dnia miesiąca. Opcjonalny.</p> |
| **monthDays** |Dzień miesiąca, który uruchomi zadanie. Można określić tylko z częstotliwością miesięcznych. |<ul><li>Dowolna wartość < = -1 i > =-31.</li><li>Dowolna wartość > = 1 i < = 31.</li><li>Tablica powyżej wartości</li></ul> |

## <a name="examples-recurrence-schedules"></a>Przykłady: Harmonogramy cyklu
Poniżej przedstawiono przykłady różnych harmonogramów cyklu — koncentrujących się na obiekt harmonogramu i jego elementów podrzędnych.

Harmonogramy poniżej wszystkich przyjęto założenie, że *interwał* jest ustawiona na 1\. Ponadto jedną założono prawo częstotliwość zgodnie z nowości w *harmonogram* — np. nie można użyć częstotliwości "day" i ma modyfikację "monthDays" w harmonogramie. Takie ograniczenia opisanych powyżej.

| **Przykład** | **Opis** |
|:--- |:--- |
| <code>{"hours":[5]}</code> |Uruchom na 5: 00 każdego dnia. Harmonogram systemu Azure dopasowania każdej wartości w "Godzina" z każdej wartości w "min", z nich, aby utworzyć listę cały czas, w którym zadanie jest uruchamiana. |
| <code>{"minutes":[15], "hours":[5]}</code> |Uruchom na 5:15:00 każdego dnia |
| <code>{"minutes":[15], "hours":[5,17]}</code> |Uruchom na 5:15:00 a 17:15 codziennie |
| <code>{"minutes":[15,45], "hours":[5,17]}</code> |Uruchom na 5:15:00, 5:45:00, 17:15:00, a 17:45 codziennie |
| <code>{"minutes":[0,15,30,45]}</code> |Uruchom co 15 minut |
| <code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code> |Uruchom co godzinę. To zadanie jest uruchamiane co godzinę. Minuta jest kontrolowany przez *startTime*, jeśli jest określony, lub jeśli nie zostanie określona, w czasie tworzenia. Na przykład, jeśli czas rozpoczęcia i godzina utworzenia (którekolwiek ma zastosowanie) jest godzina 12:25, zadanie będzie uruchamiane w na 00:25, 01:25 02:25,..., 23:25. Harmonogram jest odpowiednikiem o zadanie o *częstotliwość* "Godzina" *interwał* 1 i nie *harmonogram*. Różnica polega na to, że ten harmonogram może zostać wykorzystana z różnymi *częstotliwość* i *interwał* za tworzenie innych zadań. Na przykład jeśli *częstotliwość* zostały "miesiąc", harmonogram może działać tylko raz w miesiącu zamiast codziennie Jeśli *częstotliwość* zostały "dzień" |
| <code>{minutes:[0]}</code> |Uruchom co godzinę w ciągu godziny. To zadanie jest uruchamiane co godzinę, ale na godzinę (np. 00: 00, 1: 00, 2 AM, itp.) Jest to równoważne zadania z częstotliwością "Godzina", startTime z zero minut, a nie harmonogram częstotliwości zostały "day", ale w przypadku "week" lub "month" częstotliwości harmonogramu jest wykonywany tylko jeden dzień tygodnia lub jeden dzień w miesiącu odpowiednio. |
| <code>{"minutes":[15]}</code> |Uruchom po 15 minutach Ostatnia godzina co godzinę. Jest uruchamiana co godzinę, począwszy od 00:15 AM, 1:15:00, 2:15 AM, itp., a koniec 22:15:00 i 23:15:00. |
| <code>{"hours":[17], "weekDays":["saturday"]}</code> |Jednocześnie 17: 00 w soboty co tydzień |
| <code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Jednocześnie 17: 00 w poniedziałek, środę i piątek co tydzień |
| <code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Jednocześnie 17:15:00 a: 45 17 w poniedziałek, środę i piątek co tydzień |
| <code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Uruchom na 5: 00 a 17 w poniedziałek, środę i piątek co tydzień |
| <code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Uruchom na 5:15:00, 5:45:00, 17:15:00, a: 45 17 w poniedziałek, środę i piątek co tydzień |
| <code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Uruchom co 15 minut w dni robocze |
| <code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Uruchom co 15 minut w dni robocze miedzy 9 a 4:45 PM. |
| <code>{"weekDays":["sunday"]}</code> |Uruchom w niedziele podczas uruchamiania |
| <code>{"weekDays":["tuesday", "thursday"]}</code> |We wtorki i czwartki podczas uruchamiania |
| <code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code> |Uruchom o 6: 00 w 28 dnia o co miesiąc (przy założeniu częstotliwość miesiąc) |
| <code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code> |Uruchom o 6: 00 ostatniego dnia miesiąca. Jeśli chcesz uruchomić zadanie ostatniego dnia miesiąca, zamiast dnia 28, 29, 30 i 31 -1. |
| <code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code> |Uruchom o 6: 00 pierwszy i ostatni dzień każdego miesiąca |
| <code>{monthDays":[1,-1]}</code> |W pierwszym i ostatnim dniu co miesiąc podczas uruchamiania |
| <code>{monthDays":[1,14]}</code> |Uruchom pierwszy i czternastego dnia każdego miesiąca, w momencie rozpoczęcia |
| <code>{monthDays":[2]}</code> |Uruchom dnia miesiąca w momencie rozpoczęcia |
| <code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |Uruchom w pierwszym piątek każdego miesiąca o godzinie 5: 00 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |: Uruchomienie na pierwszy piątek każdego miesiąca w momencie rozpoczęcia |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code> |Uruchom na trzeci piątek koniec miesiąca, co miesiąc, w momencie rozpoczęcia |
| <code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Uruchom na pierwszy i ostatni piątek miesiąca w 5:15:00 |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Uruchomienie na pierwszy i ostatni piątek miesiąca w momencie rozpoczęcia |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code> |Uruchom w piątym piątek co miesiąc podczas uruchamiania. Jeśli w miesiącu nie ma żadnych piątej piątek, to nie zostanie uruchomiony, ponieważ zostało zaplanowane do uruchomienia na tylko piątej piątki. Można rozważyć użycie -1 zamiast 5 dla wystąpienia Jeśli chcesz uruchamiać zadania na ostatniego wystąpienia piątek miesiąca. |
| <code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code> |Uruchom co 15 minut na ostatni piątek miesiąca |
| <code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code> |Uruchom na 5:15:00, 5:45:00, 17:15:00 i 5:45 PM 3 środę każdego miesiąca |

## <a name="see-also"></a>Zobacz też
 [Co to jest Scheduler?](scheduler-intro.md)

 [Pojęcia i terminologia dotyczące usługi Azure Scheduler oraz hierarchia jednostek](scheduler-concepts-terms.md)

 [Rozpoczynanie pracy z usługą Scheduler w witrynie Azure Portal](scheduler-get-started-portal.md)

 [Plany i rozliczenia w usłudze Azure Scheduler](scheduler-plans-billing.md)

 [Dokumentacja interfejsu API REST usługi Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja poleceń cmdlet programu PowerShell dla usługi Azure Scheduler](scheduler-powershell-reference.md)

 [Wysoka dostępność i niezawodność usługi Azure Scheduler](scheduler-high-availability-reliability.md)

 [Limity, wartości domyślne i kody błędów usługi Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Uwierzytelnianie połączeń wychodzących usługi Azure Scheduler](scheduler-outbound-authentication.md)

