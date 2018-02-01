---
title: "Szybki start — tworzenie rejestru prywatnego platformy Docker na platformie Azure przy użyciu witryny Azure Portal"
description: "Szybka nauka tworzenia rejestru prywatnego platformy Docker przy użyciu witryny Azure Portal."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 12/06/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: eaf935c1060e53673351936111083d8bb44f05e7
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="create-a-container-registry-using-the-azure-portal"></a>Tworzenie rejestru kontenerów za pomocą witryny Azure Portal

Usługa Azure Container Registry to rejestr prywatny platformy Docker na platformie Azure, w którym można przechowywać prywatne obrazy kontenerów Docker i zarządzać nimi. W tym przewodniku Szybki start utworzysz rejestr kontenerów za pomocą witryny Azure Portal.

Aby ukończyć ten przewodnik Szybki start, musisz mieć platformę Docker zainstalowaną lokalnie. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac][docker-mac], [Windows][docker-windows] lub [Linux][docker-linux].

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do witryny Azure Portal pod adresem https://portal.azure.com.

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

Wybierz pozycje **Nowy** > **Kontenery** > **Azure Container Registry**.

![Tworzenie rejestru kontenerów w witrynie Azure Portal][qs-portal-01]

Wprowadź wartości w polach **Nazwa rejestru** i **Grupa zasobów**. Nazwa rejestru musi być unikatowa w obrębie platformy Azure i może zawierać od 5 do 50 znaków alfanumerycznych. Utwórz nową grupę zasobów o nazwie `myResourceGroup`, a w polu **SKU** wybierz opcję „Basic” (Podstawowa). Wybierz przycisk **Utwórz**, aby wdrożyć wystąpienie usługi ACR.

![Tworzenie rejestru kontenerów w witrynie Azure Portal][qs-portal-03]

W tym przewodniku Szybki start utworzymy rejestr w ramach jednostki *Podstawowa*. Usługa Azure Container Registry jest dostępna w ramach kilku różnych jednostek SKU, krótko opisanych w poniższej tabeli. Aby uzyskać szczegółowe informacji o każdej z nich, zobacz [Jednostki SKU rejestru kontenerów][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Kiedy pojawi się komunikat **Wdrażanie zakończyło się pomyślnie**, wybierz rejestru kontenerów w portalu, a następnie wybierz pozycję **Klucze dostępu**.

![Tworzenie rejestru kontenerów w witrynie Azure Portal][qs-portal-05]

W obszarze **Administrator** wybierz pozycję **Włącz**. Zanotuj następujące wartości:

* Serwer logowania
* Nazwa użytkownika
* hasło

Te wartości będą używane w poniższych krokach podczas pracy z rejestrem z poziomu interfejsu wiersza polecenia platformy Docker.

![Tworzenie rejestru kontenerów w witrynie Azure Portal][qs-portal-06]

## <a name="log-in-to-acr"></a>Zaloguj się do usługi ACR

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. Aby to zrobić, użyj polecenia [docker login][docker-login]. Zastąp wartości *username*, *password* i *login server* wartościami zanotowanymi w poprzednim kroku.

```bash
docker login --username <username> --password <password> <login server>
```

Po ukończeniu polecenie zwraca ciąg `Login Succeeded`. Może także zostać wyświetlone ostrzeżenie o zabezpieczeniach zalecające użycie parametru `--password-stdin`. Użycie tego parametru wykracza poza zakres tego artykułu, jednak zalecamy zastosowanie tego najlepszego rozwiązania. Zobacz polecenie [docker login][docker-login], aby uzyskać więcej informacji.

## <a name="push-image-to-acr"></a>Wypychanie obrazu do usługi ACR

Aby wypchnąć obrazu do usługi Azure Container Registry, najpierw musisz mieć obraz. W razie potrzeby uruchom poniższe polecenie, aby ściągnąć istniejący obraz z usługi Docker Hub.

```bash
docker pull microsoft/aci-helloworld
```

Przed wypchnięciem obrazu do rejestru musisz otagować obraz przy użyciu nazwy serwera logowania usługi ACR. Aby dodać tag do obrazu, użyj polecenia [docker tag][docker-tag]. Zastąp element *login server* zapisaną wcześniej nazwą serwera logowania.

```
docker tag microsoft/aci-helloworld <login server>/aci-helloworld:v1
```

Na koniec użyj polecenia [docker push][docker-push], aby wypchnąć obraz do wystąpienia usługi ACR. Zastąp element *login server* nazwą serwera logowania wystąpienia usługi ACR.

```
docker push <login server>/aci-helloworld:v1
```

Dane wyjściowe z pomyślnie ukończonego polecenia `docker push` są podobne do następujących:

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

Aby wyświetlić listę obrazów w wystąpieniu usługi ACR, przejdź do rejestru w portalu i wybierz pozycję **Repozytoria**, następnie wybierz repozytorium utworzone za pomocą `docker push`.

W tym przykładzie wybierz repozytorium **aci-helloworld**. Zobaczysz obraz z tagiem `v1` w obszarze **TAGS** (tagi).

![Tworzenie rejestru kontenerów w witrynie Azure Portal][qs-portal-09]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów **myResourceGroup** nie jest już potrzebna, można ją usunąć. Spowoduje to usunięcie grupy zasobów, wystąpienia usługi ACR i wszystkich obrazów kontenerów.

![Tworzenie rejestru kontenerów w witrynie Azure Portal][qs-portal-08]

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start utworzono usługę Azure Container Registry za pomocą witryny Azure Portal. Jeśli chcesz używać usługi Azure Container Registry z usługą Azure Container Instances, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczki dotyczące usługi Azure Container Instances][container-instances-tutorial-prepare-app]

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

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
