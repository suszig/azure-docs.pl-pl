---
title: "Włączenia kopii zapasowej Azure stosu z portalu administracyjnego | Dokumentacja firmy Microsoft"
description: "Infrastruktura usługi kopii zapasowej za pośrednictwem portalu administracyjnego należy włączyć, dzięki czemu stosu Azure można przywrócić po awarii."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: naS
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 456a0db9771f5963c8d4375d54a22257f6ca1c56
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Włączenia kopii zapasowej Azure stosu z portalu administracyjnego

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Infrastruktura usługi kopii zapasowej za pośrednictwem portalu administracyjnego należy włączyć, dzięki czemu stosu Azure mogą generować kopii zapasowych. Te kopie zapasowe można użyć do przywrócenia środowiska w przypadku awarii.

> [!Note]  
> Przed włączeniem kopii zapasowej za pomocą konsoli, należy skonfigurować usługi tworzenia kopii zapasowej. Można skonfigurować usługi tworzenia kopii zapasowej przy użyciu programu PowerShell. Aby uzyskać więcej informacji, zobacz [włączenia kopii zapasowej Azure stosu przy użyciu programu PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="enable-backup"></a>Włączenia kopii zapasowej

1. Otwieranie portalu administracyjnego platformy Azure stosu w [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Wybierz **więcej usług** > **infrastruktura kopii zapasowej**. Wybierz **konfiguracji** w **infrastruktura kopii zapasowej** bloku.

    ![Azure stosu — ustawienia kontrolera kopii zapasowej](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Wpisz ścieżkę do **lokalizacja magazynu kopii zapasowej**. Użyj ciągu konwencji UNC (Universal Naming) dla ścieżki do udziału plików hostowane na osobnych urządzenia. Ciąg UNC Określa lokalizację zasobów, takich jak udostępnione pliki lub urządzeń. Dla usługi używając adresu IP. Aby zapewnić dostępność kopii zapasowej danych po awarii, urządzenie powinno być w innej lokalizacji.
    > [!Note]  
    > Jeśli środowisko obsługuje rozpoznawania nazw z siecią infrastruktury Azure stosu w środowisku przedsiębiorstwa, można użyć nazwy FQDN, a nie adres IP.
4. Typ **Username** przy użyciu domena i nazwa użytkownika. Na przykład `Contoso\administrator`.
5. Typ **hasło** dla użytkownika.
5. Wpisz hasło ponownie **Potwierdź hasło**.
6. Podaj klucz wstępny w **klucza szyfrowania** pole. Pliki kopii zapasowej są szyfrowane za pomocą tego klucza. Upewnij się, że przechowywanie tego klucza w bezpiecznej lokalizacji. Po ustawić ten klucz po raz pierwszy lub Obróć klucz w przyszłości, nie można wyświetlić tego klucza z tego interfejsu. Więcej instrukcji do wygenerowania klucza wstępnego, wykonaj skrypty w [włączenia kopii zapasowej Azure stosu przy użyciu programu PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Wybierz **OK** Aby zapisać ustawienia kopii zapasowej kontrolera.

Do wykonania kopii zapasowej, należy pobrać narzędzia stosu Azure, a następnie uruchom polecenie cmdlet programu PowerShell **Start AzSBackup** w węźle Administracja stosu Azure. Aby uzyskać więcej informacji, zobacz [kopia zapasowa Azure stosu](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Kolejne kroki

 - Dowiedz się wykonać kopię zapasową. Zobacz [kopia zapasowa Azure stosu](azure-stack-backup-back-up-azure-stack.md ).
- Dowiedz się zweryfikować kopii zapasowej. Zobacz [Potwierdź kopii zapasowej zostało ukończone w portalu administracyjnym](azure-stack-backup-back-up-azure-stack.md ).
