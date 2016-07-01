<properties
    pageTitle="Przeprowadzanie migracji aplikacji logiki do wersji schematu 2015-08-01-preview | Microsoft Azure App Service"
    description="Możesz łatwo przeprowadzić migrację aplikacji logiki do najnowszej wersji schematu. Wystarczy wykonać poniższe czynności."
    services="app-service\logic"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/20/2016"
    ms.author="deonhe"/>

# Przeprowadzanie migracji aplikacji logiki do wersji schematu 2015-08-01-preview

Aby zmienić schemat istniejących aplikacji logiki na nowy, wykonaj następujące czynności:  
1. Otwórz aplikację logiki w portalu Azure.  
2. Kliknij pozycję Aktualizuj schemat:

 ![Ikona interfejsu API][step1]   
Zostanie wyświetlona strona Aktualizacja schematu z linkiem do dokumentu zawierającego szczegółowe informacje dotyczące ulepszeń w nowym schemacie: ![Ikona interfejsu API][step2]

>[AZURE.NOTE] Po wybraniu pozycji **Aktualizuj schemat** firma Microsoft automatycznie wykonuje kroki migracji i przekazuje użytkownikowi kod wyjściowy. Korzystając z tych informacji, możesz zaktualizować definicję, jednak należy przestrzegać dobrych praktyk kodowania, takich jak te opisane w sekcji **Najlepsze rozwiązania** poniżej.

## Najlepsze rozwiązania w celu przeprowadzania migracji aplikacji logiki do najnowszej wersji schematu:  

- Skopiuj zmigrowany skrypt do nowej aplikacji logiki — nie zastępuj starego skryptu do chwili, gdy zakończysz testy i potwierdzisz, że zmigrowana aplikacja działa zgodnie z oczekiwaniami.
- Przetestuj aplikację logiki **przed** użyciem jej w środowisku produkcyjnym.
- Po zakończeniu migracji rozpocznij aktualizowanie aplikacji logiki, aby korzystały z [zarządzanych interfejsów API](./apis-list.md), jeżeli jest to możliwe. Na przykład możesz zacząć używać interfejsu DropBox 2 tam, gdzie jest używany interfejs DropBox 1.


## Co dalej
-  [Dowiedz się, jak ręcznie migrować aplikacje logiki](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[krok 1]: ./media/connectors-schema-migration/migrateschema1.png
[krok 2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Jun16_HO2-->


