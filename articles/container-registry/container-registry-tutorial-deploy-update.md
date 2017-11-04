---
title: "Samouczek rejestru kontenera Azure — wypychania zaktualizowanego obrazu do wdrożenia regionalne"
description: "Wypychanie zmodyfikowany obraz Docker na platformie Azure z replikacją geograficzną zawierają rejestru, a następnie zobaczyć zmiany automatycznie wdrażane na aplikacje sieci web działające w wielu regionach. Trzy częścią serii, trzech części."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: acr, azure-container-registry, geo-replication
keywords: "Docker, kontenerów, rejestru, Azure"
ms.service: container-registry
ms.devlang: 
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 05c5149ed6c8502c31539f31bfff046f98dc633d
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="push-an-updated-image-to-regional-deployments"></a>Wypychanie zaktualizowanego obrazu do wdrożenia regionalne

Jest to część trzech w trzech części samouczka serii. W [poprzedniego samouczek](container-registry-tutorial-deploy-app.md), replikacja geograficzna została skonfigurowana dla dwóch różnych regionalnych wdrożeń aplikacji sieci Web. W tym samouczku należy najpierw zmodyfikować aplikację, następnie utwórz nowy obraz kontenera i wypchnąć go do rejestru replikacją geograficzną. Ponadto możesz wyświetlić zmianę, automatycznie wdrożone przez elementów webhook rejestru kontenera platformy Azure, w obu przypadkach aplikacji sieci Web.

W tym samouczku końcowej części w serii:

> [!div class="checklist"]
> * Modyfikowanie aplikacji sieci web HTML
> * Tworzenie i tag obrazu Docker
> * Wypchnij zmiany do rejestru kontenera platformy Azure
> * Zaktualizowana aplikacja w dwóch różnych regionach widoku

Jeśli jeszcze nie skonfigurowano dwa *aplikacji sieci Web dla kontenerów* regionalnych wdrożeń, wróć do poprzedniego samouczek z tej serii, [wdrażanie aplikacji sieci web z rejestru kontenera Azure](container-registry-tutorial-deploy-app.md).

## <a name="modify-the-web-application"></a>Modyfikowanie aplikacji sieci Web

W tym kroku należy wprowadzić zmianę do aplikacji sieci web, który ma być dobrze widoczne po push obrazu zaktualizowano kontener w rejestrze kontenera platformy Azure.

