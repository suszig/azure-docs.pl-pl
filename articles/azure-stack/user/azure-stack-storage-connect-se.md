---
title: "Połącz z subskrypcją platformy Azure stosu Eksploratora usługi Storage"
description: "Dowiedz się, jak połączyć Exporer magazynu z subskrypcją platformy Azure stosu"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: xiaofmao
ms.openlocfilehash: bad4b6b0d829fb68dc25e84406a453071a36476a
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription"></a>Połącz z subskrypcją platformy Azure stosu Eksploratora usługi Storage

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Eksplorator usługi Azure Storage (wersja zapoznawcza) jest aplikacją autonomiczną, która pozwala łatwo pracować z danymi Azure stos magazynu w systemie Windows, macOS i Linux. Istnieje kilka narzędzi tym dostępną do przenoszenia danych do i z usługi Azure Storage stosu. Aby uzyskać więcej informacji, zobacz [narzędzia do magazynu Azure stosu transferu danych](azure-stack-storage-transfer.md).

W tym artykule Dowiedz się jak nawiązać połączenie za pomocą Eksploratora usługi Storage Azure stosu kont magazynu. 

Jeśli nie został jeszcze zainstalowany Eksploratora usługi Storage [Pobierz](http://www.storageexplorer.com/) i i zainstaluj go.

Po nawiązaniu połączenia z subskrypcją Azure stosu, można użyć [artykuły Eksploratora usługi Storage Azure](../../vs-azure-tools-storage-manage-with-storage-explorer.md) do pracy z danymi stosu Azure. 

## <a name="prepare-an-azure-stack-subscription"></a>Przygotowanie subskrypcji platformy Azure stosu

Potrzebny jest dostęp do pulpitu na komputerze hosta Azure stosu lub połączenia sieci VPN dla subskrypcji Azure stosu dostępu do Eksploratora usługi Storage. Aby dowiedzieć się, jak skonfigurować połączenie VPN do usługi Azure Stack, zapoznaj się z tematem [Connect to Azure Stack with VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) (Łączenie z usługą Azure Stack przy użyciu sieci VPN).

Azure stosu Development Kit należy wyeksportować certyfikat główny urzędu stosu Azure.

### <a name="to-export-and-then-import-the-azure-stack-certificate"></a>Do wyeksportowania, a następnie zaimportuj certyfikat Azure stosu

1. Otwórz `mmc.exe` na stos Azure komputer hosta lub komputera lokalnego z połączenia sieci VPN Azure stosu. 

2. W **pliku**, wybierz pozycję **Dodaj/Usuń przystawkę**, a następnie dodaj **certyfikaty** do zarządzania **Moje konto użytkownika**.



3. W obszarze **\Trusted Root Certification Authorities\Certificates Root\Certificated konsoli (komputer lokalny)** znaleźć **AzureStackSelfSignedRootCert**.

    ![Ładowanie certyfikatu głównego usługi Azure Stack za pośrednictwem pliku mmc.exe][25]

4. Kliknij prawym przyciskiem myszy certyfikat, wybierz **wszystkie zadania** > **wyeksportować**, a następnie postępuj zgodnie z instrukcjami, aby wyeksportować certyfikat z **certyfikat x.509 szyfrowany algorytmem Base-64 (. CER)**.  

    Wyeksportowany certyfikat będzie używany w następnym kroku.
5. Uruchom Eksploratora usługi Storage (wersja zapoznawcza) i jeśli zostanie wyświetlony **nawiązywanie połączenia z usługą Azure Storage** okna dialogowego pozycję Anuluj ją.

6. Na **Edytuj** menu wskaż **certyfikaty SSL**, a następnie kliknij przycisk **importu certyfikatów**. Użyj okna dialogowego selektora plików, aby wyszukać i otworzyć certyfikat wyeksportowany w poprzednim kroku.

    Po zaimportowaniu zobaczysz monit o ponowne uruchomienie programu Storage Explorer.

    ![Importowanie certyfikatu do programu Storage Explorer (wersja zapoznawcza)][27]

Teraz można przystąpić do Eksploratora usługi Storage nawiązać połączenia z subskrypcją platformy Azure stosu.

### <a name="to-connect-an-azure-stack-subscription"></a>Aby połączyć subskrypcję Azure stosu


1. Po ponownym uruchomieniu programu Storage Explorer (wersja zapoznawcza) wybierz menu **Edytuj** i upewnij się, że pozycja **Oznacz usługę Azure Stack jako cel** została zaznaczona. W przeciwnym razie zaznacz ją i ponownie uruchom program Storage Explorer, aby zastosować zmiany. Ta konfiguracja jest wymagana w celu zachowania zgodności ze środowiskiem usługi Azure Stack.

    ![Sprawdzanie, czy pozycja Oznacz usługę Azure Stack jako cel została zaznaczona][28]

7. W lewym okienku wybierz pozycję **Zarządzaj kontami**.  
    Wyświetlane są wszystkie konta Microsoft, do których się zalogowano.

8. Aby połączyć się z kontem usługi Azure Stack, wybierz pozycję **Dodaj konto**.

    ![Dodawanie konta usługi Azure Stack][29]

9. W **nawiązywanie połączenia z usługą Azure Storage** okna dialogowego, w obszarze **środowiska platformy Azure**, wybierz pozycję **obciążonym środowisku stosu Azure**, a następnie kliknij przycisk **dalej**.

10. Aby zalogować się przy użyciu konta stosu Azure, na którym jest skojarzony z co najmniej jedną aktywną subskrypcją Azure stosu, wypełnij **Zaloguj się do środowiska stosu Azure** okno dialogowe.  

    Szczegółowe informacje dotyczące każdego pola:

    * **Nazwa środowiska**: pole z możliwością dostosowywania przez użytkownika.
    * **Punkt końcowy zasobu usługi ARM**: przykłady punktów końcowych zasobów usługi Azure Resource Manager:

        * Dla operatora chmury:<br> https://adminmanagement.local.azurestack.external   
        * Dla dzierżawcy:<br> https://management.local.azurestack.external
 
    * **Identyfikator dzierżawy**: opcjonalne. Zawiera ono wartość tylko wtedy, gdy należy określić katalog.

12. Po pomyślnym zalogowaniu się przy użyciu konta usługi Azure Stack w okienku po lewej stronie zostaną wyświetlone wszystkie subskrypcje usługi Azure Stack skojarzone z tym kontem. Wybierz subskrypcje usługi Azure Stack, z którymi chcesz pracować, a następnie wybierz przycisk **Zastosuj**. (Zaznaczenie lub usunięcie zaznaczenia pola wyboru **Wszystkie subskrypcje** powoduje przełączenie między wyświetlaniem wszystkich lub żadnej z wymienionych subskrypcji usługi Azure Stack).

    ![Wybieranie subskrypcji usługi Azure Stack po wypełnieniu pól okna dialogowego Środowisko niestandardowe chmury][30]  
    W okienku po lewej stronie są wyświetlane wszystkie konta magazynu skojarzone z wybranymi subskrypcjami usługi Azure Stack.

    ![Lista kont magazynu, w tym kont subskrypcji usługi Azure Stack][31]

## <a name="next-steps"></a>Kolejne kroki
* [Wprowadzenie do Eksploratora usługi Storage (wersja zapoznawcza)](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Magazynu Azure stosu: różnice i zagadnienia dotyczące](azure-stack-acs-differences.md)


* Aby dowiedzieć się więcej na temat usługi Azure Storage, zobacz [wprowadzenie do usługi Magazyn Microsoft Azure](../../storage/common/storage-introduction.md)

[25]: ./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png
[26]: ./media/azure-stack-storage-connect-se/export-root-cert-azure-stack.png
[27]: ./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png
[28]: ./media/azure-stack-storage-connect-se/select-target-azure-stack.png
[29]: ./media/azure-stack-storage-connect-se/add-azure-stack-account.png
[30]: ./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png
[31]: ./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png
