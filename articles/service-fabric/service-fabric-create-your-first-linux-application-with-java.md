---
title: "Tworzenie aplikacji Java z elementami Reliable Actors usługi Azure Service Fabric w systemie Linux | Microsoft Docs"
description: "Dowiedz się, jak utworzyć i wdrożyć aplikację Java z elementami Reliable Actors usługi Service Fabric w ciągu pięciu minut."
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
ms.date: 06/29/2017
ms.author: ryanwi
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 254f38a600ea4026120bc411368eeb01310e56b2
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Tworzenie pierwszej aplikacji Java z elementami Reliable Actors usługi Service Fabric w systemie Linux

Ten przewodnik Szybki start pomaga utworzyć w ciągu kilku minut pierwszą aplikację Java usługi Azure Service Fabric w środowisku deweloperskim systemu Linux.  W wyniku pracy z przewodnikiem zostanie utworzona prosta jednousługowa aplikacja Java uruchamiana w lokalnym klastrze projektowym.  

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem zainstaluj zestaw SDK usługi Service Fabric oraz interfejs wiersza polecenia platformy Azure i skonfiguruj klaster projektowy w [środowisku deweloperskim systemu Linux](service-fabric-get-started-linux.md). Jeśli używasz systemu Mac OS X, możesz [skonfigurować środowisko deweloperskie systemu Linux na maszynie wirtualnej za pomocą narzędzia Vagrant](service-fabric-get-started-mac.md).

Możesz też skonfigurować [interfejs wiersza polecenia platformy Azure 2.0](service-fabric-azure-cli-2-0.md) (zalecane) lub [interfejs wiersza polecenia XPlat](service-fabric-azure-cli.md) na potrzeby wdrażania aplikacji.

