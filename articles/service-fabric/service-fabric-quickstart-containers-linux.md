---
title: "Tworzenie aplikacji kontenera usługi Azure Service Fabric w systemie Linux | Dokumentacja firmy Microsoft"
description: "Utwórz swoją pierwszą aplikację kontenera systemu Linux w usłudze Azure Service Fabric.  Zbuduj obraz Docker za pomocą własnej aplikacji, wypchnij obraz do rejestru kontenerów, skompiluj i wdróż aplikację kontenera usługi Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/05/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 601cfb136530d2595cded0dd147703d6b272c3ce
ms.contentlocale: pl-pl
ms.lasthandoff: 09/25/2017

---

# <a name="deploy-an-azure-service-fabric-linux-container-application-on-azure"></a>Wdrażanie aplikacji kontenera systemu Linux w usłudze Azure Service Fabric na platformie Azure
Usługa Azure Service Fabric to platforma systemów rozproszonych ułatwiająca pakowanie i wdrażanie skalowalnych oraz niezawodnych mikrousług i kontenerów, a także zarządzanie nimi. 

W tym przewodniku Szybki start przedstawiono sposób wdrażania kontenerów systemu Linux w klastrze usługi Service Fabric. Po wykonaniu tych czynności będziesz mieć gotową aplikację do głosowania składającą się z frontonu internetowego języka Python oraz zaplecza Redis działającego w klastrze usługi Service Fabric. 

![quickstartpic][quickstartpic]

W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Wdrażanie kontenerów systemu Linux w usłudze Service Fabric
> * Skalowanie kontenerów i przenoszenie ich do trybu failover w usłudze Service Fabric

## <a name="prerequisite"></a>Wymagania wstępne
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/en-us/free/).
  
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, upewnij się, że uruchamiasz interfejs wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Aby znaleźć wersję, uruchom polecenie az --version. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="get-application-package"></a>Pobieranie pakietu aplikacji
Do wdrożenia kontenerów w usłudze Service Fabric potrzebujesz zestawu plików manifestu (definicja aplikacji), które opisują poszczególne kontenery i aplikację.

W powłoce chmury użyj usługi Git, aby sklonować kopię definicji aplikacji.

```azurecli-interactive
git clone https://github.com/Azure-Samples/service-fabric-dotnet-containers.git

cd service-fabric-dotnet-containers/Linux/container-tutorial/Voting
```

## <a name="deploy-the-containers-to-a-service-fabric-cluster-in-azure"></a>Wdrażanie kontenerów w klastrze usługi Service Fabric na platformie Azure
Aby wdrożyć aplikację w klastrze na platformie Azure, użyj klastra własnego lub klastra testowego.

