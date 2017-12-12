---
title: "Przy użyciu punktu końcowego uprzywilejowanych w stosie Azure | Dokumentacja firmy Microsoft"
description: "Przedstawia sposób użycia uprzywilejowanego punktu końcowego w stosie Azure (dla operatora stosu Azure)."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 949715317de69064bb66fb470a805e367512bd6f
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Przy użyciu punktu końcowego uprzywilejowanych w stosie Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Jako operator stosu Azure należy użyć portalu administratora programu PowerShell lub interfejsów API usługi Azure Resource Manager dla najbardziej codziennych zadań zarządzania. Jednak w przypadku niektórych mniej typowych operacji, należy użyć *uprzywilejowanych punktu końcowego*. Ten punkt końcowy jest wstępnie skonfigurowane zdalnej konsoli programu PowerShell, która dostarcza wystarczającego możliwości, które ułatwiają wykonywanie wymaganych zadań. Punkt końcowy korzysta z programu PowerShell JEA (tylko tyle administracyjnej) do udostępnienia tylko ograniczony zestaw poleceń cmdlet. Dostępu do uprzywilejowanych punktu końcowego i wywołać ograniczony zestaw poleceń cmdlet, konta z niskimi uprawnieniami jest używana. Żadnych kont administratora są wymagane. Aby dodatkowo zwiększyć bezpieczeństwo skryptów jest niedozwolone.

Uprzywilejowane punktu końcowego służy do wykonywania następujących zadań:

