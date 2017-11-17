---
title: "Zarządzanie usługą Azure Key Vault przy użyciu interfejsu wiersza polecenia | Dokumentacja firmy Microsoft"
description: "Użyj tego samouczka w celu automatyzacji typowych zadań w Key Vault za pomocą interfejsu wiersza polecenia"
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: barclayn
ms.openlocfilehash: 94ea95e7f40c8d47dd18422a9c0795655dae365b
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="manage-key-vault-using-cli"></a>Zarządzanie Key Vault przy użyciu interfejsu wiersza polecenia

Użyj tego samouczka, aby ułatwić szybkie wprowadzenie do usługi Azure Key Vault w celu utworzenia wzmocnionego kontenera (magazynu) na platformie Azure. Usługa Azure Key vault jest używany do przechowywania kluczy kryptograficznych i kluczy tajnych. Ten artykuł przeprowadzi Cię przez proces tworzenia magazynu przy użyciu interfejsu wiersza polecenia platformy Azure i Platform. Zostanie następnie współdziałał z magazynem do wykonywania różnych typowych operacji. 

Usługa Azure Key Vault jest dostępna w większości regionów. Aby uzyskać więcej informacji, zobacz stronę [Cennik usługi Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

**Szacowany czas trwania:** 20 minut

> [!NOTE]
> Ten samouczek nie zawiera instrukcje dotyczące sposobu pisania aplikacji platformy Azure, w jeden z kroków zawiera. Ta przykładowa aplikacja jest używana pokazanie, jak aplikacja może być upoważnionych do używania klucza lub klucza tajnego przechowywane w magazynie kluczy.
> Ten artykuł koncentruje się na konfigurowaniu usługi Azure Key Vault przy użyciu interfejsu wiersza polecenia i Platform. Aby uzyskać instrukcje programu Azure PowerShell, zobacz [tym równoważnym samouczku](key-vault-get-started.md).

Aby uzyskać ogólne informacje na temat usługi Azure Key Vault, zobacz [Co to jest usługa Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Subskrypcja platformy Microsoft Azure. Jeśli nie masz, możesz zarejestrować się w celu [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial).
* Interfejs wiersza polecenia w wersji od 0.9.1 lub nowszym. Aby zainstalować najnowszą wersję i nawiązać połączenia z subskrypcją platformy Azure, zobacz [Instalowanie i Konfigurowanie interfejsu wiersza polecenia platformy Azure i Platform](../cli-install-nodejs.md).
* Aplikacja, która zostanie skonfigurowana w celu używania klucza lub hasła utworzonego w tym samouczku. Przykładowa aplikacja jest dostępna w [Centrum pobierania Microsoft](http://www.microsoft.com/download/details.aspx?id=45343). Aby uzyskać instrukcje, zobacz załączony plik Readme.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Uzyskiwanie pomocy z interfejsu wiersza polecenia platformy Azure i Platform

Ten samouczek zakłada, że znasz interfejsu wiersza polecenia (Bash, terminali, wiersza polecenia)

Parametru pomocy lub -h można wyświetlić Pomoc dla określonego polecenia. Alternatywnie azure help [polecenie] [opcje] format również może zostać użyty do zwrócenia tych samych informacji. Na przykład poniższe polecenia wszystkie zwracają te same informacje:

    azure account set --help

    azure account set -h

    azure help account set

W razie wątpliwości o parametry wymagane przez polecenie, zajrzyj do pomocy, przy użyciu — pomoc, -h lub azure help [polecenie].

Możesz przeczytać następujące samouczki, aby zapoznać się z usługi Azure Resource Manager trwa modelu w interfejsu wiersza polecenia platformy Azure i Platform:

* [Jak zainstalować i skonfigurować interfejs wiersza polecenia i Platform Azure](../cli-install-nodejs.md)
* [Przy użyciu interfejsu wiersza polecenia platformy Azure i Platform z usługą Azure Resource Manager](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Nawiązywanie połączenia z subskrypcjami

Aby zalogować się na platformie Azure przy użyciu następującego polecenia:

```azurecli-interactive
azure login -u username -p password
```

lub jeśli chcesz się zalogować, wpisując interakcyjnego

```azurecli-interactive
azure login
```

Jeśli masz wiele subskrypcji i chcesz określić, która ma być używana przez usługę Azure Key Vault, wpisz następujące polecenie, aby zobaczyć subskrypcje przypisane do konta:

```azurecli-interactive
azure account list
```

Aby określić subskrypcję, która ma być używana, wpisz polecenie:

```azurecli-interactive
azure account set <subscription name>
```

Aby uzyskać więcej informacji o konfigurowaniu Azure Międzyplatformowego interfejsu wiersza polecenia, zobacz [Instalowanie i Konfigurowanie interfejsu wiersza polecenia i Platform Azure](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Przełącz się do korzystania z usługi Azure Resource Manager
Magazyn kluczy wymaga usługi Azure Resource Manager, dlatego wpisz następujące polecenie, aby włączyć tryb usługi Azure Resource Manager:

```azurecli-interactive
azure config mode arm
```

## <a name="create-a-new-resource-group"></a>Utwórz nową grupę zasobów
Podczas korzystania z usługi Azure Resource Manager, wszystkie powiązane zasoby są tworzone wewnątrz grupy zasobów. W tym samouczku utworzymy nową grupę zasobów "ContosoResourceGroup".

```azurecli-interactive
azure group create 'ContosoResourceGroup' 'East Asia'
```

Pierwszym parametrem jest nazwa grupy zasobów, a drugi parametr jest lokalizacji. Dla lokalizacji, użyj polecenia `azure location list` do identyfikowania sposobu Określ alternatywną lokalizację do tego, w tym przykładzie. Aby uzyskać więcej informacji, wpisz:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Rejestrowanie dostawcy zasobów magazynu kluczy
Upewnij się, że tego dostawcy zasobów usługi Key Vault jest zarejestrowany w ramach Twojej subskrypcji:

```azurecli-interactive
azure provider register Microsoft.KeyVault
```

To tylko należy jednak wykonać jeden raz dla subskrypcji.

## <a name="create-a-key-vault"></a>Tworzenie magazynu kluczy

Użyj `azure keyvault create` polecenie, aby utworzyć magazyn kluczy. Ten skrypt ma trzy obowiązkowe parametry: Nazwa grupy zasobów, nazwa magazynu kluczy i lokalizacji geograficznej.

Na przykład:
- Jeśli używasz nazwy magazynu **ContosoKeyVault**
- Nazwa grupy zasobów z **ContosoResourceGroup**
- Lokalizacja **Azja Wschodnia** typu:

```azurecli-interactive
azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Dane wyjściowe tego polecenia są wyświetlane właściwości magazynu kluczy, które zostały utworzone. Dwie najważniejsze właściwości to:

* **Nazwa**: W tym przykładzie jest to ContosoKeyVault. Ta nazwa będzie używana do innych poleceń cmdlet usługi Key Vault.
* **vaultUri**: W tym przykładzie jest to https://contosokeyvault.vault.azure.net. Aplikacje korzystające z magazynu za pomocą jego interfejsu API REST muszą używać tego identyfikatora URI.

Twoje konto platformy Azure ma teraz uprawnienia do wykonywania dowolnych operacji na tym magazynie kluczy. Nikt inny nie jest do tego upoważniony.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Dodawanie klucza lub klucza tajnego do magazynu kluczy

Usługa Azure Key Vault utworzenie klucza chronionego przez oprogramowanie do, należy użyć `azure key create` polecenia i wpisz następujące polecenie:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software
````

Jednak jeśli masz istniejący klucz w pliku PEM, zapisany jako plik lokalny w pliku o nazwie softkey.pem, który chcesz przekazać do usługi Azure Key Vault, wpisz następujące polecenie, aby zaimportować klucz z. Plik PEM, który chroni klucz przez oprogramowanie w usłudze Key Vault:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software
```

Teraz możesz odwoływać się klucz, który został utworzony lub przekazany do usługi Azure Key Vault, za pomocą jego identyfikatora URI. Użyj **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** aby zawsze uzyskać bieżącą wersję oraz **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** uzyskać tę konkretną wersję.

Aby dodać klucza tajnego w magazynie, który jest hasłem o nazwie SQLPassword i ma wartość z Pa$ $w0rd do usługi Azure Key Vault, wpisz następujące polecenie:

```azurecli-interactive
azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'
```

Teraz możesz odwoływać się do hasła dodanego do usługi Azure Key Vault za pomocą jego identyfikatora URI. Użyj adresu **https://ContosoVault.vault.azure.net/secrets/SQLPassword**, aby zawsze uzyskać bieżącą wersję, oraz adresu **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**, aby uzyskać tę konkretną wersję.

Teraz wyświetlić klucz lub klucz tajny, który został właśnie utworzony:

* Aby wyświetlić klucz, wpisz polecenie: `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Aby wyświetlić klucz tajny, wpisz polecenie: `azure keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Rejestrowanie aplikacji w usłudze Azure Active Directory

Ten krok będzie zazwyczaj wykonywany przez programistę na innym komputerze. Nie jest specyficzne dla usługi Azure Key Vault, ale został tu zawarty, aby informacje były kompletne.

> [!IMPORTANT]
> Aby ukończyć samouczek, Twoje konto magazynu i aplikacji, która zarejestruje się w tym kroku wszystkie muszą być w tej samej usłudze Azure Active directory.

Aplikacje używające magazynu kluczy muszą zostać uwierzytelnione przy użyciu tokenu z usługi Azure Active Directory. Aby to zrobić, właściciel aplikacji musi najpierw zarejestrować ją w usłudze Azure Active Directory. Na koniec rejestracji właściciel aplikacji otrzymuje następujące wartości:

- **Identyfikator aplikacji** 
- **Klucz uwierzytelniania** (znanej także jako wspólny klucz tajny). 

Aby uzyskać token, aplikacja musi przedstawić obie wartości usłudze Azure Active Directory. Sposób konfiguracji aplikacji w tym celu zależy od aplikacji. Aby uzyskać [Key Vault przykładowej aplikacji](https://www.microsoft.com/download/details.aspx?id=45343), właściciel aplikacji ustawia te wartości w pliku app.config.

Aby uzyskać szczegółowe instrukcje dotyczące rejestrowania aplikacji w usłudze Azure Active Directory należy przeczytać artykuł zatytułowany [Integrowanie aplikacji z usługą Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md) lub [Użyj portalu, aby utworzyć w usłudze Azure Active Katalog aplikacji i nazwy głównej usługi, który ma dostęp do zasobów](../azure-resource-manager/resource-group-create-service-principal-portal.md) zarejestrować aplikację w usłudze Azure Active Directory:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Po lewej stronie, kliknij przycisk **rejestracji aplikacji**. Jeśli nie widzisz rejestracji aplikacji możesz kliknąć **więcej usług** i tam znaleźć.  
    >[!NOTE]
    Musi wybrać ten sam katalog, który zawiera subskrypcję platformy Azure, z którym zostanie utworzony magazyn kluczy. 
3. Kliknij przycisk **nowej rejestracji aplikacji**.
4. Na **Utwórz** bloku Podaj nazwę aplikacji, a następnie wybierz **interfejsu API sieci WEB i/lub aplikacji sieci WEB** (ustawienie domyślne) i określ **adres URL logowania** sieci Web aplikacja. Jeśli nie masz tych informacji w tym momencie można tworzyć dla tego kroku (na przykład można określić http://test1.contoso.com). Nie ma znaczenia, czy takie witryny istnieją. 

   ![Nowej rejestracji aplikacji](./media/key-vault-manage-with-cli/new-application-registration.png)

5. Kliknij przycisk **Utwórz**.
6. Po zakończeniu rejestracji aplikacji można zapoznać się z listą aplikacji w zarejestrowany. Znajdź aplikację, którą właśnie zarejestrowany i kliknij go.
7. Polecenie **aplikacji zarejestrowanej** kopiowania bloku **identyfikator aplikacji**
8. Polecenie **wszystkie ustawienia**
9. Na **ustawienia** , kliknij polecenie bloku **kluczy**
10. Wpisz opis w **klucza opis** i wybierz czas trwania, a następnie kliknij przycisk **ZAPISAĆ**. Strona zostanie odświeżona i pojawi się na niej wartość klucza. 
11. Użyjesz **identyfikator aplikacji** i **klucza** informacji w następnym kroku można ustawić uprawnień dotyczących magazynu.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Zezwalanie aplikacji na używanie klucza lub klucza tajnego
Aby zezwolić aplikacji na dostęp do klucza lub klucza tajnego w magazynie, należy użyć:

```azurecli-interactive
azure keyvault set-policy
```

Na przykład jeśli nazwa Twojego magazynu to ContosoKeyVault i aplikacji, które chcesz autoryzować ma identyfikator klienta 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed i chcesz zezwolić aplikacji na odszyfrowywanie oraz logowanie z kluczy w magazynie, następnie uruchom następujące polecenie:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'
```

> [!NOTE]
> Uruchamiasz w wierszu polecenia systemu Windows, należy zastąpić pojedynczych cudzysłowów podwójnych cudzysłowów i również wprowadzić wewnętrzny cudzysłowów. Na przykład: "[\"odszyfrować\",\"znak\"]".
> 

Jeśli chcesz zezwolić tej samej aplikacji na odczyt kluczy tajnych w magazynie, uruchom następujące polecenie:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'
```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Użycie sprzętowego modułu zabezpieczeń (HSM, hardware security module)
W celu zapewnienia dodatkowego bezpieczeństwa możesz zaimportować lub wygenerować klucze w sprzętowych modułach zabezpieczeń (HSM), które nigdy nie opuszczają granicy modułów HSM. Moduły HSM są zweryfikowane w trybie FIPS 140-2 poziom 2. Jeżeli te wymagania nie odnoszą się do Ciebie, pomiń tę sekcję i przejdź do sekcji [Usuwanie magazynu kluczy oraz skojarzonych kluczy i kluczy tajnych](#delete-the-key-vault-and-associated-keys-and-secrets).

Aby utworzyć te klucze chronione przez moduł HSM, musi mieć subskrypcję magazynu obsługującą klucze chronione przez moduł HSM.

Podczas tworzenia keyvault, Dodaj parametr "sku":

```azurecli-interactive
azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

Do tego magazynu możesz dodać klucze chronione oprogramowaniem (jak pokazano wcześniej) oraz klucze chronione modułem HSM. Aby utworzyć klucz chroniony przez moduł HSM, ustaw dla parametru docelowego "HSM":

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'
```

Następujące polecenie służy do importowania klucza z pliku PEM, na tym komputerze. To polecenie importuje klucz do modułu HSM w usłudze Key Vault:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'
```

Następne polecenie importuje pakiet „Wprowadź własny klucz” (BYOK, bring your own key). Umożliwia to wygenerowanie własnego klucza w lokalnym module HSM i przeniesienie go do modułów HSM w usłudze Key Vault bez opuszczania przez klucz granic modułu HSM:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'
```

Aby uzyskać szczegółowe instrukcje na temat generowania pakietu BYOK, zobacz [sposobu korzystania z usługi Azure Key Vault HSM-Protected klucze](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Usuwanie magazynu kluczy oraz skojarzonych kluczy i kluczy tajnych
Jeśli nie są już potrzebne magazynu kluczy oraz klucz lub klucz tajny, który zawiera można usunąć magazynu kluczy za pomocą polecenia delete azure keyvault:

```azurecli-interactive
azure keyvault delete --vault-name 'ContosoKeyVault'
```

Możesz także usunąć całą grupę zasobów platformy Azure zawierającą magazyn kluczy oraz inne zasoby, które zostały dodane do tej grupy:

```azurecli-interactive
azure group delete --name 'ContosoResourceGroup'
```


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Inne polecenia interfejsu wiersza polecenia platformy Azure i Platform
Inne polecenia użytkownik może przydatne do zarządzania usługą Azure Key Vault.

To polecenie wyświetla tabelaryczny widok wszystkich kluczy i wybranych właściwości:

```azurecli-interactive
azure keyvault key list --vault-name 'ContosoKeyVault'
```

To polecenie wyświetla pełną listę właściwości dla określonego klucza:

```azurecli-interactive
azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

To polecenie wyświetla tabelaryczny widok wszystkich nazw kluczy tajnych i wybranych właściwości:

```azurecli-interactive
azure keyvault secret list --vault-name 'ContosoKeyVault'
```

Poniżej przedstawiono przykładowy sposób usunięcia określonego klucza:

```azurecli-interactive
azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Poniżej przedstawiono przykładowy sposób usunięcia określonego klucza tajnego:

```azurecli-interactive
azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'
```


## <a name="next-steps"></a>Następne kroki
- Odwołania dotyczące programowania znajdują się w [przewodniku dewelopera usługi Azure Key Vault](key-vault-developers-guide.md).
- Aby uzyskać ogólne informacje na temat usługi Azure Key Vault, zobacz [Co to jest usługa Azure Key Vault?](key-vault-whatis.md)
- Jak pracować z usługą Azure Key Vault przy użyciu programu Powershell [wprowadzenie usługi Azure Key Vault](key-vault-get-started.md).


