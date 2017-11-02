---
title: "Azure Analysis Services — samouczek Adventure Works | Microsoft Docs"
description: "Wprowadzenie do samouczka Adventure Works dla usług Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 0e223222c482d6d3aeaed85388f3a1ce1b53a78d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services – samouczek Adventure Works

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ten samouczek zawiera lekcje na temat sposobu tworzenia i wdrażania modelu tabelarycznego na poziomie zgodności 1400 przy użyciu programu [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  

Jeśli usługi Analysis Services i modelowanie tabelaryczne to dla Ciebie nowość, ukończenie tego samouczka jest najszybszym sposobem nauczenia się, jak tworzyć i wdrażać podstawowy model tabelaryczny. Po spełnieniu wymagań wstępnych ukończenie samouczka powinno zająć od dwóch do trzech godzin.  
  
## <a name="what-you-learn"></a>Omawiane zagadnienia   
  
-   Tworzenie nowego projektu modelu tabelarycznego na **poziomie zgodności 1400** w programie SSDT.
  
-   Importowanie danych z relacyjnej bazy danych do projektu modelu tabelarycznego.  
  
-   Tworzenie relacji między tabelami w modelu oraz zarządzanie nimi.  
  
-   Tworzenie kolumn obliczeniowych, miar i kluczowych wskaźników wydajności, które pomagają użytkownikom analizować metryki o krytycznym znaczeniu dla firmy.  
  
-   Tworzenie perspektyw i hierarchii, które ułatwiają użytkownikom przeglądanie danych modelu za pośrednictwem widoków dopasowanych do danej firmy lub aplikacji, oraz zarządzanie nimi.  
  
-   Tworzenie partycji dzielących dane tabelaryczne na mniejsze części logiczne, które mogą być przetwarzane niezależnie od innych partycji.  
  
-   Zabezpieczanie obiektów i danych modelu przez tworzenie ról z użytkownikami jako elementami członkowskimi.  
  
-   Wdrażanie modelu tabelarycznego na serwerze usług **Azure Analysis Services** lub lokalnym serwerze SQL Server 2017 Analysis Services.  
  
## <a name="prerequisites"></a>Wymagania wstępne  
Do ukończenia tego samouczka niezbędne są następujące elementy:  
  
-   Wystąpienie usług Azure Analysis Services lub serwera SQL Server 2017 Analysis Services umożliwiające wdrożenie modelu. Zarejestruj się, aby uzyskać dostęp do [wersji próbnej usług Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) i [utworzyć serwer](../analysis-services-create-server.md). Możesz też zarejestrować się i pobrać serwer [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp). 

-   Program SQL Server Data Warehouse lub usługa Azure SQL Data Warehouse z [przykładową bazą danych AdventureWorksDW2014](http://go.microsoft.com/fwlink/?LinkID=335807). Ta przykładowa baza danych zawiera dane niezbędne do wykonania tego samouczka. Pobierz [bezpłatne wersje serwera SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads). Możesz też zarejestrować się i uzyskać bezpłatną [wersję próbną bazy danych Azure SQL Database](https://azure.microsoft.com/services/sql-database/). 

    **Ważne:** jeśli przykładowa baza danych została zainstalowana na lokalnym serwerze SQL Server i model jest wdrażany na serwerze usług Azure Analysis Services, wymagana jest [lokalna brama danych](../analysis-services-gateway.md).

-   Najnowsza wersja programu [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).

-   Najnowsza wersja programu [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).    

-   Aplikacja kliencka, na przykład [Power BI Desktop](https://powerbi.microsoft.com/desktop/) lub program Excel. 

## <a name="scenario"></a>Scenariusz  
Ten samouczek jest oparty na fikcyjnej firmie Adventure Works Cycles. Adventure Works to duża, międzynarodowa firma produkcyjna zajmująca się wytwarzaniem rowerów, części i akcesoriów oraz ich dystrybucją na rynkach w Ameryce Północnej, Europie i Azji. Firma zatrudnia 500 pracowników. Ponadto firma Adventure Works zatrudnia kilka regionalnych zespołów sprzedaży obsługujących poszczególne rynki. Twój projekt polega na utworzeniu modelu tabelarycznego dla użytkowników z działu sprzedaży i marketingu, który będzie służyć do analizowania danych dotyczących sprzedaży internetowej dostępnych w bazie danych AdventureWorksDW.  
  
Ukończenie tego samouczka wymaga wykonania szeregu lekcji. Każda lekcja obejmuje zadania do wykonania. Do ukończenia lekcji niezbędne jest wykonanie poszczególnych zadań w odpowiedniej kolejności. Chociaż dana lekcja może obejmować kilka zadań, których wykonanie daje podobne wyniki, to sposób wykonania poszczególnych zadań może się nieco różnić. Ta metoda ma pokazać, że często istnieje więcej niż jeden sposób wykonania zadania, oraz zachęcić Cię do wykorzystania umiejętności nabytych w toku wcześniejszych lekcji i zadań.  
  
Celem lekcji jest przeprowadzenie Cię przez proces tworzenia podstawowego modelu tabelarycznego z użyciem wielu funkcji dostępnych w programie SSDT. Poszczególne lekcje wykorzystują materiał z wcześniejszych lekcji, należy je ukończyć w podanej kolejności.
  
Ten samouczek nie zawiera lekcji dotyczących zarządzania serwerem w witrynie Azure Portal, zarządzania serwerem lub bazą danych przy użyciu programu SSMS bądź używania aplikacji klienckiej do przeglądania danych modelu. 


## <a name="lessons"></a>Lekcje  
Ten samouczek obejmuje następujące lekcje:  
  
|Lekcja|Szacowany czas trwania|  
|----------|------------------------------|  
|[1. Tworzenie nowego projektu modelu tabelarycznego](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 minut|  
|[2. Pobieranie danych](../tutorials/aas-lesson-2-get-data.md)|10 minut|  
|[3. Oznaczanie jako tabeli dat](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 minuty|  
|[4. Tworzenie relacji](../tutorials/aas-lesson-4-create-relationships.md)|10 minut|  
|[5. Tworzenie kolumn obliczeniowych](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 minut|
|[6. Tworzenie miar](../tutorials/aas-lesson-6-create-measures.md)|30 minut|  
|[7. Tworzenie kluczowych wskaźników wydajności (KPI)](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 minut|  
|[8. Tworzenie perspektyw](../tutorials/aas-lesson-8-create-perspectives.md)|5 minut|  
|[9. Tworzenie hierarchii](../tutorials/aas-lesson-9-create-hierarchies.md)|20 minut|  
|[10. Tworzenie partycji](../tutorials/aas-lesson-10-create-partitions.md)|15 minut|  
|[11. Tworzenie ról](../tutorials/aas-lesson-11-create-roles.md)|15 minut|  
|[12. Analiza w programie Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 minut| 
|[13. Wdrażanie](../tutorials/aas-lesson-13-deploy.md)|5 minut|  
  
## <a name="supplemental-lessons"></a>Lekcje uzupełniające  
Te lekcje nie są wymagane do ukończenia samouczka, ale mogą pomóc lepiej zrozumieć zaawansowane funkcje tworzenia modelu tabelarycznego.  
  
|Lekcja|Szacowany czas trwania|  
|----------|------------------------------|  
|[Wiersze szczegółów](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 minut|
|[Zabezpieczenia dynamiczne](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 minut|
|[Niewyrównane hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 minut| 

  
## <a name="next-steps"></a>Następne kroki  
Aby rozpocząć, przejdź do [lekcji 1 „Tworzenie nowego projektu modelu tabelarycznego”](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
  
  

