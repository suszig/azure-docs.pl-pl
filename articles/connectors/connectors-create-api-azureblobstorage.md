---
title: "Dodawanie magazynu obiektów blob platformy Azure łącznika w aplikacjach logiki | Dokumentacja firmy Microsoft"
description: "Rozpoczęcie pracy i skonfigurować łącznik magazynu obiektów blob platformy Azure w aplikacji logiki"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: e12669abd41f09d161fab786af29955da54a1633
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="use-the-azure-blob-storage-connector-in-a-logic-app"></a>Korzystania z łącznika magazynu obiektów blob platformy Azure w aplikacji logiki
Łącznik magazynu obiektów Blob platformy Azure umożliwia przekazywanie, zaktualizować, Pobierz i usuwanie obiektów blob na koncie magazynu, w całości mieści się w aplikacji logiki.  

Z magazynu obiektów blob platformy Azure możesz:

* Tworzenie przepływu pracy przez przekazanie nowych projektów lub pobieranie plików, które zostały niedawno zaktualizowane.
* Akcje służy do pobierania metadanych pliku, usuń plik, kopiowania plików i inne. Na przykład po zaktualizowaniu narzędzie Azure witryny sieci web (wyzwalacz) zaktualizować pliku w magazynie obiektów blob (działanie). 

W tym temacie przedstawiono sposób korzystania z łącznika magazynu obiektów blob w aplikacji logiki.

Aby dowiedzieć się więcej na temat aplikacji logiki, zobacz [co to jest logic apps](../logic-apps/logic-apps-overview.md) i [tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-azure-blob-storage"></a>Łączenie się z magazynem obiektów blob platformy Azure
Zanim aplikację logiki można uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenia* do usługi. Połączenie stanowi połączenie między aplikacji logiki i innej usługi. Na przykład, aby połączyć konto magazynu, należy najpierw utworzyć magazynu obiektów blob *połączenia*. Aby utworzyć połączenie, wprowadź poświadczenia, zwykle używanego do uzyskania dostępu do usługi, którą jest nawiązywane. Dlatego z usługą Azure storage, wprowadź poświadczenia konta magazynu do utworzenia połączenia. 

#### <a name="create-the-connection"></a>Tworzenie połączenia
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a>Użyć wyzwalacza
Ten łącznik nie ma żadnych wyzwalaczy. Użyj innych wyzwalaczy, aby uruchomić aplikację logiki, takie jak wyzwalacz cyklu wyzwalacza HTTP elementu Webhook, wyzwalaczy dostępny z innych łączników i inne. [Tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md) przykładowego.

## <a name="use-an-action"></a>Za pomocą akcji
Akcja jest przeprowadzane przez przepływ pracy zdefiniowanych w aplikacji logiki operacji.

1. Wybierz znak plus. Zobacz kilka opcji: **Dodaj akcję**, **Dodaj warunek**, lub jeden z **więcej** opcje.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Wybierz **Dodaj akcję**.
3. W polu tekstowym wpisz "blob", aby uzyskać listę dostępnych akcji.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. W tym przykładzie wybierz **AzureBlob - Get metadanych pliku przy użyciu ścieżki**. Jeśli połączenie już istnieje, następnie wybierz **...** (Pokaż selektora), aby wybrać plik.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Jeśli zostanie wyświetlony monit o informacje dotyczące połączenia, wprowadź szczegóły, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-azureblobstorage.md#create-the-connection) w tym temacie opisano te właściwości. 
   
   > [!NOTE]
   > W tym przykładzie uzyskujemy metadane pliku. Aby wyświetlić metadane, Dodaj inną akcję, która tworzy nowy plik przy użyciu innego łącznika. Na przykład dodać akcję OneDrive, która tworzy nowy plik "test" na podstawie metadanych. 


5. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Aplikację logiki jest zapisywana i automatycznie włączone.

> [!TIP]
> [Eksplorator usługi Storage](http://storageexplorer.com/) to doskonałe narzędzie do zarządzania wieloma kontami magazynu.

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/azureblobconnector/). 

## <a name="next-steps"></a>Kolejne kroki
[Tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md). Eksploruj dostępnych łączników w aplikacjach logiki w naszym [listy interfejsów API](apis-list.md).

