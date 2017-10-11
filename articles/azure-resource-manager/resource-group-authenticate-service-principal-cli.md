---
title: "Utwórz tożsamość aplikacji usługi Azure z wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób użycia interfejsu wiersza polecenia Azure do tworzenia aplikacji usługi Azure Active Directory i nazwę główną usługi i przyznać jej dostęp do zasobów za pomocą kontroli dostępu opartej na rolach. Widoczny jest sposób uwierzytelniania aplikacji za pomocą hasła lub certyfikatu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 3c5826d58887ff1af4df8e66999d9c1a1643bcc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Use Azure CLI to create a service principal to access resources (Tworzenie jednostki usługi używanej do uzyskiwania dostępu do zasobów przy użyciu interfejsu wiersza polecenia platformy Azure)

Jeśli aplikacji lub skryptu, który ma dostęp do zasobów, można skonfigurować tożsamości aplikacji i uwierzytelniania w aplikacji przy użyciu własne poświadczenia. Ta tożsamość jest określany jako nazwy głównej usługi. Takie podejście umożliwia:

* Przypisanie uprawnień do tożsamości aplikacji, które są inne niż własnych uprawnień. Zazwyczaj te uprawnienia są ograniczone do dokładnie co aplikacja powinna wykonać.
* Użyj certyfikatu do uwierzytelnienia, podczas wykonywania skryptu instalacji nienadzorowanej.

W tym artykule przedstawiono sposób użycia [Azure CLI 1.0](../cli-install-nodejs.md) do skonfigurowania aplikacji do uruchamiania w własne poświadczenia i tożsamości. Zainstaluj najnowszą wersję pakietu [Azure CLI 1.0](../cli-install-nodejs.md) się upewnić, że środowisko odpowiada przykłady w tym artykule.

## <a name="required-permissions"></a>Wymagane uprawnienia
Do ukończenia tego tematu, należy posiadać odpowiednie uprawnienia w usłudze Azure Active Directory i Twojej subskrypcji platformy Azure. W szczególności należy utworzyć aplikację w usłudze Azure Active Directory i przypisać nazwę główną usługi do roli. 