## <a name="create-the-application"></a>Tworzenie aplikacji
Aplikacja usługi Service Fabric zawiera jedną lub więcej usług, a każda z nich pełni określoną rolę w dostarczaniu funkcjonalności aplikacji. Zestaw SDK usługi Service Fabric dla systemu Linux zawiera generator [Yeoman](http://yeoman.io/), który ułatwia tworzenie pierwszej usługi i dodawanie kolejnych w przyszłości.  Możesz też tworzyć, kompilować i wdrażać aplikacje Java usługi Service Fabric przy użyciu wtyczki dla środowiska Eclipse. Zobacz [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md) (Tworzenie i wdrażanie pierwszej aplikacji Java za pomocą środowiska Eclipse). Na potrzeby tego przewodnika Szybki start użyj narzędzia Yeoman, aby utworzyć aplikację z jedną usługą, w której jest przechowywana i pobierana wartość licznika.

1. Na terminalu wpisz ``yo azuresfjava``.
2. Nadaj nazwę aplikacji. 
3. Wybierz typ pierwszej usługi i nadaj jej nazwę. Na potrzeby tego samouczka wybierz usługę Reliable Actor Service. Aby uzyskać więcej informacji o pozostałych typach usług, zobacz [Service Fabric programming model overview](service-fabric-choose-framework.md) (Omówienie modelu programowania usługi Service Fabric).
   ![Generator Yeoman usługi Service Fabric dla platformy Java][sf-yeoman]

## <a name="build-the-application"></a>Kompilowanie aplikacji
Szablony generatora Yeoman usługi Service Fabric obejmują skrypt kompilacji dla narzędzia [Gradle](https://gradle.org/), którego można użyć do skompilowania aplikacji z poziomu terminalu. Aby skompilować aplikację i utworzyć jej pakiet, uruchom następujące polecenie:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a>Wdrażanie aplikacji
Skompilowaną aplikację można wdrożyć w klastrze lokalnym.

### <a name="using-xplat-cli"></a>Korzystanie z interfejsu wiersza polecenia XPlat

1. Połącz się z lokalnym klastrem usługi Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Uruchom skrypt instalacji udostępniony w szablonie, aby skopiować pakiet aplikacji do magazynu obrazów klastra, zarejestrować typ aplikacji i utworzyć wystąpienie aplikacji.

    ```bash
    ./install.sh
    ```

### <a name="using-azure-cli-20"></a>Korzystanie z interfejsu wiersza polecenia platformy Azure 2.0

Wdrażanie skompilowanej aplikacji przebiega tak samo jak w przypadku innych aplikacji usługi Service Fabric. Szczegółowe instrukcje są dostępne w dokumentacji dotyczącej [zarządzania aplikacją usługi Service Fabric za pomocą interfejsu wiersza polecenia platformy Azure](service-fabric-application-lifecycle-azure-cli-2-0.md).

Parametry tych poleceń można znaleźć w manifestach wygenerowanych w pakiecie aplikacji.

Po wdrożeniu aplikacji otwórz przeglądarkę i przejdź do narzędzia [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) pod adresem [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Następnie rozwiń węzeł **Aplikacje** i zwróć uwagę, że istnieje teraz wpis dla danego typu aplikacji i inny wpis dla pierwszego wystąpienia tego typu.

## <a name="start-the-test-client-and-perform-a-failover"></a>Uruchamianie klienta testowego i przechodzenie w tryb failover
Aktorzy nie pełnią samodzielnie żadnej funkcji. Wymagają wysyłania do nich komunikatów przez inną usługę lub innego klienta. Szablon aktora zawiera prosty skrypt testowy, którego można użyć do interakcji z usługą aktora.

1. Uruchom skrypt za pomocą narzędzia kontrolnego, aby wyświetlić dane wyjściowe usługi aktora.  Skrypt testowy wywołuje metodę `setCountAsync()` dla aktora w celu zwiększenia wartości licznika, wywołuje metodę `getCountAsync()` dla aktora w celu pobrania nowej wartości licznika i wyświetla tę wartość w konsoli.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. W narzędziu Service Fabric Explorer zlokalizuj węzeł, w którym znajduje się replika podstawowa usługi aktora. Na poniższym zrzucie ekranu jest to węzeł 3. Replika podstawowa usługi obsługuje operacje odczytu i zapisu.  Zmiany stanu usługi są następnie replikowane w replikach pomocniczych — na poniższym zrzucie ekranu są one uruchomione w węzłach 0 i 1.

    ![Znajdowanie repliki podstawowej w narzędziu Service Fabric Explorer][sfx-primary]

3. W lokalizacji **Węzły** kliknij węzeł znaleziony w poprzednim kroku, a następnie wybierz pozycję **Dezaktywuj (uruchom ponownie)** z menu Akcje. Ta czynność spowoduje ponowne uruchomienie węzła w replice podstawowej usługi i wymuszenie przejścia w tryb failover do jednej z replik pomocniczych uruchomionych w innym węźle.  Poziom tej repliki pomocniczej zostanie podwyższony do repliki podstawowej, inna replika pomocnicza zostanie utworzona w innym węźle oraz replika podstawowa zacznie obsługiwać operacje odczytu i zapisu. Podczas ponownego uruchamiania węzła należy zwrócić uwagę na dane wyjściowe z klienta testowego oraz to, że licznik będzie nadal się zwiększać niezależnie od trybu failover.

## <a name="remove-the-application"></a>Usuwanie aplikacji
Użyj skryptu odinstalowania udostępnionego w szablonie, aby usunąć wystąpienie aplikacji, wyrejestrować pakiet aplikacji i usunąć pakiet aplikacji z magazynu obrazów klastra.

```bash
./uninstall.sh
```

W narzędziu Service Fabric Explorer aplikacja i typ aplikacji nie są już wyświetlane w węźle **Aplikacje**.

## <a name="next-steps"></a>Następne kroki
* [Tworzenie pierwszej aplikacji Java usługi Service Fabric w systemie Linux przy użyciu środowiska Eclipse](service-fabric-get-started-eclipse.md)
* [Dowiedz się więcej o usłudze Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interakcja z klastrami usługi Service Fabric przy użyciu interfejsu wiersza polecenia platformy Azure](service-fabric-azure-cli.md)
* [Rozwiązywanie problemów z wdrażaniem](service-fabric-azure-cli.md#troubleshooting)
* Uzyskaj informacje o [opcjach pomocy technicznej usługi Service Fabric](service-fabric-support.md)

## <a name="related-articles"></a>Pokrewne artykuły:

* [Getting started with Service Fabric and Azure CLI 2.0](service-fabric-azure-cli-2-0.md) (Wprowadzenie do usługi Service Fabric i interfejsu wiersza polecenia platformy Azure 2.0)
* [Getting started with Service Fabric XPlat CLI](service-fabric-azure-cli.md) (Wprowadzenie do interfejsu wiersza polecenia XPlat usługi Service Fabric)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

