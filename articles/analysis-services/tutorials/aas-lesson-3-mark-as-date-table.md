---
title: "Samouczek Azure Analysis Services: lekcja 3 — oznaczanie jako tabeli dat | Microsoft Docs"
description: "Opisuje sposób oznaczania jako tabeli dat w projekcie samouczka usług Azure Analysis Services."
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
ms.openlocfilehash: 7a77efb21d38c7c5ff5ca6e17757cbfa9e845ad7
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="lesson-3-mark-as-date-table"></a>Lekcja 3. Oznaczanie jako tabeli dat

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

W lekcji 2 dotyczącej pobierania danych została zaimportowana tabela wymiarów o nazwie DimDate. W modelu tabela ta nosi nazwę DimDate, ale jest również nazywana *tabelą dat*, ponieważ zawiera dane dotyczące dat i godzin.  
  
Zawsze gdy używasz funkcji analizy czasowej języka DAX (np. podczas opisanego poniżej tworzenia miar), należy określić właściwości, które obejmują *tabelę dat* i unikatowy identyfikator *kolumny dat* w tej tabeli.
  
W tej lekcji oznaczysz tabelę DimDate jako *tabelę dat* i kolumnę Date Column (w tabeli dat) jako *kolumnę dat* (unikatowy identyfikator).  

Zanim oznaczysz tabelę dat i kolumnę dat, warto uporządkować nieco model, aby ułatwić jego zrozumienie. Zwróć uwagę na kolumnę **FullDateAlternateKey** w tabeli DimDate. Zawiera ona jeden wiersz dla każdego dnia w każdym roku kalendarzowym ujętym w tabeli. Ta kolumna jest bardzo często używana w formułach miar i w raportach. Jednak „FullDateAlternateKey” nie jest najlepszym identyfikatorem dla tej kolumny. Możesz zmienić jej nazwę na **Date** (Data), aby móc ją łatwiej zidentyfikować i uprościć sobie dołączanie tej kolumny do formuł. O ile to możliwe, zaleca się zmianę nazw obiektów takich jak tabele i kolumny, aby ułatwić sobie ich identyfikację w programie SSDT i klienckich aplikacjach raportowania (np. Power BI i Excel). 
  
Szacowany czas trwania lekcji: **3 minuty**  
  
## <a name="prerequisites"></a>Wymagania wstępne  
Ten temat stanowi część samouczka modelowania tabelarycznego, który należy wykonać w podanej kolejności. Przed przystąpieniem do wykonywania zadań w tej lekcji należy ukończyć poprzednią lekcję ([Lekcja 2. Pobieranie danych](../tutorials/aas-lesson-2-get-data.md)). 

### <a name="to-rename-the-fulldatealternatekey-column"></a>Aby zmienić nazwę kolumny FullDateAlternateKey

1.  W projektancie modeli kliknij tabelę **DimDate**.

2.  Kliknij dwukrotnie nagłówek kolumny **FullDateAlternateKey**, a następnie zmień jego nazwę na **Date**.

  
### <a name="to-set-mark-as-date-table"></a>Aby oznaczyć jako tabelę dat  
  
1.  Wybierz kolumnę **Date**, a następnie w oknie **Właściwości** w obszarze **Typ danych** upewnij się, że pole **Date** jest zaznaczone.  
  
2.  Kliknij menu **Tabela**, a następnie kliknij przycisk **Date**, po czym kliknij przycisk **Oznacz jako tabelę dat**.  
  
3.  W oknie dialogowym **Oznaczanie jako tabeli dat**, w polu listy **Date** zaznacz kolumnę **Date** jako unikatowy identyfikator kolumny. Zazwyczaj jest ona zaznaczona domyślnie. Kliknij przycisk **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Co dalej?
[Lekcja 4. Tworzenie relacji](../tutorials/aas-lesson-4-create-relationships.md).
  
