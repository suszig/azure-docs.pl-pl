---
title: "Rozwiązywanie problemów z wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak rozwiązać problemy z wystąpień kontenera platformy Azure"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: ff6da0ce95d0405714602c3872da34a2bff344d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Rozwiązywanie problemów dotyczących wdrożenia z wystąpień kontenera platformy Azure

W tym artykule przedstawiono sposób rozwiązywania problemów w przypadku wdrażania kontenerów do wystąpień kontenera platformy Azure. Omówiono także niektóre typowe problemy, które może napotkać.

## <a name="getting-diagnostic-events"></a>Pobieranie zdarzeń diagnostycznych

Aby wyświetlić dzienniki w kodzie aplikacji w kontenerze, można użyć [dzienniki kontenera az](/cli/azure/container#logs) polecenia. Jednak jeśli Twoje kontenera nie pomyślnie wdrożone, należy przejrzeć informacje diagnostyczne dostarczone przez dostawcę zasobów wystąpień kontenera platformy Azure. Aby wyświetlić zdarzenia z kontenera, uruchom następujące polecenie:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

Dane wyjściowe zawiera właściwości core z kontenera, wraz z wdrożenia zdarzenia:

```bash
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...

      "events": [
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Typowe problemy z wdrażaniem

Istnieje kilka typowych problemów z tego konta dla większości błędy we wdrożeniu.

### <a name="unable-to-pull-image"></a>Nie można ściągania obrazu

W przypadku nie można ściągnąć obrazu początkowo wystąpień kontenera Azure ponowi próbę niektórych okres przed niepowodzeniem po pewnym czasie. Jeśli obraz nie może być pobierane, wyświetlane są zdarzenia podobne do następujących:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

Aby rozwiązać, usunięcie kontenera i ponów próbę wdrożenia, płatności zamknięcie uwagi prawidłowo wpisana nazwa obrazu.

### <a name="container-continually-exits-and-restarts"></a>Kontener stale kończy działanie i uruchamia ponownie

Obecnie wystąpień kontenera Azure obsługuje tylko długotrwałe usług. Jeśli z kontenera uruchamia uzupełniania i wyjściach, automatycznie uruchamia ponownie i zostanie ponownie uruchomione. W takim przypadku zdarzeń, takich jak następujące po są wyświetlane. Należy pamiętać, że kontener uruchamia, a następnie szybko ponownego uruchomienia. Interfejs API wystąpień kontenera zawiera `retryCount` właściwość, która pokazuje, ile razy w określonym kontenerze została uruchomiona ponownie.

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> Większość obrazów kontener dla dystrybucje systemu Linux ustawić powłoki, takie jak bash, jako domyślne polecenie. Ponieważ powłoki samodzielnie nie jest usługą długotrwałe, kontenery natychmiast zamknąć i dzielą się na pętli ponownego uruchomienia.

### <a name="container-takes-a-long-time-to-start"></a>Kontener zajmuje dużo czasu do uruchomienia

Jeśli Twoje kontenera zajmuje dużo czasu można uruchomić, ale ostatecznie zakończy się powodzeniem, przyjrzeć rozmiar obrazu kontenera. Ponieważ wystąpień kontenera platformy Azure pobiera kontener obrazu na żądanie, czas uruchamiania, które występują jest bezpośrednio powiązana z jego rozmiaru.

Rozmiar obrazu kontenera przy użyciu interfejsu wiersza polecenia Docker można wyświetlić:

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

Klucz do przechowywania małych rozmiary obrazów jest zapewnienie, że ostatecznego obrazu nie zawiera żadnych czynności, które nie są wymagane w czasie wykonywania. Jednym ze sposobów czy jest on [kompilacje wieloetapowym](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Wieloetapowym kompilacje upewnij ułatwiają zapewnienie finalnego obrazu zawiera artefakty potrzebnych dla aplikacji i nie jakiekolwiek nadmiarowe zawartości, która nie jest wymagana podczas kompilacji.

Inny sposób, aby zmniejszyć wpływ ściągania obrazu na czas uruchamiania programu kontenera jest hosta obrazu kontenera, w tym samym regionie, w którym zamierzasz używać wystąpień kontenera Azure za pomocą rejestru kontenera platformy Azure. Skraca lokalizacji sieciowej, która obraz kontenera musi podróży, znacznie skrócić czas pobierania.

### <a name="resource-not-available-error"></a>Zasób nie jest dostępny błąd

Z powodu różnych regionalnych zasobów obciążenia na platformie Azure, zostanie zgłoszony następujący błąd podczas próby wdrożenia wystąpień kontenera:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Ten błąd wskazuje, czy ze względu na duże obciążenie w regionie, w którym chcesz wdrożyć, nie można przydzielić zasoby określone dla Twojego kontenera w tym czasie. Użyj co najmniej jednego z następujących kroki zaradcze w celu rozwiązania problemu.

* Sprawdź ustawienia wdrożenia kontenera objęte parametrów zdefiniowanych w [dostępność wystąpień kontenera platformy Azure w danym regionie](container-instances-region-availability.md)
* Określ niższe ustawienia Procesora i pamięci dla kontenera
* Wdrażanie w innym regionie Azure
* Wdrażanie w późniejszym czasie
