---
title: "Portale tworzenia i edytowania dziennika zapytań w programie Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano portali, których można używać w Azure Log Analytics można tworzyć i edytować dziennika wyszukiwania."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: magoedte; bwren
ms.openlocfilehash: b205f226d95d94b938a70a834ac0147e76d459ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portale tworzenia i edytowania dziennika zapytań w programie Azure Log Analytics

Dziennik wyszukiwania w różnych miejscach analizy dzienników służy do pobierania danych z obszaru roboczego.  Faktycznie tworzenie i edytowanie zapytań oprócz pracy interakcyjnie z dane zwrotne jednak, masz dwie opcje, które są opisane poniżej.  

## <a name="log-search"></a>Wyszukiwanie w dzienniku 
Strona wyszukiwania dziennika jest dostępna z portalu Azure.  Jest ona odpowiednia dla tworzenia podstawowych zapytań, które mogą zostać utworzone w jednym wierszu.  Dziennik wyszukiwania może być używany bez uruchamiania zewnętrznego portalu i służy do wykonywania różnych funkcji z dziennika wyszukiwania w tym tworzenie reguły alertu, tworzenie grup komputerów i eksportowanie wyników zapytania.  

Dziennik wyszukiwania zawiera wiele funkcji do edycji zapytanie bez pełnej znajomości języka kwerend.  Można uzyskać podsumowanie informacji o tych funkcji w [Utwórz dziennik wyszukiwania w Analiza dzienników Azure przy użyciu wyszukiwania dziennika](log-analytics-log-search-log-search-portal.md).


![Strona wyszukiwania dziennika](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Zaawansowane portal analityka
Portal zaawansowana analityka jest dedykowany portal, który udostępnia zaawansowane funkcje, które nie jest dostępne w dzienniku wyszukiwania z portalu Azure.  Funkcje obejmują możliwość edytowania zapytania w wielu wierszach, selektywnie wykonania kodu, kontekstowej Intellisense i analiza inteligentne.  Portal analityka zaawansowane jest najbardziej odpowiednie dla projektowania złożonych zapytań, które są albo zapisywane jako dziennik wyszukiwania lub kopiować i wklejać do innych elementów analizy dzienników.  Można uruchomić portal analityka Zaawansowane z łącza na stronie dziennik wyszukiwania.

![Zaawansowane portal analityka](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Ze względu na jego funkcje zaawansowane zwykle użyjesz portalu zaawansowana analityka jako narzędzia do głównej tworzenia i edytowania zapytania.  Po określeniu, czy zapytanie działa zgodnie z oczekiwaniami, a następnie należy go skopiować i wkleić go w innym miejscu takich jak strony wyszukiwania dziennika lub Widok projektanta.  Ponieważ portal analityka zaawansowane obsługuje jednak zapytania z wielu wierszy, należy wziąć pod uwagę następujące pod uwagę podczas kopiowania zapytania z tego portalu.

- Komentarze należy usunąć z zapytania przed skopiować i wkleić do innej lokalizacji.  Komentarz dotyczący wiersza poprzedzając ją dwa ukośniki (/ /).  Podczas wklejania wielu zapytań wiersza w jednym wierszu, podziały wiersza zostaną usunięte.  Jeśli są uwzględniane, wszystkie znaki od pierwszego komentarza są traktowane jako część komentarza.


## <a name="next-steps"></a>Następne kroki

- Zapoznaj się z artykułem samouczek na temat używania [wyszukiwania dziennika](log-analytics-tutorial-viewdata.md) Aby dowiedzieć się, jak tworzyć zapytania przy użyciu języka zapytań
- Zapoznaj się z [portal analityka zaawansowane](https://go.microsoft.com/fwlink/?linkid=856587) można tworzyć zaawansowane zapytania i używać jako środowisko projektowe dla wyszukiwań dziennika.

