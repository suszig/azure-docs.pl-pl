---
title: "Włączenia kopii zapasowej Azure stosu z portalu administracyjnego | Dokumentacja firmy Microsoft"
description: "Infrastruktura wstecz usługi za pośrednictwem portalu administracyjnego należy włączyć, dzięki czemu stosu Azure można przywrócić po awarii."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 56C948E7-4523-43B9-A236-1EF906A0304F
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: a5a9757d871c343ba663862de7b6d75b9dd21c31
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="enable-backup-for-azure-stack-from-the-administration-portal"></a>Włączenia kopii zapasowej Azure stosu z portalu administracyjnego

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Infrastruktura wstecz usługi za pośrednictwem portalu administracyjnego należy włączyć, dzięki czemu stosu Azure mogą generować kopii zapasowych. Te kopie ups przywrócić środowiska w przypadku awarii.

## <a name="enable-backup"></a>Włączenia kopii zapasowej

1. Otwieranie portalu administracyjnego platformy Azure stosu w [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Wybierz **więcej usług** > **infrastruktura kopii zapasowej**. Wybierz **konfiguracji** w **infrastruktura kopii zapasowej** bloku.

    ![Azure stosu — ustawienia kontrolera kopii zapasowej](media\azure-stack-backup\azure-stack-backup-settings.png).

3. Wpisz ścieżkę do **lokalizacja magazynu kopii zapasowej**. Ścieżka udziału plików hostowane na osobnych urządzenia, musisz użyć ciągu Universal Naming Convention (UNC). Ciąg UNC Określa lokalizację zasobów, takich jak udostępnione pliki lub urządzeń. Dla usługi używając adresu IP. Aby zapewnić dostępność kopii zapasowych danych w przypadku awarii, urządzenie powinna być w innej lokalizacji.
    > [!Note]  
    > Jeśli środowisko obsługuje rozpoznawania nazw z siecią infrastruktury Azure stosu w środowisku przedsiębiorstwa można użyć nazwy FQDN, a nie adres IP.
4. Typ **Username** przy użyciu domena i nazwa użytkownika. Na przykład `Contoso\administrator`.
5. Typ **hasło** dla użytkownika.
5. Wpisz hasło ponownie **Potwierdź hasło**.
6. Podaj klucz wstępny w **klucza szyfrowania** pole. Pliki kopii zapasowej są szyfrowane za pomocą tego klucza. Upewnij się, że przechowywanie tego klucza w bezpiecznej lokalizacji. Po ustawić ten klucz po raz pierwszy lub Obróć klucz w przyszłości, nie można wyświetlić tego klucza z tego interfejsu. Więcej instrukcji do wygenerowania klucza wstępnego, wykonaj skrypty w [włączenia kopii zapasowej Azure stosu przy użyciu programu PowerShell](azure-stack-backup-enable-backup-powershell.md#generate-a-new-encryption-key). 
7. Wybierz **OK** Aby zapisać ustawienia kopii zapasowej kontrolera.

Do wykonania kopii zapasowej, należy pobrać narzędzia stosu Azure, a następnie uruchom polecenie cmdlet programu PowerShell **Start AzSBackup** w węźle Administracja stosu Azure. Aby uzyskać więcej informacji, zobacz [kopia zapasowa Azure stosu](azure-stack-backup-back-up-azure-stack.md ).

## <a name="next-steps"></a>Kolejne kroki

 - Dowiedz się, jak uruchomić tworzenia kopii zapasowych, zobacz [kopia zapasowa Azure stosu](azure-stack-backup-back-up-azure-stack.md ).
- Dowiedz się zweryfikować kopii zapasowej, zobacz [Potwierdź kopii zapasowej zostało ukończone w portalu administracyjnym](azure-stack-backup-back-up-azure-stack.md ).
