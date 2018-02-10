---
title: "Jak bezpiecznie wykonać migrację użytkowników od licencji produktu za pomocą licencjonowania oparte na grupach w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano zalecany proces migracji użytkowników między innego produktu licencji (np. Office 365 E1 i E3) przy użyciu na podstawie grupy licencji"
services: active-directory
keywords: "Licencjonowanie usługi Azure AD"
documentationcenter: 
author: piotrci
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/07/2018
ms.author: piotrci
ms.openlocfilehash: 97654673b395fd5b8cb41afdcdeaa21aba44f61d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-safely-migrate-users-between-product-licenses-using-group-based-licensing"></a>Jak bezpiecznie wykonać migrację użytkowników od licencji produktu przy użyciu na podstawie grupy licencji

W tym artykule opisano zalecaną metodą przenoszenie użytkowników między licencje produktów, korzystając z oparte na grupach licencji. Celem tego podejścia jest upewnij się, że podczas migracji jest bez utraty danych lub usługi: użytkowników powinny bezproblemowo przełączanie między produktów. Obejmuje dwa rodzaje procesu migracji:

-   Prosty: od licencji produktu, które nie zawierają planów usługi powodujące konflikt, na przykład: *Office 365 Enterprise E3* i *Office 365 Enterprise E5*

