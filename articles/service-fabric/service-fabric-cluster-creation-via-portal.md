---
title: "Tworzenie klastra sieci szkieletowej usług w portalu Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak skonfigurować bezpieczny klastra sieci szkieletowej usług na platformie Azure przy użyciu portalu Azure i usługi Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: be880efdcf1276252c76f27c2f2fd99edd606caa
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Tworzenie klastra sieci szkieletowej usług na platformie Azure przy użyciu portalu Azure
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Witryna Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Jest to przewodnik krok po kroku, który przeprowadzi Cię przez kolejne kroki konfigurowania klastra sieci szkieletowej usług bezpiecznej platformie Azure przy użyciu portalu Azure. Ten przewodnik przeprowadzi Cię przez następujące kroki:

* Konfigurowanie magazynu kluczy do zarządzania kluczami zabezpieczeń klastra.
* Tworzenie klastra zabezpieczonych na platformie Azure za pośrednictwem portalu Azure.
* Uwierzytelnianie za pomocą certyfikatów Administratorzy.

> [!NOTE]
> Dla bardziej zaawansowanych opcji zabezpieczeń, takich jak uwierzytelnianie użytkowników w usłudze Azure Active Directory oraz konfigurowania certyfikatów do zabezpieczenia aplikacji [tworzenia klastra przy użyciu usługi Azure Resource Manager][create-cluster-arm].
> 
> 

Bezpieczne klaster jest klastrem, który uniemożliwia nieautoryzowany dostęp do operacji zarządzania, w tym wdrażanie, uaktualnianie i usuwanie aplikacji, usług i danych, które zawierają. Niezabezpieczone klastra oznacza, że każdy użytkownik w dowolnym momencie połączyć się i wykonywać operacje zarządzania. Chociaż można utworzyć klaster niezabezpieczone, jest **zdecydowanie zaleca się utworzenie klastra bezpiecznego**. Niezabezpieczone klastra **nie można zabezpieczyć później** — należy utworzyć nowy klaster.

