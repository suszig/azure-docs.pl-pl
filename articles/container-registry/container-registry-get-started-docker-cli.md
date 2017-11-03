---
title: Wypychanie obrazu platformy Docker do prywatnego rejestru platformy Azure | Microsoft Docs
description: "Wypychanie i ściąganie obrazów platformy Docker do prywatnego rejestru kontenerów na platformie Azure za pomocą interfejsu wiersza polecenia platformy Docker"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10f01e4e8c86bbbfa17cf2559caca645ff13bdcc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Wypchnij swój pierwszy obraz do prywatnego rejestru kontenerów platformy Docker za pomocą interfejsu wiersza polecenia platformy Docker
Rejestr kontenera platformy Azure przechowuje prywatne obrazy kontenerów platformy [Docker](http://hub.docker.com) i zarządza nimi podobnie, jak [koncentrator platformy Docker](https://hub.docker.com/) przechowuje publiczne obrazy platformy Docker. [Interfejsu wiersza polecenia platformy Docker](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) możesz użyć do [logowania](https://docs.docker.com/engine/reference/commandline/login/), [wypychania](https://docs.docker.com/engine/reference/commandline/push/), [ściągania](https://docs.docker.com/engine/reference/commandline/pull/) oraz innych operacji na rejestrze kontenera.

Dalsze podstawy oraz pojęcia zostały przedstawione w części [ — omówienie](container-registry-intro.md)



## <a name="prerequisites"></a>Wymagania wstępne
* **Usługa Azure Container Registry** — Tworzy rejestr kontenera w subskrypcji platformy Azure. Na przykład użyj witryny [Azure Portal](container-registry-get-started-portal.md) lub [interfejsu wiersza polecenia platformy Azure w wersji 2.0](container-registry-get-started-azure-cli.md).
* **Interfejs wiersza polecenia platformy Docker** — Aby skonfigurować lokalny komputer jako hosta platformy Docker i uzyskać dostęp do poleceń interfejsu wiersza polecenia platformy Docker, zainstaluj [aparat platformy Docker](https://docs.docker.com/engine/installation/).

## <a name="log-in-to-a-registry"></a>Zaloguj się do rejestru
Uruchom `docker login`, aby zalogować się do rejestru kontenera za pomocą Twoich [poświadczeń rejestru](container-registry-authentication.md).

Poniższy przykład przekazuje identyfikator i hasło [nazwy głównej usługi](../active-directory/active-directory-application-objects.md) Azure Active Directory. Na przykład nazwę główną usługi można było przypisać do rejestru dla scenariusza automatyzacji.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Pamiętaj o określeniu w pełni kwalifikowanej nazwy rejestru (wszystkie małe litery). W tym przykładzie jest to `myregistry.azurecr.io`.

## <a name="steps-to-pull-and-push-an-image"></a>Kroki, aby ściągnąć i wypchnąć obraz
W poniższym przykładzie obraz Nginx jest pobierany z publicznego rejestru koncentratora platformy Docker, oznaczany dla prywatnego rejestru kontenera platformy Azure, wypychany do rejestru użytkownika, a następnie ściągany ponownie.

**1. Ściągnij oficjalny obraz platformy Docker dla kontenera Nginx**

Najpierw ściągnij publiczny obraz kontenera Nginx na swój komputer lokalny.

```
docker pull nginx
```
**2. Uruchamianie kontenera Nginx**

Następujące polecenie interaktywnie uruchamia lokalny kontener Nginx na porcie 8080, umożliwiając wyświetlenie danych wyjściowych kontenera Nginx. Usuwa to działający kontener po zatrzymaniu.

```
docker run -it --rm -p 8080:80 nginx
```

Przejdź do adresu [http://localhost:8080](http://localhost:8080), aby wyświetlić działający kontener. Zobaczysz ekran podobny do poniższego.

![Kontener Nginx na komputerze lokalnym](./media/container-registry-get-started-docker-cli/nginx.png)

Aby zatrzymać działający kontener, naciśnij klawisze [CTRL]+[C].

**3. Tworzenie aliasu obrazu w rejestrze**

Następujące polecenie tworzy alias obrazu z w pełni kwalifikowaną ścieżką do Twojego rejestru. W tym przykładzie jest określana przestrzeń nazw `samples`, aby uniknąć zaśmiecania katalogu głównego rejestru.

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

**4. Wypchnięcie obrazu do rejestru**

```
docker push myregistry.azurecr.io/samples/nginx
```

**5. Ściągnięcie obrazu z rejestru**

```
docker pull myregistry.azurecr.io/samples/nginx
```

**6. Uruchamianie kontenera Nginx z rejestru**

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

Przejdź do adresu [http://localhost:8080](http://localhost:8080), aby wyświetlić działający kontener.

Aby zatrzymać działający kontener, naciśnij klawisze [CTRL]+[C].

**7. (Opcjonalnie) Usuwanie obrazu**

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a>Limit procesów współbieżnych
W niektórych scenariuszach współbieżne wykonywanie wywołań może powodować błędy. Poniższa tabela zawiera limity współbieżnych wywołań korzystających z operacji „wypychania” i „ściągania” na rejestrze kontenera platformy Azure:

| Operacja  | Limit                                  |
| ---------- | -------------------------------------- |
| ŚCIĄGANIE       | Maksymalnie 10 współbieżnych operacji ściągania na rejestr |
| WYPYCHANIE       | Maksymalnie 5 współbieżnych operacji wypychania na rejestr |

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już podstawy, możesz zacząć korzystać z rejestru! Na przykład rozpocznij wdrażanie obrazów kontenera do klastra usługi [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/).
