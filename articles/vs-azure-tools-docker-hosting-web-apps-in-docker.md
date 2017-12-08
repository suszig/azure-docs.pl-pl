---
title: "Wdrażanie kontenera platformy ASP.NET Core Linux Docker z hostem zdalnym Docker | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak użyć programu Visual Studio Tools for Docker do wdrażania aplikacji sieci web platformy ASP.NET Core w kontenerze Docker działającym na maszynie Wirtualnej platformy Azure Docker hosta systemu Linux"
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 4a87ee69f23779bf4f6f5db40bc05edbcfc7668d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Wdrażanie kontenera ASP.NET z hostem zdalnym Docker
## <a name="overview"></a>Omówienie
Docker jest aparatem lekkie kontenera, podobne w pewnym sensie do maszyny wirtualnej, która służy do obsługi aplikacji i usług.
Ten samouczek przedstawia przy użyciu [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) rozszerzenia do wdrażania aplikacji platformy ASP.NET Core z hostem platformy Docker na platformie Azure przy użyciu programu PowerShell.

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka niezbędne jest, następujące elementy:

* Tworzenie maszyny Wirtualnej platformy Azure Docker hosta, zgodnie z opisem w [sposób używania maszyny docker z platformy Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Zainstaluj najnowszą wersję pakietu [programu Visual Studio](https://www.visualstudio.com/downloads/)
* Pobierz [zestawu SDK programu Microsoft ASP.NET Core 1.0](https://go.microsoft.com/fwlink/?LinkID=809122)
* Zainstaluj [Docker dla systemu Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Tworzenie aplikacji sieci web platformy ASP.NET Core
Poniższe kroki pomocne przy tworzeniu Podstawowa aplikacja platformy ASP.NET Core, który będzie używany w tym samouczku.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Dodawanie obsługi Docker
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Użyj skryptu programu PowerShell DockerTask.ps1
1. Otwórz wiersz programu PowerShell do katalogu głównego projektu. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Sprawdź poprawność zdalnego hosta jest uruchomiony. Powinien zostać wyświetlony stan = uruchomiona 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Tworzenie aplikacji w programie parametru - kompilacji
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Uruchamianie aplikacji, za pomocą parametru - uruchomienia
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Po zakończeniu docker powinny być widoczne wyniki podobne do następujących:
   
   ![Wyświetlanie aplikacji][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
