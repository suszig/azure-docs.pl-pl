---
title: Zarejestruj stosu Azure | Dokumentacja firmy Microsoft
description: "Zarejestruj stosu Azure z subskrypcją platformy Azure."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/8/2018
ms.author: jeffgilb
ms.openlocfilehash: b58b3fc538d2237c12a860d268d550c4223155ba
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Zarejestruj stosu Azure z subskrypcją platformy Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Możesz zarejestrować [stosu Azure](azure-stack-poc.md) z platformy Azure, aby pobrać elementów marketplace z platformy Azure i ustaw commerce dane raportowania z powrotem do firmy Microsoft.

> [!NOTE]
>Rejestracja jest zalecane, ponieważ umożliwia testowanie ważne funkcje stosu Azure, takich jak zespolonego marketplace i raportowanie użycia. Po zarejestrowaniu stosu Azure użycia jest zgłaszane do handlu platformy Azure. Można to sprawdzić w ramach subskrypcji, używanego do rejestracji. Azure użytkowników stosu Development Kit nie są naliczane opłaty za bez użycia, które zgłaszają.


## <a name="get-azure-subscription"></a>Uzyskiwanie subskrypcji platformy Azure

Przed zarejestrowaniem stosu Azure przy użyciu platformy Azure, musi mieć:

- Identyfikator subskrypcji dla subskrypcji platformy Azure. Można uzyskać Identyfikatora, logowanie do platformy Azure, kliknij przycisk **więcej usług** > **subskrypcje**, kliknij subskrypcję, którego chcesz użyć, a następnie w obszarze **Essentials** można znaleźć **Identyfikator subskrypcji**. Chin, Niemczech i instytucji rządowych Stanów Zjednoczonych subskrypcje w chmurze nie są obecnie obsługiwane.
- Nazwa użytkownika i hasło dla konta, które jest właścicielem subskrypcji (kont MSA/2FA są obsługiwane).
- *Niewymagana, począwszy od wersji aktualizacji 1712 stosu Azure (180106.1):* Azure Active Directory subskrypcji platformy Azure. Ten katalog można znaleźć na platformie Azure, ustawiając kursor nad Awatar w prawym górnym rogu portalu Azure.

Jeśli nie masz subskrypcji platformy Azure, która spełnia te wymagania, możesz [utworzyć tutaj bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/free/?b=17.06). Rejestrowanie stosu Azure wiąże się bez kosztów w ramach subskrypcji platformy Azure.

## <a name="register-azure-stack-with-azure"></a>Zarejestruj stosu Azure przy użyciu platformy Azure  
> [!NOTE]
> Kroki te należy uruchomić z na komputerze, który ma dostęp do uprzywilejowanych punktu końcowego. Dla usługi Azure stosu zestawu SDK byłoby komputera-hosta. Jeśli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure.
>

1. Otwórz konsolę programu PowerShell jako administrator i [zainstalować program PowerShell Azure stosu](azure-stack-powershell-install.md).  

