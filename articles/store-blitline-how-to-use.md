---
title: "Jak używać Blitline obrazu przetwarzania - Azure funkcji Przewodnik"
description: "Dowiedz się, jak używać usługi Blitline przetwarzania obrazów w aplikacji Azure."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: cwatson
ms.openlocfilehash: 254af305592ebef755ccfcb3ae4367b27fb0fc4a
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Jak używać Blitline z platformy Azure i usługi Azure Storage
W tym przewodniku objaśnia sposób uzyskiwać dostęp do usług Blitline i przesyłanie zadań do Blitline.

## <a name="what-is-blitline"></a>Co to jest Blitline?
Blitline to usługa, która zapewnia przetwarzania obrazu poziomu przedsiębiorstwa w ułamku cen, który może kosztować kompilacji samodzielnie przetwarzania obraz oparte na chmurze.

Fakt jest, że przetwarzania obrazów przeprowadzono wielokrotnie, zwykle ponownie skompilowany od podstaw z myślą o każdym witryny sieci Web. Zdajemy sobie sprawę, to ponieważ już budujemy ich milionów razy zbyt. Jeden dzień, który zdecydowaliśmy się, że prawdopodobnie nadszedł czas, tak jak go dla wszystkich użytkowników. Wiemy, jak to zrobić, to szybkie i wydajne i zapisania wszyscy pracy w tym samym czasie.

Aby uzyskać więcej informacji, zobacz [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Co Blitline nie...
Wyjaśnienie, co jest przydatne w przypadku Blitline, często jest łatwiej zidentyfikować co Blitline, które nie są przed przeniesieniem do przodu.

* Blitline nie ma elementów widget HTML do przekazywania obrazów. Obrazy dostępne musi mieć publiczną lub z ograniczonymi uprawnieniami dostępne dla Blitline nawiązać połączenie.
* Blitline nie działa na żywo przetwarzania obrazu, takich jak Aviary.com
* Blitline nie akceptuje przekazywania obrazów, nie można wypchnąć obrazów do Blitline bezpośrednio. Należy wypychanie ich do usługi Azure Storage lub innych miejsc obsługuje Blitline, a następnie informują Blitline, skąd można je pobrać.
* Blitline jest równoległemu i nie wykonuje żadnych przetwarzania synchronicznego. Co oznacza Podaj nam postback_url i możemy pomagają stwierdzić, gdy firma Microsoft gotowe przetwarzania.

## <a name="create-a-blitline-account"></a>Utwórz konto Blitline
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Jak utworzyć zadanie Blitline
Blitline używa JSON w celu zdefiniowania akcje, które ma być wykonywana na obrazie. Ten kod JSON składa się z kilku prostych pól.

Najprostszy przykład następująco:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

W tym miejscu mamy JSON, który zajmie obrazu "src" "... boys.jpeg", a następnie zmień rozmiar obrazu do 240 x 140.

Identyfikator aplikacji jest element znajduje się w sieci **informacje o połączeniu** lub **ZARZĄDZAJ** kartę na platformie Azure. Jest identyfikatora tajny, który umożliwia uruchamianie zadań na Blitline.

Parametr "Zapisz" identyfikuje informacji o którym chcesz umieścić obraz po możemy zostały przetworzone go. W tym przypadku trivial możemy jeszcze zdefiniowana. Jeśli lokalizacja nie jest zdefiniowany Blitline zapisze ją lokalnie (i tymczasowo) w lokalizacji unikatowy chmury. Będzie można pobrać lokalizacji z zwrócony przez Blitline po wprowadzeniu Blitline JSON. Identyfikator "obrazu" jest wymagana i jest zwracane po do identyfikowania tego określonego zapisać obrazu.

Więcej informacji można znaleźć o *funkcje* obsługujemy tutaj: <http://www.blitline.com/docs/functions>

Możesz również znaleźć dokumentację dotyczącą Opcje zadania tutaj: <http://www.blitline.com/docs/api>

Po określeniu, czy kod JSON jest wszystko co należy zrobić **POST** go do`http://api.blitline.com/job`

Otrzymasz wstecz JSON, który wygląda następująco:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Oznacza to, Blitline odebrał żądanie, ma umieszcza je w kolejce przetwarzania i po zakończeniu obrazu będzie dostępna w: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_aplikacji\_identyfikator / CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Jak zapisać obrazu do konta magazynu Azure
Jeśli masz konto usługi Azure Storage, mogą być Blitline wypchnąć przetworzonych obrazów do Twojej kontenera platformy Azure. Dodając "azure_destination" należy zdefiniować lokalizacji i uprawnienia do Blitline do.

Oto przykład:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Wypełniając CAPITALIZED wartości własnymi, mogą przesyłać dane JSON do http://api.blitline.com/job i obraz "src" zostanie przetworzyć filtru rozmycia i następnie przypisany do usługi Azure docelowego.

### <a name="please-note"></a>Uwaga:
Sygnatury dostępu Współdzielonego musi zawierać cały adres url SAS, łącznie z nazwą pliku, plik docelowy.

Przykład:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Możesz przeczytać najnowszych wersji dokumentów usługi Azure Storage w Blitline [tutaj](http://www.blitline.com/docs/azure_storage).

## <a name="next-steps"></a>Następne kroki
Odwiedź stronę blitline.com, aby uzyskać informacje dotyczące wszystkich naszych innych funkcji:

* Dokumentacja interfejsu API punktu końcowego Blitline <http://www.blitline.com/docs/api>
* Funkcje interfejsu API Blitline <http://www.blitline.com/docs/functions>
* Przykłady Blitline interfejsu API <http://www.blitline.com/docs/examples>
* Trzeci część biblioteki Nuget <http://nuget.org/packages/Blitline.Net>

