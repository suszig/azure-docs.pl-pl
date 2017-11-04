---
title: "Wdrażanie maszyny wirtualnej przy użyciu certyfikatu bezpiecznie przechowywane na stosie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć maszynę wirtualną i Wypchnij certyfikatu na go za pomocą magazynu kluczy Azure stosu"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 46590eb1-1746-4ecf-a9e5-41609fde8e89
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/03/2017
ms.author: sngun
ms.openlocfilehash: 29ccdc9eca9911b2f550f9e09da83d0b1d30f9db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-and-include-certificate-retrieved-from-a-key-vault"></a>Tworzenie maszyny wirtualnej i uwzględnianie certyfikat pobrany z magazynu kluczy

Ten artykuł ułatwia tworzenie maszyny wirtualnej w stos Azure i wypychania certyfikaty na niego. 

## <a name="prerequisites"></a>Wymagania wstępne

* Należy oferta obejmuje usługi Key Vault musi subskrybować. 
* [Instalowanie programu PowerShell Azure stosu.](azure-stack-powershell-install.md)  
* [Konfigurowanie środowiska PowerShell użytkownika Azure stosu](azure-stack-powershell-configure-user.md)

Magazyn kluczy Azure stosu jest używany do przechowywania certyfikatów. Certyfikaty są przydatne w wielu scenariuszach. Na przykład Rozważmy scenariusz, w którym znajduje się maszyna wirtualna w stosie Azure, w którym uruchomiono aplikację, która wymaga certyfikatu. Ten certyfikat może służyć do szyfrowania do uwierzytelniania w usłudze Active Directory lub do używania protokołu SSL w witrynie sieci Web. Występowanie certyfikatu w pomaga magazynu kluczy, upewnij się, że jest bezpieczna.

W tym artykule firma Microsoft opisano kroki wymagane do dystrybuowania certyfikatów do maszyny wirtualnej systemu Windows Azure stosu. Korzystania z tych kroków, z Development Kit stosu Azure lub z systemem Windows klienta zewnętrznego, jeśli są połączone za pośrednictwem sieci VPN.

W poniższych krokach opisano proces wymagany do dystrybuowania certyfikatów do maszyny wirtualnej:

1. Tworzenie magazynu klucz tajny.
2. Zaktualizuj plik azuredeploy.parameters.json.
3. Wdrożenie szablonu

## <a name="create-a-key-vault-secret"></a>Tworzenie magazynu klucz tajny

Poniższy skrypt tworzy certyfikat w formacie pfx, tworzy magazyn kluczy i przechowuje certyfikat w magazynie kluczy jako klucz tajny. Należy użyć `-EnabledForDeployment` parametru podczas tworzenia magazynu kluczy. Tego parametru zapewnia, że magazyn kluczy mogą być przywoływane z szablonów usługi Azure Resource Manager.

```powershell

# Create a certificate in the .pfx format
New-SelfSignedCertificate `
  -certstorelocation cert:\LocalMachine\My `
  -dnsname contoso.microsoft.com

$pwd = ConvertTo-SecureString `
  -String "<Password used to export the certificate>" `
  -Force `
  -AsPlainText

Export-PfxCertificate `
  -cert "cert:\localMachine\my\<Certificate Thumbprint that was created in the previous step>" `
  -FilePath "<Fully qualified path where the exported certificate can be stored>" `
  -Password $pwd

# Create a key vault and upload the certificate into the key vault as a secret
$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "servicecert"
$fileName = "<Fully qualified path where the exported certificate can be stored>"
$certPassword = "<Password used to export the certificate>"

$fileContentBytes = get-content $fileName `
  -Encoding Byte

$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
$jsonObject = @"
{
"data": "$filecontentencoded",
"dataType" :"pfx",
"password": "$certPassword"
}
"@
$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -sku standard `
  -EnabledForDeployment

$secret = ConvertTo-SecureString `
  -String $jsonEncoded `
  -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
   -SecretValue $secret

```

