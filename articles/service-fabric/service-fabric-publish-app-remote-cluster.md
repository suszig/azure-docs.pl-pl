---
title: Publikowanie aplikacji do zdalnego klastra z programem Visual Studio | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak opublikować aplikację do zdalnej usługi klastra sieci szkieletowej w programie Visual Studio."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: faecd892-eb54-4d9c-8023-c67442afb8e8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/29/2016
ms.author: cawa
ms.openlocfilehash: c440c520d84fc503ff9e705555449e92555d4721
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-remove-applications-using-visual-studio"></a>Wdrażanie i usunąć aplikacje przy użyciu programu Visual Studio
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Program Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [Interfejsy API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Rozszerzenia sieć szkieletowa usług Azure dla programu Visual Studio zapewnia łatwe, powtarzalnych i skryptowe sposób publikowania aplikacji do klastra usługi sieć szkieletowa usług.

## <a name="the-artifacts-required-for-publishing"></a>Artefakty wymagane do opublikowania
### <a name="deploy-fabricapplicationps1"></a>Wdrażanie FabricApplication.ps1
Jest to skrypt PowerShell, który używa ścieżkę profilu publikowania jako parametr do publikowania aplikacji sieci szkieletowej usług. Ponieważ ten skrypt jest częścią aplikacji, to Zapraszamy zmodyfikuj go odpowiednio do potrzeb aplikacji.

### <a name="publish-profiles"></a>Profilów publikowania
Folder projektu aplikacji sieci szkieletowej usług o nazwie **PublishProfiles** zawiera pliki XML, które przechowują informacje niezbędne do publikowania aplikacji, takich jak:

* Parametry połączenia klastra sieci szkieletowej usług
* Ścieżka do pliku parametrów aplikacji
* Ustawienia uaktualniania

Domyślnie aplikacja obejmuje trzy profilów publikowania: Local.1Node.xml, Local.5Node.xml i Cloud.xml. Można dodać więcej profilów, kopiując i wklejając jedną z domyślnych plików.

### <a name="application-parameter-files"></a>Pliki parametrów aplikacji
Folder projektu aplikacji sieci szkieletowej usług o nazwie **ApplicationParameters** zawiera pliki XML dla wartości parametru manifestu aplikacji określone przez użytkownika. Pliki manifestu aplikacji mogą nadać parametry, dzięki czemu można użyć innych wartości dla ustawienia wdrażania. Aby dowiedzieć się więcej na temat parametryzacja aplikacji, zobacz [Zarządzanie wiele środowisk w sieci szkieletowej usług](service-fabric-manage-multiple-environment-app-configuration.md).

> [!NOTE]
> Dla usług aktora należy utworzyć przed próbą wykonania można edytować plik w edytorze lub za pomocą okna dialogowego publikowania projektu. Jest to spowodowane część pliki manifestu zostanie wygenerowana podczas kompilacji.

## <a name="to-publish-an-application-using-the-publish-service-fabric-application-dialog-box"></a>Aby opublikować aplikację przy użyciu okna dialogowego publikowanie aplikacji sieci szkieletowej usług
Następująca procedura przedstawia sposób publikowania aplikacji przy użyciu **publikowania aplikacji sieci szkieletowej usług** okno dialogowe dostarczane za pomocą narzędzi Visual Studio usługi sieci szkieletowej.

1. W menu skrótów projektu aplikacji sieci szkieletowej usług wybierz **publikowania...** Aby wyświetlić **publikowania aplikacji sieci szkieletowej usług** okno dialogowe.
   
    ![** Opublikować usługi sieci szkieletowej aplikacji ** — okno dialogowe][0]
   
    Wybrany plik w **docelowe profilu** jest lista rozwijana, gdy wszystkie ustawienia, z wyjątkiem **manifestu wersji**, są zapisywane. Można ponownie użyć istniejącego profilu lub utworzyć nową, wybierając **<... zarządzania profilami >** w **docelowego profilu** pole listy rozwijanej. Po wybraniu profilu publikowania, jego zawartość jest wyświetlana w odpowiednich polach okna dialogowego. Aby zapisać zmiany w dowolnym momencie, wybierz **Zapisz profil** łącza.    
2. W **punktu końcowego połączenia** Określ punkt końcowy publikowania lokalnego lub zdalnego klastra sieci szkieletowej usług firmy. Aby dodać lub zmienić punktu końcowego połączenia, kliknij na **punktu końcowego połączenia** listy rozwijanej. Lista zawiera klastra usługi sieć szkieletowa dostępne punkty końcowe połączenia do których można opublikować oparte na subskrypcjach platformy Azure. Należy pamiętać, że jeśli użytkownik nie jest już zalogowany do programu Visual Studio, pojawi się monit Aby to zrobić.
   
    Użyj okna dialogowego wyboru klastra do wyboru zbiór dostępnych subskrypcji i klastrów.
   
    ![** Okno dialogowe Wybieranie usługi sieć szkieletowa klastra **][1]
   
   > [!NOTE]
   > Jeśli chcesz opublikować do dowolnego punktu końcowego (na przykład strona klastra), zobacz **publikowania do punktu końcowego dowolnego klastra** poniższej sekcji.
   > 
   > 
   
    Po wybraniu punktu końcowego programu Visual Studio weryfikuje połączenie wybrany klaster sieci szkieletowej usług. Jeśli klaster nie jest bezpieczne, Visual Studio można się z nim połączyć natychmiast. Jednak jeśli klaster jest bezpieczne, należy zainstalować certyfikat na komputerze lokalnym przed kontynuowaniem. Zobacz [jak skonfigurować bezpieczne połączenia](service-fabric-visualstudio-configure-secure-connections.md) Aby uzyskać więcej informacji. Gdy wszystko będzie gotowe, wybierz pozycję **OK** przycisku. Wybrany klaster jest wyświetlana w **publikowania aplikacji sieci szkieletowej usług** okno dialogowe.
3. W **plik parametrów aplikacji** listy rozwijanej wybierz opcję plik parametrów aplikacji. Plik parametrów aplikacji zostały określone przez użytkownika wartości dla parametrów w pliku manifestu aplikacji. Aby dodać lub zmienić parametr, wybierz **Edytuj** przycisku. Wprowadź lub zmień wartość parametru **parametrów** siatki. Gdy wszystko będzie gotowe, wybierz pozycję **zapisać** przycisku.
   
    ![** Okno dialogowe Edytowanie parametrów **][2]
4. Użyj **uaktualnienie aplikacji** pole wyboru, aby określić, czy publikować to działanie jest uaktualnienie. Uaktualnienie publikowania akcje różnią się od normalnego publikowania akcje. Zobacz [uaktualniania aplikacji sieci szkieletowej usług](service-fabric-application-upgrade.md) listę różnic. Aby skonfigurować ustawienia uaktualnienia, wybierz **skonfigurować ustawienia uaktualnienia** łącza. Zostanie wyświetlony Edytor parametr uaktualnienia. Zobacz [skonfigurować uaktualniania aplikacji usługi Service Fabric](service-fabric-visualstudio-configure-upgrade.md) Aby dowiedzieć się więcej na temat uaktualniania parametrów.
5. Wybierz **wersji manifestu...** przycisk, aby wyświetlić **edytować wersji** okno dialogowe. Musisz zaktualizować wersje aplikacji i usługi do uaktualnienia została wykonana. Zobacz [samouczek uaktualniania aplikacji sieci szkieletowej usług](service-fabric-application-upgrade-tutorial.md) Aby dowiedzieć się więcej, wpływ procesu uaktualniania aplikacji i wersji manifestu usługi.
   
    ![** Okno dialogowe Edytowanie wersji **][3]
   
    Użycie aplikacji i wersji usługi wersjonowania semantycznego, takie jak 1.0.0 lub wartości liczbowe w formacie 1.0.0.0, wybierz **automatycznie Aktualizuj wersje aplikacji i usługi** opcji. Po wybraniu tej opcji, usługi i numery wersji aplikacji są automatycznie aktualizowane przy każdym wersja pakietu kodu, konfiguracji lub danych jest aktualizowana. Jeśli wolisz edytować wersje ręcznie, wyczyść pole wyboru, aby wyłączyć tę funkcję.
   
   > [!NOTE]
   > W przypadku wszystkich wpisów pakietu się pojawić w projekcie aktora najpierw Skompiluj projekt, aby wygenerować zapisów w plikach manifestu usługi.
   > 
   > 
6. Po zakończeniu określania wszystkie niezbędne ustawienia, wybierz **publikowania** przycisk, aby opublikować aplikację do wybranej klastra sieci szkieletowej usług. Określone ustawienia są stosowane do proces publikowania.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Publikowanie do dowolnego klastra punktu końcowego (w tym klastrów firmy)
Visual Studio publikowania środowisko jest zoptymalizowany do publikowania w zdalnych klastrów skojarzone z jednej z Twoich subskrypcji platformy Azure. Jednak istnieje możliwość publikowania do dowolnych punktów końcowych (takich jak klastry strona usługi Service Fabric), bezpośrednio edytując format XML profilu publikowania. Zgodnie z powyższym opisem trzech profilów publikowania znajdują się domyślnie —**Local.1Node.xml**, **Local.5Node.xml**, i **Cloud.xml**— ale można utworzyć — Zapraszamy dodatkowe profile dla różnych środowisk. Na przykład możesz utworzyć profil publikowania do klastrów firm, być może o nazwie **Party.xml**.

Jeśli łączysz się z niezabezpieczoną klastra punktu końcowego połączenia klastra, wszystkie, która jest wymagana jest takich jak `partycluster1.eastus.cloudapp.azure.com:19000`. W takim przypadku punktu końcowego połączenia w profilu publikowania wyglądać mniej więcej tak:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Jeśli łączysz się zabezpieczonych klastra, należy również podać szczegóły certyfikatu klienta z lokalnego magazynu mają być używane do uwierzytelniania. Aby uzyskać więcej informacji, zobacz [Konfigurowanie bezpiecznych połączeń do klastra usługi sieć szkieletowa](service-fabric-visualstudio-configure-secure-connections.md).

  Po skonfigurowaniu swój profil publikowania, możesz odwoływać się do niej w oknie dialogowym Publikowanie w sposób przedstawiony poniżej.

  ![Nowy profil publikowania w publikowania — okno dialogowe][4]

  Należy pamiętać, że w takim przypadku nowy profil publikowania punktów do jednego z domyślnych plików parametr aplikacji. Jest to odpowiednie, jeśli chcesz opublikować taką samą konfigurację aplikacji w wielu środowiskach. Z kolei w przypadkach, gdy mają różne konfiguracje dla poszczególnych środowisk, który chcesz opublikować, powinna mieć Aby utworzyć plik parametrów aplikacji.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się, jak zautomatyzować proces publikowania w środowisku ciągłej integracji, zobacz [Konfigurowanie ciągłej integracji usługi sieć szkieletowa](service-fabric-set-up-continuous-integration.md).

[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
