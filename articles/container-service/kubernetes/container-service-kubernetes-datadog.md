---
title: Monitor Azure Kubernetes klaster z Datadog | Dokumentacja firmy Microsoft
description: "Monitorowanie Kubernetes klastra usługi kontenera platformy Azure przy użyciu Datadog"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 40b34457447a8f80d8cdf77579750e0c42df22d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Monitor klastra usługi kontenera platformy Azure z DataDog

## <a name="prerequisites"></a>Wymagania wstępne
W tym przewodniku założono, że [utworzony klaster Kubernetes za pomocą usługi kontenera platformy Azure](container-service-kubernetes-walkthrough.md).

Założono również, że masz `az` interfejsu wiersza polecenia platformy Azure i `kubectl` narzędzia są zainstalowane.

Możesz przetestować, jeśli masz `az` zainstalowany, uruchamiając narzędzie:

```console
$ az --version
```

Jeśli nie masz `az` narzędzie zainstalowane, nie ma instrukcji [tutaj](https://github.com/azure/azure-cli#installation).

Możesz przetestować, jeśli masz `kubectl` zainstalowany, uruchamiając narzędzie:

```console
$ kubectl version
```

Jeśli nie masz `kubectl` zainstalowany, możesz uruchomić:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog jest usługą monitorowania, która gromadzi dane monitorowania z kontenerów w ramach klastra usługi kontenera platformy Azure. Datadog ma pulpitu nawigacyjnego Docker integracji umożliwia wyświetlenie określonych metryk w kontenerów. Metryki zebrane z kontenerów są zorganizowane według Procesora, pamięci, sieci i we/wy. Datadog dzieli metryki na kontenery i obrazów.

Należy najpierw [Tworzenie konta usługi](https://www.datadoghq.com/lpg/)

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>Instalowanie agenta Datadog z DaemonSet
DaemonSets są używane przez Kubernetes do uruchomienia pojedynczego wystąpienia kontenera na każdym hoście w klastrze.
Są one idealne w przypadku uruchamiania agenci monitorowania.

Po zalogowania do Datadog, można użyć [instrukcje Datadog](https://app.datadoghq.com/account/settings#agent/kubernetes) do instalacji agentów Datadog w klastrze za pomocą DaemonSet.

## <a name="conclusion"></a>Podsumowanie
Gotowe. Po skonfigurowaniu i uruchomieniu agentów danych w konsoli powinna zostać wyświetlona za kilka minut. Użytkownik może odwiedzić zintegrowanego [pulpitu nawigacyjnego kubernetes](https://app.datadoghq.com/screen/integration/kubernetes) wyświetlić podsumowanie klastra.
