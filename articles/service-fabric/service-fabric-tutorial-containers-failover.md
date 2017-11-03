---
title: "Tryb failover i skalowanie aplikacji kontenery sieć szkieletowa usług Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji, sposobu obsługi trybu failover w aplikacji kontenery sieć szkieletowa usług Azure.  Również sposób skalowania kontenerów i usługi działające w klastrze."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Rozwiązanie docker kontenerów, Mikrousług, sieć szkieletowa usług Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 21dd9dfbc90c26236c43e2c334305ca97f63d361
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="demonstrate-fail-over-and-scaling-of-container-services-with-service-fabric"></a>Prezentacja błędów powyżej i skalowania kontenera usług z sieci szkieletowej usług

W tym samouczku jest częścią trzech serii. Z tego samouczka dowiesz się, sposobu obsługi trybu failover w sieci szkieletowej usług kontenera aplikacji. Ponadto zostanie przedstawiony sposób skalowania kontenerów. W tym samouczku zostały wykonane następujące czynności:

> [!div class="checklist"]
> * Dowiedz się więcej o pracy awaryjnej kontenera w klastrze usługi sieć szkieletowa  
> * Skalowanie kontenerów frontonu sieci web w wnioski

## <a name="prerequisites"></a>Wymagania wstępne
Aplikacja z [część 2](service-fabric-tutorial-package-containers.md) działa w aktywnego klastra sieci szkieletowej usług.

## <a name="fail-over-a-container-in-a-cluster"></a>Przenoszenie kontenera do trybu failover w klastrze
Sieć szkieletowa usług zapewnia swoich wystąpień kontenera automatycznie przenosi do innych węzłów w klastrze, powinien wystąpić błąd. Można też ręcznie opróżniania węzła kontenerów i bezpiecznie przenieść je do innych węzłów w klastrze. Istnieje wiele sposobów skalowania usług — w tym przykładzie użyto narzędzia Service Fabric Explorer.

Aby przenieść kontener frontonu do trybu failover, wykonaj następujące czynności:

1. Otwórz narzędzie Service Fabric Explorer w klastrze — na przykład `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Polecenie **fabric: / obiektu TestContainer/azurevotefront** węzeł w widoku drzewa i rozwiń węzeł partycji (reprezentowane przez identyfikator GUID). Zwróć uwagę, nazwa węzła w elemencie treeview, przedstawiającego węzły tego kontenera jest obecnie uruchomiony w — na przykład`_nodetype_1`
3. Rozwiń węzeł **węzłów** węzła w elemencie treeview. Kliknij przycisk wielokropka (wielokropek) obok węzła, który działa kontenera.
1. Wybierz pozycję **Uruchom ponownie**, aby ponownie uruchomić ten węzeł, i potwierdź akcję ponownego uruchomienia. Ponowne uruchomienie powoduje przeniesienie kontenera do trybu failover w innym węźle klastra.

![noderestart][noderestart]

Powiadomienia, jak wskazujący nazwa węzła w przypadku, gdy jest uruchamiana kontenery frontonu, teraz zmiany do innego węzła w klastrze. Po kilku chwilach powinno mieć możliwość Wyszukaj aplikację ponownie i zobaczyć aplikację teraz uruchomiona na inny węzeł.

## <a name="scale-containers-and-services-in-a-cluster"></a>Usługi w klastrze i skalowania kontenerów
Kontenery sieci szkieletowej usług mogą być skalowane w klastrze, aby pomieścić obciążenia w ramach usług. Kontener można skalować, zmieniając liczbę wystąpień w klastrze.

Aby skalować frontonu sieci web, wykonaj następujące czynności:

1. Otwórz narzędzie Service Fabric Explorer w klastrze — na przykład `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Kliknij wielokropek (wielokropek) obok pozycji **sieci szkieletowej: / obiektu TestContainer/azurevotefront** węzeł w drzewie wyświetlania i wybierz polecenie **skali usługi**.

![sfxscale][sfxscale]

Można teraz skalowanie liczby wystąpień frontonu sieci web.

3. Zmień liczbę na **2** i kliknij pozycję **Skaluj usługę**.
4. Polecenie **fabric: / obiektu TestContainer/azurevotefront** węzeł w drzewie rozwiń węzeł partycji (reprezentowane przez identyfikator GUID).

![sfxscaledone][sfxscaledone]

Teraz możesz zobaczyć, że usługa ma dwa wystąpienia. W widoku drzewa Zobacz temat węzły, których wystąpienia Uruchom na.

Wykonując to proste zadanie zarządzania, podwoiliśmy zasoby dostępne dla naszej usługi frontonu w celu przetworzenia obciążenia użytkownika. Ważne jest, aby zrozumieć, że nie musisz mieć wielu wystąpień usługi, aby działała ona niezawodnie. W przypadku niepowodzenia usługi usługa Service Fabric zapewnia, że nowe wystąpienie usługi jest uruchamiane w klastrze.

## <a name="next-steps"></a>Następne kroki

W tym samouczku trybu failover kontenera wykazała, a także skalowanie aplikacji. Wykonano następujące czynności:

> [!div class="checklist"]
> * Dowiedz się więcej o pracy awaryjnej kontenera w klastrze usługi sieć szkieletowa  
> * Skalowanie kontenerów frontonu sieci web w wnioski

W tym samouczku, przedstawiono sposób: 
> [!div class="checklist"]
> * Tworzenie kontenera obrazów
> * Wypychanie kontener obrazów do rejestru kontenera platformy Azure
> * Kontenery pakietu dla platformy Service Fabric przy użyciu narzędzia Yeoman
> * Tworzenie i uruchamianie aplikacji sieci szkieletowej usług z kontenerami
> * Sposób obsługi trybu failover i skalowania w sieci szkieletowej usług

[noderestart]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialnoderestart.png
[sfxscale]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscale.png
[sfxscaledone]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscaledone.png
