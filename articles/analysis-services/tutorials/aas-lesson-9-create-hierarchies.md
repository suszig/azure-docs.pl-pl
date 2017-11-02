---
title: "Samouczek Azure Analysis Services: lekcja 9 — tworzenie hierarchii | Microsoft Docs"
description: 
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
ms.openlocfilehash: 28e4e24f5706e88ede25060d5459617befd4aea9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="lesson-9-create-hierarchies"></a>Lekcja 9. Tworzenie hierarchii

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

W tej lekcji utworzysz hierarchie. Hierarchie to grupy kolumn uporządkowane według poziomów. Na przykład hierarchia Geografia może zawierać poziomy podrzędne Kraj, Województwo, Powiat i Miasto. Na liście pól aplikacji raportujących klienta hierarchie mogą występować niezależnie od innych kolumn, co ułatwia nawigację i dołączanie do raportów. Aby dowiedzieć się więcej, zobacz temat [Hierarchie](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular).
  
Aby utworzyć hierarchie, należy użyć projektanta modeli w *widoku diagramu*. Tworzenie hierarchii i zarządzanie nimi nie jest obsługiwane w widoku danych.  
  
Szacowany czas trwania lekcji: **20 minut**  
  
## <a name="prerequisites"></a>Wymagania wstępne  
Ten temat stanowi część samouczka modelowania tabelarycznego, który należy wykonać w podanej kolejności. Przed przystąpieniem do wykonywania zadań w tej lekcji należy ukończyć lekcję poprzednią: [Lekcja 8: Tworzenie perspektyw](../tutorials/aas-lesson-8-create-perspectives.md).  
  
## <a name="create-hierarchies"></a>Tworzenie hierarchii  
  
#### <a name="to-create-a-category-hierarchy-in-the-dimproduct-table"></a>Tworzenie hierarchii kategorii w tabeli DimProduct  
  
1.  W projektancie modeli (w widoku diagramu) kliknij prawym przyciskiem myszy tabelę **DimProduct**, a następnie pozycję **Utwórz hierarchię**. Nowa hierarchia pojawi się w dolnej części okna tabeli. Zmień nazwę hierarchii na **Category** (Kategoria).  
  
2.  Kliknij i przeciągnij kolumnę **ProductCategoryName** do nowej hierarchii **Category**.  
  
3.  W hierarchii **Category** kliknij prawym przyciskiem myszy pozycje **ProductCategoryName** > **Zmień nazwę**, a następnie wpisz nazwę **Category**.  
  
    > [!NOTE]  
    > Zmiana nazwy kolumny w hierarchii nie powoduje zmiany nazwy tej kolumny w tabeli. Kolumna w hierarchii jest jedynie reprezentacją kolumny w tabeli.  
  
4.  Kliknij i przeciągnij kolumnę **ProductSubcategoryName** do nowej hierarchii **Category**. Zmień jej nazwę na **Subcategory** (Podkategoria). 
  
5.  Kliknij prawym przyciskiem myszy kolumnę **ModelName** > **Dodaj do hierarchii**, a następnie wybierz pozycję **Category**. Zmień jej nazwę na **Model**.

6.  Na zakończenie dodaj kolumnę **EnglishProductName** do hierarchii Category. Zmień jej nazwę na **Product** (Produkt).  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="to-create-hierarchies-in-the-dimdate-table"></a>Tworzenie hierarchii w tabeli DimDate  
  
1.  W tabeli **DimDate** utwórz hierarchię o nazwie **Calendar** (Kalendarz).  
  
3.  Dodaj następujące kolumny w podanej kolejności:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  W tabeli **DimDate** utwórz hierarchię o nazwie **Fiscal** (Fiskalne). Dodaj następujące kolumny w podanej kolejności:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Na koniec w tabeli **DimDate** utwórz hierarchię **ProductionCalendar**. Dodaj następujące kolumny w podanej kolejności:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>Co dalej?
[Lekcja 10. Tworzenie partycji](../tutorials/aas-lesson-10-create-partitions.md). 
  
  
