---
title: "Usługa Marathon specyficzna dla aplikacji lub użytkownika | Microsoft Docs"
description: "Tworzenie usługi Marathon specyficznej dla aplikacji lub użytkownika"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontenery, Marathon, mikrousługi, DC/OS, Azure"
ms.assetid: 16ecc16e-e504-480e-8dc3-cac14e9e1561
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: befd26888b653c660cc614221684732771496aed


---
# <a name="create-an-application-or-userspecific-marathon-service"></a>Tworzenie usługi Marathon specyficznej dla aplikacji lub użytkownika
Usługa kontenera platformy Azure oferuje zestaw serwerów głównych, na których wstępnie konfigurujemy usługi Apache Mesos i Marathon. Mogą one być używane do organizowania aplikacji w klastrze, ale stosowanie w tym celu serwerów głównych nie jest zalecane. Na przykład dostosowywanie konfiguracji usługi Marathon wymaga logowania do serwerów głównych i wprowadzania zmian. Oznacza to, że należy używać unikatowych serwerów głównych, które są nieco inne niż standardowe i wymagają niezależnej obsługi oraz zarządzania. Ponadto konfiguracja wymagana przez jeden zespół może nie być konfiguracją optymalną dla innego zespołu.

W tym artykule wyjaśnimy, jak dodać usługę Marathon specyficzną dla aplikacji lub użytkownika.

Ponieważ usługa ta będzie należeć do pojedynczego użytkownika lub zespołu, będzie można w dowolny sposób skonfigurować jej działanie. Ponadto usługa Azure Container Service zapewni ciągłość działania usługi. Jeśli wystąpi awaria usługi, usługa Azure Container Service uruchomi ją ponownie. W większości przypadków taki przestój będzie niezauważalny.

## <a name="prerequisites"></a>Wymagania wstępne
[Wdróż wystąpienie usługi Azure Container Service](container-service-deployment.md) z typem aranżacji DCOS i [upewnij się, że klient może połączyć się z klastrem](container-service-connect.md). Ponadto wykonaj poniższe kroki.

[!INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-userspecific-marathon-service"></a>Tworzenie usługi Marathon specyficznej dla aplikacji lub użytkownika
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




<!--HONumber=Nov16_HO2-->


