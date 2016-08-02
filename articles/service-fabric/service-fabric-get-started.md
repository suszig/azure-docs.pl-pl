<properties
   pageTitle="Konfigurowanie środowiska projektowego | Microsoft Azure"
   description="Zainstaluj środowisko uruchomieniowe, zestaw SDK i narzędzia oraz utwórz lokalny klaster projektowy. Po ukończeniu tej konfiguracji wszystko będzie gotowe do tworzenia aplikacji."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="ryanwi"/>

# Przygotowywanie środowiska projektowego
 Aby tworzyć i uruchamiać [aplikacje platformy Azure Service Fabric][1] na komputerze deweloperskim, należy zainstalować środowisko uruchomieniowe, zestaw SDK oraz narzędzia. Należy również włączyć wykonywanie skryptów programu Windows PowerShell zawartych w zestawie SDK.

## Wymagania wstępne
### Obsługiwane wersje systemu operacyjnego
Na potrzeby tworzenia aplikacji obsługiwane są następujące wersje systemu operacyjnego:

- Windows 7
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] System Windows 7 domyślnie zawiera program Windows PowerShell wyłącznie w wersji 2.0. Aby móc korzystać z poleceń cmdlet programu PowerShell dla platformy Service Fabric, należy zainstalować program PowerShell w wersji 3.0 lub nowszej. Możesz [pobrać program Windows PowerShell 5.0][powershell5-download] w Centrum pobierania Microsoft.

## Instalowanie środowiska uruchomieniowego, zestawu SDK i narzędzi

Instalator platformy sieci Web oferuje trzy konfiguracje na potrzeby tworzenia aplikacji platformy Service Fabric:

- [Instalowanie środowiska uruchomieniowego platformy Service Fabric, zestawu SDK oraz narzędzi dla programu Visual Studio 2015][full-bundle-vs2015]
- [Instalowanie środowiska uruchomieniowego platformy Service Fabric, zestawu SDK oraz narzędzi dla programu Visual Studio "15" Preview][full-bundle-dev15]
- [Instalowanie tylko środowiska uruchomieniowego platformy Service Fabric i zestawu SDK (bez narzędzi dla programu Visual Studio)][core-sdk]


## Włączanie wykonywania skryptów programu PowerShell

Platforma Service Fabric korzysta ze skryptów programu Windows PowerShell do tworzenia lokalnego klastra projektowego i do wdrażania aplikacji z programu Visual Studio. Domyślnie system Windows blokuje uruchamianie tych skryptów. Aby je włączyć, należy zmienić zasady wykonywania w programie PowerShell. Uruchom program PowerShell jako administrator i wprowadź następujące polecenie:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## Następne kroki
Po skonfigurowaniu środowiska projektowego możesz zacząć tworzyć i uruchamiać aplikacji.

- [Tworzenie pierwszej aplikacji platformy Service Fabric w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Informacje o wdrażaniu aplikacji i zarządzaniu nimi w klastrze lokalnym](service-fabric-get-started-with-a-local-cluster.md)
- [Informacje o modelach programowania: Reliable Services i Reliable Actors](service-fabric-choose-framework.md)
- [Przykłady kodu platformy Service Fabric w witrynie GitHub](https://aka.ms/servicefabricsamples)
- [Wizualizowanie klastra przy użyciu usługi Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
- [Ścieżka szkoleniowa platformy Service Fabric — pełne wprowadzenie do platformy](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Strona kampanii platformy Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Link WebPI VS 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Link WebPI Dev15"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ServiceFabricSDK "Link WebPI Core SDK"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395



<!--HONumber=Jun16_HO2-->


