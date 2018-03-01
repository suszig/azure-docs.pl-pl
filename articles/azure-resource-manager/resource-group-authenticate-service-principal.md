---
title: "Utwórz tożsamość aplikacji usługi Azure przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Opisuje sposób tworzenia aplikacji usługi Azure Active Directory i nazwy głównej usługi i przyznać jej dostęp do zasobów za pomocą kontroli dostępu opartej na rolach przy użyciu programu Azure PowerShell. Widoczny jest sposób uwierzytelniania aplikacji za pomocą hasła lub certyfikatu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/28/2017
ms.author: tomfitz
ms.openlocfilehash: 103e4ca5ffd6c9dfe5043af9d8f75763705eb939
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Use Azure PowerShell to create a service principal to access resources (Tworzenie jednostki usługi używanej do uzyskiwania dostępu do zasobów przy użyciu programu Azure PowerShell)

Jeśli aplikacji lub skryptu, który ma dostęp do zasobów, można skonfigurować tożsamości aplikacji i uwierzytelniania w aplikacji przy użyciu własne poświadczenia. Ta tożsamość jest określany jako nazwy głównej usługi. Takie podejście umożliwia:

* Przypisanie uprawnień do tożsamości aplikacji, które są inne niż własnych uprawnień. Zazwyczaj te uprawnienia są ograniczone do dokładnie co aplikacja powinna wykonać.
* Użyj certyfikatu do uwierzytelnienia, podczas wykonywania skryptu instalacji nienadzorowanej.

W tym artykule przedstawiono sposób użycia [programu Azure PowerShell](/powershell/azure/overview) skonfigurować wszystkie elementy potrzebne do uruchamiana własne poświadczenia i tożsamości aplikacji.

## <a name="required-permissions"></a>Wymagane uprawnienia
Aby ukończyć ten artykuł, należy posiadać odpowiednie uprawnienia w usłudze Azure Active Directory i Twojej subskrypcji platformy Azure. W szczególności należy utworzyć aplikację w usłudze Azure Active Directory i przypisać nazwę główną usługi do roli. 