Po uruchomieniu skryptu poprzednie dane wyjściowe zawiera tajny identyfikatora URI. Zanotuj tego identyfikatora URI. Masz do utworzenia odwołania w [certyfikat powiadomień wypychanych do szablonu Menedżera zasobów systemu Windows](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate). Pobierz [szablonu maszyny wirtualnej wypychania certyfikatu — windows](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/201-vm-windows-pushcertificate) folderu na komputerze deweloperskim. Ten folder zawiera `azuredeploy.json` i `azuredeploy.parameters.json` pliki, które będą potrzebne w następnych krokach.

Modyfikowanie `azuredeploy.parameters.json` plików zgodnie z własnymi wartościami środowiska. Parametry szczególnie ważne są nazwę magazynu, grupy zasobów magazynu i klucz tajny identyfikatora URI (jak generowane przez poprzednie skryptu). Przykładem pliku parametrów jest następującego pliku:

## <a name="update-the-azuredeployparametersjson-file"></a>Zaktualizuj plik azuredeploy.parameters.json

Zaktualizuj plik azuredeploy.parameters.json o vaultName, tajne URI VmName i inne wartości zgodnie z harmonogramem środowiska. Następujący plik JSON przedstawiono przykład pliku parametrów szablonu: 

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "newStorageAccountName": {
      "value": "kvstorage01"
    },
    "vmName": {
      "value": "VM1"
    },
    "vmSize": {
      "value": "Standard_D1_v2"
    },
    "adminUserName": {
      "value": "demouser"
    },
    "adminPassword": {
      "value": "demouser@123"
    },
    "vaultName": {
      "value": "contosovault"
    },
    "vaultResourceGroup": {
      "value": "contosovaultrg"
    },
    "secretUrlWithVersion": {
      "value": "https://testkv001.vault.local.azurestack.external/secrets/testcert002/82afeeb84f4442329ce06593502e7840"
    }
  }
}
```

## <a name="deploy-the-template"></a>Wdrożenie szablonu

Teraz można wdrożyć szablon przy użyciu następujący skrypt programu PowerShell:

```powershell
# Deploy a Resource Manager template to create a VM and push the secret onto it
New-AzureRmResourceGroupDeployment `
  -Name KVDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Jeśli szablon został wdrożony pomyślnie, wynikiem następujące dane wyjściowe:

![Dane wyjściowe wdrażanie](media/azure-stack-kv-push-secret-into-vm/deployment-output.png)

Po wdrożeniu tej maszyny wirtualnej Azure stosu wypycha certyfikat na maszynie wirtualnej. W systemie Windows certyfikat jest dodawany do LocalMachine Lokalizacja certyfikatu z magazynu certyfikatów, który użytkownik podał. W systemie Linux, certyfikat zostanie umieszczony w katalogu /var/lib/waagent z nazwą pliku &lt;UppercaseThumbprint&gt;.crt dla X509 plik certyfikatu i &lt;UppercaseThumbprint&gt;.prv klucza prywatnego .

## <a name="retire-certificates"></a>Wycofaj certyfikaty

W poprzedniej sekcji możemy wyświetlał jak push nowego certyfikatu na maszynie wirtualnej. Stary certyfikat jest nadal na maszynie wirtualnej, a nie można usunąć. Jednak można wyłączyć za pomocą starszej wersji klucza tajnego `Set-AzureKeyVaultSecretAttribute` polecenia cmdlet. Poniżej przedstawiono przykład użycia tego polecenia cmdlet. Upewnij się zastąpić nazwę magazynu, nazwa klucza tajnego i wartości wersji zgodnie ze środowiska:

```powershell
Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0
```

## <a name="next-steps"></a>Następne kroki

* [Wdrażanie maszyny wirtualnej z hasłem usługi Key Vault](azure-stack-kv-deploy-vm-with-secret.md)
* [Zezwalaj aplikacji na dostęp do magazynu kluczy](azure-stack-kv-sample-app.md)


