---
title: "Zarządzanie integracji konta artefaktu metadanych - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Dodawanie lub pobrać artefaktu metadanych z konta integracji dla usługi Azure Logic Apps"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 5eebae624ea024f2ff8c1fa4764027c05220a15f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Zarządzanie artefaktu metadanych w konta integracji dla usługi logic apps

Można zdefiniować niestandardowych metadanych dla artefaktów na kontach integracji i pobrać metadane w czasie wykonywania dla aplikacji logiki. Na przykład można określić metadanych artefaktów, takich jak partnerów, umów, schematów i map — wszystkie sklepu metadanych za pomocą pary klucz wartość. Obecnie usługa artefaktów nie można utworzyć metadanych za pośrednictwem interfejsu użytkownika, ale interfejsów API REST umożliwia utworzenie metadanych. Aby dodać metadanych podczas tworzenia lub wybierz partnera, umowy lub schematu w portalu Azure, wybierz **Edytuj**. Do pobierania metadanych artefaktów w aplikacjach logiki, służy funkcja wyszukiwania artefaktu konta integracji.

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Dodaj metadane do artefaktów na kontach integracji

1. Utwórz [konta integracji](logic-apps-enterprise-integration-create-integration-account.md).

2. Na przykład dodać artefaktów do konta integracji, [partnera](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [umowy](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements), lub [schematu](logic-apps-enterprise-integration-schemas.md).

3.  Wybierz artefakt, wybierz **Edytuj**, a następnie wprowadź szczegóły metadanych.

    ![Wprowadź metadanych](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Pobieranie metadanych z artefaktów dla usługi logic apps

1. Utwórz [aplikacji logiki](quickstart-create-first-logic-app-workflow.md).

2. Utwórz [link z aplikacji logiki do swojego konta integracji](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app). 

3. W Projektancie aplikacji logiki, dodać wyzwalacza, takich jak *żądania* lub *HTTP* do aplikacji logiki.

4.  Wybierz **następnego kroku** > **Dodaj akcję**. Wyszukaj *integracji* , można znaleźć, a następnie wybierz **konta integracji — wyszukiwanie artefaktu konta integracji**.

    ![Wybierz wyszukiwanie artefaktu konta integracji](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Wybierz **typu artefaktu**i podaj **nazwa artefaktu**.

    ![Wybierz typ artefaktu i określ nazwę artefaktów](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Przykład: Pobrać partnera metadanych

Metadane partnera ma `routingUrl` szczegóły:

![Znajdź partnera z metadanych "routingURL"](media/logic-apps-enterprise-integration-metadata/image6.png)

1. W aplikacji logiki, Dodaj Twój wyzwalacz **konta integracji — wyszukiwanie artefaktu konta integracji** akcji dla swojego partnera i **HTTP**.

    ![Dodawanie wyzwalacza, artefaktu wyszukiwania i "HTTP" do aplikacji logiki](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Aby pobrać identyfikator URI, przejdź do widoku kodu aplikacji logiki. Definicję aplikacji logiki powinna wyglądać następująco:

    ![Wyszukiwanie wyszukiwania](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a>Kolejne kroki
* [Dowiedz się więcej na temat umów](logic-apps-enterprise-integration-agreements.md "więcej informacji na temat umowy integracji dla przedsiębiorstw")  
