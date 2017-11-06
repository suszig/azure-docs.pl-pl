---
title: "Tworzenie aplikacji Java sieci szkieletowej usług Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji Java na platformie Azure przy użyciu usługi Service Fabric — przykład szybki start."
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
ms.date: 10/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: c8e598159d2139397952a5c11eac54dc38939f47
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="create-a-java-application"></a>Tworzenie aplikacji Java
Sieć szkieletowa usług Azure to platforma systemów rozproszonych wdrażania i zarządzania nimi mikrousług i kontenerów. 

Ta opcja szybkiego startu pokazano, jak wdrażanie pierwszej aplikacji Java sieci szkieletowej usług za pomocą środowiska IDE programu Eclipse na komputerze dewelopera systemu Linux. Po zakończeniu, masz aplikację do głosowania z Java web fronton, który zapisuje wyniki głosowania w stanowych usług zaplecza w klastrze.

![Zrzut ekranu aplikacji](./media/service-fabric-quickstart-java/votingapp.png)

W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Używany jest program Eclipse jako narzędzia dla aplikacji Java sieci szkieletowej usług
> * Wdrożenia aplikacji w klastrze lokalnym 
> * Wdrażanie aplikacji do klastra w systemie Azure
> * Skalowalny w poziomie aplikacji w różnych węzłach

