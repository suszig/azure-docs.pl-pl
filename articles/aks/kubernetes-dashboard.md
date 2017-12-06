---
title: "Zarządzanie klastrem Azure Kubernetes z interfejsu użytkownika sieci web"
description: "Za pomocą pulpitu nawigacyjnego Kubernetes w AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 75ee9cc7c4b40e9a724c8ce98d770a18ae654096
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="kubernetes-dashboard-with-azure-container-service-aks"></a>Pulpit nawigacyjny Kubernetes z usługi kontenera platformy Azure (AKS)

Wiersza polecenia platformy Azure mogą być używane do uruchamiania Kubernetes pulpitu nawigacyjnego. Ten dokument przeprowadzi Cię przez początkowy pulpitu nawigacyjnego Kubernetes z wiersza polecenia platformy Azure, a także przeprowadzi Cię przez niektóre operacje podstawowe pulpitu nawigacyjnego. Aby uzyskać więcej informacji na temat pulpitu nawigacyjnego Zobacz Kubernetes [pulpit nawigacyjny interfejsu użytkownika sieci Web Kubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

## <a name="before-you-begin"></a>Przed rozpoczęciem

W krokach szczegółowo opisanych w tym dokumencie założono, że klaster usługi AKS został utworzony i że zostało nawiązane połączenie kubectl z klastrem. Jeśli potrzebujesz tych elementów, zobacz [szybki start z usługą AKS](./kubernetes-walkthrough.md).

Musisz również mieć zainstalowany i skonfigurowany interfejs wiersza polecenia platformy Azure w wersji 2.0.21 lub nowszej. Aby odnaleźć wersję, uruchom polecenie az --version. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure](/cli/azure/install-azure-cli).

## <a name="start-kubernetes-dashboard"></a>Pulpit nawigacyjny Kubernetes Start

Użyj `az aks browse` polecenie, aby uruchomić Kubernetes pulpitu nawigacyjnego. Po uruchomieniu tego polecenia Zastąp nazwę grupy i klaster zasobów.

```azurecli
az aks browse --resource-group myResourceGroup --name myK8SCluster
```

To polecenie tworzy serwer proxy między systemu dla deweloperów i Kubernetes interfejsu API i otworzy w przeglądarce sieci web do pulpitu nawigacyjnego Kubernetes.

## <a name="run-an-application"></a>Uruchamianie aplikacji

Na pulpicie nawigacyjnym Kubernetes kliknij **Utwórz** przycisk w górnym prawym okienku. Nadaj nazwę wdrożenia `nginx` , a następnie wprowadź `nginx:latest` dla nazwy obrazów. W obszarze **usługi**, wybierz pozycję **zewnętrznych** , a następnie wprowadź `80` port i port docelowy.

Po wykonaniu tych czynności kliknij przycisk **Wdróż** utworzyć wdrożenie.

![Okno dialogowe Tworzenie usługi Kubernetes](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Wyświetl aplikację

Stan o aplikacji są widoczne na pulpicie nawigacyjnym Kubernetes. Gdy aplikacja jest uruchomiona, każdy składnik ma zielone pole wyboru obok niej.

![Stanowiskami Kubernetes](./media/container-service-kubernetes-ui/complete-deployment.png)

Aby wyświetlić więcej informacji na temat stanowiskami aplikacji, kliknij na **stanowiskami** w menu po lewej stronie, a następnie wybierz **NGINX** pod. Tutaj można zobaczyć informacje specyficzne dla pod, takich jak zużycia zasobów.

![Kubernetes zasobów](./media/container-service-kubernetes-ui/running-pods.png)

Aby znaleźć adres IP aplikacji, kliknij na **usług** w menu po lewej stronie, a następnie wybierz **NGINX** usługi.

![Widok nginx](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Edytowanie aplikacji

Oprócz tworzenia i aplikacje do przeglądania, pulpit nawigacyjny Kubernetes może służyć do edycji i aktualizacji wdrożenia aplikacji.

Aby edytować wdrożenia, kliknij przycisk **wdrożeń** w menu po lewej stronie, a następnie wybierz **NGINX** wdrożenia. Na koniec wybierz **Edytuj** na pasku nawigacyjnym po prawej stronie górnej.

![Edytuj Kubernetes](./media/container-service-kubernetes-ui/view-deployment.png)

Zlokalizuj `spec.replica` wartość, która powinna być równa 1, zmień tę wartość na 3. W ten sposób liczbę replik wdrożenia NGINX zwiększa się od 1 do 3.

Wybierz **aktualizacji** gdy będzie gotowe.

![Edytuj Kubernetes](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o pulpicie nawigacyjnym Kubernetes zobacz dokumentację Kubernetes.

> [!div class="nextstepaction"]
> [Pulpit nawigacyjny interfejsu użytkownika sieci Web Kubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)