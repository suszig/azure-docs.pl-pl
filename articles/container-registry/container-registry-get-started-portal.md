---
title: "Szybki Start — tworzenie prywatnych rejestru Docker na platformie Azure przy użyciu portalu Azure"
description: "Dowiedz się szybko utworzyć prywatnego rejestru kontenera Docker z portalu Azure."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 514fa3490e480647f0923c99bd9606a3726d4d30
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Tworzenie rejestru kontenerów za pomocą witryny Azure Portal

Rejestr kontenera platformy Azure jest prywatny rejestru Docker na platformie Azure, gdzie można przechowywać i zarządzanie prywatnej obrazy usługi Docker kontenera. Z tego przewodnika Szybki Start utworzysz rejestru kontenera przy użyciu portalu Azure.

Aby ukończyć tego przewodnika Szybki Start, musi mieć zainstalowane lokalnie Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do portalu Azure pod adresem https://portal.azure.com.

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

Wybierz **nowe** > **kontenery** > **rejestru kontenera platformy Azure**.

![Tworzenie rejestru kontenera w portalu Azure][qs-portal-01]

Wprowadź wartości w polach **nazwa rejestru** i **grupy zasobów**. Nazwa rejestru musi być unikatowe w obrębie platformy Azure i może zawierać znaki alfanumeryczne 5 – 50. Utwórz nową grupę zasobów o nazwie `myResourceGroup`oraz **SKU**, wybierz "Basic". Wybierz **Utwórz** wdrażania wystąpienia ACR.

![Tworzenie rejestru kontenera w portalu Azure][qs-portal-03]

W tym szybkiego startu, utworzymy *podstawowe* rejestru. Rejestru kontenera platformy Azure jest dostępna w kilku różne jednostki magazynowe, krótko opisane w poniższej tabeli. Rozszerzone szczegółowe na każdym, patrz [rejestru kontenera jednostek SKU](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Gdy **wdrożenie zakończyło się pomyślnie** pojawi się komunikat, wybierz rejestru kontenera w portalu, a następnie wybierz **klucze dostępu**.

![Tworzenie rejestru kontenera w portalu Azure][qs-portal-05]

W obszarze **administrator**, wybierz pozycję **włączyć**. Zwróć uwagę na następujące wartości:

* Serwer logowania
* Nazwa użytkownika
* hasło

Te wartości jest użycie w poniższych krokach podczas pracy z rejestru z poziomu interfejsu wiersza polecenia Docker.

![Tworzenie rejestru kontenera w portalu Azure][qs-portal-06]

## <a name="log-in-to-acr"></a>Zaloguj się do awaryjnego

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. Aby to zrobić, użyj [logowania docker](https://docs.docker.com/engine/reference/commandline/login/) polecenia. Zastąp *username*, *hasło*, i *logowania serwera* wartości z wartościami zanotowaną w poprzednim kroku.

```bash
docker login --username <username> --password <password> <login server>
```

Polecenie zwraca `Login Succeeded` po ukończeniu. Ostrzeżenie o zabezpieczeniach rekomendowania stosowania może być również wyświetlany `--password-stdin` parametru. Gdy wykorzystanie przez niego wykracza poza zakres tego artykułu, zaleca się po tym najlepszym rozwiązaniem. Zobacz [logowania docker](https://docs.docker.com/engine/reference/commandline/login/) polecenia odwołania, aby uzyskać więcej informacji.

## <a name="push-image-to-acr"></a>Obraz wypychania do awaryjnego

Aby wypchnąć obrazu do rejestru kontenera platformy Azure, najpierw musi mieć obraz. W razie potrzeby, uruchom następujące polecenie, aby pobierać istniejącego obrazu z Centrum Docker.

```bash
docker pull microsoft/aci-helloworld
```

Przed naciśnięciu obraz do rejestru musi tag obrazu z nazwą serwera ACR logowania. Tag przy użyciu obrazu [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) polecenia. Zastąp *logowania serwera* z nazwą serwera logowania wcześniej zapisane.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Na koniec użyj [wypychania docker](https://docs.docker.com/engine/reference/commandline/push/) do dystrybuowania obrazu do wystąpienia ACR. Zastąp *logowania serwera* z nazwą serwera logowania wystąpienia ACR.

```
docker push <login server>/aci-helloworld:v1
```

Dane wyjściowe z pomyślnym `docker push` jest podobne do polecenia:

```
The push refers to a repository [uniqueregistryname.azurecr.io/aci-helloworld]
7c701b1aeecd: Pushed
c4332f071aa2: Pushed
0607e25cc175: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:f2867748615cc327d31c68b1172cc03c0544432717c4d2ba2c1c2d34b18c62ba size: 1577
```

## <a name="list-container-images"></a>Tworzenie listy obrazów kontenerów

Aby wyświetlić listę obrazów w wystąpieniu ACR, przejdź do rejestru w portalu i wybierz **repozytoria**, następnie wybierz utworzony z repozytorium `docker push`.

W tym przykładzie mamy wybierz **aci helloworld** repozytorium i firma Microsoft mogą przeglądać `v1`-TIFF w obszarze **tagi**.

![Tworzenie rejestru kontenera w portalu Azure][qs-portal-09]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń **myResourceGroup** grupy zasobów. To spowoduje usunięcie grupy zasobów, wystąpienie ACR i wszystkie obrazy kontenera.

![Tworzenie rejestru kontenera w portalu Azure][qs-portal-08]

## <a name="next-steps"></a>Następne kroki

Ta opcja szybkiego startu została utworzona rejestru kontenera platformy Azure z wiersza polecenia platformy Azure. Jeśli chcesz rejestru kontenera Azure za pomocą wystąpień kontenera platformy Azure, przejdź do samouczka wystąpień kontenera platformy Azure.

> [!div class="nextstepaction"]
> [Samouczki dotyczące usługi Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-04]: ./media/container-registry-get-started-portal/qs-portal-04.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-06]: ./media/container-registry-get-started-portal/qs-portal-06.png
[qs-portal-07]: ./media/container-registry-get-started-portal/qs-portal-07.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png