- Aby wykonywać zadania niskiego poziomu, takie jak [zbierania dzienników diagnostycznych](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Do wykonywania wielu zadań integracji datacenter po wdrożeniu zintegrowanych systemów, takich jak dodawanie usług przesyłania dalej systemu nazw domen (DNS) po wdrożeniu konfigurowania integracji wykresu, integracji usługi Active Directory Federation Services (AD FS), certyfikat Obracanie itp.
- Aby pracować z obsługą przy uzyskiwaniu dostępu do tymczasowego, wysokiego poziomu do rozwiązywania problemów szczegółowe zintegrowanego systemu. 

Uprzywilejowane punktu końcowego rejestruje wszystkie akcje (i jej odpowiednie dane wyjściowe), wykonywanych w sesji programu PowerShell. Zapewnia to pełna przezroczystość i inspekcji zakończenie operacji. Można zachować te pliki dziennika na przyszłe potrzeby inspekcji.

> [!NOTE]
> W usłudze Azure stosu Development Kit (ASDK), można uruchomić niektórych poleceń, które są dostępne w punkcie końcowym uprzywilejowanych bezpośrednio z sesji programu PowerShell na hoście development kit. Można przetestować niektóre operacje przy użyciu uprzywilejowanych punktu końcowego, takich jak zbieranie danych dziennika, ponieważ jest jedyną wykonywania pewnych operacji w środowisku zintegrowanych systemów.

## <a name="access-the-privileged-endpoint"></a>Dostęp uprzywilejowany punktu końcowego

Możesz uzyskać dostęp uprzywilejowany punktu końcowego za pomocą sesji zdalnej programu PowerShell na maszynie wirtualnej, uprzywilejowane punkt końcowy. W ASDK tej maszyny wirtualnej ma nazwę AzS ERCS01. Jeśli używasz zintegrowanego systemu, istnieją trzy wystąpienia uprzywilejowanych punktu końcowego, każdy uruchomiony wewnątrz maszyny wirtualnej (*prefiksu*-ERCS01, *prefiksu*-ERCS02, lub *prefiksu*-ERCS03) na różnych hostach odporność na awarie. 

Przed rozpoczęciem tej procedury dla zintegrowany system, upewnij się, że można uzyskiwać dostęp do uprzywilejowanych punkt końcowy, za pomocą adresu IP lub przy użyciu systemu DNS. Po początkowym wdrożeniu stosu Azure uprzywilejowanych punktu końcowego są dostępne tylko za pomocą adresu IP ponieważ integracja protokołu DNS nie jest jeszcze skonfigurowany. Dostawca sprzętu OEM będzie dostarczać plik JSON o nazwie "AzureStackStampDeploymentInfo", zawierający adresy IP uprzywilejowanych punktu końcowego.

Zaleca się, że należy połączyć uprzywilejowanych punktu końcowego tylko od sprzętu hosta cyklu życia lub z dedykowanym, zablokowaną komputera, tak samo, jak [uprzywilejowanego dostępu do stacji roboczej](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. Wykonaj jedną z następujących czynności w zależności od środowiska:

    - Na zintegrowany system uruchom następujące polecenie, aby dodać uprzywilejowanych punktu końcowego jako zaufanego hosta na hoście cyklu życia sprzętu lub uprzywilejowanego dostępu do stacji roboczej.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Jeśli korzystasz z ADSK, zaloguj się do rozwoju hosta zestawu.

2. Na hoście cyklu życia sprzętu lub uprzywilejowanego dostępu do stacji roboczej otwórz sesję środowiska Windows PowerShell z podwyższonym poziomem uprawnień. Uruchom następujące polecenia, aby ustanowić sesję zdalną na maszynie wirtualnej, uprzywilejowane punkt końcowy:
 
    - Na zintegrowany system:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      `ComputerName` Parametr może być adres IP lub nazwę DNS w jednej z maszyn wirtualnych, które obsługuje uprzywilejowanych punktu końcowego. 
    - Jeśli używasz ADSK:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Po wyświetleniu monitu użyj następujących poświadczeń:

      - **Nazwa użytkownika**: Określ konto CloudAdmin w formacie  **&lt;* domeny stosu Azure*&gt;\cloudadmin**. (ASDK, nazwa użytkownika jest **azurestack\cloudadmin**.)
      - **Hasło**: wprowadź to samo hasło, które zostało podane podczas instalacji dla konta administratora domeny AzureStackAdmin.
    
3.  Po nawiązaniu połączenia wiersza zmieni się na  **[*nazwa adresu IP lub wirtualna ERCS*]: PS > ** lub **[azs ercs01]: PS >**, w zależności od środowiska. W tym miejscu, należy uruchomić `Get-Command` Aby wyświetlić listę dostępnych poleceń cmdlet.

    ![Dane wyjściowe polecenia cmdlet Get-Command przedstawiający listę dostępnych poleceń](media/azure-stack-privileged-endpoint/getcommandoutput.png)

    Wiele z tych poleceń cmdlet są przeznaczone tylko dla środowisk zintegrowany system (takich jak polecenia cmdlet związane z integracji centrum danych). W ASDK zostały zatwierdzone następujące polecenia cmdlet:

    - Wyczyść hosta
    - Zamknij PrivilegedEndpoint
    - Zakończ PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Polecenie GET
    - Klasy FormatData Get
    - Get-Help
    - Get-ThirdPartyNotices
    - Obiekt miary
    - Nowe CloudAdminUser
    - Domyślna wyjściowego
    - Usuń CloudAdminUser
    - Select-Object
    - Zestaw CloudAdminUserPassword
    - Stop-AzureStack
    - Get-Clusterlog, zmienna środowiskowa

4.  Nie zezwala na wykonywanie skryptów, dlatego nie można użyć uzupełniania po naciśnięciu tabulatora wartości parametrów. Aby uzyskać listę parametrów dla danego polecenia cmdlet, uruchom następujące polecenie:

    ````PowerShell
    Get-Command <cmdlet_name> -Syntax
    ```` 
    Jeśli spróbujesz dowolnego typu działania skryptu kończy się niepowodzeniem z powodu błędu **ScriptsNotAllowed**. Jest to oczekiwane zachowanie.

## <a name="close-the-privileged-endpoint-session"></a>Zamknij sesję uprzywilejowanych punktu końcowego

 Jak wspomniano wcześniej, uprzywilejowane punktu końcowego rejestruje wszystkie akcje (i jej odpowiednie dane wyjściowe), wykonywanych w sesji programu PowerShell. Należy zamknąć sesję przy użyciu `Close-PrivilegedEndpoint` polecenia cmdlet. To polecenie cmdlet poprawnie zamyka punktu końcowego i przesyła pliki dziennika w udziale plików zewnętrznych dla przechowywania.

Aby zamknąć sesji punktu końcowego:

1. Tworzenie udziału plików zewnętrznego, który jest dostępny dla uprzywilejowanych punktu końcowego. W środowisku development kit można po prostu utworzyć udział plików na hoście development kit.
2. Uruchom `Close-PrivilegedEndpoint` polecenia cmdlet. 
3. Zostanie wyświetlony monit o ścieżkę do przechowywania plików dziennika zapis. Określ udział plików, który został utworzony wcześniej, w formacie &#92; &#92; *servername*&#92; *Nazwa udziału*. Jeśli ścieżka nie jest określony, polecenie cmdlet nie powiedzie się i sesja pozostanie otwarta. 

    ![Dane wyjściowe polecenia cmdlet PrivilegedEndpoint Zamknij pokazujący, gdzie Określ ścieżkę docelową, zapis](media/azure-stack-privileged-endpoint/closeendpoint.png)

Po pomyślnie przesyłania plików dziennika zapis do udziału plików, są one automatycznie usuwane z uprzywilejowanego punktu końcowego. Jeśli zamknięciu sesji uprzywilejowanych punktu końcowego za pomocą polecenia cmdlet `Exit-PSSession` lub `Exit`, lub zamknij konsolę programu PowerShell, dzienniki wykaz nie transferu do udziału plików. Pozostają one w uprzywilejowanych punktu końcowego. Przy następnym uruchomieniu `Close-PrivilegedEndpoint` i obejmują udziału plików, dzienniki zapis z poprzedniej sesji również zostaną przeniesione.

## <a name="next-steps"></a>Następne kroki
[Azure stosu narzędzia diagnostyczne](azure-stack-diagnostics.md)







