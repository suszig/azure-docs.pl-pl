---
title: Zasoby w automatyzacji Azure certyfikatu | Dokumentacja firmy Microsoft
description: "Certyfikaty można bezpiecznie przechowywane w usłudze Automatyzacja Azure, są one dostępne przez elementy runbook lub konfiguracji DSC do uwierzytelniania Azure i innych zasobów.  W tym artykule szczegółowo opisano certyfikaty i sposobu pracy z nimi w tworzeniu zarówno tekstową i graficznego."
services: automation
documentationcenter: 
author: georgewallace
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2017
ms.author: magoedte;bwren
ms.openlocfilehash: b6a5ff4fa3fd0084fd910968651c6ae0fefaf2cf
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="certificate-assets-in-azure-automation"></a>Zasoby certyfikatu usługi Automatyzacja Azure

Certyfikaty mogą być bezpiecznie przechowywane w automatyzacji Azure, są one dostępne przez elementy runbook lub przy użyciu konfiguracji DSC **Get AzureRmAutomationCertificate** działania dla zasobów usługi Azure Resource Manager. Służy do tworzenia elementów runbook i konfiguracji DSC, które korzystają z certyfikatów do uwierzytelniania, i dodaje je do platformy Azure lub innych zasobów.

> [!NOTE] 
> Bezpiecznych zasobów w automatyzacji Azure obejmują poświadczeń, certyfikatów, połączeń i szyfrowane zmienne. Te zasoby są szyfrowane i przechowywane w automatyzacji Azure za pomocą Unikatowy klucz, który jest generowany dla każdego konta automatyzacji. Ten klucz jest zaszyfrowany za pomocą certyfikatu głównego i przechowywane w automatyzacji Azure. Przed zapisaniem zabezpieczonym zasobem, klucza dla konta automatyzacji zostaną odszyfrowane przy użyciu certyfikatu głównego, a następnie używany do szyfrowania elementu zawartości.
> 

## <a name="windows-powershell-cmdlets"></a>Polecenia cmdlet programu Windows PowerShell

Polecenia cmdlet w poniższej tabeli służą do tworzenia i zarządzania zasobami certyfikatu usługi Automatyzacja przy użyciu programu Windows PowerShell. One dostarczane jako część [modułu Azure PowerShell](../powershell-install-configure.md) która jest dostępna na potrzeby automatyzacji elementów runbook i konfiguracji DSC.

|Polecenia cmdlet|Opis|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcertificate?view=azurermps-4.3.1)|Pobiera informacje o certyfikat do użycia w element runbook lub konfiguracji DSC. Sam certyfikat można pobierać tylko z działania Get AutomationCertificate.|
|[Nowe AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcertificate?view=azurermps-4.3.1)|Tworzy nowy certyfikat w automatyzacji Azure.|
[Usuń AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcertificate?view=azurermps-4.3.1)|Usuwa certyfikat usługi Automatyzacja Azure.|Tworzy nowy certyfikat w automatyzacji Azure.
|[Zestaw AzureRmAutomationCertificate](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcertificate?view=azurermps-4.3.1)|Ustawia właściwości istniejącego certyfikatu, włącznie z przekazywaniem pliku certyfikatu i ustawianiem hasła dla pliku pfx.|
|[Dodaj AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Przekazywanie certyfikatu usługi dla usługi określonej chmury.|


## <a name="python2-functions"></a>Funkcje Python2

Uzyskano dostęp certyfikaty w elemencie runbook Python2 funkcji w poniższej tabeli.

| Funkcja | Opis |
|:---|:---|
| automationassets.get_automation_certificate | Pobiera informacje o zawartości certyfikatu. |

> [!NOTE]
> Należy zaimportować **automationassets** modułu na początku elementu runbook z języka Python, aby uzyskać dostęp do funkcji zasobów.


## <a name="creating-a-new-certificate"></a>Tworzenie nowego certyfikatu

Podczas tworzenia nowego certyfikatu, możesz przekazać plik cer lub PFX do automatyzacji Azure. Jeśli zostanie oznaczony jako możliwy do wyeksportowania certyfikatu, następnie można przenieść go z magazynu certyfikatów usługi Automatyzacja Azure. Jeśli nie jest możliwy do wyeksportowania, następnie można można używać tylko do podpisywania w ramach elementu runbook lub konfiguracji DSC.


### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Aby utworzyć nowy certyfikat za pomocą portalu Azure

1. Twoje konto usługi Automatyzacja kliknij **zasoby** Kafelek, aby otworzyć **zasoby** bloku.
1. Kliknij przycisk **certyfikaty** Kafelek, aby otworzyć **certyfikaty** bloku.
1. Kliknij przycisk **Dodawanie certyfikatu** w górnej części bloku.
2. Wpisz nazwę certyfikatu w **nazwa** pole.
2. Kliknij przycisk **wybierz plik** w obszarze **przekazać plik certyfikatu** do Przeglądaj w poszukiwaniu pliku cer lub pfx.  W przypadku wybrania pliku PFX, określ hasło oraz czy powinno być dozwolone do wyeksportowania.
1. Kliknij przycisk **Utwórz** do zapisywania nowego elementu zawartości certyfikatu.


### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Aby utworzyć nowy certyfikat za pomocą środowiska Windows PowerShell

W poniższym przykładzie pokazano sposób tworzenia nowego certyfikatu usługi Automatyzacja i oznacz ją jako eksportowalny. To importuje istniejący plik pfx.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a>Przy użyciu certyfikatu

Należy użyć **Get AutomationCertificate** działanie do używania certyfikatu. Nie można użyć [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) polecenia cmdlet, ponieważ zwraca informacje o zasób certyfikatu, ale nie sam certyfikat.

### <a name="textual-runbook-sample"></a>Przykład tekstowy

Następujący przykładowy kod przedstawia sposób dodawania certyfikatu do usługi w chmurze w elemencie runbook. W tym przykładzie hasło jest pobierana z zmiennej automatyzacji zaszyfrowane.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Przykładowe graficznym elementem runbook

Możesz dodać **Get-AutomationCertificate** na graficzny element runbook prawym przyciskiem myszy certyfikat w okienku Biblioteka edytora graficznego i wybierając **Dodaj do kanwy**.

![Dodawanie certyfikatu do obszaru roboczego](media/automation-certificates/automation-certificate-add-to-canvas.png)

Na poniższej ilustracji przedstawiono przykład za pomocą certyfikatu w graficznym elementem runbook.  To jest tym samym przykładzie pokazano powyżej dla Dodawanie certyfikatu usługi w chmurze z tekstowy.

![Przykład tworzenia graficznego ](media/automation-certificates/graphical-runbook-add-certificate.png)

### <a name="python2-sample"></a>Przykładowe Python2
Poniższy przykład przedstawia sposób uzyskać dostępu do certyfikatów w elementach runbook Python2.

    # get a reference to the Azure Automation certificate
    cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
    
    # returns the binary cert content  
    print cert 

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o pracy z łącza w celu kontroli przepływu logicznego wynikającego z elementem runbook służy do wykonywania działań, zobacz [łącza w tworzeniu graficznego](automation-graphical-authoring-intro.md#links-and-workflow). 
