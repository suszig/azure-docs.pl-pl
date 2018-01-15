---
title: "Obróć kluczy tajnych w stosie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Obróć tajnych w stosie Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 49071044-6767-4041-9EDD-6132295FA551
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: mabrigg
ms.openlocfilehash: 0a4118a8927e4261fafa307af5b9c29623ce5c3f
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Obróć kluczy tajnych w stosie Azure

*Dotyczy: Azure stosu zintegrowane systemy*

Zaktualizuj hasła dla składników Azure stosu w regularnych okresach.

## <a name="updating-the-passwords-for-the-baseboard-management-controller-bmc"></a>Aktualizowanie hasła do kontrolera zarządzania płytą główną (BMC)

Kontrolery zarządzania płytą główną (BMC) monitorować stan fizycznych serwerów. Specyfikacje i instrukcje na temat aktualizowania hasła bmc różnić w zależności od producenta komputera producenta sprzętu (OEM).

1. Zaktualizuj BMC na serwerach fizycznych stosu Azure zgodnie z instrukcjami producenta OEM. Hasło dla każdego kontrolera BMC w danym środowisku muszą być takie same.
2. Otwórz uprzywilejowanych punktu końcowego w sesjach stosu Azure. Aby uzyskać instrukcje, zobacz [przy użyciu punktu końcowego uprzywilejowanych w stosie Azure](azure-stack-privileged-endpoint.md).
3. Po programu PowerShell wiersz został zmieniony na **[adres IP lub wirtualna ERCS name]: PS >** lub **[azs ercs01]: PS >**w zależności od środowiska uruchamiania `Set-BmcPassword` uruchamiając `invoke-command`. Przekaż zmiennej sesji użytkownika uprzywilejowanego punktu końcowego jako parametr.  
Na przykład:
    ```powershell
    $PEPSession = New-PSSession -ComputerName <ERCS computer name> -Credential <CloudAdmin credential> -ConfigurationName "PrivilegedEndpoint"  
    
    Invoke-Command -Session $PEPSession -ScriptBlock {
        param($password)
        set-bmcpassword -bmcpassword $password
    } -ArgumentList (<LatestPassword as a SecureString>) 
    ```

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej o zabezpieczeniach i stosu Azure, zobacz [stan zabezpieczeń infrastruktury Azure stosu](azure-stack-security-foundations.md).