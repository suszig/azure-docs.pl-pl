---
title: "Dodaj elementy runbook automatyzacji Azure do planów odzyskiwania w klasycznym portalu | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak usługi Azure Site Recovery umożliwia teraz rozszerzyć planów odzyskiwania wykonać złożone zadania podczas odzyskiwania na platformie Azure przy użyciu automatyzacji Azure"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0a248e7c3f39a35ac10dc6ac64e5cef7d152e033
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="add-azure-automation-runbooks-to-recovery-plans-in-the-classic-portal"></a>Dodaj elementy runbook automatyzacji Azure do planów odzyskiwania w klasycznym portalu
W tym samouczku opisano, jak usługi Azure Site Recovery integruje się z automatyzacji Azure, który zapewni możliwość rozszerzenia do planów odzyskiwania. Plany odzyskiwania można organizować odzyskiwania maszyn wirtualnych chronionych za pomocą usługi Azure Site Recovery dla replikacji do chmury dodatkowej i replikacji do platformy Azure scenariuszy. Ułatwiają również w podejmowaniu odzyskiwania **spójnie dokładne**, **powtarzalne**, i **automatyczne**. Jeśli kończą się niepowodzeniem na maszynach wirtualnych Azure, integracja z usługi Automatyzacja Azure rozszerza planów odzyskiwania i daje możliwość wykonywania elementów runbook, dzięki czemu zaawansowanych automatyzacji zadań.