2. Dodaj konto platformy Azure, która służy do rejestrowania stosu Azure. Aby dodać konta, uruchom `Add-AzureRmAccount` polecenia cmdlet z parametrem EnvironmentName ustawionym **AzureCloud**. Zostanie wyświetlony monit o wprowadzenie poświadczeń konta platformy Azure i może być konieczne użycie uwierzytelniania wieloskładnikowego 2 na podstawie konfiguracji Twoje konto.

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureCloud"
   ```

3. Jeśli masz wiele subskrypcji, uruchom następujące polecenie, wybierz ten, który ma być używany:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Uruchom następujące polecenie, aby zarejestrować dostawcę zasobów Azure stosu w Twojej subskrypcji platformy Azure:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Usuń istniejące wersje moduły programu PowerShell, które odpowiadają rejestracji i [Pobierz najnowszą wersję z witryny GitHub](azure-stack-powershell-download.md).  

6. W katalogu "AzureStack-Tools-master", utworzony w poprzednim kroku przejdź do folderu "Rejestracji" i zaimportuj moduł ".\RegisterWithAzure.psm1":  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

7. W tej samej sesji programu PowerShell, uruchom następujący skrypt: po wyświetleniu monitu o poświadczenia, określ `azurestack\cloudadmin` jako użytkownika i hasła jest taka sama jak użytą dla administratora lokalnego podczas wdrażania.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Set-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development
   ```

   | Parametr | Opis |  
   |--------|-------------|
   | CloudAdminCredential | Poświadczenia domeny chmury, które są używane do [dostęp uprzywilejowany punktu końcowego](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Nazwa użytkownika jest w formacie  **\<domeny stosu Azure\>\cloudadmin**. Dla zestawu SDK, nazwa użytkownika ma ustawioną wartość **azurestack\cloudadmin**. Jeżeli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure, aby zyskać tę wartość.|  
   | PrivilegedEndpoint | Wstępnie skonfigurowane zdalnego programu PowerShell konsoli, która dostarcza funkcje, takie jak zbierania dzienników oraz innych post zadań wdrażania. Dla zestawu SDK uprzywilejowane punktu końcowego znajduje się na maszynie wirtualnej "AzS ERCS01". Jeżeli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure, aby zyskać tę wartość. Aby dowiedzieć się więcej, zapoznaj się [przy użyciu punktu końcowego uprzywilejowanych](azure-stack-privileged-endpoint.md) artykułu.|  
   | BillingModel | Modelu rozliczeń, który korzysta z subskrypcji. Dozwolone wartości tego parametru są- **pojemności**, **PayAsYouUse**, i **programowanie**. Dla zestawu SDK, ta wartość jest równa **programowanie**. Jeżeli używasz zintegrowanego systemu, skontaktuj się z operatorem stosu Azure, aby zyskać tę wartość. |

8. Po ukończeniu działania skryptu, zostanie wyświetlony komunikat "Azure uaktywnianie stosu (ten krok może potrwać do 10 minut)."




## <a name="verify-the-registration"></a>Sprawdź rejestrację  

1. Zaloguj się do portalu administratora (https://adminportal.local.azurestack.external).

2. Kliknij przycisk **więcej usług** > **zarządzania Marketplace** > **dodać z platformy Azure**.

3. Jeśli zobaczysz listę dostępnych z platformy Azure (na przykład WordPress) elementów, proces aktywacji zakończyło się pomyślnie.

> [!NOTE]
> Po zakończeniu rejestracji aktywne ostrzeżenie nie rejestracji nie będzie dłużej widoczny.


## <a name="modify-the-registration"></a>Modyfikowanie rejestracji

### <a name="change-the-subscription-you-use"></a>Zmień subskrypcję, których używasz
Jeśli chcesz zmienić subskrypcję, którego używasz, musi najpierw uruchom Usuń AzsRegistration, upewnij się, że użytkownik jest zalogowany do poprawny kontekst programu Azure PowerShell a następnie uruchom zestaw AzsRegistration wszystkie parametry zmienione.

  ```Powershell   
  Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```
### <a name="change-the-billing-model-or-syndication-features"></a>Zmień rozliczeń modelu lub zespolonego funkcji
Jeśli chcesz zmienić modelu rozliczeń lub zespolonego funkcji dla instalacji, należy wywołać funkcję rejestracji, aby ustawić nowe wartości. Nie trzeba najpierw usunąć bieżący rejestracji.  

  ```Powershell
     Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```


## <a name="disconnected-registration"></a>Rozłączona rejestracji
*Informacje przedstawione w tej sekcji ma zastosowanie, począwszy od wersji aktualizacji 1712 stosu Azure (180106.1) i nie jest obsługiwane we wcześniejszych wersjach.*

W przypadku rejestracji Azure stosu w środowisku bez połączenia, należy pobrać rejestracji tokenu ze środowiska Azure stosu, a następnie użyj tokenu na komputerze, na którym mogą łączyć się z platformy Azure dla rejestracji.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Pobierz rejestracji token ze środowiska Azure stosu
  1. Aby uzyskać tokenu rejestracji, uruchom następujące polecenie:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > Token rejestracji jest zapisany w określonym pliku *$env:SystemDrive\RegistrationToken.txt*.

  2. Zapisz ten token rejestracji do użycia na platformie Azure połączone maszyny.


### <a name="connect-to-azure-and-register"></a>Łączenie się z platformy Azure i rejestr
Kroki do wykonania tej procedury należy uruchomić na komputerze, na którym można nawiązać połączenia z usługi Azure.

  1. [Pobierz najnowsze moduły programu PowerShell, które odpowiadają rejestracji z usługi GitHub](azure-stack-powershell-download.md).  

  2. W katalogu "AzureStack-Tools-master", utworzony w poprzednim kroku przejdź do folderu "Rejestracji" i zaimportuj moduł ".\RegisterWithAzure.psm1":  

     ```powershell
     Import-Module .\RegisterWithAzure.psm1
     ```

  3. Kopiuj [RegisterWithAzure.psm1](https://go.microsoft.com/fwlink/?linkid=842959) do folderu, takich jak *C:\Temp*.

  4. Uruchom program PowerShell ISE jako Administrator, a następnie zaimportuj moduł RegisterWithAzure.  

  5. Upewnij się, że użytkownik jest zalogowany w poprawny kontekst programu Azure PowerShell, który ma być używany do rejestrowania środowiska Azure stosu:  

     ```Powershell
        Set-AzureRmContext -SubscriptionId $YourAzureSubscriptionId   
     ```
  6. Określ token rejestracji do rejestracji w usłudze Azure:

     ```Powershell  
       $registrationToken = "*Your Registration Token*"
       Register-AzsEnvironment -RegistrationToken $registrationToken  
     ```
    Opcjonalnie możesz użyć polecenia cmdlet Get-Content wskaż plik, który zawiera token rejestracji:

    ```Powershell  
       $registrationToken = Get-Content -Path 'C:\Temp\<Registration Token File>'
       Register-AzsEnvironment -RegistrationToken $registrationToken  
    ```
> [!NOTE]  
> Zapisz nazwę zasobu rejestracji lub token rejestracji do użytku w przyszłości.

### <a name="remove-a-registered-resource"></a>Usuń zarejestrowanych zasobów
Jeśli chcesz usunąć zasób rejestracji, należy użyć UnRegister-AzsEnvironment i podaj nazwę zasobu rejestracji lub token rejestracji użytej do rejestru AzsEnvironment.
- **Nazwa zasobu rejestracji:**

  ```Powershell    
     UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
  ```
- **Token rejestracji:**    

  ```Powershell
     $registrationToken = "*Your copied registration token*"
     UnRegister-AzsEnvironment -RegistrationToken $registrationToken
   ```


## <a name="next-steps"></a>Kolejne kroki
[Nawiązywanie połączenia z usługą Azure Stack](azure-stack-connect-azure-stack.md)
