---
title: "Samouczek Azure Analysis Services: lekcja 13 — wdrażanie | Microsoft Docs"
description: "W tym artykule opisano, jak wdrożyć projekt samouczka do usług Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 8e3e1be572aa66ab46f894a2e5f395d1e6f2ea23
ms.contentlocale: pl-pl
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-13-deploy"></a>Lekcja 13. Wdrażanie

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

W tej lekcji skonfigurujemy właściwości wdrożenia, określając serwer usług Analysis Services na platformie Azure lub serwer lokalny SQL Server vNext Analysis Services, a także nazwę modelu. Następnie wdrożymy model dla tego wystąpienia. Po wdrożeniu modelu użytkownicy mogą łączyć się z nim przy użyciu aplikacji klienckiej do raportowania. Aby dowiedzieć się więcej, zobacz artykuł [Deploy to Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy) (Wdrażanie w usługach Azure Analysis Services).  
  
Szacowany czas trwania lekcji: **5 minut**  
  
## <a name="prerequisites"></a>Wymagania wstępne  
Ten temat stanowi część samouczka modelowania tabelarycznego, który należy wykonać w podanej kolejności. Przed przystąpieniem do wykonywania zadań w tej lekcji należy ukończyć lekcję poprzednią: [Lekcja 12: Analiza w programie Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).  

**Ważne:** jeśli przykładowa baza danych AdventureWorksDW2014 została zainstalowana na lokalnym serwerze SQL i model jest wdrażany na serwerze usług Azure Analysis Services, wymagana jest [lokalna brama danych](../analysis-services-gateway.md).
  
## <a name="deploy-the-model"></a>Wdrażanie modelu  
  
#### <a name="to-configure-deployment-properties"></a>Aby skonfigurować właściwości wdrożenia  

  
1.  W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy projekt **AW Internet Sales**, a następnie kliknij **Właściwości**.  
  
2.  W oknie dialogowym **Strony właściwości projektu AW Internet Sales** wprowadź we właściwości **Serwer** w obszarze **Serwer wdrażania** nazwę serwera usług Analysis Services na platformie Azure lub serwera lokalnego.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
 
    > [!IMPORTANT]  
    > Wdrożenie wystąpienia zdalnej usługi Analysis Services wymaga uprawnień administratora.  
  
3.  We właściwości **Baza danych** wpisz **Adventure Works Internet Sales**.  
  
4.  We właściwości **Nazwa modelu** wpisz **Adventure Works Internet Sales Model**.  
  
5.  Sprawdź wybrane opcje, a następnie kliknij przycisk **OK**.  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a>Aby wdrożyć projekt Adventure Works Internet Sales
  
1.  W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy projekt **AW Internet Sales** > **Kompiluj**.  

2.  Kliknij prawym przyciskiem myszy projekt **AW Internet Sales** > **Wdróż**.

    Podczas wdrażania usług Azure Analysis Services może zostać wyświetlona prośba o wprowadzenie konta. Wprowadź swoje konto organizacyjne i hasło, na przykład nancy@adventureworks.com. To konto musi być wpisane w sekcji Administratorzy w wystąpieniu serwera.
  
    Zostanie wyświetlone okno dialogowe Wdrażanie pokazujące stan wdrożenia metadanych i każdej tabeli zawartej w modelu.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Po pomyślnym zakończeniu wdrażania kliknij przycisk **Zamknij**.  
  
## <a name="conclusion"></a>Podsumowanie  
Gratulacje! Tworzenie i wdrażanie pierwszego modelu tabelarycznego usług Analysis Services zostało ukończone. Ten samouczek pomógł przy wykonywaniu typowych zadań związanych z tworzeniem modelu tabelarycznego. Skoro model Adventure Works Internet Sales został wdrożony, można użyć programu SQL Server Management Studio do zarządzania modelem, a także utworzyć skrypty procesu i plan tworzenia kopii zapasowych. Użytkownicy mogą teraz również łączyć się z modelem, używając aplikacji klienckiej do raportowania (np. Microsoft Excel lub Power BI).  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Co dalej?
*  [Lekcja uzupełniająca — zabezpieczenia dynamiczne](../tutorials/aas-supplemental-lesson-dynamic-security.md)

*  [Lekcja uzupełniająca — wiersze szczegółów](../tutorials/aas-supplemental-lesson-detail-rows.md)

*  [Lekcja uzupełniająca — niewyrównane hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)

