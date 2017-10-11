---
title: "Przekazywania poświadczeń na platformie Azure przy użyciu usługi Konfiguracja DSC | Dokumentacja firmy Microsoft"
description: "Omówienie w bezpieczny sposób przekazywania poświadczeń do maszyn wirtualnych platformy Azure przy użyciu konfiguracji żądanego stanu programu PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: acd768c0219ec23c0453a65c575faf5213d9c616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Przekazywania poświadczeń do obsługi rozszerzenia usługi Konfiguracja DSC Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

W tym artykule omówiono rozszerzenia konfiguracji żądanego stanu dla platformy Azure. Omówienie obsługi rozszerzenia DSC można znaleźć w folderze [wprowadzenie do obsługi rozszerzenia konfiguracji żądanego stanu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="passing-in-credentials"></a>Przekazywanie poświadczeń
W ramach procesu konfiguracji może skonfigurować konta użytkowników, dostęp do usług, lub zainstalować program w kontekście użytkownika. Aby wykonać te czynności, musisz podać poświadczenia. 

DSC umożliwia sparametryzowane konfiguracji, w których poświadczenia są przekazywane do konfiguracji i bezpiecznie przechowywane w plikach MOF. Obsługi rozszerzenia Azure ułatwia zarządzanie poświadczeniami, zapewniając automatyczne zarządzanie certyfikatami. 

Rozważmy poniższy skrypt konfiguracji DSC tworzy konto użytkownika lokalnego z określonym hasłem:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Należy uwzględnić *localhost węzła* jako część konfiguracji. W przypadku braku tej instrukcji następujące kroki nie działają zgodnie z obsługi rozszerzenia w szczególności szuka instrukcji localhost węzła. Należy również uwzględnić typecast *[PsCredential]*, ponieważ rozszerzenie do szyfrowania poświadczeń wyzwala określonego typu. 

Publikuj ten skrypt do magazynu obiektów blob:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Ustawienia rozszerzenia usługi Konfiguracja DSC Azure i podaj poświadczenia:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Jak są zabezpieczone poświadczeń
Uruchomienie tego kodu wyświetla monit o podanie poświadczeń. Gdy podano, jest on przechowywany w pamięci krótko. Jeśli jest publikowany razem `Set-AzureVmDscExtension` polecenia cmdlet, jego są przesyłane za pośrednictwem protokołu HTTPS do maszyny Wirtualnej, w przypadku, gdy usługa Azure przechowuje go szyfrowane na dysku, przy użyciu lokalnego certyfikatu maszyny Wirtualnej. Następnie jest krótko odszyfrowane w pamięci i ponownie szyfrowane przekazywany do usługi Konfiguracja DSC.

To zachowanie różni się od [za pomocą bezpiecznej konfiguracji bez obsługi rozszerzenia](https://msdn.microsoft.com/powershell/dsc/securemof). Środowisko Azure udostępnia sposób przekazują dane konfiguracji w bezpieczny sposób za pomocą certyfikatów. Korzystając z procedury obsługi rozszerzenia DSC, nie jest konieczne do zapewnienia $CertificatePath lub $CertificateID / $Thumbprint wpis w ConfigurationData.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji dotyczących obsługi rozszerzenia usługi Konfiguracja DSC Azure, zobacz [wprowadzenie do obsługi rozszerzenia konfiguracji żądanego stanu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Sprawdź [szablonu usługi Azure Resource Manager dla rozszerzenia DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Aby uzyskać więcej informacji o konfiguracji DSC środowiska PowerShell [odwiedź Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Aby znaleźć dodatkowe funkcje, którymi można zarządzać w DSC środowiska PowerShell [przeglądać galerii programu PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) więcej zasobów usługi Konfiguracja DSC.

