---
title: Konfigurowanie hosta Docker z VirtualBox | Dokumentacja firmy Microsoft
description: "Instrukcje krok po kroku, aby skonfigurować domyślne wystąpienie Docker przy użyciu rozwiązania Docker maszyny i VirtualBox"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 0b1335a2-7720-42a8-8260-4e06fc00c9f6
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: e9465afb560a73d74f853c19094b3ee75b8c470c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-docker-host-with-virtualbox"></a>Konfigurowanie hosta Docker z VirtualBox
## <a name="overview"></a>Omówienie
W tym artykule przedstawiono Konfigurowanie domyślnego wystąpienia Docker przy użyciu rozwiązania Docker maszyny i VirtualBox. Jeśli używasz [Docker dla systemu Windows w wersji beta](http://beta.docker.com/), ta konfiguracja nie jest konieczne.

## <a name="prerequisites"></a>Wymagania wstępne
Następujące narzędzia muszą być zainstalowane.

* [Przybornik docker](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Konfigurowanie klienta platformy Docker przy użyciu programu Windows PowerShell
Aby skonfigurować klienta Docker, po prostu otwórz program Windows PowerShell i wykonaj następujące czynności:

1. Utwórz wystąpienie domyślne hostów docker.
   
    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
2. Sprawdź, czy domyślne wystąpienie jest skonfigurowana i uruchomiona. (Powinna zostać wyświetlona wystąpienia o nazwie "default" systemem.
   
    ```PowerShell
    docker-machine ls 
    ```
   
    ![dane wyjściowe ls maszyny docker][0]
3. Ustaw domyślny jako bieżącego hosta i skonfiguruj powłoki.
   
    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```
4. Wyświetlanie aktywnych kontenery Docker. Lista powinna być pusta.
   
    ```PowerShell
    docker ps
    ```
   
    ![dane wyjściowe ps docker][1]

> [!NOTE]
> Każdym uruchomieniu komputerze deweloperskim, będzie konieczne ponowne uruchomienie hosta lokalnego docker.
> Aby to zrobić, należy wydać następujące polecenie w wierszu polecenia: `docker-machine start default`.
> 
> 

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
