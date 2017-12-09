---
title: "Konwertuj dane JSON przy użyciu płynne transformacji - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Utwórz transformacje lub mapy dla zaawansowanych przekształcenia JSON przy użyciu Logic Apps i płynnych szablonu."
services: logic-apps
documentationcenter: 
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: LADocs; divswa
ms.openlocfilehash: cd177b1ebcb5d236ce265dc153ee6a02125a69df
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="advanced-json-transformations-using-liquid-template"></a>Zaawansowane przekształcenia JSON przy użyciu szablonu płynne
Aplikacje logiki platformy Azure obsługuje podstawowe przekształcenia JSON za pomocą natywnego akcji operacji danych, takich jak Redaguj lub przeanalizować JSON. Logic Apps teraz obsługuje również zaawansowane transformacje JSON z szablonami płynny. [Płynne](https://shopify.github.io/liquid/) jest językiem open source szablonu aplikacji sieci web elastyczne.
 
W tym artykule Dowiedz się, jak używać mapy płynne lub szablonu, który może obsługiwać bardziej złożone przekształcenia JSON, takie jak liczba iteracji, przepływami sterowania, zmienne i tak dalej. Należy zdefiniować JSON do mapowania JSON za jej pomocą płynne i przechowywania tej mapy na koncie integracji, zanim będzie można wykonać przekształcenia płynne w aplikacji logiki.

## <a name="prerequisites"></a>Wymagania wstępne
Poniżej przedstawiono wymagania wstępne dotyczące korzystania płynne akcji:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [rozpocząć pracę z bezpłatnym kontem platformy Azure](https://azure.microsoft.com/free/). W przeciwnym razie możesz [skorzystać z subskrypcji z płatnością zgodnie z rzeczywistym użyciem](https://azure.microsoft.com/pricing/purchase-options/).

* Podstawową wiedzę na temat o [sposób tworzenia aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

* Podstawowy [konta integracji](logic-apps-enterprise-integration-create-integration-account.md).


## <a name="create-and-add-liquid-template-or-map-to-integration-account"></a>Utwórz i Dodaj szablon płynne lub mapowanie do konta integracji

1. Tworzenie szablonu płynne próbki w ramach tego przykładu. Płynne szablon definiuje sposób przekształcania danych wejściowych JSON, zgodnie z opisem w tym miejscu:

   ```
   {%- assign deviceList = content.devices | Split: ', ' -%}
    {
        "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
        "firstNameUpperCase": "{{content.firstName | Upcase}}",
        "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
        "devices" : [
        {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
    }
    ```
    > [!NOTE]
    > Jeśli szablon płynne wykorzystuje dowolne [filtry](https://shopify.github.io/liquid/basics/introduction/#filters), te filtry musi zaczynać się wielkimi literami. 

2. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

3. W menu głównym Azure wybierz **wszystkie zasoby**. 

4. W polu wyszukiwania należy nadać kontu integracji. Wybierz konto.

   ![Wybierz konto integracji](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  Na kafelku konta integracji wybierz **mapy**.

   ![Wybierz mapy](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Wybierz **Dodaj** i Podaj te szczegóły mapy:
  * **Nazwa**: Nazwa mapy, czyli "JsontoJsonTemplate" w tym przykładzie.
  * **Mapowanie typu**: typ mapy. Ciąg JSON do przekształcenia JSON, należy zaznaczyć **płynne**.
  * **Mapa**: istniejące płynne szablon lub plik mapowania na potrzeby przekształcania, czyli "SimpleJsonToJsonTemplate.liquid" w tym przykładzie. Można użyć selektora plików można znaleźć tego pliku.

    ![Dodaj szablon płynne](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)

    
## <a name="add-the-liquid-action-to-transform-json-in-your-logic-app"></a>Dodaj akcję płynne do przekształcania JSON w aplikacji logiki

1. [Tworzenie aplikacji logiki](logic-apps-create-a-logic-app.md).

2. Dodaj [wyzwalacza żądania](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) do aplikacji logiki.

3. Wybierz **+ nowy krok > Dodaj akcję**. Wyszukaj *płynne* w polu wyszukiwania. Wybierz **płynnych — przekształcenia JSON na format JSON**.

  ![Wyszukiwanie — Akcja płynnych](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. W **zawartości** wybierz opcję **treści** z listy zawartości dynamicznej lub listy parametrów, pojawi się zależności. 
  
  ![Wybierz treści](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Z **mapy** listy rozwijanej wybierz szablon płynny jest JsonToJsonTemplate w tym przykładzie.

  ![Wybierz mapy](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Jeśli lista jest pusta, aplikację logiki najprawdopodobniej nie jest połączony z kontem integracji. Aby połączyć aplikację logiki konta integracji, które ma płynne szablon lub mapy, wykonaj następujące kroki:

   1. W menu aplikacji logiki, wybierz **ustawienia przepływu pracy**. 
   2. Z **wybierz konto integracji** listy, wybierz konto integracji i wybierz **zapisać**.

     ![Łącze aplikacji logiki do konta integracji](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)


## <a name="test-your-logic-app"></a>Przetestuj aplikację logiki

   Publikowania danych wejściowych JSON do aplikacji logiki z [Postman](https://www.getpostman.com/postman) lub podobnego narzędzia. Przekształcone dane wyjściowe JSON aplikacji logiki wygląda następująco:
  
   ![Przykładowe dane wyjściowe](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)


## <a name="next-steps"></a>Następne kroki
* [Dowiedz się więcej o pakiet integracyjny dla przedsiębiorstw](../logic-apps/logic-apps-enterprise-integration-overview.md "Dowiedz się więcej na temat pakiet integracyjny dla przedsiębiorstw")  
* [Dowiedz się więcej o mapy](../logic-apps/logic-apps-enterprise-integration-maps.md "Dowiedz się więcej na temat map integracji przedsiębiorstwa")  

