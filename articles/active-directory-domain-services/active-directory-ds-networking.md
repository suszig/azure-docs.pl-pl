---
title: "Usługi domenowe Azure AD: Wskazówki dotyczące sieci | Dokumentacja firmy Microsoft"
description: "Zagadnienia dotyczące sieci dla usług domenowych Azure Active Directory"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2017
ms.author: maheshu
ms.openlocfilehash: 5f9236c5cf660be00db6e09d61df617b64d978e9
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2017
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Zagadnienia dotyczące sieci dla usług domenowych Azure AD
## <a name="how-to-select-an-azure-virtual-network"></a>Jak wybrać sieć wirtualną platformy Azure
Poniższe wskazówki ułatwiają wybieranie sieci wirtualnej do korzystania z usług domenowych Azure AD.

### <a name="type-of-azure-virtual-network"></a>Typ sieci wirtualnej platformy Azure
* **Sieci wirtualne usługi Resource Manager**: można włączyć usługi domenowe Azure AD w sieciach wirtualnych utworzonych przy użyciu usługi Azure Resource Manager.
* Nie można włączyć usługi domenowe Azure AD w klasycznej sieci wirtualnej platformy Azure.
* Można połączyć innych sieci wirtualnych do sieci wirtualnej, w której włączono usługi domenowe Azure AD. Aby uzyskać więcej informacji, zobacz [połączeń sieciowych](active-directory-ds-networking.md#network-connectivity) sekcji.
* **Regionalne wirtualne sieci**: Jeśli planujesz użyć istniejącej sieci wirtualnej, upewnij się, że jest regionalną sieć wirtualną.

  * Sieci wirtualnych korzystających ze starszego mechanizmu grup koligacji nie można używać z Usługami domenowymi Azure AD.
  * Aby korzystać z usług domenowych Azure AD [migracja starszych sieci wirtualnych do regionalnych sieci wirtualnych](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

### <a name="azure-region-for-the-virtual-network"></a>Region platformy Azure dla sieci wirtualnej
* Usługi domenowe Azure AD domeny zarządzanej jest wdrażana w tym samym regionie Azure, co sieć wirtualna wybierzesz opcję Włącz usługę w.
* Wybierz sieć wirtualną w regionie platformy Azure obsługiwane przez usługi domenowe Azure AD.
* Ze strony zawierającej [usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/) dowiesz się, w których regionach platformy Azure jest dostępna usługa Azure AD Domain Services.

### <a name="requirements-for-the-virtual-network"></a>Wymagania dotyczące sieci wirtualnej
* **Zbliżeniowe obciążeniom Azure**: Wybierz sieć wirtualną, która obecnie hostuje/będzie hostować maszyny wirtualne, które wymagają dostępu do usług domenowych Azure AD. Można także połączyć sieci wirtualnych obciążeń w przypadku wdrożenia w innej sieci wirtualnej niż domeny zarządzanej.
* **Niestandardowe/bring your own serwerów DNS**: Sprawdź, czy żadnych niestandardowych serwerów DNS, które są skonfigurowane dla sieci wirtualnej. Przykład niestandardowy serwer DNS jest wystąpieniem DNS systemu Windows Server uruchomiony na maszynę Wirtualną serwera systemu Windows, która została wdrożona w sieci wirtualnej. Usługi domenowe Azure AD nie obsługują żadnych niestandardowych serwerów DNS, które są wdrożone w ramach sieci wirtualnej.
* **Domen z tą samą nazwą domeny**: Upewnij się, że nie masz istniejącej domeny o takiej samej nazwie domeny, dostępne w tej sieci wirtualnej. Na przykład załóżmy, że masz domenę o nazwie „contoso.com” już dostępną w wybranej sieci wirtualnej. Później spróbuj włączyć domeny zarządzanej usług domenowych Azure AD z tą samą nazwą domeny (który jest "contoso.com") w tej sieci wirtualnej. Wystąpi awaria podczas próby włączenia usług domenowych Azure AD. Ten błąd jest spowodowany konfliktów nazw dla nazwy domeny w sieci wirtualnej. W takiej sytuacji musisz użyć innej nazwy podczas konfigurowania domeny zarządzanej Usług domenowych Azure AD. Możesz również anulować aprowizację istniejącej domeny i kontynuować włączanie Usług domenowych Azure AD.

> [!WARNING]
> Nie można przenieść usług domenowych do innej sieci wirtualnej, po włączeniu usługi.
>
>

## <a name="network-security-groups-and-subnet-design"></a>Grupy zabezpieczeń sieci i podsieci
A [grupy zabezpieczeń sieci (NSG)](../virtual-network/virtual-networks-nsg.md) zawiera listę reguł listy kontroli dostępu (ACL), które akceptować lub odrzucać ruch sieciowy do wystąpień maszyn wirtualnych w sieci wirtualnej. Grupy NSG można kojarzyć z podsieciami lub poszczególnymi wystąpieniami maszyn wirtualnych w danej podsieci. Gdy sieciowa grupa zabezpieczeń jest skojarzona z podsiecią, reguły listy ACL dotyczą wszystkich wystąpień maszyn wirtualnych w tej podsieci. Ponadto ruch do poszczególnych maszyn wirtualnych można ograniczyć jeszcze bardziej przez skojarzenie grupy NSG bezpośrednio z tą maszyną Wirtualną.

![Zalecane podsieci](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

### <a name="guidelines-for-choosing-a-subnet"></a>Wskazówki dotyczące wybierania podsieci
* Wdrażanie usług domenowych Azure AD do **oddzielnych dedykowanych podsieci** w ramach sieci wirtualnej platformy Azure.
* Nie dotyczą grup NSG podsieci dedykowane dla domeny zarządzanej. Jeśli należy zastosować grupy NSG do podsieci dedykowanych, upewnij się, możesz **nie blokować porty wymagane do usługi i zarządzać domenę**.
* Nie ograniczaj zbyt liczba adresów IP dostępne w ramach dedykowanej podsieci dla domeny zarządzanej. To ograniczenie uniemożliwia udostępnianie dwa kontrolery domeny do domeny zarządzanej usługi.
* **Usługi domenowe Azure AD w podsieci bramy nie należy włączać** z sieci wirtualnej.

> [!WARNING]
> Po skojarzeniu grupy NSG z podsiecią, w którym usługi domenowe Azure AD jest włączona, może zakłócić zdolność firmy Microsoft do usługi i Zarządzanie domeną. Ponadto synchronizacja dzierżawy usługi Azure AD i domeny zarządzanej jest zakłócona. **Umowa SLA nie ma zastosowania do wdrożeń, w którym grupy NSG zastosowano blokujący usług domenowych Azure AD z aktualizacji i Zarządzanie domeną.**
>
>

### <a name="ports-required-for-azure-ad-domain-services"></a>Porty wymagane przez usługi domenowe Azure AD
Następujące porty są wymagane dla usług domenowych Azure AD do usługi i obsługa domeny zarządzanej. Upewnij się, że te porty nie są blokowane dla podsieci, w którym włączono domeny zarządzanej.

| Numer portu | Przeznaczenie |
| --- | --- |
| 443 |Synchronizacja z dzierżawy usługi Azure AD |
| 3389 |Zarządzanie domeny |
| 5986 |Zarządzanie domeny |
| 636 |Bezpieczny dostęp protokołu LDAP (LDAPS) do domeny zarządzanej |

Portu 5986 służy do wykonywania zadań zarządzania w domenie zarządzanej przy użyciu komunikacji zdalnej programu PowerShell. Kontrolery domeny dla domeny zarządzanej nie Nasłuchuj zwykle na tym porcie. Tylko wtedy, gdy operacja zarządzania lub konserwacji musi zostać wykonana dla domeny zarządzanej usługi zostanie otwarty ten port na zarządzanych kontrolerach domeny. Zaraz po zakończeniu operacji, Usługa zamyka tego portu, na kontrolerach domeny zarządzanej.

Port 3389 jest używany do obsługi połączeń pulpitu zdalnego do domeny zarządzanej. Również portu pozostanie wyłączony przede wszystkim na domeny zarządzanej. Usługa ta umożliwia tego portu, tylko wtedy, gdy należy nawiązać domeny zarządzanej w celu rozwiązywania problemów w odpowiedzi na żądanie obsługi, który został zainicjowany. Ten mechanizm nie jest używany w sposób ciągły, ponieważ do zarządzania i monitorowania zadań są wykonywane przy użyciu komunikacji zdalnej programu PowerShell. Port ten jest używany tylko w rzadkich, czego potrzebujemy, aby nawiązać połączenie zdalne domeny zarządzanej do zaawansowanego rozwiązywania problemów. Port jest zamknięty, natychmiast po zakończeniu operacji rozwiązywania problemów.


### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Przykład grupy NSG dla sieci wirtualnych z usług domenowych Azure AD
W poniższej tabeli przedstawiono przykład grupy NSG można skonfigurować sieć wirtualną z domeny zarządzanej usług domenowych Azure AD. Ta zasada umożliwia ruch przychodzący przez wymagane porty, aby upewnić się, Twoje pozostaje domeny zarządzanej poprawkami, aktualizacji i mogą być monitorowane przez firmę Microsoft. Domyślna reguła "DenyAll" dotyczy cały ruch przychodzący z Internetu.

Ponadto grupa NSG również ilustruje sposób zablokowanie bezpiecznego dostępu LDAP w Internecie. Pomiń tę regułę, jeśli nie włączono bezpiecznego dostępu LDAP do domeny zarządzanej za pośrednictwem Internetu. Grupa NSG zawiera zestaw reguł, które zezwolić na przychodzący LDAPS dostęp za pośrednictwem portu TCP 636 tylko z określonego zestawu adresów IP. Reguły NSG umożliwiają LDAPS dostęp przez internet z określonych adresów IP ma wyższy priorytet niż reguły DenyAll NSG.

![Przykład grupy NSG bezpieczny dostęp do LDAPS w Internecie](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Więcej informacji** - [Utwórz grupę zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Połączenie sieciowe
Domeny zarządzanej usług domenowych Azure AD można włączyć tylko w obrębie jednej sieci wirtualnych na platformie Azure.

### <a name="scenarios-for-connecting-azure-networks"></a>Scenariusze dotyczące łączenia sieci platformy Azure
Połączenie sieci wirtualnych platformy Azure, aby użyć domeny zarządzanej w jednym z następujących scenariuszy wdrażania:

#### <a name="use-the-managed-domain-in-more-than-one-azure-virtual-network"></a>Użyj domeny zarządzanej w więcej niż jedną sieć wirtualną platformy Azure
Sieć wirtualna platformy Azure, w której włączono usługi domenowe Azure AD umożliwia nawiązywanie innych sieci wirtualnych platformy Azure. Połączenia sieci VPN/wirtualnej komunikacji równorzędnej umożliwia domeny zarządzanej za pomocą obciążeń wdrożonych w innych sieciach wirtualnych.

![Łączność klasycznej sieci wirtualnej](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Użyj domeny zarządzanej w sieci wirtualnej Resource Manager
Azure classic sieci wirtualnej, w której włączono usługi domenowe Azure AD umożliwia nawiązywanie sieci wirtualnych Menedżera zasobów. To połączenie umożliwia domeny zarządzanej za pomocą obciążeń wdrożonych w sieci wirtualnej na podstawie Menedżera zasobów.

![Menedżer zasobów z łącznością klasycznej sieci wirtualnej](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Opcje połączeń sieciowych
* **Aby wirtualnymi połączenia wirtualnej sieci komunikacji równorzędnej**: sieci wirtualnej komunikacji równorzędnej jest mechanizm, który łączy dwie sieci wirtualne w tym samym regionie za pośrednictwem sieci Azure sieci szkieletowej. Po połączeniu za pomocą komunikacji równorzędnej dwie sieci wirtualne są traktowane jako jedna do wszystkich celów związanych z łącznością. Są one nadal zarządzane jako oddzielne zasoby, ale maszyny wirtualne w tych sieciach wirtualnych mogą komunikować się bezpośrednio przy użyciu prywatnych adresów IP.

    ![Łączność sieci wirtualnej za pomocą komunikacji równorzędnej](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Więcej informacji - wirtualnych sieci komunikacji równorzędnej](../virtual-network/virtual-network-peering-overview.md)
    
* **Wirtualnymi do połączeń przy użyciu połączeń VPN lokacja lokacja**: łączenie sieci wirtualnej do innej sieci wirtualnej (aby wirtualnymi) jest podobny do łączenia sieci wirtualnej do lokalizacji lokalnej. Oba typy połączeń wykorzystują bramę sieci VPN, aby zapewnić bezpieczny tunel z użyciem protokołu IPsec/IKE.

    ![Łączność sieci wirtualnej przy użyciu bramy sieci VPN](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Więcej informacji - połączyć sieci wirtualnych za pomocą bramy sieci VPN](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

<br>

## <a name="related-content"></a>Powiązana zawartość
* [Komunikacja równorzędna sieci wirtualnej platformy Azure](../virtual-network/virtual-network-peering-overview.md)
* [Konfigurowanie połączenia do wirtualnymi klasycznym modelu wdrażania](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Grupy zabezpieczeń sieci platformy Azure](../virtual-network/virtual-networks-nsg.md)
* [Utwórz grupę zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
