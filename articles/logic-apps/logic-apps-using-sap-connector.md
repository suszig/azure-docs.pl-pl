---
title: "Nawiązać połączenia z lokalnego systemu SAP w aplikacjach logiki platformy Azure | Dokumentacja firmy Microsoft"
description: "Nawiązywanie połączenia lokalnego systemu SAP z przepływu pracy aplikacji logiki za pośrednictwem bramy danych lokalnych"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a>Nawiązywanie połączenia lokalnego systemu SAP z aplikacji logiki z łącznikiem SAP 

Brama lokalna danych pozwala na zarządzanie danymi i bezpieczny dostęp do zasobów, które znajdują się na obszarze. W tym temacie przedstawiono sposób podłączenia aplikacji logiki do lokalnego systemu SAP. W tym przykładzie aplikację logiki żądań IDOC za pośrednictwem protokołu HTTP i wysyła odpowiedź ponownie.    

> [!NOTE]
> Bieżące ograniczenia: 
> - Jeśli wszystkie kroki wymagane do odpowiedzi nie zakończy się w ramach limitu czasu aplikację logiki [limit czasu żądania](./logic-apps-limits-and-config.md). W tym scenariuszu może pobrać zablokowane żądania. 
> - Selektor plików nie są wyświetlane wszystkie dostępne pola. W tym scenariuszu można ręcznie dodać ścieżki.

## <a name="prerequisites"></a>Wymagania wstępne

- Instalowanie i Konfigurowanie r [bramy danych lokalnych](https://www.microsoft.com/download/details.aspx?id=53127) wersji 1.15.6150.1 lub nowszej. [Jak połączyć się z bramą danych lokalnych w aplikacji logiki](http://aka.ms/logicapps-gateway) zawiera listę czynności. Bramy muszą można zainstalować na maszynie lokalnej, zanim przejdziesz dalej.

- Pobierz i zainstaluj najnowsze biblioteki klienckiej SAP na tym samym komputerze, na którym zainstalowano bramę danych. Użyj jednej z następujących wersji programu SAP: 
    - Serwerem SAP
        - Dowolnego serwera SAP, który obsługuje łącznik .NET (NCo) 3.0
 
    - Klient programu SAP
        - Łącznik .NET programu SAP (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a>Dodaj wyzwalacze i akcje w celu nawiązania systemu SAP

Łącznik programu SAP ma działania, ale nie wyzwalaczy. Tak musimy użyć innego wyzwalacza podczas uruchamiania przepływu pracy. 

1. Dodaj wyzwalacza żądanie/odpowiedź, a następnie wybierz **nowy krok**.

2. Wybierz **Dodaj akcję**, a następnie wybierz łącznik programu SAP, wpisując `SAP` w polu wyszukiwania:    

     ![Wybierz serwer aplikacji SAP lub serwer komunikatów SAP](media/logic-apps-using-sap-connector/sap-action.png)

3. Wybierz [ **serwera aplikacji SAP** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) lub [ **serwer komunikatów SAP**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), oparte na ustawieniach SAP. Nie masz istniejące połączenie, monit o jego utworzenie.

   1. Wybierz **Połącz za pośrednictwem bramy danych lokalnych**, a następnie wprowadź szczegóły systemu SAP:   

       ![Dodaj ciąg połączenia SAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. W obszarze **bramy**, wybierz istniejącą bramę lub aby zainstalować nową bramę, **zainstalować bramę**.

        ![Instalowanie nowej bramy](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Po wprowadzeniu wszystkich szczegółów zaznacz **Utwórz**. 
   Logic Apps konfiguruje i testuje połączenie, upewniając się, że połączenie działa poprawnie.

4. Wprowadź nazwę dla połączenia SAP.

5. Teraz są dostępne różne opcje SAP. Aby znaleźć kategorię IDOC, wybierz z listy. Ręcznie wpisać ścieżkę lub wybierz odpowiedź HTTP w **treści** pola:

     ![Akcja SAP](media/logic-apps-using-sap-connector/picture3.png)

6. Dodaj akcję tworzenia **odpowiedzi HTTP**. Komunikat odpowiedzi powinna być z danych wyjściowych SAP.

7. Zapisz aplikację logiki. Przetestować, wysyłając IDOC za pomocą adresu URL wyzwalacza HTTP. Po wysłaniu IDOC oczekiwania na odpowiedź z aplikacji logiki:   

     > [!TIP]
     > Sprawdź informacje dotyczące sposobu [Monitoruj aplikacje logiki](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Teraz, gdy łącznik SAP jest dodawany do aplikacji logiki, rozpocząć eksplorowanie inne funkcje:

- BAPI
- RFC

## <a name="get-help"></a>Uzyskiwanie pomocy

Aby zadawać pytania, odpowiadać na nie i patrzeć, co robią inni użytkownicy usługi Azure Logic Apps, odwiedź [forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Aby pomóc w ulepszaniu usługi Azure Logic Apps, przesyłaj pomysły lub głosuj na nie w [witrynie opinii użytkowników usługi Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak sprawdzanie poprawności, przekształcania i inne funkcje podobne BizTalk w [pakiet integracyjny dla przedsiębiorstw](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Połącz się z lokalnymi danymi](../logic-apps/logic-apps-gateway-connection.md) z aplikacji logiki
