---
title: "Więcej informacji na temat zasad zabezpieczeń Azure mikrousług | Dokumentacja firmy Microsoft"
description: "Omówienie sposobu uruchamiania aplikacji usługi Service Fabric w ramach systemu i kont zabezpieczeń lokalnych, w tym punktu SetupEntry, których wymaga aplikacji do wykonania uprzywilejowanych akcji przed rozpoczęciem"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: aae828489b708a5b538df1d63c12be23d0423da7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-security-policies-for-your-application"></a>Konfigurowanie zasad zabezpieczeń aplikacji
Za pomocą usługi Azure Service Fabric, można zabezpieczyć aplikacji uruchomionych w klastrze, w obszarze konta innego użytkownika. Zabezpieczanie zasobów, które są używane przez aplikacje w czasie wdrażania na kontach użytkowników — na przykład, plików, katalogów i certyfikatów pomaga również w sieci szkieletowej usług. Dzięki temu uruchamianie aplikacji, nawet w środowisku hostowanej udostępnionego bardziej bezpieczne od siebie nawzajem.

Domyślnie aplikacje sieci szkieletowej usług są uruchamiane na koncie, działającą proces Fabric.exe. Usługi sieć szkieletowa dostępne są także możliwość uruchamiania aplikacji w ramach konta użytkownika lokalnego lub konta system lokalny, które jest określone w manifeście aplikacji. Obsługiwany system lokalny typy kont to **LocalUser**, **NetworkService**, **Usługa lokalna**, i **LocalSystem**.

 Jeśli korzystasz z sieci szkieletowej usług w systemie Windows Server w centrum danych za pomocą autonomicznego Instalatora, można użyć konta domeny usługi Active Directory, łącznie z konta usług zarządzane przez grupę.

Można zdefiniować i Utwórz grupy użytkowników, dzięki czemu można dodać co najmniej jednego użytkownika do każdej grupy do wspólnego zarządzania. Jest to przydatne, jeśli jest wielu użytkowników do innej usługi, punkty wejścia i muszą mieć niektórych typowych uprawnień, które są dostępne na poziomie grupy.

## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Skonfiguruj zasady dla punktu wejścia instalacji usługi
Zgodnie z opisem w [model aplikacji](service-fabric-application-model.md), ustawienia punktu wejścia, **SetupEntryPoint**, to punkt wejścia uprzywilejowanych uruchamiane przy użyciu tych samych poświadczeń jako sieci szkieletowej usług (zazwyczaj  *Usługa sieciowa* konta) przed innymi punktu wejścia. Plik wykonywalny, który jest określony przez **punktu wejścia** jest zazwyczaj długotrwałe hosta usługi. Dlatego o punktu wejścia oddzielne ustawienia pozwala uniknąć konieczności uruchamiania pliku wykonywalnego hosta usługi z wysokiego poziomu uprawnień przez dłuższy czas. Plik wykonywalny który **punktu wejścia** określa po zakończeniu **SetupEntryPoint** kończy się pomyślnie. Wynikowy proces jest monitorowane i ponownie uruchomione i ponownie rozpoczyna się od **SetupEntryPoint** Jeśli kiedykolwiek kończy lub ulegnie awarii.

