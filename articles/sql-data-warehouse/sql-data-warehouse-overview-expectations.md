<properties
   pageTitle="Wersja zapoznawcza usługi SQL Data Warehouse — oczekiwania | Microsoft Azure"
   description="Podsumowanie możliwości publicznej wersji zapoznawczej i celów firmy Microsoft dotyczących ogólnej dostępności usługi SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/05/2016"
   ms.author="nicw;barbkess;sonyama"/>


# Wersja zapoznawcza usługi SQL Data Warehouse — oczekiwania

W tym artykule opisano możliwości wersji zapoznawczej usługi SQL Data Warehouse i cele firmy Microsoft dotyczące ogólnej dostępności usługi. Te informacje będą aktualizowane na bieżąco w miarę zwiększania możliwości publicznej wersji zapoznawczej.

Cele firmy Microsoft dotyczące usługi SQL Data Warehouse:

- Przewidywalna wydajność i skalowalność liniowa do poziomu petabajtów danych
- Wysoka niezawodność wszystkich operacji magazynu danych
- Krótki czas między załadowaniem danych i dostępem do nich dla danych relacyjnych i nierelacyjnych

W trakcie udostępniania wersji zapoznawczej usługi SQL Data Warehouse firma Microsoft prowadzi ciągłe prace mające na celu osiągnięcie tych celów.

## Przewidywalna i skalowalna wydajność

Usługa SQL Data Warehouse platformy Azure wprowadza jednostki magazynu danych (DWU, Data Warehouse Unit) jako sposób pomiaru zasobów obliczeniowych (procesorów, pamięci, we/wy magazynu) dostępnych dla magazynu danych. Zwiększenie liczby jednostek DWU powoduje zwiększenie zasobów. W miarę zwiększania liczby jednostek DWU usługa SQL Data Warehouse uruchomia operacje równoległe (np. ładowanie danych lub zapytanie) pomiędzy bardziej rozproszonymi zasobami. Zmniejsza to opóźnienie i zwiększa wydajność.

Każdy magazyn danych ma dwie podstawowe metryki wydajności:

- Szybkość ładowania. Liczba rekordów, które mogą zostać załadowane do magazynu danych w ciągu sekundy. Firma Microsoft mierzy liczbę rekordów, które można importować za pomocą programu PolyBase z magazynu obiektów blob platformy Azure do tabeli z indeksem klastrowanym typu Column-Store.
- Szybkość skanowania. Liczba rekordów, które można pobrać sekwencyjnie z magazynu danych w ciągu sekundy. Firma Microsoft mierzy liczbę rekordów zwróconych przez zapytanie wykonywane w oparciu o klastrowany indeks magazynu kolumn.

Firma Microsoft notuje istotną poprawę wydajności w niektórych obszarach i wkrótce udostępni oczekiwane wskaźniki. Podczas udostępniania wersji zapoznawczej firma Microsoft będzie wprowadzać ciągłe ulepszenia (np. zwiększenie kompresji i buforowania) w celu poprawy tych wskaźników i zapewnienia ich przewidywalnego skalowania.  

## Ochrona danych

Usługa SQL Data Warehouse przechowuje wszystkie dane w magazynie Azure za pomocą lokalnie nadmiarowego magazynu. Wiele synchronicznych kopii danych jest przechowywanych w lokalnym centrum danych, co gwarantuje przezroczystą ochronę danych w przypadku zlokalizowanych awarii. 

## Tworzenie kopii zapasowych

Usługa SQL Data Warehouse platformy Azure wykonuje kopię zapasową wszystkich danych nie rzadziej niż co 8 godzin przy użyciu programu Azure Storage Snapshots. Te migawki są przechowywane przez 7 dni. Pozwala to na przywrócenie danych do co najmniej 21 momentów w ciągu ostatnich 7 dni do chwili, w której została wykonana ostatnia migawka. Dane z migawki można przywrócić przy użyciu programu PowerShell lub interfejsów API REST.

## Niezawodność kwerend

Usługa SQL Data Warehouse jest zbudowana w oparciu o architekturę masowego przetwarzania równoległego (MPP, massively parallel processing). Usługa SQL Data Warehouse automatycznie wykrywa i wykonuje migrację awarii węzłów obliczeniowych i sterujących. Jednak operacja (np. ładowanie danych lub zapytanie) może zakończyć się niepowodzeniem w wyniku awarii węzła lub migracji. Podczas udostępniania wersji zapoznawczej firma Microsoft wprowadza ciągłe ulepszenia, prowadzące do pomyślnego ukończenia operacji niezależnie od awarii węzłów.


## Uaktualnianie i przestoje

Usługa SQL Data Warehouse będzie okresowo uaktualniania w celu wprowadzenia nowych funkcji i zainstalowania krytycznych poprawek.  Te uaktualnienia mogą stanowić zakłócenie i nie są w tej chwili wykonywane zgodnie z przewidywalnym harmonogramem.  Jeśli okaże się, że ten proces sprawia za dużo problemów, firma Microsoft zachęca do [utworzenia biletu pomocy technicznej][], dzięki któremu pomoże rozwiązać ten problem.


## Następne kroki

[Rozpoczynanie pracy][] z publiczną wersją zapoznawczą.

<!--Image references-->

<!--Article references-->
[utworzenia biletu pomocy technicznej]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Rozpoczynanie pracy]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other Web references-->



<!--HONumber=Jun16_HO2-->


