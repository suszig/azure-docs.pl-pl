---
title: "Samouczek rejestru kontenera platformy Azure — wdrażanie aplikacji sieci web z rejestru kontenera platformy Azure"
description: "Wdrażanie aplikacji sieci web opartych na systemie Linux przy użyciu obrazu kontenera z rejestru replikacją geograficzną kontenera platformy Azure. Dwa częścią serii, trzech części."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: d775a17cb8069a7521788d850d7d52b92cc67526
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-web-app-from-azure-container-registry"></a>Wdrażanie aplikacji sieci web z rejestru kontenera platformy Azure

Jest to część dwie z trzech części samouczka serii. W [jedną część](container-registry-tutorial-prepare-registry.md)utworzono rejestru kontenera prywatne i replikacją geograficzną, a obraz kontenera została zbudowana ze źródła i przypisany do rejestru. W tym artykule wdrożenia kontenera na dwa wystąpienia aplikacji sieci Web w dwóch różnych regionach platformy Azure mógł korzystać z sieci Zamknij aspektów rejestru replikacją geograficzną.

W tym samouczku część dwóch serii:

> [!div class="checklist"]
> * Wdrożyć obraz kontenera na dwa *aplikacji sieci Web dla kontenerów* wystąpień
> * Sprawdź wdrożonej aplikacji

Jeśli jeszcze nie utworzono z replikacją geograficzną rejestru i wypychana obrazu konteneryzowanych przykładowej aplikacji do rejestru, wróć do poprzedniego samouczek z tej serii, [przygotowanie rejestru kontenera platformy Azure z replikacją geograficzną](container-registry-tutorial-prepare-registry.md).

W następnej części serii można zaktualizować aplikacji, a następnie Wypchnij nowy obraz kontenera w rejestrze. Na koniec przechodzenia do każdemu uruchomionemu wystąpieniu aplikacji sieci Web, aby zobaczyć zmianę automatycznie odzwierciedlane w obu, przedstawiający replikacja geograficzna rejestru kontenera Azure i elementów webhook w akcji.

## <a name="automatic-deployment-to-web-apps-for-containers"></a>Automatyczne wdrażanie aplikacji sieci Web dla kontenerów

Rejestru kontenera Azure zapewnia obsługę wdrażania konteneryzowanych aplikacji bezpośrednio do [aplikacji sieci Web dla kontenerów](../app-service/containers/index.yml). Aby wdrożyć obraz kontenera utworzonej w poprzednim samouczku do dwóch planów aplikacji sieci web znajdujących się w różnych regionach platformy Azure, w tym samouczku, użyj portalu Azure.

Podczas wdrażania aplikacji sieci web za pomocą obrazu kontenera w rejestrze, a masz rejestru replikacją geograficzną, w tym samym regionie, rejestru kontenera Azure tworzy wdrożenia obrazu [webhook](container-registry-webhook.md) dla Ciebie. Po naciśnięciu nowy obraz do repozytorium, kontener elementu webhook przejmuje zmiany i automatycznie wdraża nowy obraz kontenera do aplikacji sieci web.

## <a name="deploy-a-web-app-for-containers-instance"></a>Wdrażanie aplikacji sieci Web dla wystąpienia kontenerów

W tym kroku utworzysz aplikację sieci Web dla wystąpienia kontenery w *zachodnie stany USA* regionu.

