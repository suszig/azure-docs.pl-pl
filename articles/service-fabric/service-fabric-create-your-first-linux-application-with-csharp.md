---
title: "Tworzenie pierwszej aplikacji mikrousług platformy Azure w systemie Linux przy użyciu języka C# | Microsoft Docs"
description: "Tworzenie i wdrażanie aplikacji usługi Service Fabric przy użyciu platformy C#"
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 4baf144cc28eeff0ab8f8b60e837f8a2bad903af
ms.contentlocale: pl-pl
ms.lasthandoff: 07/01/2017


---
<a id="create-your-first-azure-service-fabric-application" class="xliff"></a>

# Tworzenie pierwszej aplikacji usługi Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# — Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java — Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# — Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Usługa Service Fabric udostępnia zestawy SDK do kompilowania usług w systemie Linux przy użyciu platform .NET Core i Java. W tym samouczku przedstawiony zostanie sposób kompilowania aplikacji dla systemu Linux i tworzenia usługi przy użyciu platformy C# (.NET Core).

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne
Przed rozpoczęciem upewnij się, że masz [skonfigurowane środowisko programowania systemu Linux](service-fabric-get-started-linux.md). Jeśli używasz systemu Mac OS X, możesz [skonfigurować jednopunktowe środowisko systemu Linux na maszynie wirtualnej za pomocą narzędzia Vagrant](service-fabric-get-started-mac.md).

Możesz też skonfigurować [interfejs wiersza polecenia platformy Azure 2.0](service-fabric-azure-cli-2-0.md) (zalecane) lub [interfejs wiersza polecenia XPlat](service-fabric-azure-cli.md) na potrzeby wdrażania aplikacji.

<a id="create-the-application" class="xliff"></a>