Klastry testowe to bezpłatne, działające przez ograniczony czas klastry usługi Service Fabric hostowane na platformie Azure. Są one obsługiwane przez zespół usługi Service Fabric, gdzie każdy może wdrażać aplikacje i dowiadywać się więcej o platformie. Aby uzyskać dostęp do klastra testowego, [postępuj zgodnie z instrukcjami](http://aka.ms/tryservicefabric). 

Aby uzyskać informacje na temat tworzenia własnego klastra, zobacz [Tworzenie pierwszego klastra usługi Service Fabric na platformie Azure](service-fabric-get-started-azure-cluster.md).

> [!Note]
> Usługa frontonu internetowego została skonfigurowana do nasłuchiwania ruchu przychodzącego na porcie 80. Upewnij się, że port w klastrze został otwarty. Jeśli używasz klastra testowego, ten port jest otwarty.
>

### <a name="deploy-the-application-manifests"></a>Wdrażanie manifestów aplikacji 
Zainstaluj wiersz polecenia usługi Service Fabric (sfctl) w środowisku interfejsu wiersza polecenia

```azurecli-interactive
pip3 install --user sfctl 
export PATH=$PATH:~/.local/bin
```
Połącz się z klastrem usługi Service Fabric na platformie Azure przy użyciu interfejsu wiersza polecenia platformy Azure. Punkt końcowy to punkt końcowy zarządzania klastrem — na przykład `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.

```azurecli-interactive
sfctl cluster select --endpoint http://linh1x87d1d.westus.cloudapp.azure.com:19080
```

Użyj udostępnionego skryptu instalacji, aby skopiować definicję aplikacji do głosowania do klastra, zarejestrować typ aplikacji i utworzyć wystąpienie aplikacji.

```azurecli-interactive
./install.sh
```

Otwórz przeglądarkę i przejdź do narzędzia Service Fabric Explorer pod adresem http://\<adres_URL_mojego_klastra_usługi_azure_service_fabric>:80 — na przykład `http://linh1x87d1d.westus.cloudapp.azure.com:80`. Rozwiń węzeł aplikacji, aby sprawdzić, czy istnieje teraz wpis dla typu aplikacji do głosowania i utworzonego wystąpienia.

![Service Fabric Explorer][sfx]

Nawiąż połączenie z działającym kontenerem.  Otwórz przeglądarkę internetową, wskazując na adres URL klastra — na przykład `http://linh1x87d1d.westus.cloudapp.azure.com:80`. W przeglądarce powinna zostać wyświetlona aplikacja do głosowania.

![quickstartpic][quickstartpic]

## <a name="fail-over-a-container-in-a-cluster"></a>Przenoszenie kontenera do trybu failover w klastrze
Usługa Service Fabric zapewnia, że wystąpienia kontenera są automatycznie przenoszone do innych węzłów w klastrze, jeśli wystąpi awaria. Można również ręcznie opróżnić węzeł na potrzeby kontenerów, a następnie bezproblemowo przejść do innych węzłów w klastrze. Istnieje wiele sposobów skalowania usług — w tym przykładzie użyto narzędzia Service Fabric Explorer.

Aby przenieść kontener frontonu do trybu failover, wykonaj następujące czynności:

1. Otwórz narzędzie Service Fabric Explorer w klastrze — na przykład `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Kliknij węzeł **fabric:/Voting/azurevotefront** w widoku drzewa i rozwiń węzeł partycji (reprezentowany przez identyfikator GUID). W widoku drzewa zwróć uwagę na nazwę węzła przedstawiającą węzły, w których obecnie uruchomiono kontener — na przykład `_nodetype_4`
3. Rozwiń węzeł **Węzły** w widoku drzewa. Kliknij wielokropek (trzy kropki) obok węzła działającego w kontenerze.
4. Wybierz pozycję **Uruchom ponownie**, aby ponownie uruchomić ten węzeł, i potwierdź akcję ponownego uruchomienia. Ponowne uruchomienie powoduje przeniesienie kontenera do trybu failover w innym węźle klastra.

![sfxquickstartshownodetype][sfxquickstartshownodetype]

## <a name="scale-applications-and-services-in-a-cluster"></a>Skalowanie aplikacji i usług w klastrze
Usługi Service Fabric można łatwo skalować w klastrze w celu uwzględnienia obciążenia w usługach. Skalowanie usługi odbywa się przez zmienianie liczby wystąpień uruchomionych w klastrze.

Aby skalować usługę internetową frontonu, wykonaj następujące czynności:

1. Otwórz narzędzie Service Fabric Explorer w klastrze — na przykład `http://linh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Kliknij wielokropek (trzy kropki) obok węzła **fabric:/Voting/azurevotefront** w widoku drzewa i wybierz pozycję **Skaluj usługę**.

    ![containersquickstartscale][containersquickstartscale]

  Teraz możesz skalować liczbę wystąpień usługi internetowej frontonu.

3. Zmień liczbę na **2** i kliknij pozycję **Skaluj usługę**.
4. Kliknij węzeł **fabric:/Voting/azurevotefront** w widoku drzewa i rozwiń węzeł partycji (reprezentowany przez identyfikator GUID).

    ![containersquickstartscaledone][containersquickstartscaledone]

    Teraz możesz zobaczyć, że usługa ma dwa wystąpienia. W widoku drzewa możesz zobaczyć, w których węzłach uruchomiono wystąpienia.

Wykonując to proste zadanie zarządzania, podwoiliśmy zasoby dostępne dla naszej usługi frontonu w celu przetworzenia obciążenia użytkownika. Ważne jest, aby zrozumieć, że nie musisz mieć wielu wystąpień usługi, aby działała ona niezawodnie. W przypadku niepowodzenia usługi usługa Service Fabric zapewnia, że nowe wystąpienie usługi jest uruchamiane w klastrze.

## <a name="clean-up"></a>Czyszczenie
Użyj skryptu dezinstalacji udostępnionego w szablonie, aby usunąć wystąpienie aplikacji z klastra i wyrejestrować typ aplikacji. To polecenie wymaga pewnego czasu, aby wyczyścić wystąpienie. Polecenia „install'sh” nie należy uruchamiać natychmiast po tym skrypcie. 

```bash
./uninstall.sh
```

## <a name="next-steps"></a>Następne kroki
W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Wdrażanie aplikacji kontenera systemu Linux na platformie Azure
> * Przenoszenie kontenera do trybu failover w klastrze usługi Service Fabric
> * Skalowanie kontenera w klastrze usługi Service Fabric

* Dowiedz się więcej o uruchamianiu [kontenerów w usłudze Service Fabric](service-fabric-containers-overview.md).
* Uzyskaj informacje o [cyklu życia aplikacji](service-fabric-application-lifecycle.md) usługi Service Fabric.
* Sprawdź [przykłady kodu kontenera usługi Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-containers) w witrynie GitHub.

[sfx]: ./media/service-fabric-quickstart-containers-linux/containersquickstartappinstance.png
[quickstartpic]: ./media/service-fabric-quickstart-containers-linux/votingapp.png
[sfxquickstartshownodetype]:  ./media/service-fabric-quickstart-containers-linux/containersquickstartrestart.png
[containersquickstartscale]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscale.png
[containersquickstartscaledone]: ./media/service-fabric-quickstart-containers-linux/containersquickstartscaledone.png

