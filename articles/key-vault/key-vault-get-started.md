---
title: "Rozpoczęcie pracy z usługą Azure Key Vault | Microsoft Docs"
description: "Ten samouczek ułatwi Ci rozpoczęcie pracy usługą Azure Key Vault w celu utworzenia wzmocnionego kontenera na platformie Azure do przechowywania kluczy kryptograficznych i kluczy tajnych na platformie Azure oraz zarządzania nimi."
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/16/2017
ms.author: barclayn
ms.openlocfilehash: d689fc0724b613abf2b319037dbd5555372622a8
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="get-started-with-azure-key-vault"></a>Rozpoczynanie pracy z usługą Azure Key Vault
Usługa Azure Key Vault jest dostępna w większości regionów. Aby uzyskać więcej informacji, zobacz stronę [Cennik usługi Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Wprowadzenie
Ten samouczek ułatwi Ci rozpoczęcie pracy z usługą Azure Key Vault w celu utworzenia wzmocnionego kontenera (magazynu) na platformie Azure do przechowywania kluczy kryptograficznych i kluczy tajnych na platformie Azure oraz zarządzania nimi. Przeprowadzi Cię on przez proces tworzenia magazynu przy użyciu programu Azure PowerShell. Magazyn zawiera klucz lub hasło, którego możesz używać w aplikacji Azure. Następnie pokaże Ci, w jaki sposób aplikacja może użyć tego klucza lub hasła.

**Szacowany czas trwania:** 20 minut

> [!NOTE]
> Ten samouczek nie zawiera instrukcji dotyczących sposobu pisania aplikacji platformy Azure, ale jeden z kroków zawiera instrukcję dotyczącą sposobu zezwalania aplikacji na używanie klucza lub klucza tajnego w magazynie kluczy.
>
> W tym samouczku jest używany program Azure PowerShell. Instrukcje dotyczące wieloplatformowego interfejsu wiersza polecenia znajdują się w [tym równoważnym samouczku](key-vault-manage-with-cli2.md).
>
>

Aby uzyskać ogólne informacje na temat usługi Azure Key Vault, zobacz [Co to jest usługa Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka niezbędne są następujące elementy:

* Subskrypcja platformy Microsoft Azure. Jeśli jej nie masz, możesz zarejestrować się w celu [utworzenia bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/).
* Program Azure PowerShell, **minimalna wersja 1.1.0**. Aby zainstalować program Azure PowerShell i skojarzyć go z subskrypcją platformy Azure, zobacz [Sposób instalowania i konfigurowania programu Azure PowerShell](/powershell/azure/overview). Jeśli masz już zainstalowany program Azure PowerShell, ale nie wiesz, z jakiej wersji korzystasz, w konsoli programu Azure PowerShell wpisz polecenie `(Get-Module azure -ListAvailable).Version`. Jeśli masz zainstalowany program Azure PowerShell w wersji od 0.9.1 do 0.9.8, nadal możesz korzystać tego samouczka z niewielkimi zmianami. Na przykład musisz użyć polecenia `Switch-AzureMode AzureResourceManager`. Do tego niektóre polecenia usługi Azure Key Vault uległy zmianie. Aby uzyskać listę poleceń cmdlet usługi Key Vault dla wersji od 0.9.1 do 0.9.8, zobacz [Polecenia cmdlet usługi Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).
* Aplikacja, która zostanie skonfigurowana w celu używania klucza lub hasła utworzonego w tym samouczku. Przykładowa aplikacja jest dostępna w [Centrum pobierania Microsoft](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Aby uzyskać instrukcje, zobacz załączony plik Readme.

Ten samouczek jest przeznaczony dla początkujących użytkowników programu Azure PowerShell, ale przyjęto założenie, że rozumiesz podstawowe pojęcia, takie jak moduły, polecenia cmdlet i sesje. Aby uzyskać więcej informacji, zobacz artykuł [Wprowadzenie do programu Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx).

Aby uzyskać szczegółowe informacje na temat każdego polecenia cmdlet, które pojawi się w tym samouczku, użyj polecenia cmdlet **Get-Help**.

    Get-Help <cmdlet-name> -Detailed

Na przykład aby uzyskać pomoc dotyczącą polecenia cmdlet **Login-AzureRmAccount**, wpisz:

    Get-Help Login-AzureRmAccount -Detailed

Aby zapoznać się z usługą Azure Resource Manager w programie Azure PowerShell, możesz przeczytać następujące samouczki:

* [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview)
* [Używanie programu Azure PowerShell z usługą Resource Manager](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Nawiązywanie połączenia z subskrypcjami
Uruchom sesję programu PowerShell Azure i zaloguj się na konto platformy Azure przy użyciu następującego polecenia:  

    Login-AzureRmAccount

Pamiętaj, że jeśli używasz konkretnego wystąpienia platformy Azure, na przykład Azure dla instytucji rządowych, użyj parametru środowiska za pomocą tego polecenia. Na przykład: `Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

W podręcznym oknie przeglądarki wprowadź nazwę użytkownika i hasło dla konta platformy Azure. Program Azure PowerShell pobierze wszystkie subskrypcje, które są skojarzone z tym kontem, i domyślnie użyje pierwszej.

Jeśli masz wiele subskrypcji i chcesz określić, która ma być używana przez usługę Azure Key Vault, wpisz następujące polecenie, aby zobaczyć subskrypcje przypisane do konta:

    Get-AzureRmSubscription

Aby określić subskrypcję, która ma być używana, wpisz polecenie:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Aby uzyskać więcej informacji na temat konfigurowania programu Azure PowerShell, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

## <a id="resource"></a>Tworzenie nowej grupy zasobów
Podczas korzystania z usługi Azure Resource Manager wszystkie powiązane zasoby są tworzone wewnątrz grupy zasobów. W tym samouczku utworzysz nową grupę zasobów o nazwie **ContosoResourceGroup**:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Tworzenie magazynu kluczy
Użyj polecenia cmdlet [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault), aby utworzyć magazyn kluczy. To polecenie cmdlet ma trzy obowiązkowe parametry: **nazwa grupy zasobów**, **nazwa magazynu kluczy** oraz **lokalizacja geograficzna**.

Na przykład jeśli chcesz użyć nazwy magazynu **ContosoKeyVault**, grupy zasobów **ContosoResourceGroup** i lokalizacji **East Asia**, wpisz:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Dane wyjściowe polecenia cmdlet pokazują właściwości magazynu kluczy, który właśnie został utworzony. Dwie najważniejsze właściwości to:

* **Nazwa magazynu**: w tym przykładzie jest to **ContosoKeyVault**. Ta nazwa będzie używana do innych poleceń cmdlet usługi Key Vault.
* **Identyfikator URI magazynu**: w tym przykładzie jest to https://contosokeyvault.vault.azure.net/. Aplikacje korzystające z magazynu za pomocą jego interfejsu API REST muszą używać tego identyfikatora URI.

Twoje konto platformy Azure ma teraz uprawnienia do wykonywania dowolnych operacji na tym magazynie kluczy. Nikt inny nie jest do tego upoważniony.

> [!NOTE]
> Jeśli podczas próby utworzenia nowego magazynu kluczy zostanie wyświetlony błąd **Subskrypcja nie jest zarejestrowana w celu używania przestrzeni nazw „Microsoft.KeyVault”**, uruchom polecenie `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"`, a następnie uruchom ponownie polecenie New-AzureRmKeyVault. Aby uzyskać więcej informacji, zobacz temat [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Dodawanie klucza lub klucza tajnego do magazynu kluczy
Jeśli chcesz, aby usługa Azure Key Vault utworzyła klucz chroniony oprogramowaniem, użyj polecenia cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) i wpisz następujące polecenie:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Jeśli jednak masz istniejący klucz chroniony oprogramowaniem w pliku PFX zapisanym na dysku C:\, w pliku o nazwie softkey.pfx, który chcesz przekazać do usługi Azure Key Vault, wpisz następujące polecenie, aby ustawić wartość zmiennej **securepfxpwd** na hasło **123** dla pliku PFX:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Następnie wpisz następujące polecenie, aby zaimportować klucz z pliku PFX, który chroni klucz w usłudze Key Vault przy użyciu oprogramowania:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Teraz możesz odwoływać się do klucza, który został utworzony lub przekazany do usługi Azure Key Vault, za pomocą jego identyfikatora URI. Użyj adresu **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey**, aby zawsze uzyskać bieżącą wersję, oraz adresu **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**, aby uzyskać tę konkretną wersję.  

Aby wyświetlić identyfikator URI dla tego klucza, wpisz polecenie:

    $Key.key.kid

Aby dodać do magazynu klucz tajny, który jest hasłem o nazwie SQLPassword i ma wartość Pa$$w0rd w usłudze Azure Key Vault, najpierw przekonwertuj wartość Pa$$w0rd na bezpieczny ciąg, wpisując następujące polecenie:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Następnie wpisz następujące polecenie:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Teraz możesz odwoływać się do hasła dodanego do usługi Azure Key Vault za pomocą jego identyfikatora URI. Użyj adresu **https://ContosoVault.vault.azure.net/secrets/SQLPassword**, aby zawsze uzyskać bieżącą wersję, oraz adresu **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**, aby uzyskać tę konkretną wersję.

Aby wyświetlić identyfikator URI dla tego klucza tajnego, wpisz polecenie:

    $secret.Id

Możesz teraz wyświetlić utworzony klucz lub klucz tajny:

* Aby wyświetlić klucz, wpisz polecenie: `Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
* Aby wyświetlić klucz tajny, wpisz polecenie: `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Teraz magazyn kluczy oraz klucz lub klucz tajny jest gotowy do użycia przez aplikacje. Musisz zezwolić aplikacjom na korzystanie z nich.  

## <a id="register"></a>Rejestrowanie aplikacji w usłudze Azure Active Directory
Ten krok będzie zazwyczaj wykonywany przez programistę na innym komputerze. Nie jest to krok specyficzny dla usługi Azure Key Vault, ale został tu zawarty, aby informacje były kompletne.

> [!IMPORTANT]
> Aby ukończyć samouczek, Twoje konto, magazyn i aplikacja, która zostanie zarejestrowana w tym kroku, muszą należeć do tego samego katalogu Azure.
>
>

Aplikacje używające magazynu kluczy muszą zostać uwierzytelnione przy użyciu tokenu z usługi Azure Active Directory. Aby to zrobić, właściciel aplikacji musi najpierw zarejestrować ją w usłudze Azure Active Directory. Na koniec rejestracji właściciel aplikacji otrzymuje następujące wartości:

* **Identyfikator aplikacji** (znany także jako Identyfikator klienta) oraz **klucz uwierzytelniania** (znany także jako wspólny klucz tajny). Aby uzyskać token, aplikacja musi przedstawić obie wartości usłudze Azure Active Directory. Sposób konfiguracji aplikacji w tym celu zależy od aplikacji. W przypadku przykładowej aplikacji usługi Key Vault właściciel aplikacji ustawia te wartości w pliku app.config.

Aby zarejestrować aplikację w usłudze Azure Active Directory:

1. Zaloguj się do klasycznego portalu Azure.
2. Po lewej stronie kliknij pozycję **Usługa Active Directory**, a następnie wybierz katalog, w którym chcesz zarejestrować aplikację. <br> <br> **Uwaga:** musisz wybrać ten sam katalog, który zawiera subskrypcję platformy Azure użytą do utworzenia magazynu kluczy. Jeśli nie wiesz, który to katalog, kliknij pozycję **Ustawienia**, zidentyfikuj subskrypcję, za pomocą której został utworzony magazyn kluczy, i zanotuj nazwę katalogu wyświetloną w ostatniej kolumnie.
3. Kliknij pozycję **APLIKACJE**. Jeśli żadna aplikacja nie została dodana do katalogu, strona zawiera tylko link **Dodaj aplikację**. Kliknij link lub alternatywnie kliknij przycisk **DODAJ** znajdujący się na pasku poleceń.
4. W kreatorze **DODAWANIE APLIKACJI** na stronie **Co chcesz zrobić?** kliknij pozycję **Dodawanie aplikacji opracowywanej przez moją organizację**.
5. Na stronie **Powiedz nam o swojej aplikacji** określ nazwę swojej aplikacji, a następnie wybierz pozycję **APLIKACJA SIECI WEB I/LUB INTERFEJS API SIECI WEB** (opcja domyślna). Kliknij ikonę **Dalej**.
6. Na stronie **Właściwości aplikacji** określ **ADRES URL LOGOWANIA** i **IDENTYFIKATOR URI APLIKACJI** dla swojej aplikacji sieci Web. Jeśli aplikacja nie ma tych wartości, możesz je wymyślić na potrzeby tego kroku (na przykład możesz wpisać adres http://test1.contoso.com w obu polach). Nie ma znaczenia, czy takie witryny istnieją. Ważne jest, aby każda aplikacja w katalogu miała inny identyfikator URI aplikacji. Katalog używa tego ciągu do identyfikowania Twojej aplikacji.
7. Kliknij ikonę **Zakończ**, aby zapisać zmiany w kreatorze.
8. Na stronie **Szybki start** kliknij pozycję **KONFIGURUJ**.
9. Przewiń do sekcji **Klucze**, wybierz czas trwania, a następnie kliknij przycisk **ZAPISZ**. Strona zostanie odświeżona i pojawi się na niej wartość klucza. Musisz skonfigurować aplikację przy użyciu tej wartości klucza i wartości **IDENTYFIKATOR KLIENTA**. (Instrukcje dotyczące tej konfiguracji są specyficzne dla aplikacji).
10. Skopiuj wartość identyfikatora klienta z tej strony. Użyjesz jej w następnym kroku w celu ustawienia uprawnień dotyczących magazynu.

## <a id="authorize"></a>Zezwalanie aplikacji na używanie klucza lub klucza tajnego
Aby przyznać aplikacji dostęp do klucza lub klucza tajnego w magazynie, użyj polecenia cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy).

Na przykład jeśli nazwa Twojego magazynu to **ContosoKeyVault**, a identyfikator klienta aplikacji, której chcesz przyznać dostęp, to 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed i chcesz zezwolić aplikacji na odszyfrowywanie oraz logowanie przy użyciu kluczy w magazynie, uruchom następujące polecenie:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Jeśli chcesz zezwolić tej samej aplikacji na odczyt kluczy tajnych w magazynie, uruchom następujące polecenie:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Użycie sprzętowego modułu zabezpieczeń (HSM, hardware security module)
W celu zapewnienia dodatkowego bezpieczeństwa możesz zaimportować lub wygenerować klucze w sprzętowych modułach zabezpieczeń (HSM), które nigdy nie opuszczają granicy modułów HSM. Moduły HSM są zweryfikowane w trybie FIPS 140-2 poziom 2. Jeżeli te wymagania nie odnoszą się do Ciebie, pomiń tę sekcję i przejdź do sekcji [Usuwanie magazynu kluczy oraz skojarzonych kluczy i kluczy tajnych](#delete).

Aby utworzyć te klucze chronione modułem HSM, należy użyć [usługi Azure Key Vault w warstwie Premium do obsługi kluczy chronionych modułem HSM](https://azure.microsoft.com/pricing/free-trial/). Ponadto warto zauważyć, że funkcja ta nie jest dostępna dla chińskiej wersji platformy Azure.

Po utworzeniu magazynu kluczy dodaj parametr **- SKU**:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Do tego magazynu kluczy możesz dodać klucze chronione oprogramowaniem (jak pokazano wcześniej) oraz klucze chronione modułem HSM. Aby utworzyć klucz chroniony modułem HSM, ustaw parametr **-Destination** w opcji „HSM”:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Następujące polecenie służy do importowania klucza z pliku PFX znajdującego się na Twoim komputerze. To polecenie importuje klucz do modułu HSM w usłudze Key Vault:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Następne polecenie importuje pakiet „Wprowadź własny klucz” (BYOK, bring your own key). Ten scenariusz umożliwia wygenerowanie własnego klucza w lokalnym module HSM i przeniesienie go do modułów HSM w usłudze Key Vault bez opuszczania przez klucz granic modułu HSM:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Aby uzyskać szczegółowe instrukcje na temat generowania pakietu BYOK, zobacz artykuł [Generowanie i przenoszenie kluczy chronionych modułem HSM dla usługi Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Usuwanie magazynu kluczy oraz skojarzonych kluczy i kluczy tajnych
Jeśli magazyn kluczy oraz zawarte w nim klucze i klucze tajne nie są już potrzebne, możesz je usunąć przy pomocy polecenia cmdlet [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault):

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Możesz także usunąć całą grupę zasobów platformy Azure zawierającą magazyn kluczy oraz inne zasoby, które zostały dodane do tej grupy:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Inne polecenia cmdlet programu Azure PowerShell
Inne polecenia, które mogą być przydatne do zarządzania usługą Azure Key Vault:

* `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: To polecenie wyświetla tabelaryczny widok wszystkich kluczy i wybranych właściwości.
* `$Keys[0]`: To polecenie wyświetla pełną listę właściwości dla określonego klucza
* `Get-AzureKeyVaultSecret`: To polecenie wyświetla tabelaryczny widok wszystkich nazw kluczy tajnych i wybranych właściwości.
* `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Przykładowy sposób usunięcia określonego klucza.
* `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Przykładowy sposób usunięcia określonego klucza tajnego.

## <a id="next"></a>Następne kroki
Aby zapoznać się z kolejnym samouczkiem, w którym jest używana usługa Azure Key Vault w aplikacji sieci Web, zobacz artykuł [Użycie usługi Azure Key Vault z aplikacji sieci Web](key-vault-use-from-web-application.md).

Aby zobaczyć sposób użycia Twojego magazynu kluczy, zobacz artykuł [Rejestrowanie usługi Azure Key Vault](key-vault-logging.md).

Aby zapoznać się z listą najnowszych poleceń cmdlet programu Azure PowerShell dla usługi Azure Key Vault, zobacz artykuł [Polecenia cmdlet w usłudze Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).

Odwołania dotyczące programowania znajdują się w [przewodniku dewelopera usługi Azure Key Vault](key-vault-developers-guide.md).
