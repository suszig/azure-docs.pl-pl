---
title: "Wdrażanie aplikacji Spring Boot w usłudze Azure Service Fabric | Microsoft Docs"
description: "Wdróż aplikację Spring Boot dla usługi Azure Service Fabric, korzystając z przewodnika Wprowadzenie do platformy Spring Boot."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: 8e57ed29560baeea6279a45e8769f4005c123b56
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="deploy-a-spring-boot-application"></a>Wdrażanie aplikacji Spring Boot
Usługa Azure Service Fabric to platforma systemów rozproszonych umożliwiająca wdrażanie mikrousług i kontenerów, a także zarządzanie nimi. 

Ten przewodnik Szybki start przedstawia sposób wdrażania aplikacji Spring Boot na platformie Service Fabric. W tym przewodniku Szybki start użyto przykładu [Wprowadzenie](https://spring.io/guides/gs/spring-boot/) z witryny internetowej platformy Spring. W tym przewodniku Szybki start objaśniono wdrażanie przykładu ze środowiska Spring Boot jako aplikacji usługi Service Fabric za pomocą znanych narzędzi wiersza polecenia. Po zakończeniu w usłudze Service Fabric będzie działać przykład Wprowadzenie środowiska Spring Boot. 

![Zrzut ekranu aplikacji](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)

W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie aplikacji Spring Boot w usłudze Service Fabric
> * Wdrażanie aplikacji w klastrze lokalnym 
> * Wdrażanie aplikacji w klastrze na platformie Azure
> * Skalowanie aplikacji w poziomie na wiele węzłów
> * Przenoszenie usługi w tryb failover bez wywierania wpływu na jej dostępność

## <a name="prerequisites"></a>Wymagania wstępne
Aby ukończyć ten przewodnik Szybki start:
1. [Zainstaluj zestaw SDK usługi Service Fabric i interfejs wiersza polecenia (CLI) usługi Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Zainstaluj oprogramowanie Git](https://git-scm.com/)
3. [Zainstaluj narzędzie Yeoman](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
4. [Skonfiguruj środowisko Java](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development)

## <a name="download-the-sample"></a>Pobierz przykład
W oknie polecenia uruchom następujące polecenie, aby sklonować przykładową aplikację Wprowadzenie środowiska Spring Boot na komputer lokalny.
```
git clone https://github.com/spring-guides/gs-spring-boot.git
```

## <a name="package-the-spring-boot-application"></a>Tworzenie pakietu aplikacji Spring Boot 
1. W sklonowanym katalogu `gs-spring-boot` uruchom polecenie `yo azuresfguest`. 

2. Wprowadź następujące szczegóły dla każdego monitu. 

    ![Wpisy narzędzia Yeoman](./media/service-fabric-quickstart-java-spring-boot/yeomanspringboot.png)

3. W folderze `SpringServiceFabric/SpringServiceFabric/SpringGettingStartedPkg/code` utwórz plik o nazwie `entryPoint.sh`. Dodaj następującą zawartość do pliku. 

    ```bash
    #!/bin/bash
    BASEDIR=$(dirname $0)
    cd $BASEDIR
    java -jar gs-spring-boot-0.1.0.jar
    ```

Na tym etapie utworzono aplikację usługi Service Fabric dla przykładu Wprowadzenie środowiska Spring Boot, którą można wdrożyć w usłudze Service Fabric.

## <a name="run-the-application-locally"></a>Uruchamianie aplikacji lokalnie
1. Uruchom klaster lokalny, uruchamiając następujące polecenie:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Uruchamianie klastra lokalnego zajmuje nieco czasu. Aby potwierdzić, że klaster jest w pełni uruchomiony, otwórz narzędzie Service Fabric Explorer dostępne pod adresem **http://localhost:19080**. Pięć węzłów w dobrej kondycji oznacza, że klaster lokalny jest uruchomiony. 
    
    ![Klaster lokalny w dobrej kondycji](./media/service-fabric-quickstart-java-spring-boot/sfxlocalhost.png)

2. Przejdź do folderu `gs-spring-boot/SpringServiceFabric`.
3. Uruchom następujące polecenie, aby połączyć się z klastrem lokalnym. 

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```
4. Uruchom skrypt `install.sh`. 

    ```bash
    ./install.sh
    ```

5. Uruchom przeglądarkę internetową i uzyskaj dostęp do aplikacji, przechodząc do adresu **http://localhost:8080**. 

    ![Fronton aplikacji — lokalny](./media/service-fabric-quickstart-java-spring-boot/springbootsflocalhost.png)
    
Teraz możesz uzyskiwać dostęp do aplikacji Spring Boot, która została wdrożona w klastrze usługi Service Fabric.  

## <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure

### <a name="set-up-your-azure-service-fabric-cluster"></a>Konfigurowanie klastra usługi Azure Service Fabric
Aby wdrożyć aplikację w klastrze na platformie Azure, utwórz własny klaster.

Klastry testowe to bezpłatne, działające przez ograniczony czas klastry usługi Service Fabric hostowane na platformie Azure. Są one obsługiwane przez zespół usługi Service Fabric. Każdy może wdrażać w nich aplikacje i poznawać szczegółowo platformę. Aby uzyskać dostęp do klastra testowego, [postępuj zgodnie z instrukcjami](http://aka.ms/tryservicefabric). 

W celu wykonywania operacji zarządzania w zabezpieczonym klastrze testowym można użyć narzędzia Service Fabric Explorer, interfejsu wiersza polecenia lub programu PowerShell. Aby korzystać z narzędzia Service Fabric Explorer, należy załadować plik PFX z witryny internetowej klastra testowego i zaimportować certyfikat do magazynu certyfikatów (w systemie Windows lub Mac) lub do przeglądarki (w systemie Ubuntu). Nie ma żadnego hasła dla certyfikatów z podpisem własnym z klastra testowego. 

Aby wykonywać operacje zarządzania za pomocą programu PowerShell lub interfejsu wiersza polecenia, potrzebny będzie plik PFX (w przypadku programu PowerShell) lub PEM (w przypadku interfejsu wiersza polecenia). Aby przekonwertować plik PFX na plik PEM, uruchom następujące polecenie:  

```bash
openssl pkcs12 -in party-cluster-1277863181-client-cert.pfx -out party-cluster-1277863181-client-cert.pem -nodes -passin pass:
```

Aby uzyskać informacje na temat tworzenia własnego klastra, zobacz [Tworzenie klastra usługi Service Fabric na platformie Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> Usługa Spring Boot została skonfigurowana do nasłuchiwania ruchu przychodzącego na porcie 8080. Upewnij się, że port w klastrze został otwarty. Jeśli używasz klastra testowego, ten port jest otwarty.
>

### <a name="deploy-the-application-using-cli"></a>Wdrażanie aplikacji przy użyciu interfejsu wiersza polecenia
Kiedy aplikacja i klaster są gotowe, możesz wdrożyć aplikację w klastrze bezpośrednio z wiersza polecenia.

1. Przejdź do folderu `gs-spring-boot/SpringServiceFabric`.
2. Uruchom następujące polecenie, aby połączyć się z klastrem platformy Azure. 

    ```bash
    sfctl cluster select --endpoint http://<ConnectionIPOrURL>:19080
    ```
    
    Gdy klaster jest zabezpieczony certyfikatem z podpisem własnym, uruchamiane jest następujące polecenie: 

    ```bash
    sfctl cluster select --endpoint https://<ConnectionIPOrURL>:19080 --pem <path_to_certificate> --no-verify
    ```
3. Uruchom skrypt `install.sh`. 

    ```bash
    ./install.sh
    ```

4. Uruchom przeglądarkę internetową i uzyskaj dostęp do aplikacji, przechodząc do adresu **http://\<ConnectionIPOrUrl>:8080**. 

    ![Fronton aplikacji — lokalny](./media/service-fabric-quickstart-java-spring-boot/springbootsfazure.png)
    
Teraz możesz uzyskiwać dostęp do aplikacji Spring Boot, która została wdrożona w klastrze usługi Service Fabric.  
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Skalowanie aplikacji i usług w klastrze
Usługi można skalować na klaster w celu dostosowania ich do zmiany obciążenia. Skalowanie usługi odbywa się przez zmienianie liczby wystąpień uruchomionych w klastrze. Istnieje wiele sposobów skalowania usług. Można użyć skryptów lub poleceń interfejsu wiersza polecenia usługi Service Fabric (sfctl). W tym przykładzie używane jest narzędzie Service Fabric Explorer.

Narzędzie Service Fabric Explorer działa we wszystkich klastrach usługi Service Fabric i można uzyskać do niego dostęp z przeglądarki, przechodząc do portu HTTP zarządzania klastrami (19080), na przykład `http://localhost:19080`.

Aby skalować usługę internetową frontonu, wykonaj następujące czynności:

1. Otwórz narzędzie Service Fabric Explorer w klastrze — na przykład `http://localhost:19080`.
2. Kliknij wielokropek (trzy kropki) obok węzła **fabric:/SpringServiceFabric/SpringGettingStarted** w widoku drzewa i wybierz pozycję **Skaluj usługę**.

    ![Usługa skalowania narzędzia Service Fabric Explorer](./media/service-fabric-quickstart-java-spring-boot/sfxscaleservicehowto.png)

    Teraz możesz skalować liczbę wystąpień usługi.

3. Zmień liczbę na **3** i kliknij pozycję **Skaluj usługę**.

    Alternatywny sposób skalowania usługi to użycie wiersza polecenia w następujący sposób.

    ```bash 
    # Connect to your local cluster
    sfctl cluster select --endpoint http://localhost:19080

    # Run Bash command to scale instance count for your service
    sfctl service update --service-id 'SpringServiceFabric~SpringGettingStarted` --instance-count 3 --stateless 
    ``` 

4. Kliknij węzeł **fabric:/SpringServiceFabric/SpringGettingStarted** w widoku drzewa i rozwiń węzeł partycji (reprezentowany przez identyfikator GUID).

    ![Usługa skalowania narzędzia Service Fabric Explorer — zakończone](./media/service-fabric-quickstart-java-spring-boot/sfxscaledservice.png)

    Usługa ma trzy wystąpienia, a widok drzewa pokazuje węzły, na których wystąpienia są uruchomione.

Dzięki temu prostemu zadaniu zarządzania zwiększona została liczba zasobów dostępnych dla naszej usługi Spring w celu przetworzenia obciążenia użytkownika. Ważne jest, aby zrozumieć, że nie musisz mieć wielu wystąpień usługi, aby działała ona niezawodnie. W przypadku niepowodzenia usługi usługa Service Fabric zapewnia, że nowe wystąpienie usługi jest uruchamiane w klastrze.

## <a name="fail-over-services-in-a-cluster"></a>Przenoszenie usług do trybu failover w klastrze 
Aby przedstawić przenoszenie usługi w tryb failover, ponowne uruchomienie węzła jest symulowane przy użyciu narzędzia Service Fabric Explorer. Upewnij się, że jest uruchomione tylko jedno wystąpienie usługi. 

1. Otwórz narzędzie Service Fabric Explorer w klastrze — na przykład `http://localhost:19080`.
2. Kliknij wielokropek (trzy kropki) obok węzła, w którym uruchomiono wystąpienie usługi, i uruchom ponownie węzeł. 

    ![Service Fabric Explorer — ponowne uruchamianie węzła](./media/service-fabric-quickstart-java-spring-boot/sfxhowtofailover.png)
3. Wystąpienie usługi zostanie przeniesione do innego węzła bez przestoju w działaniu aplikacji. 

    ![Service Fabric Explorer — ponowne uruchamianie węzła zakończone powodzeniem](./media/service-fabric-quickstart-java-spring-boot/sfxfailedover.png)

## <a name="next-steps"></a>Następne kroki
W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie aplikacji Spring Boot w usłudze Service Fabric
> * Wdrażanie aplikacji w klastrze lokalnym 
> * Wdrażanie aplikacji w klastrze na platformie Azure
> * Skalowanie aplikacji w poziomie na wiele węzłów
> * Przenoszenie usługi w tryb failover bez wywierania wpływu na jej dostępność

* Więcej informacji na temat [tworzenia mikrousług języka Java przy użyciu modeli programowania usługi Service Fabric](service-fabric-quickstart-java-reliable-services.md)
* Dowiedz się więcej o [konfigurowaniu ciągłej integracji i ciągłego wdrażania za pomocą usługi Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
* Zapoznaj się z innymi [przykładami w języku Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)