Znajdź `AcrHelloworld/Views/Home/Index.cshtml` pliku źródła aplikacji [sklonować z serwisu GitHub](container-registry-tutorial-prepare-registry.md#get-application-code) w poprzednich instrukcji i otwórz go w edytorze tekstu. Dodaj następujący wiersz powyżej `<img>` wiersza:

```html
<h1>MODIFIED</h1>
```

Twoje zmodyfikowanych `Index.cshtml` powinien wyglądać podobnie do:

```html
@{
    ViewData["Title"] = "Azure Container Registry :: Geo-replication";
}
<h1>MODIFIED</h1>
<img width="700" src="~/images/@ViewData["MAPIMAGE"]" />
<ul>
<li>Registry URL: @ViewData["REGISTRYURL"]</li>
<li>Registry IP: @ViewData["REGISTRYIP"]</li>
<li>HostEntry: @ViewData["HOSTENTRY"]</li>
<li>Region: @ViewData["REGION"]</li>
<li>Map: @ViewData["MAPIMAGE"]</li>
</ul>
```

## <a name="rebuild-the-image"></a>Odbuduj obrazu

Teraz, gdy użytkownik zaktualizował aplikacji sieci web, należy ponownie utworzyć obrazu jego kontenera. Jak wcześniej Użyj nazwy obrazu w pełni kwalifikowana, adres URL serwera logowania dla tagu w tym:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="run-the-container-locally"></a>Uruchamianie kontenera w środowisku lokalnym

Przed wdrożeniem w rejestrze kontenera platformy Azure, uruchom obrazu lokalnie w celu sprawdzenia, czy kompilacja zakończyła się pomyślnie.

```bash
docker run -d -p 8080:80 <acrName>.azurecr.io/acr-helloworld:v1
```

Przejdź do adresem http://localhost: 8080 w przeglądarce sieci web, aby potwierdzić, że kontener jest uruchomiona, i modyfikacja jest wyświetlany.

![KONTENER LOKALNEJ OBRAZU][local-container-01]

## <a name="push-image-to-azure-container-registry"></a>Obraz wypychania do rejestru kontenera platformy Azure

Teraz, push zaktualizowanego *acr helloworld* kontener obrazu do rejestru replikacją geograficzną. W tym miejscu jest wykonywane jeden `docker push` polecenie, aby wdrożyć zaktualizowanego obrazu do replik rejestru zarówno *zachodnie stany USA* i *wschodnie stany USA* regionów.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Dane wyjściowe powinny wyglądać podobnie jak poniżej:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
c003ed6fc8b8: Pushed
02b11afef3fd: Layer already exists
cf17b6f921be: Layer already exists
c93ae914d31e: Layer already exists
2eea44510cee: Layer already exists
670f809bd6d5: Layer already exists
a75caa09eb1f: Layer already exists
v1: digest: sha256:e44c0956a21c91e1f5f7bc83f23f1de710c798246df1e0e508c0c88025449646 size: 1792
```

## <a name="view-the-webhook-logs"></a>Wyświetl dzienniki elementu webhook

Gdy obraz jest replikowany, widać elementów webhook rejestru kontenera platformy Azure są wyzwalane.

Aby zobaczyć regionalnych elementów webhook, które zostały utworzone podczas wdrażania kontener, aby *aplikacji sieci Web dla kontenerów* w poprzednim samouczek, przejdź do rejestru kontenera w portalu Azure, a następnie wybierz **elementów Webhook**w obszarze **usług**.

![Kontener rejestru elementów Webhook w portalu Azure][tutorial-portal-01]

Wybierz każdego elementu Webhook, aby wyświetlić historię swoich wywołań i odpowiedzi. Powinien zostać wyświetlony wiersz **wypychania** działania w dziennikach obu elementów Webhook. W tym miejscu znajduje się w dzienniku dla elementu Webhook *zachodnie stany USA* pokazuje region **wypychania** akcji wyzwalane przez `docker push` w poprzednim kroku:

![Kontener rejestru elementu Webhook dziennika w portalu Azure (zachodnie stany USA)][tutorial-portal-02]

## <a name="view-the-updated-web-app"></a>Widok zaktualizowano aplikację sieci web

Elementów Webhook powiadamia aplikacje sieci Web, że nowy obraz został przypisany do rejestru, w którym jest wdrażana automatycznie zaktualizowane kontenera do dwóch aplikacji regionalnej sieci web.

Sprawdź, czy aplikacja została zaktualizowana w obu wdrożeniach przechodząc do obu regionalnych wdrożenia aplikacji sieci Web w przeglądarce sieci web. Dla przypomnienia można znaleźć adres URL wdrożonej aplikacji sieci web w prawym górnym rogu każdej karcie Przegląd usługi aplikacji.

![Omówienie usługi aplikacji w portalu Azure][tutorial-portal-03]

Aby wyświetlić zaktualizowaną aplikację, wybierz łącze w obszarze Przegląd usługi aplikacji. Oto przykład widoku aplikacji uruchomionych w *zachodnie stany USA*:

![Widok przeglądarki aplikacji web zmodyfikowane w regionu zachodnie stany USA][deployed-app-westus-modified]

Sprawdź również wdrożonego obrazu zaktualizowano kontener do *wschodnie stany USA* wdrożenia, wyświetlając go w przeglądarce.

![Widok w przeglądarce aplikacji sieci web zmodyfikowane uruchomionej w regionie wschodnie stany USA][deployed-app-eastus-modified]

Za pomocą jednej `docker push`zaktualizował oba regionalnych wdrożenia aplikacji sieci Web i rejestru kontenera Azure obsługiwanych obrazów kontenera z repozytoriami Zamknij sieci.

## <a name="next-steps"></a>Następne kroki

W tym samouczku aktualizowane i wypychana nowej wersji kontenera aplikacji sieci web do rejestru replikacją geograficzną. Elementów Webhook w rejestrze kontenera Azure powiadomienie aplikacji sieci Web dla kontenerów aktualizacji, które uruchamiane lokalnego ściągnięcia z replik rejestru.

W tym ostatnim samouczek z tej serii, możesz:

> [!div class="checklist"]
> * Zaktualizowana aplikacja sieci web HTML
> * Wbudowane i tagged Docker image
> * Wypychana zmiany do rejestru kontenera platformy Azure
> * Wyświetlić zaktualizowana aplikacja w dwóch różnych regionach.

<!-- IMAGES -->
[deployed-app-eastus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-eastus-modified.png
[deployed-app-westus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-westus-modified.png
[local-container-01]: ./media/container-registry-tutorial-deploy-update/local-container-01.png
[tutorial-portal-01]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-01.png
[tutorial-portal-02]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-02.png
[tutorial-portal-03]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-03.png