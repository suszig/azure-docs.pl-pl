---
title: "Reagowanie na zdarzenia magazynu obiektów Blob Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Użyj siatki zdarzeń platformy Azure, aby subskrybować zdarzenia magazynu obiektów Blob."
services: storage,event-grid
keywords: 
author: cbrooksmsft
ms.author: cbrooks
ms.date: 08/25/2017
ms.topic: article
ms.service: storage
ms.openlocfilehash: f7a43d0a7255b326cd550fbcbb92bba93905d293
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="reacting-to-blob-storage-events-preview"></a>Reagowanie na zdarzenia magazynu obiektów Blob (wersja zapoznawcza)

Zdarzenia magazynu obiektów Blob platformy Azure umożliwiają aplikacjom zareagować na tworzenie i usuwanie obiektów blob przy użyciu nowoczesnych architekturach niekorzystającą i bez konieczności skomplikowane, kodu lub mało wydajne i ekonomiczne sondowania usług.  Zamiast tego zdarzenia są usuwane [siatki zdarzeń Azure](https://azure.microsoft.com/services/event-grid/) do subskrybentów, takich jak [usługi Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), lub nawet własne odbiornik http niestandardowego, a tylko płać za można użyć.

Typowe scenariusze zdarzenia magazynu obiektów Blob obejmują przetwarzania obrazu lub wideo, indeksowanie wyszukiwania lub przepływu pracy zorientowane na plik.  Przekazywania plików asynchroniczne są doskonałe dopasowanie dla zdarzeń.  Zmiany są rzadko wykonywane, kiedy dany scenariusz wymaga natychmiastowej reakcji, architektura oparta na zdarzenie może być szczególnie skuteczne.

Zdarzenia siatki jest obecnie w wersji zapoznawczej i jest dostępny dla kont w ***zachodnie centralnej nam*** lub ***zachodnie stany USA 2*** lokalizacji.  Spójrz na [zdarzenia magazynu obiektów Blob trasy do punktu końcowego sieci web niestandardowego](storage-blob-event-quickstart.md) dla prosty przykład.

![Model siatki zdarzeń](./media/storage-blob-event-overview/event-grid-functional-model.png)

## <a name="blob-storage-accounts"></a>Konta usługi Blob Storage
Zdarzenia magazynu obiektów blob są dostępne w [konta usługi Blob storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) (a nie na kontach magazynu ogólnego przeznaczenia).  Konto usługi Blob Storage to specjalne konto magazynu służące do przechowywania danych niestrukturalnych w formie obiektów blob w usłudze Azure Storage. Konta magazynu obiektów blob są podobne do kont magazynu ogólnego przeznaczenia i udostępniać wszystkie trwałości, dostępności, skalowalności i wydajności zalety że używane obecnie, łącznie z 100% spójnością interfejsu API dla blokowych obiektów blob i uzupełnialnych obiektów blob. W przypadku aplikacji wymagających tylko magazynu obiektów blokowych lub uzupełnialnych obiektów blob zalecamy używanie kont usługi Blob Storage.

## <a name="available-blob-storage-events"></a>Dostępne zdarzenia magazynu obiektów Blob
Siatka zdarzeń używa [subskrypcji zdarzeń](../../event-grid/concepts.md#event-subscriptions) do wyznaczania tras wiadomościom zdarzeń do subskrybentów.  Subskrypcja zdarzeń magazynu obiektów blob może zawierać dwa typy zdarzeń:  

> |Nazwa zdarzenia|Opis|
> |----------|-----------|
> |`Microsoft.Storage.BlobCreated`|Uruchamiany podczas tworzenia obiektu blob lub zastąpione przez `PutBlob`, `PutBlockList`, lub `CopyBlob` operacji|
> |`Microsoft.Storage.BlobDeleted`|Uruchamiany po usunięciu obiektu blob za pośrednictwem `DeleteBlob` operacji|

## <a name="event-schema"></a>Schematu zdarzeń
Zdarzenia magazynu obiektów blob zawiera wszystkich informacji potrzebnych do reagowania na zmiany w danych.  Można zidentyfikować zdarzenia magazynu obiektów Blob, ponieważ właściwość eventType rozpoczyna się od "Microsoft.Storage."  
Dodatkowe informacje dotyczące użycia właściwości zdarzenia zdarzeń siatki jest udokumentowany w [schematu zdarzeń siatki zdarzeń](../../event-grid/event-schema.md).  

> |Właściwość|Typ|Opis|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |Temat|Ciąg|Pełny identyfikator usługi Azure Resource Manager konta magazynu, który emituje zdarzenia.|
> |Temat|Ciąg|Ścieżka względna zasobów do obiektu, który jest przedmiotem zdarzenia, przy użyciu tego samego formatu usługi Azure Resource Manager rozszerzonej używana do opisu konta magazynu, usług i kontenery dla Azure RBAC.  Ten format obejmuje nazwę zachowania w przypadku obiektów blob.|
> |eventTime|Ciąg|Data/Godzina wygenerowania zdarzenia, w formacie ISO 8601|
> |Typ zdarzenia|Ciąg|"Microsoft.Storage.BlobCreated" lub "Microsoft.Storage.BlobDeleted"|
> |Identyfikator|Ciąg|Unikatowy identyfikator, jeśli to zdarzenie|
> |Dane|Obiekt|Zbieranie danych zdarzeń specyficznych dla magazynu obiektów blob|
> |data.contentType|Ciąg|Typ zawartości obiektu blob, czy zwrócony w nagłówku Content-Type z obiektu blob|
> |data.contentLength|Numer|Rozmiar obiektu blob, tak jak liczbę całkowitą reprezentującą liczbę bajtów, czy zwrócony w nagłówku Content-Length z obiektu blob.  Wysyłane z BlobCreated zdarzeń, ale nie z BlobDeleted.|
> |Data.URL|Ciąg|Adres url obiektu, który jest przedmiotem zdarzenia|
> |data.eTag|Ciąg|Element etag obiektu, gdy to zdarzenie jest wywoływane.  Nie jest dostępna dla zdarzenia BlobDeleted.|
> |Data.API|Ciąg|Nazwa operacji interfejsu api, która wyzwoliła tego zdarzenia.  W przypadku zdarzeń BlobCreated ta wartość jest "Metody PutBlob", "Metoda PutBlockList" lub "CopyBlob".  W przypadku zdarzeń BlobDeleted ta wartość jest "DeleteBlob".  Te wartości są takie same nazwy interfejsu api, które znajdują się w dziennikach diagnostycznych usługi Azure Storage.  Zobacz [rejestrowane operacje i komunikaty o stanie](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).|
> |Data.Sequencer|Ciąg|Ciąg z ogólnym opisem reprezentującej logicznej sekwencji zdarzeń dla dowolnej nazwy określonego obiektu blob.  Użytkownicy mogą używać standardowego porównywania ciągów, aby zrozumieć względną sekwencję dwóch zdarzeń na tej samej nazwie obiektu blob.|
> |data.requestId|Ciąg|Identyfikator żądania generowanych przez usługi dla operacji interfejsu API magazynu.  Może służyć do skorelowania do magazynu Azure diagnostycznych dzienników, korzystając z pola "żądanie id-header" w dziennikach i zostanie zwrócona przez inicjowanie wywołań interfejsu API w nagłówku "x-ms-request-id". Zobacz [Format dziennika](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.clientRequestId|Ciąg|Identyfikator żądania dostarczonych przez klienta do przechowywania operacji interfejsu API.  Służy do skorelowania do dzienników diagnostycznych usługi Azure Storage, korzystając z pola "client-request-id" w dziennikach i może zostać dostarczona w żądaniach klienta przy użyciu nagłówka "x-ms-client żądania id". Zobacz [Format dziennika](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.storageDiagnostics|Obiekt|Dane diagnostyczne czasami uwzględniony przez usługę Azure Storage.  Jeśli jest obecny, powinny być ignorowane przez odbiorców zdarzeń.|

Oto przykład zdarzenia BlobCreated:
```json
[{
  "topic": "/subscriptions/319a9601-1ec0-0000-aebc-8fe82724c81e/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file1.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T01:57:26.005121Z",
  "id": "602a88ef-0001-00e6-1233-1646070610ea",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "799304a4-bbc5-45b6-9849-ec2c66be800a",
    "requestId": "602a88ef-0001-00e6-1233-164607000000",
    "eTag": "0x8D4E44A24ABE7F1",
    "contentType": "text/plain",
    "contentLength": 447,
    "blobType": "BlockBlob",
    "url": "https://myaccount.blob.core.windows.net/testcontainer/file1.txt",
    "sequencer": "00000000000000EB000000000000C65A",
  }
}]

```

Aby uzyskać więcej informacji, zobacz [schematu zdarzenia magazynu obiektów Blob](../../event-grid/event-schema.md#azure-blob-storage).

## <a name="filtering-events"></a>Filtrowanie zdarzeń
Subskrypcje zdarzeń obiektu blob można filtrować na podstawie typu zdarzenia oraz nazwa kontenera i nazwa obiektu blob obiektu, który został utworzony lub usunięty.  Filtry tematu w pracach siatki zdarzenia na podstawie "rozpoczyna się od ciągu" i "kończy się wyrazem" dopasowań, dzięki czemu zdarzenia z podmiotu pasującego są dostarczane do subskrybenta.
Podmiot zdarzenia magazynu obiektów Blob w formacie:
```json
/blobServices/default/containers/<containername>/blobs/<blobname>
```
Aby uwzględnić wszystkie zdarzenia dla konta magazynu, można pozostawić filtry tematu puste.

Aby dopasować zdarzenia z utworzone w zestawie kontenery udostępnianie prefiks obiektów blob, użyj `subjectBeginsWith` filtrowania, takie jak:
```json
/blobServices/default/containers/containerprefix
```
Aby dopasować zdarzenia z utworzone w określonym kontenerze obiektów blob, użyj `subjectBeginsWith` filtrowania, takie jak:
```json
/blobServices/default/containers/containername/
```
Aby dopasować zdarzenia z obiektów blob utworzone w określonym kontenerze udostępnianie prefiksu nazwy obiektu blob, użyj `subjectBeginsWith` filtrowania, takie jak:
```json
/blobServices/default/containers/containername/blobs/blobprefix
```

Aby dopasować zdarzenia z obiektów blob utworzone w określonego kontenera udostępnianie sufiks obiektów blob, użyj `subjectEndsWith` filtru, takich jak "log" lub ".jpg"

Aby uzyskać więcej informacji, zobacz [pojęcia siatki zdarzeń](../../event-grid/concepts.md#filters).

## <a name="practices-for-consuming-events"></a>Wskazówki dotyczące używania zdarzeń
Aplikacje obsługujące zdarzenia magazynu obiektów Blob, należy wykonać kilka zaleceń:
> [!div class="checklist"]
> * Jak wiele subskrypcji można skonfigurować do trasy zdarzenia do tej procedury obsługi zdarzeń, jest ważne, nie przyjęto założenie, że są zdarzenia z danego źródła, ale sprawdź temat wiadomości, aby upewnić się, że pochodzą z konta magazynu, które są oczekiwane.
> * Podobnie Sprawdź, czy zdarzenia jest jednym są przygotowane do procesu, a nie założono, że wszystkie zdarzenia, które otrzymujesz będzie oczekiwane typy.
> * Nadejścia nowych wiadomości mogą poza kolejnością i po pewne opóźnienie, użyj pól etag, aby zrozumieć, jeśli Twoje informacje o obiektach są nadal aktualne.  Należy także użyć pola sequencer zrozumienie kolejność zdarzeń w dowolnym określonego obiektu.
> * Użyj pola blobType, aby zrozumieć, jaki typ operacji są dozwolone w obiekcie blob i typów biblioteki klienckiej, która powinna być używana do dostępu obiektu blob.
> * Użyj pola adres url z `CloudBlockBlob` i `CloudAppendBlob` konstruktorów można uzyskać dostępu do obiektu blob.
> * Ignoruj pola, które nie zrozumieć.  Takie rozwiązanie będzie zapewnić Ci odporne na nowe funkcje, które mogą zostać dodane w przyszłości.


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat zdarzeń siatki i wypróbuj zdarzenia magazynu obiektów Blob:

- [Event Grid — informacje](../../event-grid/overview.md)
- [Trasy do punktu końcowego sieci web niestandardowego magazynu obiektów Blob zdarzenia](storage-blob-event-quickstart.md)