## <a name="prerequisites"></a>Wymagania wstępne
Aby ukończyć ten przewodnik Szybki Start:
1. [Zainstaluj usługi sieć szkieletowa SDK & Usługa sieci szkieletowej interfejsu wiersza polecenia (CLI)](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Zainstaluj oprogramowanie Git](https://git-scm.com/)
3. [Instalowanie programu Eclipse](https://www.eclipse.org/downloads/)
4. [Konfigurowanie środowiska Java](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development), wprowadzania wiesz, które należy wykonać opcjonalne kroki, aby zainstalować wtyczkę Eclipse 

## <a name="download-the-sample"></a>Pobierz przykład
W oknie poleceń uruchom następujące polecenie sklonować repozytorium przykładowej aplikacji na komputerze lokalnym.
```
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Uruchamianie aplikacji lokalnie
1. Uruchom klaster lokalny, uruchamiając następujące polecenie:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Uruchomienie klastra lokalnego dopiero po pewnym czasie. Aby upewnić się, że klaster jest w pełni się, dostęp do Eksploratora usługi sieć szkieletowa w **http://localhost: 19080**. Pięć węzłów w prawidłowym stanie wskazująca, że klaster lokalny jest uruchomiona. 
    
    ![Klaster lokalny dobrej kondycji](./media/service-fabric-quickstart-java/localclusterup.png)

2. Otwórz środowisko Eclipse.
3. Kliknij menu Plik -> otwarcia projektów z systemu plików... 
4. Kliknij katalog i wybierz polecenie `Voting` katalogu z `service-fabric-java-quickstart` folderu sklonować z serwisu Github. Kliknij przycisk Zakończ. 

    ![Okno dialogowe importowania programu Eclipse](./media/service-fabric-quickstart-java/eclipseimport.png)
    
5. Masz teraz `Voting` projekt w Eksploratorze pakiet dla programu Eclipse. 
6. Kliknij prawym przyciskiem myszy projekt i wybierz **publikowania aplikacji...**  w obszarze **sieci szkieletowej usług** listy rozwijanej. Wybierz **PublishProfiles/Local.json** jako docelowy profil i kliknij polecenie Publikuj. 

    ![Publikowanie lokalnego okna dialogowego](./media/service-fabric-quickstart-java/localjson.png)
    
7. Otwórz przeglądarkę sieci web Ulubione oraz dostęp do aplikacji, uzyskując dostęp do **adresem http://localhost: 8080**. 

    ![Lokalny frontonu aplikacji](./media/service-fabric-quickstart-java/runninglocally.png)
    
Można teraz dodać zestaw głosowania opcje i Rozpocznij tworzenie głosów. Aplikacja jest uruchamiana i przechowuje wszystkie dane w klastrze usługi sieć szkieletowa, bez konieczności oddzielnej bazy danych.

## <a name="deploy-the-application-to-azure"></a>Wdrażanie aplikacji na platformie Azure

### <a name="set-up-your-azure-service-fabric-cluster"></a>Konfigurowanie klastra sieci szkieletowej usług Azure
Aby wdrożyć aplikację do klastra na platformie Azure, Utwórz własnego klastra lub użyj klastra strony.

Klastry testowe to bezpłatne, działające przez ograniczony czas klastry usługi Service Fabric hostowane na platformie Azure. Są one uruchamiane przez zespół usługi sieć szkieletowa, gdzie każda osoba, która wdrażania aplikacji i Dowiedz się więcej o platformie. Aby uzyskać dostęp do klastra testowego, [postępuj zgodnie z instrukcjami](http://aka.ms/tryservicefabric). 

Aby uzyskać informacje na temat tworzenia własnego klastra, zobacz [Tworzenie pierwszego klastra usługi Service Fabric na platformie Azure](service-fabric-get-started-azure-cluster.md).

> [!Note]
> Usługa frontonu sieci web jest skonfigurowane do nasłuchiwania na porcie 8080 dla ruchu przychodzącego. Upewnij się, że port w klastrze został otwarty. Jeśli używasz klastra strona ten port jest otwarty.
>

### <a name="deploy-the-application-using-eclipse"></a>Wdrażanie aplikacji przy użyciu programu Eclipse
Teraz, aplikacji i klastra jest gotowe, możesz go wdrożyć do klastra bezpośrednio z programu Eclipse.

1. Otwórz **Cloud.json** plików w obszarze **PublishProfiles** katalogu i wypełnij `ConnectionIPOrURL` i `ConnectionPort` odpowiednio pola. Przykładem jest dostępne: 

    ```bash
    {
         "ClusterConnectionParameters": 
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "",
            "ClientCert": ""
         }
    }
    ```

2. Kliknij prawym przyciskiem myszy projekt i wybierz **publikowania aplikacji...**  w obszarze **sieci szkieletowej usług** listy rozwijanej. Wybierz **PublishProfiles/Cloud.json** jako docelowy profil i kliknij polecenie Publikuj. 

    ![Publikowanie chmury okna dialogowego](./media/service-fabric-quickstart-java/cloudjson.png)

3. Otwórz przeglądarkę sieci web Ulubione oraz dostęp do aplikacji, uzyskując dostęp do **http://\<ConnectionIPOrURL >: 8080**. 

    ![Chmura frontonu aplikacji](./media/service-fabric-quickstart-java/runningcloud.png)
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Skalowanie aplikacji i usług w klastrze
Usługi mogą być skalowane w klastrze, aby uwzględnić zmiany w obciążenia w ramach usług. Skalowanie usługi odbywa się przez zmienianie liczby wystąpień uruchomionych w klastrze. Istnieje wiele sposobów skalowania usługi, możesz użyć skryptów lub poleceń z usługi sieci szkieletowej interfejsu wiersza polecenia (sfctl). W tym przykładzie użyto Service Fabric Explorer.

Service Fabric Explorer działa we wszystkich klastrach sieci szkieletowej usług i jest dostępny z poziomu przeglądarki, przechodząc do portu zarządzania klastrami HTTP (19080), na przykład `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.

Aby skalować usługę internetową frontonu, wykonaj następujące czynności:

1. Otwórz narzędzie Service Fabric Explorer w klastrze — na przykład `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.
2. Kliknij wielokropek (wielokropek) obok pozycji **fabric: / głosowania/VotingWeb** węzła w elemencie treeview i wybierz polecenie **skali usługi**.

    ![Usługa skalowania Eksploratora sieci szkieletowej usług](./media/service-fabric-quickstart-java/scaleservicejavaquickstart.png)

    Teraz możesz skalować liczbę wystąpień usługi internetowej frontonu.

3. Zmień liczbę na **2** i kliknij pozycję **Skaluj usługę**.
4. Polecenie **fabric: / głosowania/VotingWeb** węzeł w widoku drzewa i rozwiń węzeł partycji (reprezentowane przez identyfikator GUID).

    ![Usługa sieci szkieletowej Explorer skali usług ukończone](./media/service-fabric-quickstart-java/servicescaled.png)

    Usługa ma dwa wystąpienia, czy w widoku drzewa Zobacz węzły, których wystąpienia Uruchom na będą teraz widoczne.

Wykonując to proste zadanie zarządzania, podwoiliśmy zasoby dostępne dla naszej usługi frontonu w celu przetworzenia obciążenia użytkownika. Ważne jest, aby zrozumieć, że nie musisz mieć wielu wystąpień usługi, aby działała ona niezawodnie. W przypadku niepowodzenia usługi usługa Service Fabric zapewnia, że nowe wystąpienie usługi jest uruchamiane w klastrze.

## <a name="next-steps"></a>Następne kroki
W tym przewodniku Szybki start zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Używany jest program Eclipse jako narzędzia dla aplikacji Java sieci szkieletowej usług
> * Wdrażanie aplikacji Java do klastra lokalnego 
> * Wdrażanie aplikacji Java do klastra w systemie Azure
> * Skalowalny w poziomie aplikacji w różnych węzłach

* Dowiedz się więcej o [debugowanie usług na języku Java za pomocą programu Eclipse](service-fabric-debugging-your-application-java.md)
* Dowiedz się więcej o [Konfigurowanie danej integreation ciągłe wdrażanie przy użyciu Wpięć](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* Wyewidencjonowanie innych [przykłady Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)