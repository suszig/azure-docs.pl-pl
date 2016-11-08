---
title: Tworzenie pierwszej aplikacji usługi Service Fabric w systemie Linux przy użyciu platformy C# | Microsoft Docs
description: Tworzenie i wdrażanie aplikacji usługi Service Fabric przy użyciu platformy C#
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: ''

ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2016
ms.author: subramar

---
# Tworzenie pierwszej aplikacji usługi Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# — Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java — Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# — Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Usługa Service Fabric udostępnia zestawy SDK do kompilowania usług w systemie Linux przy użyciu platform .NET Core i Java. W tym samouczku przedstawiony zostanie sposób kompilowania aplikacji dla systemu Linux i tworzenia usługi przy użyciu platformy C# (.NET Core).

## Wymagania wstępne
Przed rozpoczęciem upewnij się, że masz [skonfigurowane środowisko programowania systemu Linux](service-fabric-get-started-linux.md). Jeśli używasz systemu Mac OS X, możesz [skonfigurować jednopunktowe środowisko systemu Linux na maszynie wirtualnej za pomocą narzędzia Vagrant](service-fabric-get-started-mac.md).

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

## Kompilowanie aplikacji
Szablony generatora Yeoman usługi Service Fabric obejmują skrypt kompilacji, którego można użyć do skompilowania aplikacji z poziomu terminala (po przejściu do folderu aplikacji).

  ```bash
 cd myapp 
 ./build.sh 
  ```

## Wdrażanie aplikacji
Po skompilowaniu aplikację można wdrożyć w klastrze lokalnym za pomocą interfejsu wiersza polecenia platformy Azure.

1. Połącz się z lokalnym klastrem usługi Service Fabric.
   
    ```bash
    azure servicefabric cluster connect
    ```
2. Użyj skryptu instalacji udostępnionego w szablonie, aby skopiować pakiet aplikacji do magazynu obrazów klastra, zarejestrować typ aplikacji i utworzyć wystąpienie aplikacji.
   
    ```bash
    ./install.sh
    ```
3. Otwórz przeglądarkę i przejdź do narzędzia Service Fabric Explorer dostępnego pod adresem http://localhost:19080/Explorer (zastąp łańcuch localhost prywatnym adresem IP maszyny wirtualnej w przypadku używania narzędzia Vagrant w systemie Mac OS X).
4. Rozwiń węzeł Aplikacje i zwróć uwagę, że istnieje teraz wpis dla danego typu aplikacji i inny wpis dla pierwszego wystąpienia tego typu.

## Uruchamianie klienta testowego i przechodzenie w tryb failover
Projekty aktora nie działają samodzielnie. Wymagają one wysyłania im komunikatów przez inną usługę lub klienta. Szablon aktora zawiera prosty skrypt testowy, którego można użyć do interakcji z usługą aktora.

1. Uruchom skrypt za pomocą narzędzia kontrolnego, aby wyświetlić dane wyjściowe usługi aktora.
   
    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. W narzędziu Service Fabric Explorer zlokalizuj węzeł, w którym znajduje się replika podstawowa usługi aktora. Na poniższym zrzucie ekranu jest to węzeł 3.
   
    ![Znajdowanie repliki podstawowej w narzędziu Service Fabric Explorer][sfx-primary]
3. Kliknij węzeł znaleziony w poprzednim kroku, a następnie wybierz pozycję **Dezaktywuj (uruchom ponownie)** z menu Akcje. Spowoduje to ponowne uruchomienie jednego z pięciu węzłów w klastrze lokalnym i wymuszenie przejścia w tryb failover do repliki pomocniczej uruchomionej na innym węźle. Podczas wykonywania tej czynności należy zwrócić uwagę na dane wyjściowe z klienta testowego oraz to, że w liczniku będzie nadal występować przyrost niezależnie od trybu failover.

## Następne kroki
* [Dowiedz się więcej o usłudze Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interakcja z klastrami usługi Service Fabric przy użyciu interfejsu wiersza polecenia platformy Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png



<!--HONumber=Oct16_HO3-->


