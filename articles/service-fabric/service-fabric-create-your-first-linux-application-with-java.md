---
title: "Tworzenie pierwszej aplikacji mikrousług platformy Azure w systemie Linux przy użyciu platformy Java | Microsoft Docs"
description: "Tworzenie i wdrażanie aplikacji usługi Service Fabric przy użyciu platformy Java"
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/02/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: e229602b4bfa72977c9b15e854d796ed09fa55d2
ms.contentlocale: pl-pl
ms.lasthandoff: 07/01/2017


---
<a id="create-your-first-service-fabric-java-application-on-linux" class="xliff"></a>

# Tworzenie pierwszej aplikacji Java usługi Service Fabric w systemie Linux
> [!div class="op_single_selector"]
> * [C# — Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java — Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# — Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Ten przewodnik Szybki start pomaga utworzyć w ciągu kilku minut pierwszą aplikację Java usługi Azure Service Fabric w środowisku deweloperskim systemu Linux.  W wyniku pracy z przewodnikiem zostanie utworzona prosta jednousługowa aplikacja Java uruchomiona w lokalnym klastrze projektowym.  

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne
Przed rozpoczęciem zainstaluj zestaw SDK usługi Service Fabric oraz interfejs wiersza polecenia platformy Azure i skonfiguruj klaster projektowy w [środowisku deweloperskim systemu Linux](service-fabric-get-started-linux.md). Jeśli używasz systemu Mac OS X, możesz [skonfigurować środowisko deweloperskie systemu Linux na maszynie wirtualnej za pomocą narzędzia Vagrant](service-fabric-get-started-mac.md).

Możesz też skonfigurować [interfejs wiersza polecenia platformy Azure 2.0](service-fabric-azure-cli-2-0.md) (zalecane) lub [interfejs wiersza polecenia XPlat](service-fabric-azure-cli.md) na potrzeby wdrażania aplikacji.

<a id="create-the-application" class="xliff"></a>

## Tworzenie aplikacji
Aplikacja usługi Service Fabric zawiera jedną lub więcej usług, a każda z nich pełni określoną rolę w dostarczaniu funkcjonalności aplikacji. Zestaw SDK usługi Service Fabric dla systemu Linux zawiera generator [Yeoman](http://yeoman.io/), który ułatwia tworzenie pierwszej usługi i dodawanie kolejnych w przyszłości.  Możesz też tworzyć, kompilować i wdrażać aplikacje Java usługi Service Fabric przy użyciu wtyczki dla środowiska Eclipse. Zobacz [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java za pomocą środowiska Eclipse). Na potrzeby tego przewodnika Szybki start użyj narzędzia Yeoman, aby utworzyć aplikację z jedną usługą, w której jest przechowywana i pobierana wartość licznika.

1. Na terminalu wpisz ``yo azuresfjava``.
2. Nadaj nazwę aplikacji. 
3. Wybierz typ pierwszej usługi i nadaj jej nazwę. Na potrzeby tego samouczka wybierz usługę Reliable Actor Service. Aby uzyskać więcej informacji o pozostałych typach usług, zobacz [Service Fabric programming model overview](service-fabric-choose-framework.md) (Omówienie modelu programowania usługi Service Fabric).
   ![Generator Yeoman usługi Service Fabric dla platformy Java][sf-yeoman]

<a id="build-the-application" class="xliff"></a>

## Kompilowanie aplikacji
Szablony generatora Yeoman usługi Service Fabric obejmują skrypt kompilacji dla narzędzia [Gradle](https://gradle.org/), którego można użyć do skompilowania aplikacji z poziomu terminala. Aby skompilować aplikację i utworzyć pakiet aplikacji, uruchom następujące polecenie:

  ```bash
  cd myapp
  gradle
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
Aktorzy nie pełnią samodzielnie żadnej funkcji. Wymagają wysyłania do nich komunikatów przez inną usługę lub innego klienta. Szablon aktora zawiera prosty skrypt testowy, którego można użyć do interakcji z usługą aktora.

1. Uruchom skrypt za pomocą narzędzia kontrolnego, aby wyświetlić dane wyjściowe usługi aktora.  Skrypt testowy wywołuje metodę `setCountAsync()` dla aktora w celu zwiększenia wartości licznika, wywołuje metodę `getCountAsync()` dla aktora w celu pobrania nowej wartości licznika i wyświetla tę wartość w konsoli.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. W narzędziu Service Fabric Explorer zlokalizuj węzeł, w którym znajduje się replika podstawowa usługi aktora. Na poniższym zrzucie ekranu jest to węzeł 3. Replika podstawowa usługi obsługuje operacje odczytu i zapisu.  Zmiany stanu usługi są następnie replikowane w replikach pomocniczych — na poniższym zrzucie ekranu są one uruchomione w węzłach 0 i 1.

    ![Znajdowanie repliki podstawowej w narzędziu Service Fabric Explorer][sfx-primary]

3. W lokalizacji **Węzły** kliknij węzeł znaleziony w poprzednim kroku, a następnie wybierz pozycję **Dezaktywuj (uruchom ponownie)** z menu Akcje. Ta czynność spowoduje ponowne uruchomienie węzła w replice podstawowej usługi i wymuszenie przejścia w tryb failover do jednej z replik pomocniczych uruchomionych w innym węźle.  Poziom tej repliki pomocniczej zostanie podwyższony do repliki podstawowej, inna replika pomocnicza zostanie utworzona w innym węźle oraz replika podstawowa zacznie obsługiwać operacje odczytu i zapisu. Podczas ponownego uruchamiania węzła należy zwrócić uwagę na dane wyjściowe z klienta testowego oraz to, że licznik będzie nadal się zwiększać niezależnie od trybu failover.

<a id="add-another-service-to-the-application" class="xliff"></a>

## Dodawanie kolejnej usługi do aplikacji
Aby dodać kolejną usługę do istniejącej aplikacji utworzonej przy użyciu polecenia `yo`, wykonaj następujące czynności:
1. Zmień katalog na katalog główny istniejącej aplikacji.  Na przykład wpisz polecenie `cd ~/YeomanSamples/MyApplication`, jeśli aplikacja `MyApplication` to aplikacja utworzona przez narzędzie Yeoman.
2. Uruchom polecenie `yo azuresfjava:AddService`
3. Skompiluj i wdróż aplikację, tak jak w poprzednich krokach.

<a id="remove-the-application" class="xliff"></a>

## Usuwanie aplikacji
Użyj skryptu dezinstalacji udostępnionego w szablonie, aby usunąć wystąpienie aplikacji, wyrejestrować pakiet aplikacji i usunąć pakiet aplikacji z magazynu obrazów klastra.

```bash
./uninstall.sh
```

W narzędziu Service Fabric Explorer aplikacja i typ aplikacji nie są już wyświetlane w węźle **Aplikacje**.

<a id="next-steps" class="xliff"></a>

## Następne kroki
* [Tworzenie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu środowiska Eclipse](service-fabric-get-started-eclipse.md)
* [Dowiedz się więcej o usłudze Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interakcja z klastrami usługi Service Fabric przy użyciu interfejsu wiersza polecenia platformy Azure](service-fabric-azure-cli.md)
* [Rozwiązywanie problemów z wdrażaniem](service-fabric-azure-cli.md#troubleshooting)
* Uzyskaj informacje o [opcjach pomocy technicznej usługi Service Fabric](service-fabric-support.md)

<a id="related-articles" class="xliff"></a>

## Pokrewne artykuły:

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Wprowadzenie do usługi Service Fabric i interfejsu wiersza polecenia platformy Azure 2.0)
* [Getting started with Service Fabric XPlat CLI](service-fabric-azure-cli.md) (Wprowadzenie do interfejsu wiersza polecenia XPlat usługi Service Fabric)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

