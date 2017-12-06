---
title: "Rozwiązywanie problemów z wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak rozwiązać problemy z wystąpień kontenera platformy Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 11/18/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6d8fbddc2f26fe739dd725f417961d7b3d7f77e6
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Rozwiązywanie problemów dotyczących wdrożenia z wystąpień kontenera platformy Azure

W tym artykule przedstawiono sposób rozwiązywania problemów w przypadku wdrażania kontenerów do wystąpień kontenera platformy Azure. Omówiono także niektóre typowe problemy, które możesz napotkać.

## <a name="get-diagnostic-events"></a>Zdarzenia diagnostyczne

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

## <a name="unable-to-pull-image"></a>Nie można ściągania obrazu

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

## <a name="container-continually-exits-and-restarts"></a>Kontener stale kończy działanie i uruchamia ponownie

Jeśli Twoje kontenera jest uruchamiane w celu ukończenia i automatyczne ponowne uruchomienie, może być konieczne ustawić [ponowne uruchomienie zasad](container-instances-restart-policy.md) z **OnFailure** lub **nigdy**. Jeśli określisz **OnFailure** i nadal Zobacz ciągłe ponowne uruchomienie, może wystąpić problem z aplikacją lub skrypt wykonywany w kontenerze sieci.

Interfejs API wystąpień kontenera zawiera `restartCount` właściwości. Aby sprawdzić liczbę ponownych uruchomień kontenera, można użyć [Pokaż kontenera az](/cli/azure/container#az_container_show) w 2.0 interfejsu wiersza polecenia platformy Azure. W poniższych przykładowe dane wyjściowe (który został obcięty do skrócenia), zobacz `restartCount` właściwości na końcu danych wyjściowych.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Większość obrazów kontener dla dystrybucje systemu Linux ustawić powłoki, takie jak bash, jako domyślne polecenie. Ponieważ powłoki samodzielnie nie jest usługą długotrwałe, kontenery natychmiast zamknąć i dzielą się na pętli ponownego uruchomienia, gdy skonfigurowana przy użyciu domyślnego **zawsze** ponowne uruchomienie zasad.

## <a name="container-takes-a-long-time-to-start"></a>Kontener zajmuje dużo czasu do uruchomienia

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

## <a name="resource-not-available-error"></a>Zasób nie jest dostępny błąd

Z powodu różnych regionalnych zasobów obciążenia na platformie Azure, zostanie zgłoszony następujący błąd podczas próby wdrożenia wystąpień kontenera:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Ten błąd wskazuje, czy ze względu na duże obciążenie w regionie, w którym chcesz wdrożyć, nie można przydzielić zasoby określone dla Twojego kontenera w tym czasie. Użyj co najmniej jednego z następujących kroki zaradcze w celu rozwiązania problemu.

* Sprawdź ustawienia wdrożenia kontenera objęte parametrów zdefiniowanych w [dostępność wystąpień kontenera platformy Azure w danym regionie](container-instances-region-availability.md)
* Określ niższe ustawienia Procesora i pamięci dla kontenera
* Wdrażanie w innym regionie Azure
* Wdrażanie w późniejszym czasie
