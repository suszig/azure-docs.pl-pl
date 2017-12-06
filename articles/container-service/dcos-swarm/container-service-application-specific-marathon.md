---
title: "Aplikacja lub usługa Marathon specyficzne dla użytkownika"
description: "Tworzenie usługi Marathon specyficznej dla aplikacji lub użytkownika"
services: container-service
author: rgardler
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/12/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 61cc379bf32f3e0db11d8fd9dae36d9ecb6b1a6c
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a>Tworzenie usługi Marathon specyficznej dla aplikacji lub użytkownika

Usługa kontenera platformy Azure oferuje zestaw serwerów głównych, na których wstępnie konfigurujemy usługi Apache Mesos i Marathon. Mogą one być używane do organizowania aplikacji w klastrze, ale stosowanie w tym celu serwerów głównych nie jest zalecane. Na przykład dostosowywanie konfiguracji usługi Marathon wymaga logowania do serwerów głównych i wprowadzania zmian. Oznacza to, że należy używać unikatowych serwerów głównych, które są nieco inne niż standardowe i wymagają niezależnej obsługi oraz zarządzania. Ponadto konfiguracja wymagana przez jeden zespół może nie być konfiguracją optymalną dla innego zespołu.

W tym artykule wyjaśnimy, jak dodać usługę Marathon specyficzną dla aplikacji lub użytkownika.

Ponieważ usługa ta będzie należeć do pojedynczego użytkownika lub zespołu, będzie można w dowolny sposób skonfigurować jej działanie. Ponadto usługa Azure Container Service zapewni ciągłość działania usługi. Jeśli wystąpi awaria usługi, usługa Azure Container Service uruchomi ją ponownie. W większości przypadków taki przestój będzie niezauważalny.

## <a name="prerequisites"></a>Wymagania wstępne
[Wdróż wystąpienie usługi Azure Container Service](container-service-deployment.md) z typem aranżacji DCOS i [upewnij się, że klient może połączyć się z klastrem](../container-service-connect.md). Ponadto wykonaj poniższe kroki.

[!INCLUDE [install the DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Tworzenie usługi Marathon specyficznej dla aplikacji lub użytkownika
Rozpocznij od utworzenia pliku konfiguracji JSON definiującego nazwę usługi aplikacji, którą chcesz utworzyć. W tym miejscu użyjemy ciągu `marathon-alice` jako nazwy szablonu. Zapisz plik z nazwą typu `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Następnie użyj interfejsu wiersza polecenia DC/OS, aby zainstalować wystąpienie usługi Marathon z opcjami ustawionymi w pliku konfiguracji:

```bash
dcos package install --options=marathon-alice.json marathon
```

Usługa `marathon-alice` powinna teraz zostać wyświetlona jako działająca na karcie usług interfejsu użytkownika DC/OS. Jeśli chcesz bezpośrednio uzyskiwać dostęp do interfejsu użytkownika, zostanie użyta wartość `http://<hostname>/service/marathon-alice/`.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Ustawianie interfejsu wiersza polecenia DC/OS w celu uzyskiwania dostępu do usługi
Opcjonalnie możesz skonfigurować interfejs wiersza polecenia DC/OS do uzyskiwania dostępu do nowej usługi, ustawiając właściwość `marathon.url`, aby wskazywała wystąpienie `marathon-alice` w następujący sposób:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Możesz sprawdzić, z jakim wystąpieniem usługi Marathon działa interfejs wiersza polecenia, używając polecenia `dcos config show`. Możesz wrócić do korzystania z głównej usługi Marathon przy użyciu polecenia `dcos config unset marathon.url`.