Najłatwiejszym sposobem sprawdzenia, czy Twoje konto ma odpowiednie uprawnienia, jest skorzystanie z portalu. Zobacz [, czy wymagane uprawnienia](resource-group-create-service-principal-portal.md#required-permissions).

Teraz przejdź do sekcji w celu uwierzytelniania z:

* [Hasło](#create-service-principal-with-password)
* [certyfikat z podpisem własnym](#create-service-principal-with-self-signed-certificate)
* [certyfikat od urzędu certyfikacji](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>Polecenia programu PowerShell

Aby skonfigurować nazwy głównej usługi, należy użyć:

| Polecenie | Opis |
| ------- | ----------- | 
| [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Tworzy nazwę główną usługi Azure Active Directory |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Przypisuje określonej roli RBAC określony podmiot zabezpieczeń w podanym zakresie. |


## <a name="create-service-principal-with-password"></a>Tworzenie nazwy głównej usługi z hasłem

Aby utworzyć nazwy głównej usługi roli współautora dla Twojej subskrypcji, należy użyć: 

```powershell
Login-AzureRmAccount
$password = convertto-securestring {provide-password} -asplaintext -force
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password $password
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Przykład zostanie uśpiony na 20 sekund, pewien czas dla nowej usługi głównej propagację w usłudze Azure Active Directory. Jeśli skrypt nie oczekuje się wystarczająco długi, pojawi się o błędzie informujący: "{ID} podmiot zabezpieczeń nie istnieje w katalogu {DIR-ID}."

Poniższy skrypt można określić zakres innych niż domyślne subskrypcji i ponowi próbę przypisania roli, jeśli wystąpi błąd:

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $SecurePassword = convertto-securestring $Password -asplaintext -force
 
 # Create Service Principal for the AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $SecurePassword
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Kilka elementów należy pamiętać o skrypt:

* Aby udzielić dostępu tożsamości do subskrypcji domyślne, nie należy podać parametry grupa zasobów lub subskrypcji o identyfikatorze.
* Określ parametr ResourceGroup tylko wtedy, gdy chcesz ograniczyć zakres przypisania roli do grupy zasobów.
*  W tym przykładzie należy dodać nazwy głównej usługi do roli współautora. Dla innych ról, zobacz [RBAC: role wbudowane](../active-directory/role-based-access-built-in-roles.md).
* Skrypt zostanie uśpiony na 15 sekund, pewien czas dla nowej usługi głównej propagację w usłudze Azure Active Directory. Jeśli skrypt nie oczekuje się wystarczająco długi, pojawi się o błędzie informujący: "{ID} podmiot zabezpieczeń nie istnieje w katalogu {DIR-ID}."
* Aby udzielić dostępu główną usługi do więcej subskrypcji lub grupy zasobów, należy uruchomić `New-AzureRMRoleAssignment` polecenie cmdlet ponownie z różnymi zakresami.


### <a name="provide-credentials-through-powershell"></a>Podaj poświadczenia, za pomocą programu PowerShell
Teraz musisz zalogować się jako aplikacji w celu wykonania operacji. Dla nazwy użytkownika użyj `ApplicationId` utworzonej dla aplikacji. Hasła należy użyć jednej określone podczas tworzenia konta. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-ID}
```

Identyfikator dzierżawcy nie jest liter, dlatego można ją osadzić bezpośrednio w skrypcie. Można pobrać Identyfikatora dzierżawy, należy użyć:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Tworzenie nazwy głównej usługi o certyfikat z podpisem własnym

Aby utworzyć nazwy głównej usługi o certyfikat z podpisem własnym i roli współautora dla Twojej subskrypcji, należy użyć: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Przykład zostanie uśpiony na 20 sekund, pewien czas dla nowej usługi głównej propagację w usłudze Azure Active Directory. Jeśli skrypt nie oczekuje się wystarczająco długi, pojawi się o błędzie informujący: "{ID} podmiot zabezpieczeń nie istnieje w katalogu {DIR-ID}."

Poniższy skrypt można określić zakres innych niż domyślne subskrypcji i ponowi próbę przypisania roli, jeśli wystąpi błąd. Musi mieć Azure PowerShell 2.0 w systemie Windows 10 lub Windows Server 2016.

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Kilka elementów należy pamiętać o skrypt:

* Aby udzielić dostępu tożsamości do subskrypcji domyślne, nie należy podać parametry grupa zasobów lub subskrypcji o identyfikatorze.
* Określ parametr ResourceGroup tylko wtedy, gdy chcesz ograniczyć zakres przypisania roli do grupy zasobów.
* W tym przykładzie należy dodać nazwy głównej usługi do roli współautora. Dla innych ról, zobacz [RBAC: role wbudowane](../active-directory/role-based-access-built-in-roles.md).
* Skrypt zostanie uśpiony na 15 sekund, pewien czas dla nowej usługi głównej propagację w usłudze Azure Active Directory. Jeśli skrypt nie oczekuje się wystarczająco długi, pojawi się o błędzie informujący: "{ID} podmiot zabezpieczeń nie istnieje w katalogu {DIR-ID}."
* Aby udzielić dostępu główną usługi do więcej subskrypcji lub grupy zasobów, należy uruchomić `New-AzureRMRoleAssignment` polecenie cmdlet ponownie z różnymi zakresami.

Jeśli użytkownik **bez zainstalowanego systemu Windows 10 lub Windows Server 2016 Technical Preview**, należy pobrać [generator certyfikatu z podpisem własnym](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z Microsoft Script Center. Wyodrębnij jego zawartość i zaimportuj polecenia cmdlet, które są potrzebne.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
W skrypcie Zastąp następujące dwa wiersze w celu wygenerowania certyfikatu.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Podaj certyfikat przy użyciu zautomatyzowanego skryptu PowerShell
Gdy zalogujesz się jako nazwy głównej usługi, należy podać identyfikator dzierżawcy katalogu dla aplikacji usługi AD. Dzierżawa jest wystąpieniem usługi Azure Active Directory. Jeśli masz tylko jedną subskrypcję, należy użyć:

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

Identyfikator aplikacji i Identyfikatora dzierżawcy nie są poufne, więc można go osadzić bezpośrednio w skrypcie. Można pobrać Identyfikatora dzierżawy, należy użyć:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Aby uzyskać identyfikator aplikacji, należy użyć:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Tworzenie nazwy głównej usługi o certyfikat od urzędu certyfikacji
Aby używać certyfikatu wystawionego przez urząd certyfikacji, tworzenie nazwy głównej usługi, użyj następującego skryptu:

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

Kilka elementów należy pamiętać o skrypt:

* Obejmuje dostęp do subskrypcji.
* W tym przykładzie należy dodać nazwy głównej usługi do roli współautora. Dla innych ról, zobacz [RBAC: role wbudowane](../active-directory/role-based-access-built-in-roles.md).
* Skrypt zostanie uśpiony na 15 sekund, pewien czas dla nowej usługi głównej propagację w usłudze Azure Active Directory. Jeśli skrypt nie oczekuje się wystarczająco długi, pojawi się o błędzie informujący: "{ID} podmiot zabezpieczeń nie istnieje w katalogu {DIR-ID}."
* Aby udzielić dostępu główną usługi do więcej subskrypcji lub grupy zasobów, należy uruchomić `New-AzureRMRoleAssignment` polecenie cmdlet ponownie z różnymi zakresami.

### <a name="provide-certificate-through-automated-powershell-script"></a>Podaj certyfikat przy użyciu zautomatyzowanego skryptu PowerShell
Gdy zalogujesz się jako nazwy głównej usługi, należy podać identyfikator dzierżawcy katalogu dla aplikacji usługi AD. Dzierżawa jest wystąpieniem usługi Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

Identyfikator aplikacji i Identyfikatora dzierżawcy nie są poufne, więc można go osadzić bezpośrednio w skrypcie. Można pobrać Identyfikatora dzierżawy, należy użyć:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Aby uzyskać identyfikator aplikacji, należy użyć:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Zmiana poświadczeń

Aby zmienić poświadczenia dla aplikacji usługi AD, albo z powodu naruszenia zabezpieczeń lub wygaśnięcia poświadczeń, należy użyć [AzureRmADAppCredential Usuń](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) i [AzureRmADAppCredential nowy](/powershell/module/azurerm.resources/new-azurermadappcredential) polecenia cmdlet.

Aby usunąć wszystkie poświadczenia dla aplikacji, należy użyć:

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

Aby dodać hasło, należy użyć:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

Aby dodać wartość certyfikatu, Utwórz certyfikat z podpisem własnym, jak pokazano w tym artykule. Następnie należy użyć:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-to-simplify-log-in"></a>Zapisz token dostępu w celu uproszczenia dziennika w
Aby uniknąć, podając poświadczenia główne usługi za każdym razem, należy zalogować się, można zapisać tokenu dostępu.

Aby użyć bieżącego tokenu dostępu w późniejszym sesji, zapisywanie profilu.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Otwórz profilu i przejrzyj jego zawartość. Zwróć uwagę, że zawiera on tokenu dostępu. Zamiast ręcznego zalogować się ponownie później, załadować profilu.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> Wygaśnięcia tokenu dostępu, dzięki użyciu zapisywanego profilu działa tylko dla, tak długo, jak token jest prawidłowy.
>  

Alternatywnie można wywołać operacji REST z programu PowerShell, aby się zalogować. Z odpowiedzi uwierzytelniania można pobrać tokenu dostępu do użycia z innych operacji. Na przykład pobierania tokenu dostępu za pomocą operacji REST, zobacz [utworzenia żądania](/rest/api/#create-the-request).

## <a name="debug"></a>Debugowanie

Podczas tworzenia nazwy głównej usługi, mogą wystąpić następujące błędy:

* **"Authentication_Unauthorized"** lub **"subskrypcji nie znaleziono w kontekście".** — Został wyświetlony ten błąd, gdy Twoje konto nie ma [wymagane uprawnienia](#required-permissions) w usłudze Azure Active Directory w celu rejestracji aplikacji. Zwykle zostanie wyświetlony ten błąd, gdy tylko Administrator użytkowników w usłudze Azure Active Directory można zarejestrować aplikacji, a konto użytkownika nie jest administratorem. Skontaktuj się z administratorem, albo przypisanie do roli administratora lub aby użytkownicy mogli zarejestrować aplikacji.

* Twoje konto **"nie ma autoryzacji do wykonania akcji"Microsoft.Authorization/roleAssignments/write"w zakresie"/subscriptions/ {guid}"."**  — Zostanie wyświetlony ten błąd, gdy Twoje konto nie ma wystarczających uprawnień, aby przypisać rolę do tożsamości. Poproś administratora subskrypcji możesz dodać do roli Administrator dostępu użytkowników.

## <a name="sample-applications"></a>Przykładowe aplikacje
Aby uzyskać informacje o zalogowanie się jako aplikacji za pomocą różnych platform zobacz:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Kolejne kroki
* Aby uzyskać szczegółowe instrukcje dotyczące integrowania aplikacji na platformie Azure do zarządzania zasobami, zobacz [przewodnik dewelopera do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](resource-manager-api-authentication.md).
* Aby uzyskać bardziej szczegółowy opis aplikacji i nazwy główne usług, zobacz [obiekty aplikacji i nazwy głównej usługi](../active-directory/active-directory-application-objects.md). 
* Aby uzyskać więcej informacji na temat uwierzytelniania usługi Azure Active Directory, zobacz [scenariusze uwierzytelniania dla usługi Azure AD](../active-directory/active-directory-authentication-scenarios.md).
* Aby uzyskać listę dostępnych akcji, które można udzielić lub odmówić dla użytkowników, zobacz [operacji dostawcy zasobów usługi Azure Resource Manager](../active-directory/role-based-access-control-resource-provider-operations.md).

