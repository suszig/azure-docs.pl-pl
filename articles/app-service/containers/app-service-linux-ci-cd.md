---
title: "Ciągłe wdrażanie z rejestru kontenera Docker z aplikacji sieci Web dla kontenerów - Azure | Dokumentacja firmy Microsoft"
description: "Jak można skonfigurować ciągłego wdrażania od rejestru kontenera Docker w aplikacji sieci Web dla kontenerów."
keywords: "Usługa aplikacji Azure, linux, docker, acr, oss"
services: app-service
documentationcenter: 
author: ahmedelnably
manager: cfowler
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: cccbd4952c66d3d8140e2a03e3b76afaa5ba3fbf
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Ciągłe wdrażanie w aplikacji sieci Web dla kontenerów

W tym samouczku, skonfiguruj ciągłe wdrażanie dla kontenera niestandardowego obrazu z zarządzanego [rejestru kontenera Azure](https://azure.microsoft.com/services/container-registry/) repozytoria lub [Centrum Docker](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do [portalu Azure](https://portal.azure.com)

## <a name="enable-container-continuous-deployment-feature"></a>Włączanie funkcji ciągłego wdrażania kontenera

Można włączyć za pomocą funkcji ciągłego wdrażania [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) i wykonując następujące polecenie

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

W  **[portalu Azure](https://portal.azure.com/)**, kliknij przycisk **usługi aplikacji** opcję z lewej strony.

Kliknij nazwę aplikacji, który chcesz skonfigurować wdrożenie ciągłe Centrum Docker.

W **ustawień aplikacji**, dodaj aplikację nosi nazwę `DOCKER_ENABLE_CI` z wartością `true`.

![Wstaw obraz ustawienia aplikacji](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-webhook-url"></a>Przygotowanie adresu URL elementu Webhook

Można uzyskać przy użyciu adresu URL elementu Webhook [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) i wykonując następujące polecenie

```azurecli-interactive
az webapp deployment container show-cd-url -n sname1 -g rgname
```

Adres URL elementu Webhook, musisz mieć następujący punkt końcowy: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Można uzyskać z `publishingusername` i `publishingpwd` pobierając aplikacji sieci web Publikowanie profilu przy użyciu portalu Azure.

![Wstaw obraz dodawania elementu webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-web-hook"></a>Dodaj haku sieci web

### <a name="azure-container-registry"></a>Azure Container Registry

W bloku portalu rejestru, kliknij **elementów Webhook**, Utwórz nowy element webhook, klikając **Dodaj**. W **tworzenia elementu webhook** bloku, nazwę użytkownika elementu webhook. Dla identyfikatora URI elementu Webhook, musisz podać adres URL uzyskane z **krok 3**

Upewnij się, zdefiniuj zakres jako repozytorium, który zawiera obraz kontenera.

![Wstaw obraz elementu webhook](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Gdy obraz zostanie zaktualizowany, aplikacji sieci web aktualizowany automatycznie z nowego obrazu.

### <a name="docker-hub"></a>Centrum docker

Na stronie Centrum Docker kliknij **elementów Webhook**, następnie **tworzenia elementu WEBHOOK A**.

![Wstaw obraz dodawania elementu webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Dla adresu URL elementu Webhook, musisz podać adres URL uzyskane z **krok 3**

![Wstaw obraz dodawania elementu webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Gdy obraz zostanie zaktualizowany, aplikacji sieci web aktualizowany automatycznie z nowego obrazu.

## <a name="next-steps"></a>Następne kroki

* [Co to jest Azure App Service w systemie Linux?](./app-service-linux-intro.md)
* [Rejestru kontenera platformy Azure](https://azure.microsoft.com/services/container-registry/)
* [W usłudze Azure App Service w systemie Linux przy użyciu platformy .NET Core](quickstart-dotnetcore.md)
* [W usłudze Azure App Service w systemie Linux przy użyciu Ruby](quickstart-ruby.md)
* [Sposób użycia niestandardowego obrazu Docker dla aplikacji sieci Web dla kontenerów](quickstart-custom-docker-image.md)
* [Usługa aplikacji Azure aplikacji sieci Web dla kontenerów — często zadawane pytania](./app-service-linux-faq.md)
* [Zarządzanie aplikacją sieci Web dla kontenerów używa interfejsu wiersza polecenia platformy Azure w wersji 2.0](./app-service-linux-cli.md)