Najłatwiejszym sposobem sprawdzenia, czy Twoje konto ma odpowiednie uprawnienia, jest skorzystanie z portalu. Zobacz [Sprawdzanie wymaganego uprawnienia w portalu](resource-group-create-service-principal-portal.md#required-permissions).

Teraz, przejdź do sekcji w obu [hasło](#create-service-principal-with-password) lub [certyfikatu](#create-service-principal-with-certificate) uwierzytelniania.

## <a name="create-service-principal-with-password"></a>Tworzenie nazwy głównej usługi z hasłem
W tej sekcji wykonaj kroki umożliwiające utworzenie aplikacji usługi AD przy użyciu hasła i przypisać rolę czytelnika do nazwy głównej usługi.

1. Zaloguj się do swojego konta.
   
   ```azurecli
   azure login
   ```
2. Aby utworzyć tożsamość aplikacji, należy podać nazwę aplikacji i hasło, jak pokazano w poniższym poleceniu:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   Zwracany jest nową główną nazwę usługi. Identyfikator obiektu jest potrzebna do przyznawania uprawnień. Identyfikator guid na liście z główne nazwy usług jest potrzebne podczas logowania. Ten identyfikator guid jest taką samą wartość jak identyfikator aplikacji. W przykładowych aplikacji, ta wartość jest określana jako `Client ID`. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. Przyznaj uprawnienia głównej usługi w ramach subskrypcji. W tym przykładzie należy dodać nazwy głównej usługi do roli Czytelnik, co powoduje przyznanie uprawnień do odczytu wszystkich zasobów w subskrypcji. Dla innych ról, zobacz [RBAC: role wbudowane](../active-directory/role-based-access-built-in-roles.md). Dla parametru objectid Podaj identyfikator obiektu, który został użyty podczas tworzenia aplikacji. Przed uruchomieniem tego polecenia, musisz zezwolić na pewien czas dla nowej nazwy głównej usługi propagację w usłudze Azure Active Directory. Po uruchomieniu tych poleceń ręcznie, zwykle wystarczająco dużo czasu upłynął między zadaniami. W skrypcie, należy dodać krok w stan uśpienia poleceń (takich jak `sleep 15`). Jeśli zostanie wyświetlony komunikat o błędzie informujący, że podmiot zabezpieczeń nie istnieje w katalogu, ponownie uruchom polecenie.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
Gotowe. Twojej aplikacji usługi AD i nazwę główną usługi są skonfigurowane. Następnej sekcji przedstawiono sposób Zaloguj się przy użyciu poświadczeń przy użyciu wiersza polecenia platformy Azure. Jeśli chcesz użyć poświadczeń w kodzie aplikacji, nie należy kontynuować w tym temacie. Można przejść do [przykładowe aplikacje](#sample-applications) przykłady logowania się za pomocą identyfikatora aplikacji id oraz hasła. 

### <a name="provide-credentials-through-azure-cli"></a>Podaj poświadczenia, za pomocą wiersza polecenia platformy Azure
Teraz musisz zalogować się jako aplikacji w celu wykonania operacji.

1. Gdy zalogujesz się jako nazwy głównej usługi, należy podać identyfikator dzierżawcy katalogu dla aplikacji usługi AD. Dzierżawa jest wystąpieniem usługi Azure Active Directory. Aby pobrać identyfikator dzierżawcy, uwierzytelnionym subskrypcji, należy użyć:
   
   ```azurecli
   azure account show
   ```
   
   Polecenie to zwraca:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Jeśli konieczne jest uzyskanie identyfikatora dzierżawy innej subskrypcji, użyj następującego polecenia:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Można pobrać identyfikatora klienta na potrzeby logowania, należy użyć:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     Wartość do użycia na potrzeby rejestrowania jest identyfikatorem guid, na liście głównych nazw usługi.
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. Zaloguj się jako nazwy głównej usługi.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    Zostanie wyświetlony monit o hasło. Podaj hasło, które można określić podczas tworzenia aplikacji usługi AD.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

Obecnie użytkownik jest uwierzytelniony jako nazwy głównej usługi dla nazwy głównej usługi, który został utworzony.

Alternatywnie można wywołać operacji REST w wierszu polecenia, aby się zalogować. Z odpowiedzi uwierzytelniania można pobrać tokenu dostępu do użycia z innych operacji. Na przykład pobierania tokenu dostępu za pomocą operacji REST, zobacz [generowania tokenu dostępu](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>Tworzenie nazwy głównej usługi o certyfikat
W tej sekcji możesz wykonać kroki, aby:

* Utwórz certyfikat z podpisem własnym
* Tworzenie aplikacji usługi AD z certyfikatu i nazwy głównej usługi
* przypisać rolę czytelnika do nazwy głównej usługi

Aby wykonać te kroki, musisz mieć [OpenSSL](http://www.openssl.org/) zainstalowane.

1. Tworzenie certyfikatu z podpisem własnym.
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. Poprzedni krok utworzyć dwa pliki - privkey.pem i cert.pem. Łączenie klucze publiczne i prywatne w jednym pliku.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Otwórz **examplecert.pem** pliku i poszukaj długo sekwencja znaków między **---BEGIN CERTIFICATE---** i **---END CERTIFICATE---**. Skopiuj dane certyfikatu. Te dane należy przekazać jako parametru podczas tworzenia głównej usługi.

4. Zaloguj się do swojego konta.

   ```azurecli
   azure login
   ```
5. Aby utworzyć nazwy głównej usługi, należy podać nazwę aplikacji i danych certyfikatu, jak pokazano w poniższym poleceniu:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   Zwracany jest nową główną nazwę usługi. Identyfikator obiektu jest potrzebna do przyznawania uprawnień. Identyfikator guid na liście z główne nazwy usług jest potrzebne podczas logowania. Ten identyfikator guid jest taką samą wartość jak identyfikator aplikacji. W przykładowych aplikacji ta wartość jest określana jako identyfikator klienta. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. Przyznaj uprawnienia głównej usługi w ramach subskrypcji. W tym przykładzie należy dodać nazwy głównej usługi do roli Czytelnik, co powoduje przyznanie uprawnień do odczytu wszystkich zasobów w subskrypcji. Dla innych ról, zobacz [RBAC: role wbudowane](../active-directory/role-based-access-built-in-roles.md). Dla parametru objectid Podaj identyfikator obiektu, który został użyty podczas tworzenia aplikacji. Przed uruchomieniem tego polecenia, musisz zezwolić na pewien czas dla nowej nazwy głównej usługi propagację w usłudze Azure Active Directory. Po uruchomieniu tych poleceń ręcznie, zwykle wystarczająco dużo czasu upłynął między zadaniami. W skrypcie, należy dodać krok w stan uśpienia poleceń (takich jak `sleep 15`). Jeśli zostanie wyświetlony komunikat o błędzie informujący, że podmiot zabezpieczeń nie istnieje w katalogu, ponownie uruchom polecenie.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Podaj certyfikat przy użyciu zautomatyzowanego skryptu wiersza polecenia platformy Azure
Teraz musisz zalogować się jako aplikacji w celu wykonania operacji.

1. Gdy zalogujesz się jako nazwy głównej usługi, należy podać identyfikator dzierżawcy katalogu dla aplikacji usługi AD. Dzierżawa jest wystąpieniem usługi Azure Active Directory. Aby pobrać identyfikator dzierżawcy, uwierzytelnionym subskrypcji, należy użyć:
   
   ```azurecli
   azure account show
   ```
   
   Polecenie to zwraca:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Jeśli konieczne jest uzyskanie identyfikatora dzierżawy innej subskrypcji, użyj następującego polecenia:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Aby pobrać odcisk palca certyfikatu i usunąć niepotrzebne znaków, należy użyć:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Która zwróci wartość odcisku palca są podobne do:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Można pobrać identyfikatora klienta na potrzeby logowania, należy użyć:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   Wartość do użycia na potrzeby rejestrowania jest identyfikatorem guid, na liście głównych nazw usługi.
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. Zaloguj się jako nazwy głównej usługi.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

Obecnie użytkownik jest uwierzytelniony jako nazwy głównej usługi dla aplikacji usługi Azure Active Directory, utworzony.

## <a name="change-credentials"></a>Zmiana poświadczeń

Aby zmienić poświadczenia dla aplikacji usługi AD, albo z powodu naruszenia zabezpieczeń lub wygaśnięcia poświadczeń, należy użyć `azure ad app set`.

Aby zmienić hasło, należy użyć:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

Aby zmienić wartość certyfikatu, należy użyć:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

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

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać szczegółowe instrukcje dotyczące integrowania aplikacji na platformie Azure do zarządzania zasobami, zobacz [przewodnik dewelopera do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](resource-manager-api-authentication.md).
* Aby uzyskać więcej informacji o używaniu certyfikatów i interfejsu wiersza polecenia Azure, zobacz [uwierzytelniania opartego na certyfikatach z podmiotami zabezpieczeń usługi Azure z wiersza polecenia systemu Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
* Aby uzyskać listę dostępnych akcji, które można udzielić lub odmówić dla użytkowników, zobacz [operacji dostawcy zasobów usługi Azure Resource Manager](../active-directory/role-based-access-control-resource-provider-operations.md).