Poniżej przedstawiono przykład manifestu usługi simple, pokazujący element SetupEntryPoint i główny punkt wejścia dla usługi.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-the-policy-by-using-a-local-account"></a>Skonfiguruj zasady za pomocą konta lokalnego
Po skonfigurowaniu usługi ma punktu wejścia instalacji można zmienić uprawnienia zabezpieczeń, które zostanie uruchomiony w w manifeście aplikacji. Poniższy przykład pokazuje, jak skonfigurować usługę, aby była uruchamiana z uprawnieniami konta administratora.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
```

Najpierw utwórz **podmiotów** sekcji przy użyciu nazwy użytkownika, takie jak SetupAdminUser. Oznacza to, że użytkownik jest członkiem grupy administratorów systemu.

Następnie w obszarze **ServiceManifestImport** Skonfiguruj zasady do zastosowania tego podmiotu zabezpieczeń do **SetupEntryPoint**. Ta wartość informuje usługi sieć szkieletowa że w przypadku **MySetup.bat** uruchomieniu pliku, powinien być `RunAs` z uprawnieniami administratora. Biorąc pod uwagę, że masz *nie* zastosować zasady do główny punkt wejścia, kod w **MyServiceHost.exe** działa w systemie **NetworkService** konta. Jest to wszystkich punktów wejścia usługi są uruchamiane jako konto domyślne.

Teraz Dodajmy MySetup.bat plik do projektu programu Visual Studio, aby przetestować uprawnienia administratora. W programie Visual Studio kliknij prawym przyciskiem myszy projekt usługi, a następnie dodaj nowy plik o nazwie MySetup.bat.

Następnie upewnij się, że plik MySetup.bat znajduje się w pakiecie usługi. Domyślnie nie jest. Wybierz plik, kliknij prawym przyciskiem myszy, aby uzyskać menu kontekstowego, a następnie wybierz pozycję **właściwości**. W oknie dialogowym właściwości, upewnij się, że **Kopiuj do katalogu wyjściowego** ustawiono **Kopiuj, jeśli nowszy**. Zobacz poniższy zrzut ekranu.

![Visual Studio CopyToOutput SetupEntryPoint pliku wsadowego][image1]

Teraz Otwórz plik MySetup.bat i dodaj następujące polecenia:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Następnie tworzenia i wdrażania rozwiązania do lokalnego klastra projektowego. Po uruchomieniu usługi, jak pokazano w narzędziu Service Fabric Explorer, widać, że plik MySetup.bat zakończyła się pomyślnie na dwa sposoby. Otwórz wiersz polecenia programu PowerShell i wpisz:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Zanotuj nazwę węzła, w którym usługa została wdrożona i uruchomiona w narzędziu Service Fabric Explorer — na przykład 2 węzła. Następnie przejdź do folderu roboczego wystąpienia aplikacji można znaleźć pliku out.txt, który zawiera wartości **TestVariable**. Na przykład, jeśli ta usługa została wdrożona na węźle 2, następnie można przejść do tej ścieżki dla **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Skonfiguruj zasady za pomocą konta system lokalny
Często zaleca się uruchomienie skryptu uruchamiania przy użyciu konta systemu lokalnego, a nie konta administratora. Uruchamianie zasad RunAs jako członek grupy Administratorzy, zwykle nie działa prawidłowo, ponieważ maszyny ma dostęp kontroli użytkownika (UAC) domyślnie włączone. W takich przypadkach **zaleca się Uruchom element SetupEntryPoint jako system lokalny, a nie jako użytkownik lokalny dodany do grupy administratorów**. W poniższym przykładzie przedstawiono ustawienie SetupEntryPoint do uruchamiania jako system lokalny:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

W przypadku klastrów systemu Linux do uruchamiania usługi lub ustawienia punkt wejścia jako **głównego**, można określić **AccountType** jako **LocalSystem**.

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>Uruchom polecenia programu PowerShell z punktu wejścia instalacji
Do uruchamiania programu PowerShell z **SetupEntryPoint** punktu, możesz uruchomić **PowerShell.exe** w pliku wsadowego, który wskazuje plik programu PowerShell. Najpierw dodaj plik programu PowerShell do projektu usługi — na przykład **MySetup.ps1**. Należy pamiętać o ustawieniu *Kopiuj, jeśli nowszy* właściwość tak, że plik znajduje się również w pakiecie usługi. W poniższym przykładzie przedstawiono przykładowy plik partii, która rozpoczyna się plik programu PowerShell o nazwie MySetup.ps1, który określa zmienną środowiskową systemu o nazwie **TestVariable**.

MySetup.bat, aby uruchomić plik programu PowerShell:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

W pliku programu PowerShell należy dodać następujące polecenie, aby ustawić zmienną środowiskową systemu:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Domyślnie podczas uruchamiania pliku wsadowego wygląda w folderze aplikacji o nazwie **pracy** plików. W takim przypadku po uruchomieniu MySetup.bat chcemy, aby znaleźć plik MySetup.ps1 w tym samym folderze, który jest aplikacja **pakietu kodu** folderu. Aby zmienić ten folder, ustawianie folderu roboczego:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="use-console-redirection-for-local-debugging"></a>Użyj konsoli dla debugowania lokalnego
Czasami jest przydatne wyświetlić dane wyjściowe konsoli uruchamiania skryptu na potrzeby debugowania. Aby to zrobić, można ustawić zasad przekierowania konsoli, który zapisuje dane wyjściowe do pliku. Plik wyjściowy jest zapisywany w folderze aplikacji o nazwie **dziennika** w węźle, w którym aplikacja jest wdrożona i uruchom. (Zobacz, gdzie można znaleźć w powyższym przykładzie).

> [!WARNING]
> Nigdy nie używaj zasad przekierowania konsoli w aplikacji, które zostało wdrożone w środowisku produkcyjnym, ponieważ może to wpłynąć na pracę awaryjną aplikacji. *Tylko* użyć tej funkcji dla rozwoju lokalnych i debugowania.  
> 
> 

W poniższym przykładzie przedstawiono ustawienie przekierowywania konsoli z wartością FileRetentionCount:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Jeśli zmienisz teraz MySetup.ps1 pliku do zapisu **Echo** polecenia, to będzie zapisywać do pliku wyjściowego na potrzeby debugowania:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

**Po debugowania skryptu, natychmiast usunąć te zasady przekierowania konsoli**.

## <a name="configure-a-policy-for-service-code-packages"></a>Konfigurowanie zasad dla pakietów kodu usługi
W poprzednich krokach pokazano, jak zastosować zasady RunAs do elementu SetupEntryPoint. Oto bardziej szczegółowe w sposób tworzenia różnych podmiotów zabezpieczeń, które mogą być stosowane jako zasady usługi.

### <a name="create-local-user-groups"></a>Tworzenie grup użytkowników lokalnych
Można zdefiniować i Utwórz grupy użytkowników, zezwalających na co najmniej jednego użytkownika, które mają zostać dodane do grupy. Jest to przydatne, jeśli jest wielu użytkowników do innej usługi, punkty wejścia i muszą mieć niektórych typowych uprawnień, które są dostępne na poziomie grupy. W poniższym przykładzie pokazano to grupa lokalna o nazwie **LocalAdminGroup** z uprawnieniami administratora. Dwóch użytkowników, Customer1 i Customer2, zostają członkami tej grupy lokalnej.

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a>Tworzenie użytkowników lokalnych
Można utworzyć użytkownika lokalnego, który może służyć do zabezpieczania usług w aplikacji. Gdy **LocalUser** typ konta jest określone w sekcji podmiotów manifestu aplikacji usługi sieć szkieletowa usług tworzy lokalne konta użytkowników na komputerach, na którym aplikacja jest wdrażana. Domyślnie te konta nie mają tej samej nazwy jak określone w manifeście aplikacji (na przykład Customer3 w następującym przykładowym). Zamiast tego są generowane dynamicznie i losowego hasła.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Jeśli aplikacja wymaga, aby konto użytkownika i hasło być taka sama na wszystkich komputerach (na przykład, aby włączyć uwierzytelnianie NTLM), manifestu klastra musi ustawić NTLMAuthenticationEnabled na wartość true. Manifest klastra, należy także określić NTLMAuthenticationPasswordSecret, który służy do generowania tego samego hasła na wszystkich komputerach.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-to-the-service-code-packages"></a>Zasady przypisać pakietów kodu usługi
**RunAsPolicy** sekcji **ServiceManifestImport** Określa konto, z sekcji podmiotów zabezpieczeń, które mają być używane do uruchamiania pakiet kodu. Pakiety kodu z manifestu usługi również kojarzy z kontami użytkowników w sekcji podmiotów zabezpieczeń. Określenie tej konfiguracji lub punkty wejścia głównego, lub możesz określić `All` do obu. W poniższym przykładzie przedstawiono różne zasady są stosowane:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Jeśli **EntryPointType** nie zostanie określony, domyślnie jest zaznaczona EntryPointType = "Main". Określanie **SetupEntryPoint** jest szczególnie przydatne, gdy chcesz uruchomić pewnych operacji ustawienia wysokiego poziomu przy użyciu konta system. Kod rzeczywiste usługi mogą być uruchamiane w konta dolna uprawnień.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Zastosuj zasady domyślne na wszystkie pakiety kodu usługi
Możesz użyć **DefaultRunAsPolicy** sekcji, aby określić konto użytkownika domyślna dla całego kodu pakietami, które nie mają określonego **RunAsPolicy** zdefiniowane. Jeśli większość pakietów kodu, które są określone w manifeście usługi używany przez aplikację muszą działać w ramach tego samego użytkownika, aplikacja właśnie można zdefiniować zasady RunAs domyślne przy użyciu tego konta użytkownika. Poniższy przykład określa, że jeśli pakiet kodu nie ma **RunAsPolicy** określony pakiet kodu należy uruchamiać w ramach **MyDefaultAccount** określone w sekcji podmiotów zabezpieczeń.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a>Użyj grupy domeny usługi Active Directory lub użytkownika
W przypadku wystąpienia usługi Service Fabric, który został zainstalowany w systemie Windows Server przy użyciu Autonomiczny Instalator można uruchomić usługi przy użyciu poświadczeń użytkownika usługi Active Directory lub konta grupy. To jest usługa Active Directory lokalnie w domenie, a nie z usługą Azure Active Directory (Azure AD). Przy użyciu użytkownika domeny lub grupy, można następnie dostęp do innych zasobów domeny (na przykład udziałów plików), które ma odpowiednie uprawnienia.

W poniższym przykładzie przedstawiono użytkownika usługi Active Directory o nazwie *TestUser* z ich domeny o nazwie hasła szyfrowane przy użyciu certyfikatu *MyCert*. Można użyć `Invoke-ServiceFabricEncryptText` polecenie programu PowerShell, aby utworzyć tajny zaszyfrowanego tekstu. Zobacz [Zarządzanie kluczy tajnych w aplikacji usługi Service Fabric](service-fabric-application-secret-management.md) szczegółowe informacje.

Należy wdrożyć klucza prywatnego certyfikatu można odszyfrować hasła na komputerze lokalnym przy użyciu metody poza pasmem (na platformie Azure jest za pośrednictwem usługi Azure Resource Manager). Następnie gdy usługa sieć szkieletowa wdraża pakiet usługi do maszyny, jest w stanie odszyfrować klucza tajnego i (wraz z nazwą użytkownika) uwierzytelniania za pomocą usługi Active Directory do uruchomienia w ramach tych poświadczeń.

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a>Użyj grupy zarządzane konto usługi.
W przypadku wystąpienia usługi Service Fabric, który został zainstalowany w systemie Windows Server przy użyciu Autonomiczny Instalator można uruchomić usługi jako grupa kont usług zarządzanych (gMSA). Należy pamiętać, że jest to usługi Active Directory lokalnie w domenie, a nie z usługą Azure Active Directory (Azure AD). Przy użyciu gMSA nie jest żadne hasło lub zaszyfrowane hasło jest przechowywane w `Application Manifest`.

Poniższy przykład pokazuje, jak utworzyć konto usługi zarządzane przez grupę o nazwie *testu svc$*; sposobu wdrażania tego konta usługi zarządzanego przez węzły klastra; i sposobie konfigurowania głównej nazwy użytkownika.

##### <a name="prerequisites"></a>Wymagania wstępne.
- Domena musi mieć klucz główny KDS.
- Domena musi być na poziomie systemu Windows Server 2012 lub nowszego poziomu funkcjonalności.

##### <a name="example"></a>Przykład
1. Poproś administratora domeny usługi active directory utworzyć, używając konta usług zarządzane przez grupę `New-ADServiceAccount` apletu polecenia i upewnij się, że `PrincipalsAllowedToRetrieveManagedPassword` zawiera wszystkie węzły klastra sieci szkieletowej usług. Należy pamiętać, że `AccountName`, `DnsHostName`, i `ServicePrincipalName` muszą być unikatowe.
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. Na każdym z węzłów klastra usługi sieć szkieletowa (na przykład `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), zainstalować i przetestować przez grupę.
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. Skonfiguruj głównej nazwy użytkownika oraz RunAsPolicy, aby odwołać się użytkownika.
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Przypisać zasady zabezpieczeń dostępu dla punktów końcowych HTTP i HTTPS
Jeśli stosowanie zasad RunAs do usługi i manifestu usługi deklaruje punktu końcowego zasobów przy użyciu protokołu HTTP, należy określić **SecurityAccessPolicy** do zapewnienia, że porty przydzielone do tych punktów końcowych są poprawnie dla konta użytkownika Uruchom jako, które usługa będzie uruchamiana na liście kontroli dostępu. W przeciwnym razie **http.sys** nie ma dostępu do usługi i uzyskać awarii przy wywołaniach z klienta. Poniższy przykład dotyczy punktu końcowego o nazwie konto Customer1 **EndpointName**, które zapewnia pełne prawa dostępu.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Dla punktu końcowego protokołu HTTPS należy również wskazywać nazwę certyfikatu do zwrócenia do klienta. Można to zrobić za pomocą **EndpointBindingPolicy**, przy użyciu certyfikatu zdefiniowanej w sekcji certyfikaty w manifeście aplikacji.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a>Uaktualnianie wielu aplikacji z punktów końcowych https
Należy zachować ostrożność, aby nie używać **tego samego portu** dla różnych wystąpień tej samej aplikacji przy użyciu protokołu http**s**. Przyczyną jest to usługa sieci szkieletowej nie można uaktualnić certyfikatu dla jednego wystąpienia aplikacji. Na przykład, jeśli aplikacja 1 lub aplikacji 2 zarówno chcesz uaktualnić ich cert 1 do 2 certyfikatu. W przypadku uaktualnienia sieci szkieletowej usług może mieć wyczyścić rejestracji certyfikatu 1 przy użyciu składnika http.sys, nawet jeśli jest nadal używa jej inna aplikacja. Aby tego uniknąć, usługi Service Fabric wykrywa, że nie jest już inne wystąpienie aplikacji zarejestrowanych na porcie z certyfikatem (z powodu http.sys), a nie operacji.