-   Bardziej złożone: między produktów zawierających niektóre plany usługi powodujące konflikt, na przykład: *Office 365 Enterprise E1* i *Office 365 Enterprise E3*. Więcej informacji dotyczących konfliktów [tutaj](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans) i [tutaj](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-product-and-service-plan-reference#service-plans-that-cannot-be-assigned-at-the-same-time).

Ten artykuł zawiera przykładowy kod programu PowerShell, który może służyć do wykonania kroków migracji i sprawdzania. Jest ona szczególnie przydatna w przypadku operacji na dużą skalę, gdy nie są możliwe ręczne wykonanie czynności.

## <a name="before-you-begin"></a>Przed rozpoczęciem
Przed wykonaniem migracji, jest ważne sprawdzić, czy następujące założenia mają wartość true dla wszystkich użytkowników podlegających migracji. Jeśli nie, migracja może zakończyć się niepowodzeniem w przypadku niektórych użytkowników i w związku z tym ich utratę dostępu do usług lub dane:

-   Użytkownicy mają *licencji źródła* przypisane przy użyciu grupy na podstawie licencjonowania. Licencji produktu przenieść od są dziedziczone z grupy jednego źródła i nie są przypisywane bezpośrednio. Uwaga: Jeśli licencje również są przypisane bezpośrednio, ich może uniemożliwiać stosowania *licencji docelowej*. Dowiedz się więcej o przypisanie licencji direct i grupy [tutaj](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-advanced#direct-licenses-coexist-with-group-licenses). Można użyć skryptu programu PowerShell, takie jak [tego](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-ps-examples#check-if-user-license-is-assigned-directly-or-inherited-from-a-group) do sprawdzenia, czy użytkownicy mają bezpośrednich licencji.

-   Masz za mało dostępnych licencji produktu docelowej. Jeśli nie, niektórzy użytkownicy mogą nie można pobrać *licencji docelowej*. Można sprawdzić liczbę dostępnych licencji [tutaj](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products).

-   Użytkownicy nie mają inne licencje produktów przypisane, które mogą powodować konflikt z *licencji docelowej* lub *licencji źródła* przed usunięciem (na przykład dodatek produktów, takie jak miejsce pracy Analityka lub Project Online, które zależy od innych produktów).

-   Zrozumienie sposobu zarządzania grup w danym środowisku. Na przykład jeśli zarządzania grupami lokalnymi i zsynchronizować je z usługą Azure AD przy użyciu AAD Connect, musisz usunąć użytkowników przy użyciu systemu lokalnego i potrwa trochę czasu, zanim zmiany do synchronizacji z usługą AAD i pobrać pobrana przez oparte na grupach licencji. Jeśli używasz usługi Azure AD członkostwa grup dynamicznych, można będzie Dodaj/Usuń użytkowników zamiast zmieniając ich atrybutów. Jednak proces migracji ogólnej nie zmienia się, jedyną różnicą jest sposób użytkownicy będą dodane/usunięte z grupy.

## <a name="migrating-users-between-products-without-conflicting-service-plans"></a>Migracja użytkowników między produktów bez planów usługi powodujące konflikt
Celem jest za pomocą licencjonowania na podstawie grupy można zmienić licencji użytkownika z *licencji źródła* (w tym przykładzie: *Office 365 Enterprise E3*) do *licencji docelowej* (w tym przykładzie: *Usługi office 365 Enterprise E5*). Te dwa produkty nie zawierają planów usługi powodujące konflikt, więc może być pełni przypisane w tym samym czasie, bez konflikt. W żadnym punkcie podczas migracji należy użytkownicy utracą dostęp do usług i danych. Ponadto migracji jest wykonywane partiami małe"", więc jest stosowany do sprawdzania poprawności wynik dla każdej z partii i zminimalizować zakres wszelkie problemy, które mogą wystąpić w trakcie procesu. Ogólne proces przebiega w następujący sposób:
1.  Użytkownicy są członkami grupy źródłowej i dziedziczą *licencji źródła* z tej grupy.
2.  Tworzenie grupy docelowej z *licencji docelowego* , ale bez żadnych elementów członkowskich.
3.  Dodaj zbiorczo użytkowników do grupy docelowej. Spowoduje to oparte na grupach licencji (GBL) zmiana i przypisać *licencji docelowej*. Należy pamiętać, że to może trochę potrwać w zależności od rozmiaru partii i innych działań w dzierżawie.
4.  Sprawdź, czy partii użytkowników został w pełni przetworzony przez GBL i każdy użytkownik faktycznie ma *licencji docelowej* przypisane. Sprawdź, czy użytkownicy nie zakończył w stanie błędu, takie jak powoduje konflikt z innymi produktami lub braku wystarczającą liczbę licencji. Więcej informacji na temat błędów [tutaj](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).
5.  W tym momencie użytkownicy mają zarówno źródło i *target licencji* przypisane.
6.  Usunięcie tej samej partii użytkownika z grupy źródłowej. Zmiana będzie odpowiadać GBL i *źródła licencji* zostaną usunięte z użytkowników.
7.  Należy powtórzyć dla partii kolejnych użytkowników.

### <a name="migrating-a-single-user-using-azure-portal"></a>Migrowanie pojedynczego użytkownika przy użyciu portalu Azure
Jest to prosty wskazówki dla pojedynczego użytkownika migrowane.

- **Krok 1:** użytkownik ma *licencji źródła* odziedziczone po grupie i nie bezpośredniego przypisania licencji.
![Użytkownik z licencją źródła odziedziczona po grupie](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInherited.png)

- **Krok 2:** użytkownik został dodany do grupy docelowej i GBL przetworzył zmianę; użytkownik ma teraz zarówno *źródła* i *docelowej* licencji dziedziczone z obu tych grup.
![Użytkownik z licencją źródła i celu dziedziczone z grup](media/active-directory-licensing-group-product-migration/UserWithBothSourceAndTargetLicense.png)

- **Krok 3:** użytkownik został usunięty z grupy źródłowej i GBL przetworzył zmianę; użytkownika teraz obejmuje tylko *licencji docelowej*
![użytkownika z licencją docelowy odziedziczona po grupie](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssigned.png)

### <a name="automating-migration-using-powershell"></a>Automatyzowanie migracji za pomocą programu PowerShell
> [!NOTE]
> Ten przykładowy kod korzysta z funkcji programu PowerShell uwzględnionych w [ostatniej sekcji](#powershell-automation-of-migration-and-verification-steps) tego dokumentu.

Ta Wstawka kodu pokazuje, jak można zautomatyzować proces migracji na większą skalę.
```
#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com','MigrationUser2@tailspinonline.com'

###############NON-CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:ENTERPRISEPACK'      #<- this is the O365 E3 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPREMIUM'   #<- this is the O365 E5 product

if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will assign the target license $targetSkuId to all users"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceandTargetLicensePresent} 'STEP 2: Checking if all users still have the source license and now also have the target license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

Przykładowe dane wyjściowe (migrowanie użytkowników 2):
```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPREMIUM licenses available (13) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.

STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will assign the target license TailspinOnline:ENTERPRISEPREMIUM to all users
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:ENTERPRISEPACK.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

## <a name="migrating-users-between-products-with-conflicting-service-plans"></a>Migracja użytkowników między produkty z planami usługi powodujące konflikt
Celem jest za pomocą licencjonowania na podstawie grupy można zmienić licencji użytkownika z *licencji źródła* (w tym przykładzie: *Office 365 Enterprise E1*) do *licencji docelowej* (w tym przykładzie: *Usługi office 365 Enterprise E3*). Te dwa produkty zawierają planów usługi powodujące konflikt (Dowiedz się więcej o konfliktach [tutaj](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans)), więc mamy będzie obejść, które bezproblemowo migrację użytkowników. W żadnym punkcie podczas migracji należy użytkownicy utracą dostęp do usług i danych. Ponadto migracji jest wykonywane partiami małe"", więc jest stosowany do sprawdzania poprawności wynik dla każdej z partii i zminimalizować zakres wszelkie problemy, które mogą wystąpić w trakcie procesu. Ogólne proces przebiega w następujący sposób:
1.  Użytkownicy są członkami grupy źródłowej i dziedziczą *licencji źródła* z tej grupy.
2.  Tworzenie grupy docelowej z *licencji docelowego* , ale bez żadnych elementów członkowskich.
3.  Dodaj zbiorczo użytkowników do grupy docelowej. Spowoduje to spowodować oparte na grupach licencji (GBL) do pobrania zmiany i spróbuj przypisać *licencji docelowej*. Przypisanie zakończy się niepowodzeniem z powodu konfliktów między usługami w dwóch produktów, a zamiast tego GBL zarejestruje błąd dla poszczególnych użytkowników.
Należy pamiętać, że to może trochę potrwać w zależności od rozmiaru partii i innych działań w dzierżawie.
4.  Sprawdź, czy partii użytkowników został w pełni przetworzony przez GBL i każdy użytkownik ma błąd konfliktu rejestrowane. Sprawdź, czy w przypadku niektórych użytkowników nie zakończył w stanie wystąpił nieoczekiwany błąd. Więcej informacji na temat błędów [tutaj](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).
5.  W tym momencie użytkownicy nadal mają *licencji źródła* i błąd konflikt, aby uzyskać *licencji docelowej* — nie mają *licencji docelowej* rzeczywiście, jeszcze przypisany.
6.  Usunięcie tej samej partii użytkownika z grupy źródłowej. GBL będzie odpowiadać na zmianę i *licencji źródła* zostaną usunięte z każdego użytkownika oraz, w tym samym czasie, błąd konfliktu zostaną również usunięte (przy założeniu, że żadna licencja produktu wnosi wkład do błędu) i *target licencji* zostanie przypisana. Dzięki temu bez utraty danych lub usług podczas przejścia.
7.  Należy powtórzyć dla partii kolejnych użytkowników.

### <a name="migrating-a-single-user-using-azure-portal"></a>Migrowanie pojedynczego użytkownika przy użyciu portalu Azure
Jest to prosty wskazówki dla pojedynczego użytkownika migrowane.

- **Krok 1:** użytkownik ma *licencji źródła* odziedziczone po grupie i nie bezpośredniego przypisania licencji.
![Użytkownik z licencją źródła odziedziczona po grupie](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInheritedConflictScenario.png)

- **Krok 2:** użytkownik został dodany do grupy docelowej i GBL przetworzył zmianę; użytkownik nadal ma *licencji źródła* , a teraz mają *licencji docelowej* w stan błędu ze względu na konflikt .
![Użytkownik z licencją źródła odziedziczone po grupie i obiekt docelowy licencji w stan błędu](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseAndTargetLicenseInConflict.png)

- **Krok 3:** użytkownik został usunięty z grupy źródłowej i GBL przetworzył zmianę; *licencji docelowej* teraz zastosowano użytkownikowi ![użytkownika z licencją docelowy odziedziczona po grupie](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssignedConflictScenario.png)

### <a name="automating-migration-using-powershell"></a>Automatyzowanie migracji za pomocą programu PowerShell
> [!NOTE]
> Ten przykładowy kod korzysta z funkcji programu PowerShell uwzględnionych w [ostatniej sekcji](#powershell-automation-of-migration-and-verification-steps) tego dokumentu.

Ta Wstawka kodu pokazuje, jak można zautomatyzować proces migracji na większą skalę.
```
#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

###############CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             #<- this is the O365 E1 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           #<- this is the O365 E3 product

#Assumptions before migration:
#1. Users are already in the source group and they have the source license assigned from that group
#2. Users do not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
#This is important - if not true, removing users from the source group in Step 3 is not going to result in the target license getting applied correctly
#3. There are enough available licenses for the target license to assign to the users we are migrating.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

Przykładowe dane wyjściowe (migrowanie użytkowników 2):
```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPACK licenses available (61) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.
STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will put target license TailspinOnline:ENTERPRISEPACK in conflict state with the source license TailspinOnline:STANDARDPACK
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:STANDARDPACK and remove the conflict on target license TailspinOnline:ENTERPRISEPACK which will become assigned.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

## <a name="powershell-automation-of-migration-and-verification-steps"></a>Automatyzacja programu PowerShell kroków migracji i sprawdzania
Ta sekcja zawiera kod programu PowerShell wymaganych do wykonywania skryptów używane we wcześniejszej części tego artykułu.

>[!WARNING]
>Ten kod stanowi przykład dla celów demonstracyjnych. Jeśli zamierzasz używać go w środowisku, należy wziąć pod uwagę testowanie go najpierw na małą skalę lub w dzierżawie oddzielne testu. Może być konieczne dostosowanie kod w celu spełnienia specyficznych potrzeb danego środowiska.

Aby wykonać kod, należy użyć [programu Azure AD PowerShell v1.0 bibliotek](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0] for instructions). Wykonanie *msolservice połączyć* polecenia cmdlet, aby najpierw zalogować się do dzierżawy przed wykonaniem skryptu.
```
#BEGIN: Helper functions used in the script

#Retrieves user object based on ObjectId or UserPrincipalName
function GetUserObject
{
    [OutputType([Microsoft.Online.Administration.User])]
    Param([object]$userId)

    $userIdType = $userId.GetType()

    if($userIdType -eq [Guid])
    {
        return Get-MsolUser -ObjectId $userId
    }
    elseif($userIdType -eq [string])
    {
        return Get-MsolUser -UserPrincipalName $userId
    }
    else
    {
        throw "User Id type must be a Guid or a string (UserPrincipalName). The user id type was: $($userIdType.Name)"
    }
}

#Gets a Guid objectId for a user, even if a UserPrincipal string was passed in. Guid ids are needed for group membership manipulation, where UPNs cannot be used
function GetGuidUserId
{
    [OutputType([Guid])]
    Param([object]$userId)

    [Guid]$guidId = [Guid]::Empty
    if($userId.GetType() -eq [String])
    {
        $user = GetUserObject $userId
        return $user.ObjectId
    }
    elseif($userId.GetType() -eq [Guid])
    {
        return $userId
    }
    else
    {
        throw "UserId type must be a Guid or a string (UserPrincipalName). The user id type was: $($userId.GetType().Name)"
    }
}

#Adds a collection of users to a group. Note: this fails if a user is already a member of the group
function AddUsersToGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Adding user $userId to group $groupId"
        Add-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

#Removes a collection of users from a group. Note: this fails if a user is not a member of the group
function RemoveUsersFromGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Removing user $userId from group $groupId"
        Remove-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

#Returns the license object corresponding to the skuId. Returns NULL if not found
function GetUserLicense
{
    [OutputType([Microsoft.Online.Administration.UserLicense])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    #we look for the specific license SKU in all licenses assigned to the user
    foreach($license in $user.Licenses)
    {
        if ($license.AccountSkuId -ieq $skuId)
        {
            return $license
        }
    }
    return $null
}

#Checks if the specific SKU license is assigned to the user, regardless of how it may be assigned (directly or via GBL)
function IsLicenseAssignedToUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [string]$skuId)

    $license = GetUserLicense $user $skuId

    return ($license -ne $null)
}

function GetObjectIdsAssigningLicense
{
    [OutputType([Guid[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    $license = GetUserLicense $user $skuId

    if ($license -ne $null)
    {
        return [Guid[]]$license.GroupsAssigningLicense
    }
    return [Array]::CreateInstance([Guid],0)
}

#Returns TRUE if the user is inheriting the license from the specific group.
#Note: this returns true only if the license is successfully assigned from the group. If the license is in error state, this return false
function UserHasLicenseAssignedFromThisGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId, [Guid]$groupId)

    [Guid[]]$objectsAssigningLicense = GetObjectIdsAssigningLicense $user $skuId

    #GroupsAssigningLicense contains a collection of IDs of objects assigning the license
    #This could be a group object or a user object (contrary to what the name suggests)
    foreach ($assignmentSource in $objectsAssigningLicense)
    {
        #If the collection contains at least one ID not matching the user ID this means that the license is inherited from a group.
        #Note: the license may also be assigned directly in addition to being inherited
        if ($assignmentSource -ieq $groupId)
        {
            return $true
        }
    }
    return $false
}

#Returns error objects for a specific license
function GetErrorsForLicense
{
    [OutputType([Microsoft.Online.Administration.IndirectLicenseError[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    [Microsoft.Online.Administration.IndirectLicenseError[]] $errorObjects = $user.IndirectLicenseErrors | Where {"$($_.AccountSku.AccountName):$($_.AccountSku.SkuPartNumber)" -ieq $skuId}

    if($errorObjects -eq $null)
    {
        #there are no errors at all
        return [Array]::CreateInstance([Microsoft.Online.Administration.IndirectLicenseError],0)
    }

    return $errorObjects
}
#Returns an error label associated with a specific license inherited from a specific group. return $null if there is no error
function GetErrorForLicenseFromGroup
{
    [OutputType([string])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId)


    #There are some errors. Check if any of them is associated with the group
    foreach($licenseError in GetErrorsForLicense $user $skuId)
    {
        if($licenseError.ReferencedObjectId -eq $groupId)
        {
            return $licenseError.Error
        }
    }
    return $null
}

#Checks if the license is in an expected state for a given group.
#If expectedError is set to a value, this looks if the license is in that specific error state from the group
#If expectedError is NULL, this checks if the license is successfully assigned from the group
function IsExpectedLicenseStateForGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId, [string]$expectedError)

    #we expect the license to be fully assigned from the group and not in error state
    if([string]::IsNullOrEmpty($expectedError))
    {
        #check if the assigned license is inherted from the expected group, without an error on it
        return (UserHasLicenseAssignedFromThisGroup $user $skuId $groupId)
    }
    #we expect the license to be in the specific error state on the specific group
    else
    {
        $error = GetErrorForLicenseFromGroup $user $groupId $skuId
        return ($error -ieq $expectedError)
    }
}

#Detects if the licenses are in the specific state where the source license is still assigned, but the target license is in conflict state
#Note: if the source license is not present, this throws an exception to abort the script, because that state may signify something went wrong with the migration steps and user lost access to services
function VerifySourceLicensePresentAndTargetLicenseInConflictState
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check user still has source license - if not, abort because something is seriously wrong
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is in conflict, as expected
    $conflictError = 'MutuallyExclusiveViolation'
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $conflictError)
}

#Detects if the licenses are in the specific state where the source license is no longer present, but the target license is correctly assigned
#Note: if the source license is gone, but target license is not present, this throws an exception to abort the script, because something went wrong and the user may have lost access to services
function VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check if user has the source license completely removed, which is a prerequisite to the target license eventually kicking in
    if(IsLicenseAssignedToUser $user $sourceSkuId)
    {
        return $false
    }

    #check user has the target license at all - if not, abort because something is seriously wrong
    if(-Not (IsLicenseAssignedToUser $user $targetSkuId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $targetSkuId assigned, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is assigned from the expected target group, and not in error state anymore
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $null)
}

#Detects if the licenses are in the specific state where the source license is still assigned and the target license is assigned as well
#Note: if the source license is not present, this throws an exception to abort the script, because that state may signify something went wrong with the migration steps and user lost access to services
function VerifySourceandTargetLicensePresent
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check user still has source license - if not, abort because something is seriously wrong
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is also assigned from the target group
    return (UserHasLicenseAssignedFromThisGroup $user $targetSkuId $targetGroupId)
}


#Verifies basic assumptions that should be true for a user before we execute the migration process.
#Returns TRUE if all assumptions are true. Prints details
function VerifyAssumptionsForUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    $userName = $user.UserPrincipalName
    #1. User has the source license assigned from the source group.
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        Write-Host "$userName does not have source license $sourceSkuId assigned from source group $sourceGroupId."
        return $false
    }

    #2. User does not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
    [Guid[]]$otherObjectsAssigningLicense = GetObjectIdsAssigningLicense $user $sourceSkuId | Where {$_ -ne $sourceGroupId}
    foreach($otherObject in $otherObjectsAssigningLicense)
    {
        if($otherObject -eq $user.ObjectId)
        {
            Write-Host "$userName has source license assigned directly to the user."
        }
        else
        {
            Write-Host "$username has source license assigned from an additional unexpected group $otherObject."
        }
    }
    if($otherObjectsAssigningLicense -and $otherObjectsAssigningLicense.Count -gt 0)
    {
        return $false
    }

    #3. User does not have the target license assigned
    if(IsLicenseAssignedToUser $user $targetSkuId)
    {
        Write-Host "$userName already has target license assigned."
        return $false
    }

    #4. User does not have the target license in error state from some groups
    [Microsoft.Online.Administration.IndirectLicenseError[]]$licenseErrors = GetErrorsForLicense $user $targetSkuId
    foreach($licenseError in $licenseErrors)
    {
        Write-Host "$userName has target license in error state $($licenseError.Error) from unexpected group $($licenseError.ReferencedObjectId)."
    }
    if($licenseErrors -and $licenseErrors.Count -gt 0)
    {
        return $false
    }

    Write-Host "$userName`t`tOK"
    return $true
}

#Checks if all users to be migrated are in correct state
function VerifyAssumptions
{
    [OutputType([bool])]
    Param([object[]]$userIds, [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    Write-Host "Verifying initial assumptions:"

    #Check if there are enough target licenses for all users
    $skuState = Get-MsolAccountSku | Where {$_.AccountSkuId -ieq $targetSkuId}

    if($skuState -eq $null)
    {
        Write-Host "Target license SKU $targetSkuId does not exist in the tenant at all."
        return $false
    }

    $availableLicenses = $skuState.ActiveUnits - $skuState.ConsumedUnits

    if($userIds.Count -gt $availableLicenses)
    {
        Write-Host "Not enough licenses for all users. User count: $($userIds.Count), licenses available: $availableLicenses"
        return $false
    }
    else
    {
        Write-Host "Enough $targetSkuId licenses available ($availableLicenses) for users: $($userIds.Count)."
    }

    #Check if each user to be migrated is in expected state
    $usersOK = 0
    $usersNotOK = 0
    foreach($userId in $userIds)
    {
        $user = GetUserObject $userId
        if(VerifyAssumptionsForUser $user $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId)
        {
            ++$usersOK
        }
        else
        {
            ++$usersNotOK
        }
    }
    if($usersNotOK -gt 0)
    {
        Write-Host "Checks passed for $usersOK users, but failed for $usersNotOK users."
        return $false
    }
    Write-Host "Checks passed for all $usersOK users."
    return $true
}

#Helper function: it executes one of the verification functions (passed in as a delegate using $checkFunction) for each user, keeps track of how many users passed/failed verification
#and repeats the loop until all users have passed the check. The loop may never terminate if some users never reach the expected state, which should be investigated.
#Note: if the verification function fails with an exception (e.g. because it detected unexpected user state) this loop will terminate and investigation into user state is needed
function ExecuteVerificationLoop
{
    Param([System.Management.Automation.ScriptBlock]$checkFunction, [string]$consoleMessage, [object[]]$userIds,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #how long to wait until the loop is retried
    $sleepIntervalSeconds = 60
    $retryIteration = 1

    While($true)
    {
        Write-Host "`n$consoleMessage. Check iteration: $retryIteration`n"

        $usersInExpectedState = 0
        $usersNotYet = 0

        foreach ($userId in $userIds)
        {
            $user = GetUserObject $userId
            if($checkFunction.InvokeReturnAsIs($user, $sourceGroupId, $sourceSkuId, $targetGroupId, $targetSkuId))
            {
                Write-Host "$userId`t`tExpected state: YES"
                ++$usersInExpectedState
            }
            else
            {
                Write-Host "$userId`t`tExpected state: NO"
                ++$usersNotYet
            }
        }

        Write-Host "`nTotal users checked: $($userIds.Count). In expected state: $usersInExpectedState. Not yet: $usersNotYet"

        if($usersNotYet -eq 0)
        {
            Write-Host "Check passed for all users. Exiting check loop."
            return
        }

        ++$retryIteration
        Write-Host "Not all users are in expected state. Waiting $sleepIntervalSeconds seconds to try again."
        Start-Sleep -Seconds $sleepIntervalSeconds
    }
}
#END: Helper functions used in the script

#BEGIN: Execute script

#enable strict mode
Set-StrictMode -Version latest
#stop on first exception thrown
$ErrorActionPreference = "Stop"

#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

###############CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             #<- this is the O365 E1 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           #<- this is the O365 E3 product

#Assumptions before migration:
#1. Users are already in the source group and they have the source license assigned from that group
#2. Users do not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
#This is important - if not true, removing users from the source group in Step 3 is not going to result in the target license getting applied correctly
#3. There are enough available licenses for the target license to assign to the users we are migrating.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#END: Execute script
```

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej na temat innych scenariuszy zarządzania licencji za pomocą grup, do odczytu

* [Identyfikowanie i rozwiązywanie problemów z licencji dla grupy w usłudze Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Przykłady programu PowerShell oparta na grupy licencjonowania w usłudze Azure AD](active-directory-licensing-ps-examples.md)
* [Usługa Azure Active Directory na podstawie grupy licencjonowania dodatkowe scenariusze](active-directory-licensing-group-advanced.md)
