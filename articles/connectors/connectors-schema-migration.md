---
title: Przeprowadzanie migracji aplikacji logiki do wersji schematu 2015-08-01-preview | Microsoft Docs
description: "Możesz łatwo przeprowadzić migrację aplikacji logiki do najnowszej wersji schematu. Wystarczy wykonać poniższe czynności."
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ab22e0369781f9f9afb9b3df9e7fdd54660a959d


---
# <a name="how-to-migrate-logic-apps-to-schema-version-20150801preview"></a>Przeprowadzanie migracji aplikacji logiki do wersji schematu 2015-08-01-preview
Aby zmienić schemat istniejących aplikacji logiki na nowy, wykonaj następujące czynności:  

1. Otwórz aplikację logiki w witrynie Azure Portal.  
2. Kliknij pozycję Aktualizuj schemat:
   
   ![Ikona interfejsu API][step1]   
   Zostanie wyświetlona strona Aktualizacja schematu z linkiem do dokumentu zawierającego szczegółowe informacje dotyczące ulepszeń w nowym schemacie: ![Ikona interfejsu API][step2]

> [!NOTE]
> Po wybraniu pozycji **Aktualizuj schemat** firma Microsoft automatycznie wykonuje kroki migracji i przekazuje użytkownikowi kod wyjściowy. Korzystając z tych informacji, możesz zaktualizować definicję, jednak należy przestrzegać dobrych praktyk kodowania, takich jak te opisane w sekcji **Najlepsze rozwiązania** poniżej.
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Najlepsze rozwiązania w celu przeprowadzania migracji aplikacji logiki do najnowszej wersji schematu:
* Skopiuj zmigrowany skrypt do nowej aplikacji logiki — nie zastępuj starego skryptu do chwili, gdy zakończysz testy i potwierdzisz, że zmigrowana aplikacja działa zgodnie z oczekiwaniami.
* Przetestuj aplikację logiki **przed** użyciem jej w środowisku produkcyjnym.
* Po zakończeniu migracji rozpocznij aktualizowanie aplikacji logiki, aby korzystały z [zarządzanych interfejsów API](apis-list.md), jeżeli jest to możliwe. Na przykład możesz zacząć używać interfejsu DropBox 2 tam, gdzie jest używany interfejs DropBox 1.

## <a name="whats-next"></a>Co dalej
* [Dowiedz się, jak ręcznie migrować aplikacje logiki](../app-service-logic/app-service-logic-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Nov16_HO2-->


