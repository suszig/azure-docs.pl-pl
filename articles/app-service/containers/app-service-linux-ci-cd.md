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
ms.openlocfilehash: 9759eac01e2db62154cd7f6bf9fcccf13282b4b4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
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

W **kontenera Docker**, wybierz "Na", a następnie naciśnij przycisk Zapisz, aby włączyć ciągłego wdrażania.

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

## <a name="next-steps"></a>Kolejne kroki

* [Co to jest Azure App Service w systemie Linux?](./app-service-linux-intro.md)
* [Rejestru kontenera platformy Azure](https://azure.microsoft.com/services/container-registry/)
* [Using .NET Core in Azure App Service on Linux](quickstart-dotnetcore.md) (Korzystanie z platformy .NET Core w usłudze Azure App Service w systemie Linux)
* [Using Ruby in Azure App Service on Linux](quickstart-ruby.md) (Używanie języka Ruby w usłudze Azure App Service w systemie Linux)
* [How to use a custom Docker image for Web App for Containers](quickstart-docker-go.md) (Używanie niestandardowego obrazu platformy Docker dla usługi Web App for Containers)
* [Azure App Service Web App for Containers FAQ](./app-service-linux-faq.md) (Usługa Web App for Containers w usłudze Azure App Service — często zadawane pytania)
* [Zarządzanie aplikacją sieci Web dla kontenerów używa interfejsu wiersza polecenia platformy Azure w wersji 2.0](./app-service-linux-cli.md)
