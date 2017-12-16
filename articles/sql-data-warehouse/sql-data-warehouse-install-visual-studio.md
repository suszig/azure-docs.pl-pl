---
title: "Instalacja programu Visual Studio i narzędzi SSDT dla usługi SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Instalacja programu Visual Studio i narzędzi SQL Server Data Tools (SSDT) dla usługi Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 12/14/2017
ms.author: kevin;barbkess
ms.openlocfilehash: e8170eefb2e359719684e08749180a4e7784f9b6
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/16/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Instalacja programu Visual Studio i narzędzi SSDT dla usługi SQL Data Warehouse
Do tworzenia aplikacji dla usługi SQL Data Warehouse, firma Microsoft zaleca używanie najnowszej wersji programu Visual Studio z najnowszej wersji programu SQL Server Data Tools (SSDT).  W celu zapewnienia zgodności z poprzednimi wersjami możliwa jest również obsługa programu Visual Studio 2013 Update 5 z narzędziami SSDT.  

Użycie programu Visual Studio z narzędziami SSDT pozwala wizualnie eksplorować tabele, widoki, procedury składowane i wiele innych obiektów w usłudze SQL Data Warehouse przy użyciu Eksploratora obiektów SQL Server, a także wykonywać zapytania.

> [!NOTE]
> Usługa SQL Data Warehouse jeszcze nie obsługuje projektów bazy danych programu Visual Studio.  Ta funkcja zostanie dodana w przyszłej wersji. Aby otrzymywać okresowe aktualizacje na tej funkcji, sprawdź oddawać głosy na [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Krok 1: Zainstaluj program Visual Studio
Skorzystaj z poniższych linków, aby pobrać i zainstalować program Visual Studio. Jeśli masz już programu Visual Studio 2013 lub nowszy, możesz przejść do kroku 2, należy zainstalować narzędzia SSDT.

1. [Pobierz program Visual Studio][].
2. Postępuj zgodnie z [Instalowanie programu Visual Studio] [ Installing Visual Studio] prowadzą w witrynie MSDN i wybierz domyślne konfiguracje.

## <a name="step-2-install-ssdt"></a>Krok 2: instalowanie narzędzi SSDT
Aby zainstalować narzędzia SSDT dla programu Visual Studio, po prostu wyszukaj aktualizację SSDT z poziomu programu Visual Studio w następujący sposób.

1. W programie Visual Studio kliknij **narzędzia** / **rozszerzenia i aktualizacje...** / **Aktualizacje**
2. Wybierz opcję **Aktualizacje produktu** i poszukaj **Microsoft SQL Server Update do oprzyrządowania bazy danych**

Jeśli aktualizacja nie zostanie znaleziona, zapewne masz już zainstalowaną najnowszą wersję.  Aby upewnić się, że narzędzia SSDT są już zainstalowane, kliknij menu **Pomoc** / **Microsoft Visual Studio — informacje** i poszukaj narzędzi SQL Server Data Tools na liście.  Najnowsza wersja narzędzi SSDT to 14.0.60525.0.  Jeśli opcja instalacji nie jest dostępna w programie Visual Studio, możesz też użytkownik może odwiedzić [pobieranie narzędzi SSDT] [ SSDT Download] strony, aby pobrać i zainstalować narzędzia SSDT ręcznie.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz najnowszą wersję narzędzi SSDT, możesz przystąpić do [połączyć] [ connect] do magazynu danych SQL.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Pobierz program Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