## Tworzenie aplikacji
Aplikacja usługi Service Fabric może zawierać jedną lub więcej usług, a każda z nich pełni określoną rolę w dostarczaniu funkcjonalności aplikacji. Zestaw SDK usługi Service Fabric dla systemu Linux zawiera generator [Yeoman](http://yeoman.io/), który ułatwia tworzenie pierwszej usługi i dodawanie kolejnych w przyszłości. Utwórzmy zatem aplikację z jedną usługą za pomocą generatora Yeoman.

1. W terminalu wpisz następujące polecenie i rozpocznij tworzenie szkieletu: `yo azuresfcsharp`
2. Nadaj nazwę aplikacji.
3. Wybierz typ pierwszej usługi i nadaj jej nazwę. Na potrzeby tego samouczka wybierzmy usługę Reliable Actor Service.

   ![Generator Yeoman usługi Service Fabric dla platformy C#][sf-yeoman]

> [!NOTE]
> Aby uzyskać więcej informacji o opcjach, zobacz [Service Fabric programming model overview](service-fabric-choose-framework.md) (Omówienie modelu programowania usługi Service Fabric).
>
>

<a id="build-the-application" class="xliff"></a>

## Kompilowanie aplikacji
Szablony generatora Yeoman usługi Service Fabric obejmują skrypt kompilacji, którego można użyć do skompilowania aplikacji z poziomu terminala (po przejściu do folderu aplikacji).

  ```sh
 cd myapp
 ./build.sh
  ```

<a id="deploy-the-application" class="xliff"></a>

## Wdrażanie aplikacji

Skompilowaną aplikację można wdrożyć w klastrze lokalnym.

<a id="using-xplat-cli" class="xliff"></a>

### Korzystanie z interfejsu wiersza polecenia XPlat

1. Połącz się z lokalnym klastrem usługi Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Uruchom skrypt instalacji udostępniony w szablonie, aby skopiować pakiet aplikacji do magazynu obrazów klastra, zarejestrować typ aplikacji i utworzyć wystąpienie aplikacji.

    ```bash
    ./install.sh
    ```

<a id="using-azure-cli-20" class="xliff"></a>

### Korzystanie z interfejsu wiersza polecenia platformy Azure 2.0

Wdrażanie skompilowanej aplikacji przebiega tak samo jak w przypadku innych aplikacji usługi Service Fabric. Szczegółowe instrukcje są dostępne w dokumentacji dotyczącej [zarządzania aplikacją usługi Service Fabric za pomocą interfejsu wiersza polecenia platformy Azure](service-fabric-application-lifecycle-azure-cli-2-0.md).

Parametry tych poleceń można znaleźć w manifestach wygenerowanych w pakiecie aplikacji.

Po wdrożeniu aplikacji otwórz przeglądarkę i przejdź do narzędzia [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) pod adresem [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Następnie rozwiń węzeł **Aplikacje** i zwróć uwagę, że istnieje teraz wpis dla danego typu aplikacji i inny wpis dla pierwszego wystąpienia tego typu.

<a id="start-the-test-client-and-perform-a-failover" class="xliff"></a>

## Uruchamianie klienta testowego i przechodzenie w tryb failover
Projekty aktora nie działają samodzielnie. Wymagają one wysyłania im komunikatów przez inną usługę lub klienta. Szablon aktora zawiera prosty skrypt testowy, którego można użyć do interakcji z usługą aktora.

1. Uruchom skrypt za pomocą narzędzia kontrolnego, aby wyświetlić dane wyjściowe usługi aktora.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. W narzędziu Service Fabric Explorer zlokalizuj węzeł, w którym znajduje się replika podstawowa usługi aktora. Na poniższym zrzucie ekranu jest to węzeł 3.

    ![Znajdowanie repliki podstawowej w narzędziu Service Fabric Explorer][sfx-primary]
3. Kliknij węzeł znaleziony w poprzednim kroku, a następnie wybierz pozycję **Dezaktywuj (uruchom ponownie)** z menu Akcje. Spowoduje to ponowne uruchomienie jednego węzła w klastrze lokalnym i wymuszenie przejścia w tryb failover do repliki pomocniczej uruchomionej na innym węźle. Podczas wykonywania tej czynności należy zwrócić uwagę na dane wyjściowe z klienta testowego oraz to, że w liczniku będzie nadal występować przyrost niezależnie od trybu failover.

<a id="adding-more-services-to-an-existing-application" class="xliff"></a>

## Dodawanie kolejnych usług do istniejącej aplikacji

Aby dodać kolejną usługę do aplikacji utworzonej już przy użyciu polecenia `yo`, wykonaj następujące czynności: 
1. Zmień katalog na katalog główny istniejącej aplikacji.  Na przykład wpisz polecenie `cd ~/YeomanSamples/MyApplication`, jeśli aplikacja `MyApplication` to aplikacja utworzona przez narzędzie Yeoman.
2. Uruchom polecenie `yo azuresfcsharp:AddService`

<a id="migrating-from-projectjson-to-csproj" class="xliff"></a>

## Migrowanie z plików project.json do plików csproj
1. Uruchomienie polecenia „dotnet migrate” w katalogu głównym projektu spowoduje zmigrowanie wszystkich plików project.json do formatu csproj.
2. Zaktualizuj odpowiednio odwołania do projektu w plikach projektu tak, aby wskazywały pliki csproj.
3. Zaktualizuj nazwy plików projektu w plikach csproj w skrypcie build.sh.

<a id="next-steps" class="xliff"></a>

## Następne kroki
* [Dowiedz się więcej o usłudze Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interakcja z klastrami usługi Service Fabric przy użyciu interfejsu wiersza polecenia platformy Azure](service-fabric-azure-cli.md)
* Uzyskaj informacje o [opcjach pomocy technicznej usługi Service Fabric](service-fabric-support.md)

<a id="related-articles" class="xliff"></a>

## Pokrewne artykuły:

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Wprowadzenie do usługi Service Fabric i interfejsu wiersza polecenia platformy Azure 2.0)
* [Getting started with Service Fabric XPlat CLI](service-fabric-azure-cli.md) (Wprowadzenie do interfejsu wiersza polecenia XPlat usługi Service Fabric)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png