Dlatego usługi sieć szkieletowa nie obsługuje uaktualnienia z dwóch różnych usług za pomocą **tego samego portu** w innej aplikacji wystąpień. Innymi słowy nie można użyć tego samego certyfikatu w różnych usługach w tym samym porcie. Jeśli musisz mieć certyfikat udostępnione w tym samym porcie, należy się upewnić, czy usługi są umieszczane na kilka różnych maszyn z ograniczeń umieszczania. Lub należy wziąć pod uwagę przy użyciu portów dynamicznych sieci szkieletowej usług, jeśli to możliwe dla każdej usługi w każdego wystąpienia aplikacji. 

Jeśli widzisz uaktualnienia kończy się niepowodzeniem z protokołu https, wystąpił błąd, ostrzeżenie, informujący o tym "Interfejsu API serwera HTTP systemu Windows nie obsługuje wielu certyfikatów dla aplikacji, które współużytkują port".

## <a name="a-complete-application-manifest-example"></a>W przykładzie manifestu kompletna aplikacja
Następujące manifest aplikacji zawiera wiele różnych ustawień:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
* [Zrozumienie model aplikacji](service-fabric-application-model.md)
* [Określ zasoby w manifeście usługi](service-fabric-service-manifest-resources.md)
* [Wdrażanie aplikacji](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
