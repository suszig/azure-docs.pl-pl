<properties
   pageTitle="Instalacja programu Visual Studio i narzędzi SSDT dla usługi SQL Data Warehouse | Microsoft Azure"
   description="Instalacja programu Visual Studio i narzędzi SQL Server Data Tools (SSDT) dla usługi Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>


# Instalacja programu Visual Studio 2015 i narzędzi SSDT dla usługi SQL Data Warehouse

Do tworzenia aplikacji dotyczących usługi SQL Data Warehouse zalecamy używanie programu Visual Studio 2015 z najnowszą wersją narzędzi SQL Server Data Tools (SSDT).  W celu zapewnienia zgodności z poprzednimi wersjami możliwa jest również obsługa programu Visual Studio 2013 Update 5 z narzędziami SSDT.  

Użycie programu Visual Studio z narzędziami SSDT pozwala wizualnie eksplorować tabele, widoki, procedury składowane i wiele innych obiektów w usłudze SQL Data Warehouse przy użyciu Eksploratora obiektów SQL Server, a także wykonywać zapytania.

> [AZURE.NOTE] Usługa SQL Data Warehouse jeszcze nie obsługuje projektów bazy danych programu Visual Studio.  Ta funkcja zostanie dodana w przyszłej wersji.

## Krok 1: instalacja programu Visual Studio 2015

Skorzystaj z poniższych linków, aby pobrać i zainstalować program Visual Studio 2015. Jeśli masz już zainstalowany program Visual Studio 2013 lub 2015, przejdź od razu do kroku 2, aby zainstalować narzędzia SSDT.

1. [Pobierz program Visual Studio 2015][].
2. Postępuj zgodnie z przewodnikiem [Instalowanie programu Visual Studio][] w witrynie MSDN i wybierz domyślne konfiguracje.

## Krok 2: instalowanie narzędzi SSDT

Aby zainstalować narzędzia SSDT dla programu Visual Studio, po prostu wyszukaj aktualizację SSDT z poziomu programu Visual Studio w następujący sposób.

1. W programie Visual Studio kliknij menu **Narzędzia** / **Rozszerzenia i aktualizacje...** / **Aktualizacje**
2. Wybierz opcję **Aktualizacje produktu** i poszukaj **Microsoft SQL Server Update do oprzyrządowania bazy danych**

Jeśli aktualizacja nie zostanie znaleziona, zapewne masz już zainstalowaną najnowszą wersję.  Aby upewnić się, że narzędzia SSDT są już zainstalowane, kliknij menu **Pomoc** / **Microsoft Visual Studio — informacje** i poszukaj narzędzi SQL Server Data Tools na liście.  Najnowsza wersja narzędzi SSDT to 14.0.60525.0.  Jeśli opcja instalacji nie jest dostępna w programie Visual Studio, możesz zamiast tego odwiedzić stronę [Download SQL Server Data Tools (SSDT)][] (Pobieranie narzędzi SSDT), aby pobrać i zainstalować narzędzia SSDT ręcznie.

## Następne kroki

Teraz, kiedy masz najnowszą wersję narzędzi SSDT, możesz przystąpić do [nawiązywania połączenia][] z usługą SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[nawiązywania połączenia]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Pobierz program Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Instalowanie programu Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx



<!--HONumber=Sep16_HO3-->


