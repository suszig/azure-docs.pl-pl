---
title: "Jak używać magazynu kolejek w oprogramowaniu Node.js | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać usługi kolejek platformy Azure do tworzenia i usuwania kolejki, Wstaw, Pobierz i usunąć wiadomości. Przykłady zapisywane w środowisku Node.js."
services: storage
documentationcenter: nodejs
author: craigshoemaker
manager: jeconnoc
editor: tysonn
ms.assetid: a8a92db0-4333-43dd-a116-28b3147ea401
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: cshoe
ms.openlocfilehash: 2565f56324a070368c499a62ab54bb98830d8c20
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="how-to-use-queue-storage-from-nodejs"></a>Jak używać Magazynu kolejek w oprogramowaniu Node.js
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Przegląd
W tym przewodniku przedstawiono sposób wykonywania typowych scenariuszy przy użyciu usługi Microsoft Azure kolejki. Przykłady są napisane przy użyciu interfejsu API środowiska Node.js. Omówione scenariusze obejmują **wstawianie**, **wybierania**, **pobierania**, i **usuwanie** kolejki komunikatów, a także **tworzenie i usuwanie kolejek**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Tworzenie aplikacji Node.js
Utwórz pustą aplikację Node.js. Aby uzyskać instrukcje tworzenia aplikacji Node.js, zobacz [tworzenie aplikacji sieci web Node.js w usłudze Azure App Service](../../app-service/app-service-web-get-started-nodejs.md), [tworzenia i wdrażania aplikacji Node.js do usługi w chmurze platformy Azure](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) przy użyciu programu Windows PowerShell lub [ Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="configure-your-application-to-access-storage"></a>Konfigurowanie aplikacji na dostęp do magazynu
Korzystanie z usługi Azure storage, wymaga zestawu SDK usługi Magazyn Azure dla środowiska Node.js, w tym zestaw wygody bibliotek, które komunikują się z magazynu usługi REST.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Umożliwia uzyskanie pakietu węzeł Menedżera pakietów (NPM)
1. Użyj interfejsu wiersza polecenia, takich jak **PowerShell** (system Windows), **terminali** (Mac), lub **Bash** (Unix), przejdź do folderu, w którym utworzono przykładowej aplikacji.
2. Typ **magazyn azure instalacji narzędzia npm** w oknie wiersza polecenia. Dane wyjściowe polecenia jest podobny do poniższego przykładu.
 
    ```bash
    azure-storage@0.5.0 node_modules\azure-storage
    +-- extend@1.2.1
    +-- xmlbuilder@0.4.3
    +-- mime@1.2.11
    +-- node-uuid@1.4.3
    +-- validator@3.22.2
    +-- underscore@1.4.4
    +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
    +-- xml2js@0.2.7 (sax@0.5.2)
    +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    ```

3. Możesz ręcznie uruchomić **ls** polecenie, aby sprawdzić, czy **węzła\_modułów** folder został utworzony. Wewnątrz tego folderu znajdują się **magazyn azure** pakiet, który zawiera biblioteki muszą uzyskać dostęp do magazynu.

### <a name="import-the-package"></a>Importowanie pakietu
Za pomocą Notatnika lub innego edytora tekstu, Dodaj następujący element do góry **server.js** pliku aplikacji, których zamierzasz używać magazynu:

```javascript
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Ustawienia połączenia z magazynem Azure
Moduł azure odczyta zmiennych środowiskowych AZURE\_MAGAZYNU\_konto i AZURE\_MAGAZYNU\_dostępu\_klucz lub AZURE\_MAGAZYNU\_połączenia\_ciąg informacje wymagane do łączenia się z kontem magazynu platformy Azure. Jeśli te zmienne środowiskowe nie są skonfigurowane, należy określić informacje o koncie podczas wywoływania metody **createQueueService**.

Ustawianie zmiennych środowiskowych, na przykład [Azure Portal](https://portal.azure.com) dla witryny sieci Web Azure, zobacz [aplikacji sieci web Node.js przy użyciu usługi Azure tabeli](../../cosmos-db/table-storage-cloud-service-nodejs.md).

## <a name="how-to-create-a-queue"></a>Porady: Tworzenie kolejki
Poniższy kod tworzy **QueueService** obiektu, który umożliwia pracę z kolejki.

```javascript
var queueSvc = azure.createQueueService();
```

Użyj **createQueueIfNotExists** metody, która zwraca określoną kolejkę, jeśli już istnieje lub tworzy nową kolejkę o określonej nazwie, jeśli jeszcze nie istnieje.

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

Jeśli kolejka została utworzona, `result.created` ma wartość true. Jeśli kolejka istnieje, `result.created` ma wartość false.

### <a name="filters"></a>Filtry
Opcjonalne operacjach filtrowania może odnosić się do operacji wykonywanych przy użyciu **QueueService**. Filtrowanie operacje mogą obejmować rejestrowania, Automatyczne ponawianie próby itp. Obiekty, które implementują metodę o sygnaturze są następujące filtry:

```javascript
function handle (requestOptions, next)
```

Po wykonaniu przetwarzanie wstępne opcje żądania, metoda musi wywołać "dalej" przekazywanie wywołania zwrotnego z następującą sygnaturą:

```javascript
function (returnObject, finalCallback, next)
```

W tym wywołania zwrotnego, a po przetworzeniu returnObject (odpowiedź z żądania do serwera) wywołania zwrotnego musi wywołać obok, jeśli istnieje kontynuować przetwarzanie inne filtry lub po prostu Wywołaj finalCallback inaczej tworzyć wywołania usługi.

Dwa filtry, które implementują logikę ponawiania wchodzą w skład zestawu Azure SDK dla środowiska Node.js, **ExponentialRetryPolicyFilter** i **LinearRetryPolicyFilter**. Tworzy następujące **QueueService** obiekt, który używa **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Porady: Wstawianie komunikatu do kolejki
Aby wstawić komunikat do kolejki, użyj **polecenie createMessage** metodę, aby utworzyć nową wiadomość i dodaj go do kolejki.

```javascript
queueSvc.createMessage('myqueue', "Hello world!", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a>Porady: Podgląd kolejnego komunikatu
Możesz uzyskać wgląd w komunikat z przodu kolejki bez jego usuwania z kolejki, wywołując **peekMessages** metody. Domyślnie **peekMessages** wglądu w pojedynczym komunikacie.

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

`result` Zawiera komunikat.

> [!NOTE]
> Przy użyciu **peekMessages** gdy nie ma żadnych komunikatów w kolejce nie zwróci błąd, jednak zostanie zwrócony żaden komunikat.
> 
> 

## <a name="how-to-dequeue-the-next-message"></a>Porady: Następny komunikat usuwania z kolejki
Przetwarza komunikat jest procesem dwuetapowym:

1. Usuwania z kolejki wiadomości.
2. Usunięcie wiadomości.

Aby usunąć wiadomości z kolejki, użyj **getMessages**. Dzięki temu wiadomości niewidoczne w kolejce, więc może je przetwarzać żadnych innych klientów. Po przetworzeniu komunikatu aplikacji wywołać **deleteMessage** go usunąć z kolejki. Poniższy przykład pobiera komunikat, a następnie usuwa je:

```javascript
queueSvc.getMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
    var message = results[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

> [!NOTE]
> Domyślnie komunikat jest ukryty tylko przez 30 sekund, po którym jest widoczny dla innych klientów. Określ inną wartość, przy użyciu `options.visibilityTimeout` z **getMessages**.
> 
> [!NOTE]
> Przy użyciu **getMessages** gdy nie ma żadnych komunikatów w kolejce nie zwróci błąd, jednak zostanie zwrócony żaden komunikat.
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Porady: Zmiana zawartości komunikatu w kolejce
Można zmienić zawartość komunikatu w miejscu przy użyciu kolejki **updateMessage**. Poniższy przykład aktualizuje tekst wiadomości:

```javascript
queueSvc.getMessages('myqueue', function(error, getResults, getResponse){
  if(!error){
    // Got the message
    var message = getResults[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, updateResults, updateResponse){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Porady: Dodatkowych opcji usuwania komunikatów
Istnieją dwa sposoby dostosowania pobierania komunikatów z kolejki:

* `options.numOfMessages` -Pobrać partii wiadomości (do 32).
* `options.visibilityTimeout` -Ustawienia limit czasu niewidoczności dłuższy lub krótszy.

W poniższym przykładzie użyto **getMessages** metodę, aby pobrać 15 komunikatów w jednym wywołaniu. Następnie przetwarza każdy komunikat przy użyciu pętli for. Ustawia również limitu czasu niewidoczności na pięć minut dla wszystkich wiadomości zwracane przez tę metodę.

```javascript
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, results, getResponse){
  if(!error){
    // Messages retrieved
    for(var index in result){
      // text is available in result[index].messageText
      var message = results[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, deleteResponse){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

## <a name="how-to-get-the-queue-length"></a>Porady: Pobieranie długości kolejki
**GetQueueMetadata** zwraca metadane dotyczące kolejki, w tym przybliżoną liczbę komunikatów oczekujących w kolejce.

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Porady: Lista kolejek
Aby uzyskać listę kolejek, użyj **listQueuesSegmented**. Aby uzyskać listę filtrowane według określonego prefiksu, użyj **listQueuesSegmentedWithPrefix**.

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

Jeśli nie można zwrócić wszystkich kolejek, `result.continuationToken` może być używany jako pierwszy parametr **listQueuesSegmented** lub drugi parametr funkcji **listQueuesSegmentedWithPrefix** można pobrać więcej wyników.

## <a name="how-to-delete-a-queue"></a>Porady: Usuwanie kolejki
Aby usunąć kolejkę i wszystkie zawarte w niej komunikaty, wywołaj **deleteQueue** metody dla obiekt kolejki.

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Aby wyczyścić wszystkie komunikaty z kolejki bez jego usuwania, należy użyć **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Porady: Praca z sygnatury dostępu współdzielonego
Udostępniony sygnatur dostępu (SAS) to bezpieczny sposób zapewnienia szczegółowej dostępu do kolejek bez podawania Twojej nazwy konta magazynu i klucze. Skojarzenia zabezpieczeń są często używane do udzielany ograniczony dostęp do kolejek, na przykład pozwala aplikacji mobilnej do przesyłania wiadomości.

Generuje zaufanych aplikacji, takich jak jest usługą opartą na chmurze przy użyciu sygnatury dostępu Współdzielonego **generateSharedAccessSignature** z **QueueService**i udostępnia go do niezaufanych lub częściowo zaufanych aplikacji. Na przykład aplikacji mobilnej. Sygnatury dostępu Współdzielonego jest generowany przy użyciu zasad, opisujący daty rozpoczęcia i zakończenia, w których sygnatury dostępu Współdzielonego jest prawidłowy, a także poziom dostępu przyznane posiadacz sygnatury dostępu Współdzielonego.

Poniższy przykład generuje nowe zasady dostępu współdzielonego, które umożliwia właścicielowi SAS dodać wiadomości do kolejki i wygasa 100 minut po jego utworzeniu.

```javascript
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};

var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
var host = queueSvc.host;
```

Należy pamiętać, że informacji o hoście należy podać także jako jest wymagana podczas próby dostępu do kolejki posiadacz sygnatury dostępu Współdzielonego.

Następnie aplikacja kliencka używa SAS z **QueueServiceWithSAS** wykonywanie operacji względem kolejki. Poniższy przykład łączy do kolejki i tworzy komunikat.

```javascript
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Od czasu skojarzenia zabezpieczeń został wygenerowany z Dodawanie dostępu, jeśli podjęta odczytywać, aktualizować lub usuwać wiadomości, czy zwracany błąd.

### <a name="access-control-lists"></a>Listy kontroli dostępu
Listy kontroli dostępu (ACL) umożliwia także ustawić zasady dostępu dla sygnatury dostępu Współdzielonego. Jest to przydatne, jeśli chcesz umożliwić wielu klientów dostępu do kolejki, ale zawiera różne zasady dostępu dla każdego klienta.

Listy ACL jest zaimplementowana przy użyciu tablicy zasad dostępu w usłudze identyfikator skojarzony z każdej zasady. W poniższym przykładzie zdefiniowano dwie zasady; jeden dla "Użytkownik1" i jeden dla "uzytkownik2":

```javascript
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Poniższy przykład pobiera bieżącej listy ACL dla **Moja_kolejka**, następnie dodaje nowe zasady przy użyciu **setQueueAcl**. Takie podejście umożliwia:

```javascript
var extend = require('extend');
queueSvc.getQueueAcl('myqueue', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Po ustawieniu listy ACL następnie można utworzyć na podstawie Identyfikatora zasady sygnatury dostępu Współdzielonego. Poniższy przykład tworzy nowe sygnatury dostępu Współdzielonego dla "uzytkownik2":

```javascript
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już podstawy magazynu kolejek, skorzystaj z poniższych linków, aby dowiedzieć się więcej o bardziej skomplikowanych zadaniach magazynu.

* Odwiedź stronę [Blog zespołu usługi Magazyn Azure][Azure Storage Team Blog].
* Odwiedź stronę [zestawu SDK usługi Magazyn Azure dla węzła] [ Azure Storage SDK for Node] repozytorium w witrynie GitHub.



[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx

[Azure Portal]: https://portal.azure.com

[Tworzenie aplikacji sieci web Node.js w usłudze Azure App Service](../../app-service/app-service-web-get-started-nodejs.md)

[Tworzenie i wdrażanie aplikacji Node.js do usługi w chmurze platformy Azure](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)

[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/
