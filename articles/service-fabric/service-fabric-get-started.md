---
title: "Konfigurowanie środowiska projektowego | Microsoft Docs"
description: "Zainstaluj środowisko uruchomieniowe, zestaw SDK i narzędzia oraz utwórz lokalny klaster projektowy. Po ukończeniu tej konfiguracji wszystko będzie gotowe do tworzenia aplikacji."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/26/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7ae0fcc689d51479a92c506ea48ab8af2003acfe


---
# <a name="prepare-your-development-environment"></a>Przygotowywanie środowiska projektowego
> [!div class="op_single_selector"]
> -[ Windows](service-fabric-get-started.md)
> 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Aby tworzyć i uruchamiać [aplikacje usługi Azure Service Fabric][1] na maszynie deweloperskiej, należy zainstalować środowisko uruchomieniowe, zestaw SDK oraz narzędzia. Należy również włączyć wykonywanie skryptów programu Windows PowerShell zawartych w zestawie SDK.

## <a name="prerequisites"></a>Wymagania wstępne
### <a name="supported-operating-system-versions"></a>Obsługiwane wersje systemu operacyjnego
Na potrzeby tworzenia aplikacji obsługiwane są następujące wersje systemu operacyjnego:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows 10

> [!NOTE]
> System Windows 7 domyślnie zawiera program Windows PowerShell wyłącznie w wersji 2.0. Polecenia cmdlet programu PowerShell usługi Service Fabric wymagają programu PowerShell w wersji 3.0 lub nowszej. Możesz [pobrać program Windows PowerShell 5.0][powershell5-download] w Centrum pobierania Microsoft.
> 
> 

## <a name="install-the-runtime-sdk-and-tools"></a>Instalowanie środowiska uruchomieniowego, zestawu SDK i narzędzi
Instalator platformy sieci Web oferuje dwie konfiguracje na potrzeby tworzenia aplikacji platformy Service Fabric:

* [Instalowanie środowiska uruchomieniowego platformy Service Fabric, zestawu SDK oraz narzędzi dla programu Visual Studio 2015 (wymaga programu Visual Studio 2015 z aktualizacją 2 lub nowszą)][full-bundle-vs2015]
* [Instalowanie tylko środowiska uruchomieniowego platformy Service Fabric i zestawu SDK (bez narzędzi dla programu Visual Studio)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Włączanie wykonywania skryptów programu PowerShell
Platforma Service Fabric korzysta ze skryptów programu Windows PowerShell do tworzenia lokalnego klastra projektowego i do wdrażania aplikacji z programu Visual Studio. Domyślnie system Windows blokuje uruchamianie tych skryptów. Aby je włączyć, należy zmienić zasady wykonywania w programie PowerShell. Uruchom program PowerShell jako administrator i wprowadź następujące polecenie:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Następne kroki
Po skonfigurowaniu środowiska projektowego możesz zacząć kompilować i uruchamiać aplikacje.

* [Tworzenie pierwszej aplikacji platformy Service Fabric w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Informacje o wdrażaniu aplikacji i zarządzaniu nimi w klastrze lokalnym](service-fabric-get-started-with-a-local-cluster.md)
* [Informacje o modelach programowania: Reliable Services i Reliable Actors](service-fabric-choose-framework.md)
* [Przykłady kodu platformy Service Fabric w witrynie GitHub](https://aka.ms/servicefabricsamples)
* [Wizualizowanie klastra przy użyciu narzędzia Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Ścieżka szkoleniowa platformy Service Fabric — pełne wprowadzenie do platformy](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Strona kampanii platformy Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Link WebPI VS 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Link WebPI Dev15"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Link WebPI Core SDK"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395



<!--HONumber=Nov16_HO2-->