Pojęcia są takie same dla tworzenia bezpiecznych klastrów, czy są klastry z systemem Windows lub Linux klastry. Aby uzyskać więcej informacji i pomocnika skryptów tworzenia bezpiecznych klastrów systemu Linux, zobacz [tworzenia bezpiecznych klastrów](service-fabric-cluster-creation-via-arm.md). Parametry uzyskany przez pomocnika skryptów można wprowadzić bezpośrednio w portalu, zgodnie z opisem w sekcji [tworzenia klastra w portalu Azure](#create-cluster-portal).

## <a name="configure-key-vault"></a>Konfiguruj usługę Key Vault 
### <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Ten przewodnik po używa [programu Azure PowerShell][azure-powershell]. Przy uruchamianiu nowej sesji programu PowerShell, zaloguj się do konta platformy Azure i wyboru subskrypcji przed wykonaniem polecenia platformy Azure.

Zaloguj się do konta platformy azure:

```powershell
Login-AzureRmAccount
```

Wybierz subskrypcję:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="set-up-key-vault"></a>Konfigurowanie usługi Key Vault
Ta część przewodnika przeprowadzi Cię przez proces tworzenia magazyn kluczy klastra sieci szkieletowej usług platformy Azure i dla aplikacji sieci szkieletowej usług. Aby uzyskać pełne informacje na Key Vault, zobacz [Key Vault Wprowadzenie — przewodnik][key-vault-get-started].

Sieć szkieletowa usług używa certyfikatów X.509 do zabezpieczenia klastra. Usługa Azure Key Vault służy do zarządzania certyfikatami dla klastrów sieci szkieletowej usług platformy Azure. Po wdrożeniu klastra na platformie Azure, odpowiedzialną za tworzenie klastrów sieci szkieletowej usług dostawcy zasobów platformy Azure pobiera certyfikaty z magazynu kluczy i instaluje je w klastrze maszyn wirtualnych.

Na poniższym diagramie przedstawiono relację między magazyn kluczy klastra sieci szkieletowej usług i dostawcy zasobów platformy Azure, który wykorzystuje certyfikaty przechowywane w magazynie kluczy, podczas tworzenia klastra:

![Instalacja certyfikatu][cluster-security-cert-installation]

#### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Pierwszym krokiem jest aby utworzyć nową grupę zasobów dla usługi Key Vault. Wprowadzenie klucza magazynu do własnego grupy zasobów jest zalecane, dzięki czemu można usunąć grup zasobów obliczeniowych i przestrzeni dyskowej — takich jak grupy zasobów o klastra usługi sieć szkieletowa — bez utraty z kluczy i kluczy tajnych. Grupy zasobów, która ma magazyn kluczy musi być w tym samym regionie co klaster, który go używa.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

#### <a name="create-key-vault"></a>Tworzenie usługi Key Vault
Utwórz magazyn kluczy w nowej grupy zasobów. Magazyn kluczy **musi być włączona dla wdrożenia** umożliwia dostawcy zasobów usługi sieć szkieletowa uzyskanie certyfikatów i zainstalować na węzłach klastra:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```

Jeśli masz istniejący magazyn kluczy, możesz je włączyć, dla wdrożenia przy użyciu jednej z następujących sposobów:

##### <a name="azure-powershell"></a>Azure PowerShell

```powershell
PS C:\Users\vturecek> Set-AzureRmKeyVaultAccessPolicy -VaultName 'myvault' -EnabledForDeployment
```

##### <a name="azure-cli"></a>Azure CLI:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


### <a name="add-certificates-to-key-vault"></a>Dodaj certyfikaty do magazynu kluczy
W usłudze Service Fabric używa się certyfikatów, aby zapewniać uwierzytelnianie i szyfrowanie w celu zabezpieczania różnych aspektów klastra i jego aplikacji. Aby uzyskać więcej informacji dotyczących używania certyfikatów w sieci szkieletowej usług, zobacz [scenariusze zabezpieczeń klastra sieci szkieletowej usług][service-fabric-cluster-security].

#### <a name="cluster-and-server-certificate-required"></a>Klaster a serwera certyfikatów (wymagane)
Ten certyfikat jest wymagany do zabezpieczania klastra i uniemożliwić nieautoryzowany dostęp do niego. Udostępnia ona zabezpieczeń klastra na kilka sposobów:

* **Uwierzytelnianie klastra:** uwierzytelnia komunikacji między węzłami w Federacji klastra. Tylko węzły, które można potwierdzić swoją tożsamość, z tym certyfikatem można dołączyć do klastra.
* **Uwierzytelnianie serwera:** uwierzytelnia punktów końcowych klastra zarządzania do klienta zarządzania, aby klient zarządzania znała rozmawia rzeczywistych klastra. Ten certyfikat zapewnia również SSL interfejsu API zarządzania HTTPS oraz Service Fabric Explorer, za pośrednictwem protokołu HTTPS.

Do obsługi tych celów, certyfikat musi spełniać następujące wymagania:

* Certyfikat musi zawierać klucz prywatny.
* Certyfikat musi zostać utworzony dla wymiany kluczy, można eksportować do pliku wymiany informacji osobistych (pfx).
* Nazwa podmiotu certyfikatu musi odpowiadać domeny używane do dostępu do klastra usługi sieć szkieletowa usług. Jest to wymagane jest podanie protokołu SSL dla punktów końcowych HTTPS zarządzania i Service Fabric Explorer klastra. Nie można uzyskać certyfikat od urzędu certyfikacji (CA) dla `.cloudapp.azure.com` domeny. Możliwość nabycia niestandardowej nazwy domeny dla klastra. Gdy żądanie certyfikatu od urzędu certyfikacji nazwa podmiotu certyfikatu musi odpowiadać nazwie domeny niestandardowej, używane dla klastra.

#### <a name="client-authentication-certificates"></a>Certyfikaty uwierzytelniania klienta
Certyfikaty klienta dodatkowe uwierzytelnianie Administratorzy dla zadań zarządzania w klastrze. Sieć szkieletowa usług ma dwa poziomy dostępu: **admin** i **użytkownika tylko do odczytu**. Co najmniej jeden certyfikat dla dostępu administracyjnego należy używać. Dodatkowe dostępu na poziomie użytkownika należy podać oddzielny certyfikat. Aby uzyskać więcej informacji na dostęp do ról, zobacz [kontroli dostępu opartej na rolach dla klientów usługi sieć szkieletowa][service-fabric-cluster-security-roles].

Nie należy przekazywać certyfikatów uwierzytelniania klienta do usługi Key Vault do pracy z sieci szkieletowej usług. Tylko te certyfikaty należy podać użytkownikom, którzy mają uprawnienia do zarządzania klastrem. 

> [!NOTE]
> Azure Active Directory jest zalecanym sposobem uwierzytelniania klientów dla operacji zarządzania klastrem. Aby korzystać z usługi Azure Active Directory, należy najpierw [Tworzenie klastra przy użyciu usługi Azure Resource Manager][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Certyfikaty aplikacji (opcjonalnie)
Dowolna liczba dodatkowych certyfikatów można zainstalować w klastrze dla aplikacji ze względów bezpieczeństwa. Przed utworzeniem klastra, należy wziąć pod uwagę scenariusze zabezpieczeń aplikacji, które wymagają certyfikatów do zainstalowania w węzłach, takich jak:

* Szyfrowanie i odszyfrowywanie wartości konfiguracji aplikacji
* Szyfrowanie danych w węzłach podczas replikacji 

Nie można skonfigurować certyfikaty aplikacji, podczas tworzenia klastra za pośrednictwem portalu Azure. Aby skonfigurować certyfikaty aplikacji podczas konfiguracji klastra, należy najpierw [Tworzenie klastra przy użyciu usługi Azure Resource Manager][create-cluster-arm]. Można również dodać certyfikatów aplikacji do klastra, po jego utworzeniu.

#### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatowanie certyfikatów do użytku dostawcy zasobów platformy Azure
Pliki klucza prywatnego (pfx) można dodany i używany bezpośrednio za pomocą usługi Key Vault. Dostawcy zasobów platformy Azure wymaga jednak przechowywać w formacie JSON specjalny, który zawiera PFX jako base-64 kluczy zakodowany ciąg i hasło klucza prywatnego. Aby uwzględnić te wymagania, klucze musi być umieszczona w ciągu JSON i następnie przechowywane jako *kluczy tajnych* w magazynie kluczy.

Aby ułatwić ten proces, moduł programu PowerShell jest [dostępne w witrynie GitHub][service-fabric-rp-helpers]. Wykonaj następujące kroki, aby użyć modułu programu:

1. Pobierz całą zawartość repozytorium do katalogu lokalnego. 
2. Zaimportuj moduł w oknie programu PowerShell:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

`Invoke-AddCertToKeyVault` Polecenia w tym module środowiska PowerShell automatycznie formatuje klucz prywatny certyfikatu do ciągu JSON i przekazuje go do magazynu kluczy. Umożliwia on Dodawanie certyfikatu klastra i wszelkich certyfikatów dodatkową aplikację do magazynu kluczy. Powtórz ten krok dla dodatkowych certyfikatów, które chcesz zainstalować w klastrze.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Są to wszystkie usługi Key Vault wymaganiach wstępnych konfigurowania szablonu usługi Resource Manager klastra sieci szkieletowej usług, który instaluje certyfikatów dla uwierzytelniania węzła, zarządzania punktu końcowego zabezpieczeń i uwierzytelniania oraz wszelkie dodatkowe zabezpieczenia aplikacji Funkcje, które korzystają z certyfikatów X.509. W tym momencie teraz powinny mieć następujące ustawienia na platformie Azure:

* Grupa zasobów magazynu kluczy
  * Usługa Key Vault
    * Certyfikat uwierzytelniania serwera klastra

< /a "Tworzenie klastra portal" ></a>

## <a name="create-cluster-in-the-azure-portal"></a>Tworzenie klastra w portalu Azure
### <a name="search-for-the-service-fabric-cluster-resource"></a>Wyszukaj zasobu klastra usługi sieć szkieletowa
![Wyszukiwanie szablonu klastra sieci szkieletowej usług w portalu Azure.][SearchforServiceFabricClusterTemplate]

1. Zaloguj się w witrynie [Azure Portal][azure-portal].
2. Kliknij przycisk **nowy** można dodać nowego szablonu zasobów. Wyszukaj szablonu klastra sieci szkieletowej usług w **Marketplace** w obszarze **wszystko**.
3. Wybierz **klastra sieci szkieletowej usług** z listy.
4. Przejdź do **klastra sieci szkieletowej usług** bloku, kliknij przycisk **Utwórz**,
5. **Klastra tworzenia sieci szkieletowej usług** bloku obejmuje następujące cztery kroki.

#### <a name="1-basics"></a>1. Podstawy
![Zrzut ekranu przedstawiający tworzenie nowej grupy zasobów.][CreateRG]

W bloku podstawowe służące musisz podać podstawowe szczegóły dotyczące klastra.

1. Wprowadź nazwę klastra.
2. Wprowadź **nazwy użytkownika** i **hasło** dla pulpitu zdalnego dla maszyn wirtualnych.
3. Upewnij się wybrać **subskrypcji** mają klastra do wdrożenia, zwłaszcza, jeśli masz wiele subskrypcji.
4. Utwórz **nową grupę zasobów**. Zaleca się nadać mu taką samą nazwę jak klastra, ponieważ pomaga w znajdowaniu je później, szczególnie w przypadku, gdy chcesz wprowadzić zmiany do wdrożenia lub usunąć klaster.
   
   > [!NOTE]
   > Mimo że można użyć istniejącej grupy zasobów, jest dobrym rozwiązaniem, aby utworzyć nową grupę zasobów. Ułatwia to usuwać klastry, które nie ma potrzeby.
   > 
   > 
5. Wybierz **region** , w której chcesz utworzyć klaster. Należy użyć tego samego regionu, który jest magazyn kluczy.

#### <a name="2-cluster-configuration"></a>2. Konfiguracja klastra
![Tworzenie typu węzła][CreateNodeType]

Skonfiguruj węzły klastra. Typy węzłów zdefiniować rozmiary maszyn wirtualnych, liczbę maszyn wirtualnych i ich właściwości. Klaster może mieć więcej niż jeden typ węzła, ale typ węzła podstawowego (pierwszego zdefiniowanego w portalu) musi mieć co najmniej pięć maszyn wirtualnych, jak jest to typ węzła rozmieszczenia usługi sieć szkieletowa usług systemowych. Nie należy konfigurować **właściwości umieszczania** ponieważ domyślna właściwość umieszczania "NodeTypeName" są dodawane automatycznie.

> [!NOTE]
> Typowy scenariusz dla wielu typów węzła jest aplikacja, która zawiera usługi frontonu i zaplecza. Chcesz umieścić usługi frontonu w mniejszych maszyn wirtualnych (na przykład D2 rozmiarów maszyn wirtualnych) z otwartych portów do Internetu, ale mają być umieszczone na maszynach wirtualnych większych (z rozmiarów maszyn wirtualnych, takie jak D4, D6 D15 i tak dalej) z żadnych portów skierowane do Internetu Otwórz usługi zaplecza.
> 
> 

1. Wybierz nazwę dla danego typu węzła (od 1 do 12 znaków zawierających tylko litery i cyfry).
2. Minimalna **rozmiar** maszyn wirtualnych dla węzła podstawowego typu jest wymuszany przez **trwałości** warstwy wybierz dla klastra. Wartość domyślna warstwa trwałości to brązowa. Aby uzyskać więcej informacji o trwałości, zobacz [Wybieranie niezawodność klastra sieci szkieletowej usług i trwałość][service-fabric-cluster-capacity].
3. Wybierz rozmiar maszyny Wirtualnej i warstwę cenową. Maszyny wirtualne D-series mają dyski SSD i zaleca aplikacji stanowych. Użyj dowolnej jednostki SKU maszyny Wirtualnej z częściowa rdzeni lub nie mają mniej niż 7 GB pojemności dysku. 
4. Minimalna **numer** maszyn wirtualnych dla węzła podstawowego typu jest wymuszany przez **niezawodności** wybrania warstwy. Wartość domyślna warstwa niezawodności to Silver. Aby uzyskać więcej informacji o niezawodności, zobacz [Wybieranie niezawodność klastra sieci szkieletowej usług i trwałość][service-fabric-cluster-capacity].
5. Wybierz liczbę maszyn wirtualnych dla typu węzła. Możesz skalować w górę lub w dół liczbę maszyn wirtualnych w typ węzła później, ale dla typu węzła podstawowego minimum wynikają z wybranego poziomu niezawodności. Inne typy węzłów może mieć co najmniej 1 maszyny wirtualnej.
6. Skonfiguruj niestandardowe punkty końcowe. To pole umożliwia wprowadź rozdzielaną przecinkami listę portów, które chcesz udostępnić za pośrednictwem usługi równoważenia obciążenia Azure do publicznego Internetu dla aplikacji. Na przykład jeśli planujesz wdrożyć aplikację sieci web do klastra, wprowadź "80" w tym miejscu zezwalająca na ruch na porcie 80 do klastra. Aby uzyskać więcej informacji dotyczących punktów końcowych, zobacz [komunikacji z aplikacjami][service-fabric-connect-and-communicate-with-services]
7. Konfigurowanie klastra **diagnostyki**. Domyślnie diagnostyki są włączone w klastrze celu ułatwienia rozwiązywania problemów. Jeśli chcesz wyłączyć zmiany diagnostyki **stan** Przełącz, aby **poza**. Wyłączenie diagnostyki jest **nie** zalecane.
8. Wybierz tryb uaktualniania sieci szkieletowej ma ustawioną wartość klastra. Wybierz **automatyczne**, jeśli system ma automatycznie Podnieś najnowszej dostępnej wersji i spróbuj uaktualnić klaster do niego. Ustaw tryb **ręcznego**, jeśli chcesz wybrać obsługiwanej wersji.

> [!NOTE]
> Firma Microsoft obsługuje tylko klastry, które są z obsługiwanymi wersjami usługi sieci szkieletowej. Wybierając **ręcznego** trybie podjęcia na odpowiedzialność do uaktualnienia klastra do obsługiwanej wersji. Więcej informacji na temat sieci szkieletowej uaktualnić tryb zobacz [dokumentu usługi sieci szkieletowej klastra uaktualnienia.][service-fabric-cluster-upgrade]
> 
> 

#### <a name="3-security"></a>3. Bezpieczeństwo
![Zrzut ekranu przedstawiający konfiguracjach zabezpieczeń w portalu Azure.][SecurityConfigs]

Ostatnim krokiem jest zapewnienie informacji o certyfikacie do zabezpieczania klastra przy użyciu magazynu kluczy i certyfikatów informacji utworzony wcześniej.

* Wypełniania pól podstawowego certyfikatu z danych wyjściowych uzyskane z przekazywania **certyfikatu klastra** przy użyciu usługi Key Vault `Invoke-AddCertToKeyVault` polecenia programu PowerShell.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* Sprawdź **Skonfiguruj ustawienia zaawansowane** pole służące do wprowadzania certyfikaty klientów dla **klienta administrator** i **klienta tylko do odczytu**. W tych polach wprowadź odcisk palca certyfikatu klienta administratora i odcisk palca certyfikatu klienta użytkownika tylko do odczytu, jeśli ma to zastosowanie. Gdy administratorzy próbować połączyć się z klastrem, otrzymali dostępu tylko wtedy, gdy mają certyfikat z odciskiem palca, który odpowiada wartości odcisku palca wprowadzanym w tym miejscu.  

#### <a name="4-summary"></a>4. Podsumowanie

Aby zakończyć tworzenie klastra, kliknij przycisk **Podsumowanie** Zobacz konfiguracji, które zostały podane lub pobrać szablonu usługi Azure Resource Manager, który używany do wdrażania klastra. Po dostarczeniu ustawienia obowiązkowe **OK** przycisk staje się zielony i procesu tworzenia klastra można uruchomić, klikając go.

Możesz zobaczyć postępy tworzenia w powiadomieniach. (Kliknij ikonę „Dzwonka” w pobliżu paska stanu w prawym górnym rogu ekranu). Jeśli kliknięto **Przypnij do tablicy startowej** podczas tworzenia klastra, zobaczysz **wdrażanie klastra sieci szkieletowej usług** przypięty do **Start** tablicy.

### <a name="view-your-cluster-status"></a>Wyświetl stan sieci klastra
![Zrzut ekranu przedstawiający szczegółów klastra na pulpicie nawigacyjnym.][ClusterDashboard]

Po utworzeniu klastra można sprawdzić w portalu klastra:

1. Przejdź do **Przeglądaj** i kliknij przycisk **klastry usługi sieć szkieletowa**.
2. Znajdź klastra i kliknij ją.
3. Możesz teraz wyświetlić szczegóły klastra na pulpicie nawigacyjnym, w tym publiczny punkt końcowy klastra oraz link do narzędzia Service Fabric Explorer.

**Monitor węzła** sekcji w bloku pulpit nawigacyjny klastra wskazuje liczbę maszyn wirtualnych, które są w dobrej kondycji i nie działa prawidłowo. Można znaleźć więcej szczegółów na temat kondycji klastra na [wprowadzenie modelu kondycji sieci szkieletowej usług][service-fabric-health-introduction].

> [!NOTE]
> Klastry usługi sieć szkieletowa usług wymagają określonej liczby węzłów pozwala uzyskać się zawsze zachować dostępność i zachować stan — nazywany "utrzymywania kworum". W związku z tym zazwyczaj nie jest bezpiecznie wyłączyć wszystkie komputery w klastrze, chyba że najpierw wykonali [pełnej kopii zapasowej według stanu][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Zdalnego łączenia się z wystąpieniem zestawu skalowania maszyn wirtualnych lub węzła klastra
Każdy z elementów NodeType, określ w wynikach klastra w zestawu skalowania maszyn wirtualnych pobieranie konfiguracji. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Następne kroki
W tym momencie masz bezpiecznego klastra przy użyciu certyfikatów dla uwierzytelniania zarządzania. Następnie [połączyć się z klastrem](service-fabric-connect-to-secure-cluster.md) i Dowiedz się, jak [Zarządzanie klucze tajne aplikacji](service-fabric-application-secret-management.md).  Zapoznaj się z [opcje pomocy technicznej usługi sieć szkieletowa](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
<!--[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node -->
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
