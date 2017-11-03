---
title: "Szybki Start — tworzenie Twojego pierwszego kontenera wystąpień kontenera platformy Azure przy użyciu portalu Azure"
description: "Wdrażaj i rozpocznij pracę z usługą Azure Container Instances"
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 0179107ece1e150246ab40836783d810425be3ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Tworzenie pierwszego kontenera w usłudze Azure Container Instances

Usługa Azure Container Instances ułatwia tworzenie kontenerów i zarządzanie nimi na platformie Azure. Tego przewodnika Szybki Start służy do tworzenia kontenera na platformie Azure i uwidacznia go do Internetu za pomocą publicznego adresu IP. Ta operacja trwa za pomocą portalu Azure. Za pomocą kilku kliknięć zostanie wyświetlony to w przeglądarce:

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-container-instance"></a>Utwórz wystąpienie kontenera

Wybierz **nowy** > **kontenery** > **wystąpień kontenera platformy Azure (wersja zapoznawcza)**.

![Rozpocznij tworzenie nowego wystąpienia kontenera w portalu Azure][aci-portal-01]

Wprowadź następujące wartości w **nazwa kontenera**, **obrazu kontenera**, i **grupy zasobów** pól tekstowych. Pozostaw pozostałe wartości domyślne, a następnie kliknij przycisk **OK**.

* Nazwa kontenera:`mycontainer`
* Kontener obrazu:`microsoft/aci-helloworld`
* Grupa zasobów:`myResourceGroup`

![Konfigurowanie ustawień podstawowych dla nowego wystąpienia kontenera w portalu Azure][aci-portal-03]

Kontenery w systemach Windows i Linux można tworzyć w wystąpień kontenera platformy Azure. W tym szybkiego startu, pozostanie domyślne ustawienie **Linux** od nas określony kontener opartych na systemie Linux (`microsoft/aci-helloworld`) w poprzednim kroku.

Pozostaw ustawienia w **konfiguracji** konfiguracji domyślnej, następnie kliknij przycisk **OK** do sprawdzenia poprawności konfiguracji.

![Konfigurowanie nowego wystąpienia kontenera w portalu Azure][aci-portal-04]

Po zakończeniu sprawdzania poprawności, przedstawiono podsumowanie ustawień kontenera. Wybierz **OK** Aby przesłać żądanie wdrożenia kontenera.

![Ustawienia podsumowania dla nowego wystąpienia kontenera w portalu Azure][aci-portal-05]

Po uruchomieniu wdrożenia kafelka znajduje się na pulpicie nawigacyjnym portalu wskazujący postęp wdrażania. Po zakończeniu wdrażania, Kafelek jest aktualizowana w celu wyświetlenia nowego **myc1 mojkontener** grupy kontenerów.

![Postęp tworzenia nowego wystąpienia kontenera w portalu Azure][aci-portal-08]

Wybierz **myc1 mojkontener** grupy kontenerów, aby wyświetlić właściwości grupy kontenera. Zwróć uwagę na **adres Ip** grupy kontenera, jak również **stanu** Twojego kontenera.

![Kontener grupy — omówienie w portalu Azure][aci-portal-06]

Gdy przesuwa kontenera **systemem** stanu, przejdź do adresu IP, możesz zauważyć, że w poprzednim kroku, aby wyświetlić hostowanej aplikacji w Twojej nowy kontener.

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-02]: ./media/container-instances-quickstart-portal/qs-portal-02.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-app-browser]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png

## <a name="next-steps"></a>Następne kroki

W tego przewodnika Szybki Start utworzono wystąpienie kontenera platformy Azure z obrazu w publicznych repozytorium Centrum Docker. Jeśli chcesz spróbować tworzenia kontenera samodzielnie, a następnie wdrożyć go do wystąpień kontenera Azure za pomocą rejestru kontenera platformy Azure, przejdź do samouczka wystąpień kontenera Azure.

> [!div class="nextstepaction"]
> [Samouczki dotyczące usługi Azure Container Instances](./container-instances-tutorial-prepare-app.md)