Jeśli ma nie wiesz o usługi Automatyzacja Azure jeszcze, zarejestruj się [tutaj](https://azure.microsoft.com/services/automation/) i pobierania ich przykładowe skrypty [tutaj](https://azure.microsoft.com/documentation/scripts/). Przeczytaj więcej na temat [usługi Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) i organizowania odzyskiwania na platformie Azure przy użyciu planów odzyskiwania [tutaj](https://azure.microsoft.com/blog/?p=166264).

W tym samouczku krótkich przedstawiono sposób integrowania elementu runbook usługi Automatyzacja Azure do planów odzyskiwania. Firma Microsoft będzie zautomatyzować prostych zadań, które wcześniej wymagana ręczna interwencja i zobacz, jak przekonwertować odzyskiwania krok multi na akcję odzyskiwania jednym kliknięciem. Firma Microsoft będzie też przyjrzeć poznać sposoby rozwiązywania prostego skryptu, jeśli jego nieprawidłowość.

## <a name="protect-the-application-to-azure"></a>Ochrona aplikacji Azure
Daj nam rozpoczynać się od prostej aplikacji składający się z dwóch maszyn wirtualnych. W tym miejscu zostały HRweb stosowania firmy Fabrikam. Firma Fabrikam-HRweb frontonu i zaplecza Hrweb-Fabrikam są dwie maszyny wirtualne chronione na platformie Azure przy użyciu usługi Azure Site Recovery. Aby włączyć ochronę maszyn wirtualnych przy użyciu usługi Azure Site Recovery, wykonaj poniższe kroki.

1. Włącz ochronę maszyn wirtualnych.
2. Upewnij się, że maszyny wirtualne zakończyły replikację początkową i jest replikowana.
3. Zaczekaj do ukończenia replikacji początkowej, a stan replikacji mówi chronione.

## ![](media/site-recovery-runbook-automation/01.png)
W tym samouczku utworzymy planu odzyskiwania do trybu failover aplikacji na platformie Azure aplikacji HRweb firmy Fabrikam. Następnie firma Microsoft będzie zintegrować ją z elementu runbook, który spowoduje utworzenie punktu końcowego na nieudane za pośrednictwem maszyny wirtualnej platformy Azure do obsługi stron sieci web na porcie 80.

Najpierw utwórz plan odzyskiwania do naszej aplikacji.

## <a name="create-the-recovery-plan"></a>Tworzenie planu odzyskiwania
Aby odzyskać aplikacji na platformie Azure, należy utworzyć plan odzyskiwania.
Przy użyciu planu odzyskiwania, które można określić kolejność odzyskiwania maszyn wirtualnych. Maszyna wirtualna umieszczona w grupie 1 będzie odzyskać i uruchomić się jako pierwszy, i postępuj maszyny wirtualnej z grupy 2.

Tworzenie planu odzyskiwania, który wygląda jak poniżej.

![](media/site-recovery-runbook-automation/12.png)

Aby dowiedzieć się więcej o planach odzyskiwania, przeczytaj dokumentację [tutaj](https://msdn.microsoft.com/library/azure/dn788799.aspx "tutaj").

Następnie utwórz artefakty niezbędne w automatyzacji Azure.

## <a name="create-the-automation-account-and-its-assets"></a>Tworzenie konta automatyzacji i jego zasoby.
Musisz mieć konto usługi Automatyzacja Azure do tworzenia elementów runbook. Jeśli nie masz już konto, przejdź do karty usługi Automatyzacja Azure wskazywane przez ![](media/site-recovery-runbook-automation/02.png)i Utwórz nowe konto.

1. Nadaj nazwę identyfikującą z konta.
2. Określ region geograficzny, w której chcesz umieścić konta.

Zalecane jest umieszczenie konta w tym samym regionie co magazyn usługi ASR.

![](media/site-recovery-runbook-automation/03.png)

Następnie należy utworzyć następujące zasoby w ramach konta.

### <a name="add-a-subscription-name-as-asset"></a>Dodaj nazwę subskrypcji jako zasobów
1. Dodaj nowe ustawienie ![](media/site-recovery-runbook-automation/04.png) w zasoby automatyzacji Azure i wybrać opcję![](media/site-recovery-runbook-automation/05.png)
2. Wybierz typ zmiennej jako **ciągu**
3. Określ nazwę zmiennej jako **AzureSubscriptionName**

   ![](media/site-recovery-runbook-automation/06.png)
4. Określ nazwę rzeczywiste subskrypcji platformy Azure jako wartość zmiennej.

   ![](media/site-recovery-runbook-automation/07_1.png)

Można określić nazwę subskrypcji ze strony ustawień konta w portalu Azure.

### <a name="add-an-azure-login-credential-as-asset"></a>Dodaj poświadczeń logowania do systemu Azure jako zasobów
Automatyzacja Azure korzysta z programu Azure PowerShell do nawiązania połączenia z subskrypcją i działa na artefakty istnieje. W tym celu należy przeprowadzić uwierzytelniania za pomocą konta Microsoft lub konta służbowego.
Poświadczenia konta można przechowywać w zasób ma być bezpiecznie używany przez element runbook.

1. Dodaj nowe ustawienie ![](media/site-recovery-runbook-automation/04.png) w zasoby automatyzacji Azure i wybierz pozycję![](media/site-recovery-runbook-automation/09.png)
2. Wybierz typ poświadczenia jako **poświadczenie programu PowerShell systemu Windows**
3. Określ nazwę jako **AzureCredential**

   ![](media/site-recovery-runbook-automation/10.png)
4. Określ nazwę użytkownika i hasło do logowania z.

Teraz oba te ustawienia są dostępne w zasobów.

![](media/site-recovery-runbook-automation/11.png)

Więcej informacji na temat nawiązywania połączenia z subskrypcją za pomocą programu PowerShell znajduje się [tutaj](/powershell/azure/overview).

Następnie spowoduje utworzenie elementu runbook w automatyzacji Azure, które można dodać punktu końcowego dla frontonu maszyny wirtualnej po pracy awaryjnej.

## <a name="azure-automation-context"></a>Kontekst usługi Automatyzacja Azure
Funkcja automatycznego odzyskiwania systemu przekazuje zmiennej kontekstu do elementu runbook, który ułatwia pisanie skryptów deterministyczna. Co można argumentowało nazwy usługi w chmurze i maszyny wirtualnej są przewidywalne, ale się stanie, że nie jest zawsze przypadku z powodu niektórych scenariuszy, takich jak, której nazwę maszyny wirtualnej mogła ulec zmianie z powodu nieobsługiwanych znaków na platformie Azure. Dlatego te informacje są przesyłane do planu odzyskiwania ASR jako część *kontekstu*.

Poniżej przedstawiono przykładowy wygląd zmiennej kontekstu.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Poniższa tabela zawiera nazwę i opis dla każdej zmiennej w kontekście.

| **Nazwa zmiennej** | **Opis** |
| --- | --- |
| RecoveryPlanName |Nazwa planu uruchomione. Pomaga podjąć działania na podstawie nazwy przy użyciu tego samego skryptu |
| FailoverType |Określa, czy tryb failover jest przetestować, planowane lub nieplanowane. |
| Element FailoverDirection |Określ, czy odzyskiwania jest podstawowym lub pomocniczym |
| Identyfikator grupy |Określ liczbę grupy w ramach planu odzyskiwania, po uruchomieniu planu |
| VmMap |Tablica wszystkich maszyn wirtualnych w grupie |
| Klucz VMMap |Klucz unikatowy (identyfikator GUID) dla każdej maszyny Wirtualnej. Jest taki sam jak identyfikator maszyny wirtualnej VMM stosownych. |
| RoleName |Nazwa maszyny Wirtualnej platformy Azure, która jest przywracana |
| CloudServiceName |Nazwa usługi w chmurze platformy Azure w której jest tworzony maszyny wirtualnej. |

Aby zidentyfikować klucza VmMap w kontekście można również przejść do strony właściwości maszyny Wirtualnej w usłudze ASR i przyjrzyj się właściwości GUID maszyny Wirtualnej.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Tworzenie elementu runbook automatyzacji
Teraz utworzyć element runbook, aby otworzyć port 80 na maszynie wirtualnej frontonu.

1. Utwórz nowy element runbook w ramach konta usługi Automatyzacja Azure o nazwie **OpenPort80**

   ![](media/site-recovery-runbook-automation/14.png)
2. Przejdź do widoku autora elementu runbook, a następnie wprowadź trybie wersji roboczej.
3. Najpierw określ zmienną, która ma być używana jako kontekst planu odzyskiwania

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. Następnym połączeniu z subskrypcji przy użyciu nazwy poświadczeń i subskrypcji

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect to Azure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   Należy pamiętać, że używasz zasobów platformy Azure — **AzureCredential** i **AzureSubscriptionName** tutaj.
5. Teraz określić szczegóły punktu końcowego i identyfikator GUID maszyny wirtualnej, dla którego chcesz udostępnić punktu końcowego. W takim przypadku frontonu maszyny wirtualnej.

   ```
       # Specify the parameters to be used by the script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   Określa protokół punktu końcowego platformy Azure, portów lokalnych na maszynie Wirtualnej i jej zamapowanych port publiczny. Te zmienne są parametry wymagane przez polecenia Azure, które dodania punktów końcowych do maszyn wirtualnych. VMGUID przechowuje maszynę wirtualną, którą muszą działać na identyfikator GUID.
6. Skrypt teraz wyodrębnić kontekst dla podanego identyfikatora GUID maszyny Wirtualnej i utworzyć punktu końcowego na maszynie wirtualnej odwołuje się on.

   ```
       #Read the VM GUID from the context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. Po zakończeniu tej operacji, kliknij przycisk Publikuj ![](media/site-recovery-runbook-automation/20.png) umożliwia skrypt, aby był dostępny do wykonania.

Ukończ skrypt poniżej podano użytkownikowi

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Dodaj skrypt do planu odzyskiwania
Gdy skrypt jest gotowy, można dodać go do planu odzyskiwania, który został utworzony wcześniej.

1. W planie odzyskiwania, który został utworzony wybierz dodać skrypt po grupie 2. ![](media/site-recovery-runbook-automation/15.png)
2. Określ nazwę skryptu. Jest to po prostu przyjazną nazwę dla tego skryptu do projekcji w ramach planu odzyskiwania.
3. W tryb failover do platformy Azure skryptu — wybierz nazwę konta usługi Automatyzacja Azure.
4. W elementach Runbook Azure wybierz element runbook Twojego autorstwa.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Skrypty po stronie podstawowej
Po przejściu w tryb failover na platformie Azure są wykonywane, można również wykonać skrypty po stronie głównej. Skrypty te zostanie uruchomiony na serwerze programu VMM w trybie failover.
Skrypty po stronie podstawowej są dostępne tylko w przypadku zamknięcia wstępne tylko i post etapy zamknięcia. Jest tak, ponieważ w lokacji głównej jest zwykle niedostępne, gdy uderzenia awarii.
Podczas nieplanowanego trybu failover tylko wtedy, gdy uczestnictwa w operacjach lokacji głównej, spróbuje uruchomić skrypty po stronie głównej. Jeśli nie są dostępne lub limit czasu, przejście w tryb failover będzie odzyskiwanie maszyn wirtualnych.
Skrypty po stronie głównej dostępnych cofanie witryn VMware/fizyczna/funkcji Hyper-v bez programu VMM chronione na platformie Azure — podczas failover do platformy Azure.
Jednak gdy możesz powrotu po awarii z platformy Azure do środowiska lokalnego, skrypty po stronie głównej (elementów Runbook) może służyć do wszystkich miejsc docelowych oprócz VMware.

## <a name="test-the-recovery-plan"></a>Testuj plan odzyskiwania
Po dodaniu elementu runbook do planu można zainicjować test trybu failover i zobaczyć ją w akcji. Zalecane jest zawsze uruchomić testowy tryb failover do testowania aplikacji i planu odzyskiwania, aby upewnić się, że nie ma żadnych błędów.

1. Wybierz plan odzyskiwania i zainicjuj test trybu failover.
2. Podczas wykonywania planu widać, czy element runbook jest wykonywana za pomocą jego stan.

   ![](media/site-recovery-runbook-automation/17.png)
3. Stan wykonania szczegółowe elementu runbook można również sprawdzić na stronie zadań usługi Automatyzacja Azure dla elementu runbook.

   ![](media/site-recovery-runbook-automation/18.png)
4. Po zakończeniu trybu failover, w wyniku wykonania elementu runbook, z wyjątkiem widoczny czy wykonywanie zakończy się pomyślnie lub nie, odwiedzając stronę maszyny wirtualnej platformy Azure i patrzeć punktów końcowych.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Przykładowe skrypty
Podczas Rezygnacja za pomocą automatyzacji jedną powszechnie używane zadania dodawania punktu końcowego na maszynie wirtualnej platformy Azure, w tym samouczku, może wykonać szereg inne zadania automatyzacji zaawansowanych przy użyciu automatyzacji Azure. Firma Microsoft i społecznością usługi Automatyzacja Azure znajdują się przykładowe elementy runbook, które mogą pomóc rozpocząć tworzenie własnych rozwiązań i narzędzie elementów runbook, którego można użyć jako bloków konstrukcyjnych dla większych automatyzacji zadań. Rozpocznij korzystanie z nich z galerii i tworzenie planów odzyskiwania jednym kliknięciem zaawansowanych aplikacji przy użyciu usługi Azure Site Recovery.

## <a name="additional-resources"></a>Dodatkowe zasoby
[Przegląd automatyzacji Azure](http://msdn.microsoft.com/library/azure/dn643629.aspx "Omówienie usługi Automatyzacja Azure")

[Przykładowe skrypty usługi Automatyzacja Azure](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "przykładowe skrypty usługi Automatyzacja Azure")