Zaloguj się do [portalu Azure](https://portal.azure.com) i przejdź do rejestru utworzony w poprzednim samouczka.

Wybierz **repozytoria** > **acr helloworld**, kliknij prawym przyciskiem myszy **v1** tagów w obszarze **tagów** i wybierz **Wdrażanie aplikacji sieci web**.

![Wdrażanie w usłudze app service w portalu Azure][deploy-app-portal-01]

W obszarze **aplikacji sieci Web dla kontenerów** która jest wyświetlana, określ następujące wartości dla każdego ustawienia:

| Ustawienie | Wartość |
|---|---|
| **Nazwa witryny** | Globalnie unikatowej nazwy dla aplikacji sieci web. W tym przykładzie używamy format `<acrName>-westus` łatwo zidentyfikować rejestru i aplikacji sieci web jest wdrażany z regionu. |
| **Grupa zasobów** | **Użyj istniejących** > `myResourceGroup` |
| **Lokalizacja planu usługi aplikacji** | Utwórz nowy plan o nazwie `plan-westus` w **zachodnie stany USA** regionu. |
| **Obraz** | `acr-helloworld:v1`

Wybierz **Utwórz** do udostępnienia aplikacji sieci web do *zachodnie stany USA* regionu.

![Aplikacja sieci Web w systemie Linux konfiguracji w portalu Azure][deploy-app-portal-02]

## <a name="view-the-deployed-web-app"></a>Wyświetlanie wdrożonej aplikacji sieci web

Po zakończeniu wdrażania uruchomionej aplikacji można wyświetlić, przechodząc do adresu URL w przeglądarce.

W portalu, wybierz **usługi aplikacji**, a następnie aplikacji sieci web udostępniane w poprzednim kroku. W tym przykładzie aplikacja sieci web o nazwie *uniqueregistryname westus*.

Wybierz Link adres URL aplikacji sieci web w prawym górnym rogu z **usługi aplikacji** Przegląd, aby wyświetlić w przeglądarce uruchomionej aplikacji.

![Aplikacja sieci Web w systemie Linux konfiguracji w portalu Azure][deploy-app-portal-04]

Po wdrożeniu obrazu Docker z replikacją geograficzną kontenera rejestru witryna jest wyświetlana obrazu reprezentujące region platformy Azure obsługującym rejestru kontenera.

![Wdrożonej aplikacji sieci web w przeglądarce][deployed-app-westus]

## <a name="deploy-second-web-app-for-containers-instance"></a>Wdrażanie drugi aplikacji sieci Web dla wystąpienia kontenerów

Użyj procedury opisane w poprzedniej sekcji, aby wdrażanie drugi aplikacji sieci web do *wschodnie stany USA* regionu. W obszarze **aplikacji sieci Web dla kontenerów**, określ następujące wartości:

| Ustawienie | Wartość |
|---|---|
| **Nazwa witryny** | Globalnie unikatowej nazwy dla aplikacji sieci web. W tym przykładzie używamy format `<acrName>-eastus` łatwo zidentyfikować rejestru i aplikacji sieci web jest wdrażany z regionu. |
| **Grupa zasobów** | **Użyj istniejących** > `myResourceGroup` |
| **Lokalizacja planu usługi aplikacji** | Utwórz nowy plan o nazwie `plan-eastus` w **wschodnie stany USA** regionu. |
| **Obraz** | `acr-helloworld:v1`

Wybierz **Utwórz** do udostępnienia aplikacji sieci web do *wschodnie stany USA* regionu.

![Aplikacja sieci Web w systemie Linux konfiguracji w portalu Azure][deploy-app-portal-06]

## <a name="view-the-deployed-web-app"></a>Wyświetlanie wdrożonej aplikacji sieci web

Jako przed, można wyświetlić działającej aplikacji przez przejście do adresu URL w przeglądarce.

W portalu, wybierz **usługi aplikacji**, a następnie aplikacji sieci web udostępniane w poprzednim kroku. W tym przykładzie aplikacja sieci web o nazwie *uniqueregistryname eastus*.

Wybierz Link adres URL aplikacji sieci web w prawym górnym rogu z **usługi aplikacji — omówienie** do wyświetlania w przeglądarce uruchomionej aplikacji.

![Aplikacja sieci Web w systemie Linux konfiguracji w portalu Azure][deploy-app-portal-07]

Po wdrożeniu obrazu Docker z replikacją geograficzną kontenera rejestru witryna jest wyświetlana obrazu reprezentujące region platformy Azure obsługującym rejestru kontenera.

![Wdrożonej aplikacji sieci web w przeglądarce][deployed-app-eastus]

## <a name="next-steps"></a>Następne kroki

W tym samouczku wdrożono dwóch aplikacji sieci Web dla wystąpień kontenery z rejestru replikacją geograficzną kontenera platformy Azure. Wykonując kroki opisane w tym samouczku, można:

> [!div class="checklist"]
> * Wdrożone obrazu kontenera na dwa *aplikacji sieci Web dla kontenerów* wystąpień
> * Zweryfikować wdrożonej aplikacji

Przejdź do następnego samouczkiem, aby zaktualizować, a następnie wdrożyć nowy obraz kontenera w rejestrze kontenera, a następnie sprawdź, czy aplikacje sieci web działa w obu regionów zostały zaktualizowane automatycznie.

> [!div class="nextstepaction"]
> [Wdrożenia aktualizacji do obrazu kontenera replikacją geograficzną](./container-registry-tutorial-deploy-update.md)

<!-- IMAGES -->
[deploy-app-portal-01]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-01.png
[deploy-app-portal-02]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-02.png
[deploy-app-portal-03]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-03.png
[deploy-app-portal-04]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-04.png
[deploy-app-portal-05]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-05.png
[deploy-app-portal-06]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-06.png
[deploy-app-portal-07]: ./media/container-registry-tutorial-deploy-app/deploy-app-portal-07.png
[deployed-app-westus]: ./media/container-registry-tutorial-deploy-app/deployed-app-westus.png
[deployed-app-eastus]: ./media/container-registry-tutorial-deploy-app/deployed-app-eastus.png