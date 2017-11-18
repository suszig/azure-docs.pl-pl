---
title: "Punktów końcowych usługi sieci wirtualnej i zasady usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Oznacz podsieci jako punkt końcowy usługi sieci wirtualnej. Następnie punktu końcowego, co sieć wirtualna reguł do listy ACL bazy danych SQL Azure. Bazy danych SQL można następnie akceptuje komunikację z wszystkich maszyn wirtualnych i inne węzły w podsieci."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: VNet Service endpoints
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 11/13/2017
ms.author: genemi
ms.openlocfilehash: 66dbc9c2c3ba9b9f0c7eb405dbafbd002ce50fbc
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-sql-database"></a>Użyj punktów końcowych usługi sieci wirtualnej i reguł bazy danych SQL Azure

*Zasady sieci wirtualnej* są jedną funkcję zabezpieczeń zapory, która kontroluje, czy serwer bazy danych SQL Azure akceptuje komunikacji, które są wysyłane z określonej podsieci w sieci wirtualnych. W tym artykule opisano, dlaczego funkcja reguły sieci wirtualnej jest czasami najlepszym dotycząca bezpiecznie zezwolenie na komunikację z bazą danych SQL Azure.

Aby utworzyć regułę sieci wirtualnej, najpierw należy [punkt końcowy usługi sieci wirtualnej] [ vm-virtual-network-service-endpoints-overview-649d] odwołanie do reguły.


> [!NOTE]
> Bazy danych SQL Azure ta funkcja jest dostępna w wersji zapoznawczej we wszystkich regionach chmurze publicznej systemu Azure.

#### <a name="how-to-create-a-virtual-network-rule"></a>Tworzenie reguły sieci wirtualnej

Jeśli tworzysz tylko zasady sieci wirtualnych, możesz przejść od razu do kroków i wyjaśnienie [dalszej części tego artykułu](#anchor-how-to-by-using-firewall-portal-59j).






<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologia i opis

**Sieć wirtualna:** może mieć skojarzone z subskrypcją platformy Azure sieci wirtualnych.

**Podsieci:** sieć wirtualna zawiera **podsieci**. Wszelkie Azure maszyn wirtualnych (VM), które masz przypisanych do podsieci. W jednej podsieci może zawierać wiele maszyn wirtualnych lub innych węzłów obliczeniowych. Obliczenia bazy danych węzłów, które znajdują się poza sieci wirtualnej nie może uzyskiwać dostępu do sieci wirtualnej, dopiero po skonfigurowaniu zabezpieczeń, aby zezwolić na dostęp.

**Punkt końcowy usługi sieci wirtualnej:** A [punkt końcowy usługi sieci wirtualnej] [ vm-virtual-network-service-endpoints-overview-649d] podsieć, w których wartości właściwości zawierają jedną lub więcej nazw typu formalnego usługi Azure. W tym artykule Dbamy o nazwę typu **Microsoft.Sql**, które odwołuje się do usługi Azure o nazwie bazy danych SQL.

**Reguła sieci wirtualnej:** podsieć, która znajduje się na liście kontroli dostępu (ACL) serwera bazy danych SQL jest reguła sieci wirtualnej serwera bazy danych SQL. Się na liście ACL dla Twojej bazy danych SQL, musi zawierać podsieci **Microsoft.Sql** nazwy typu.

Reguła sieci wirtualnej informuje serwer bazy danych SQL, aby akceptował komunikację z każdego węzła, który znajduje się w podsieci.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Korzyści wynikające z reguły sieci wirtualnej

Aż do wprowadzenia akcji, maszyn wirtualnych w podsieci nie może komunikować się z bazą danych SQL. Uzasadnienie wyboru podejście reguły sieci wirtualnej w celu umożliwienia komunikacji wymaga dyskusji Porównaj kontrast, obejmujące konkurencyjnych opcji zabezpieczeń oferowanych przez zaporę.

#### <a name="a-allow-access-to-azure-services"></a>A. Zezwalaj na dostęp do usług platformy Azure

W okienku zapory ma **ON/OFF** przycisk o nazwie **zezwolić na dostęp do usług platformy Azure**. **ON** ustawienie umożliwia komunikację z wszystkie adresy IP platformy Azure i wszystkie podsieci platformy Azure. Te adresy IP platformy Azure lub podsieci nie może należeć do przez użytkownika. To **ON** ustawienie jest prawdopodobnie bardziej otwarte niż baza danych SQL jako. Funkcja reguły sieci wirtualnej oferuje znacznie skuteczniejszą kontrolę.

#### <a name="b-ip-rules"></a>B. Reguły IP

Zapory bazy danych SQL pozwala na określanie zakresów adresów IP, z których łączności są akceptowane do bazy danych SQL. Ta metoda jest poprawnie stabilna adresów IP jest spoza sieci prywatnej platformy Azure. Ale wiele węzłów w prywatnej sieci platformy Azure są skonfigurowane przy użyciu *dynamiczne* adresów IP. Dynamiczne adresy IP mogą ulec zmianie, np. podczas ponownego uruchamiania maszyny Wirtualnej. Byłoby folly, aby określić dynamicznego adresu IP w regule zapory w środowisku produkcyjnym.

Opcji IP może być odzyskana, uzyskując *statycznych* adres IP dla maszyny Wirtualnej. Aby uzyskać więcej informacji, zobacz [skonfigurować prywatnych adresów IP dla maszyny wirtualnej przy użyciu portalu Azure][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Jednak ujęciu statycznego adresu IP może być trudne do zarządzania i jest kosztowne, jeśli zostaną wykonane na dużą skalę. Zasady sieci wirtualnej mają ułatwić ustalenie, jak i do zarządzania.

#### <a name="c-cannot-yet-have-sql-database-on-a-subnet"></a>C. Nie można jeszcze bazy danych SQL w podsieci

Jeśli serwer bazy danych SQL Azure była węzła w podsieci w sieci wirtualnej, wszystkie węzły w sieci wirtualnej może komunikować się z bazą danych programu SQL. W takim przypadku maszyn wirtualnych może komunikować się z bazy danych SQL bez konieczności wszystkie reguły sieci wirtualnej lub reguły IP.

Jednak począwszy od września 2017 r. usługi baza danych SQL Azure nie jest jeszcze pomiędzy usługami, które można przypisać do podsieci.






<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Szczegółowe informacje o zasadach sieci wirtualnej

W tej sekcji opisano kilka szczegółowe informacje o regułach sieci wirtualnej.

#### <a name="only-one-geographic-region"></a>Tylko jeden region geograficzny

Każdy punkt końcowy usługi sieci wirtualnej dotyczy tylko jeden region platformy Azure. Punkt końcowy nie obsługuje innych regionów akceptował komunikację z podsieci.

Wszystkie reguły sieci wirtualnej jest ograniczone do regionu, który dotyczy jej podstawowy punkt końcowy.

#### <a name="server-level-not-database-level"></a>Poziom serwera, nie poziom bazy danych

Każda reguła sieci wirtualnej ma zastosowanie do całego serwera bazy danych SQL Azure, nie tylko do jednej konkretnej bazy danych na serwerze. Innymi słowy sieć wirtualną reguła jest stosowana na poziomie serwera, nie na poziomie bazy danych.

- Z kolei IP reguły mogą dotyczyć na tym poziomie.

#### <a name="security-administration-roles"></a>Role zabezpieczeń administracji

Brak separacji ról zabezpieczeń w administrowanie punktów końcowych usługi sieci wirtualnej. Akcja jest wymagany z każdej z następujących ról:

- **Administrator sieci:** &nbsp; włączyć punkt końcowy.
- **Administrator bazy danych:** &nbsp; aktualizowanie listy kontroli dostępu (ACL), aby dodać jednej podsieci na serwerze bazy danych SQL.

*Alternatywa RBAC:* 

Role administratora sieci i administratora bazy danych ma więcej możliwości niż są wymagane do zarządzania zasadami sieci wirtualnej. Wymagany jest tylko podzestaw możliwości ich.

Istnieje możliwość użycia [kontroli dostępu opartej na rolach (RBAC)] [ rbac-what-is-813s] na platformie Azure, aby utworzyć jednej roli niestandardowej, która ma niezbędne podzestaw możliwości. Tworzona rola niestandardowa można użyć zamiast z administratorem sieci lub administratorem bazy danych. Powierzchni z zagrożenie bezpieczeństwa jest mniejszy, jeśli użytkownik zostanie dodany do niestandardowej roli zabezpieczeń, i dodanie użytkownika do dwóch innych ról administratora głównych.






## <a name="limitations"></a>Ograniczenia

Bazy danych SQL Azure funkcja reguł sieć wirtualna ma następujące ograniczenia:

- W chwili obecnej, aplikacji sieci Web platformy Azure w podsieci, która ma **punktów końcowych usługi** włączona ma nie jeszcze funkcja zgodnie z oczekiwaniami. Pracujemy nad włączenie tej funkcji.
    - Dopóki ta funkcja jest w pełni zaimplementowana, zaleca się przenoszenie aplikacji sieci Web do innej podsieci, która nie ma punktów końcowych usługi włączona dla bazy danych SQL.

- W zaporze dla Twojej bazy danych SQL każda reguła sieci wirtualnej odwołuje się do podsieci. Te przywoływanego podsieci muszą być obsługiwane w tym samym regionie geograficznym, który jest hostem bazy danych SQL.

- Każdy serwer bazy danych SQL Azure może mieć maksymalnie 128 wpisy list kontroli dostępu dla dowolnego danej sieci wirtualnej.

- Reguły sieci wirtualnej mają zastosowanie tylko do sieci wirtualnych Azure Resource Manager. i nie [klasycznego modelu wdrażania] [ arm-deployment-model-568f] sieci.

- Włączanie ON punktów końcowych usługi sieci wirtualnej do bazy danych SQL Azure umożliwia również punktów końcowych usługi MySQL i PostGres Azure. Jednak z ON punktów końcowych próby połączenia się z punktów końcowych wystąpień programu MySQL lub Postgres zakończy się niepowodzeniem.
    - Podstawowej przyczyn jest to, że MySQL i PostGres nie obsługują obecnie ACLing.

- W zaporze zakresów adresów IP są stosowane do następujących elementów sieci, ale zasady sieci wirtualnej nie:
    - [Lokacja-lokacja (S2S) wirtualnej sieci prywatnej (VPN)][vpn-gateway-indexmd-608y]
    - Lokalnymi za pośrednictwem [ExpressRoute][expressroute-indexmd-744v]

#### <a name="expressroute"></a>ExpressRoute

Jeśli sieć jest podłączony do sieci Azure przy użyciu [ExpressRoute][expressroute-indexmd-744v], każdy obwód jest skonfigurowany z dwóch publicznych adresów IP w Microsoft Edge. Dwa adresy IP są używane nawiązać Services firmy Microsoft, takich jak do magazynu Azure, korzystając z publicznej komunikacji równorzędnej platformy Azure.

Aby umożliwić komunikację z obwodu bazą danych SQL Azure, należy utworzyć zasady sieci IP dla publicznych adresów IP w sieci obwodów. Aby można było znaleźć publiczne adresy IP obwodu ExpressRoute, otwórz bilet pomocy technicznej z ExpressRoute przy użyciu portalu Azure.


<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->





## <a name="errors-40914-and-40615"></a>Błędy 40914 i 40615

Błąd połączenia 40914 odnosi się do *zasady sieci wirtualnej*, jak określono w okienku zapory w portalu Azure. Błąd 40615 jest podobny, z wyjątkiem odnosi się do *reguły adres IP* na zaporze.

#### <a name="error-40914"></a>Błąd 40914

*Tekst komunikatu:* nie można otworzyć serwera "*[nazwa serwera]*" żądanego podczas logowania. Klient nie może uzyskać dostęp do serwera.

*Opis błędu:* klient znajduje się w podsieci, która ma punkty końcowe serwera sieci wirtualnej. Jednak serwer bazy danych SQL Azure nie ma sieci wirtualnej reguły przyznająca podsieci prawo do komunikacji z bazą danych SQL.

*Błąd podczas rozpoznawania:* na zaporze okienku portalu Azure, użyj do kontrolować zasady sieci wirtualnej [Dodaj regułę sieci wirtualnej](#anchor-how-to-by-using-firewall-portal-59j) podsieci.

#### <a name="error-40615"></a>Błąd 40615

*Tekst komunikatu:* nie można otworzyć serwera "{0}" żądanego podczas logowania. Klient o adresie IP "{1}" nie jest dozwolony dostęp do serwera.

*Opis błędu:* klient próbuje nawiązać połączenie z adresu IP, który nie ma autoryzacji do łączenia się z serwerem bazy danych SQL Azure. Zapora serwera nie ma adresu IP adres reguły umożliwiającą klientów do komunikacji z danego adresu IP do bazy danych SQL.

*Błąd podczas rozpoznawania:* jako reguła IP wprowadź adres IP klienta. W tym celu za pomocą okienka zapory w portalu Azure.


Lista komunikaty o błędach bazy danych SQL jest udokumentowany [tutaj][sql-database-develop-error-messages-419g].





<a name="anchor-how-to-by-using-firewall-portal-59j" />

## <a name="portal-can-create-a-virtual-network-rule"></a>Portal można utworzyć regułę sieci wirtualnej

W tej części przedstawiono, jak używasz [portalu Azure] [ http-azure-portal-link-ref-477t] utworzyć *reguły sieci wirtualnej* w bazie danych SQL Azure. Reguła określa, że baza danych SQL do akceptowania komunikacji od określonej podsieci, które zostały oznaczone jako *punkt końcowy usługi sieci wirtualnej*.

#### <a name="powershell-alternative"></a>Zamiast programu PowerShell

Skrypt programu PowerShell, można również tworzyć reguły sieci wirtualnej. Polecenia cmdlet ważnych **AzureRmSqlServerVirtualNetworkRule nowy**. W przypadku pytań, zobacz [PowerShell, aby utworzyć regułę i punkt końcowy usługi sieci wirtualnej dla bazy danych SQL Azure][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

#### <a name="prerequisites"></a>Wymagania wstępne

Podsieć, która jest oznakowany z określonym punktem końcowym usługi sieci wirtualnej musi mieć już *nazwy typu* istotne dla bazy danych SQL Azure.

- Nazwa typu odpowiedniego punktu końcowego jest **Microsoft.Sql**.
- Jeżeli podsieć nie mogą być oznaczane Nazwa typu, zobacz [Sprawdź podsieć jest punktem końcowym][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200" />

### <a name="azure-portal-steps"></a>Kroki w portalu Azure

1. Zaloguj się do [portalu Azure][http-azure-portal-link-ref-477t].

2. Następnie przejdź do portalu **serwerów SQL** &gt; **zapory / sieci wirtualnej**.

3. Ustaw **zezwolić na dostęp do usług platformy Azure** kontroli OFF.

    > [!IMPORTANT]
    > Pozostawienie formancie ustawiony na wartość, następnie serwer bazy danych SQL Azure akceptuje komunikację z żadnej podsieci, która może być zbyt dostępu z punktu widzenia zabezpieczeń. Funkcja punktu końcowego usługi Microsoft Azure Virtual Network, w połączeniu z funkcją reguły sieci wirtualnej bazy danych SQL, można zmniejszyć razem obszaru powierzchni zabezpieczeń.

4. Kliknij przycisk **+ Dodaj istniejący** kontroli w **sieci wirtualnych** sekcji.

    ![Kliknij przycisk Dodaj istniejący (podsieci punkt końcowy, zgodnie z zasadą SQL).][image-portal-firewall-vnet-add-existing-10-png]

5. W nowym **Create/Update** okienka, wypełnienie w formantach o nazwach zasobów platformy Azure.
 
    > [!TIP]
    > Należy wprowadzić poprawny **prefiks adresu** dla podsieci. Wartość można znaleźć w portalu. Przejdź **wszystkie zasoby** &gt; **wszystkie typy** &gt; **sieci wirtualnych**. Filtr wyświetla sieci wirtualne. Kliknij sieci wirtualnej, a następnie kliknij przycisk **podsieci**. **Zakres adresów** kolumna zawiera prefiks adresu należy.

    ![Wypełnij pola dla nowej reguły.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Kliknij przycisk **OK** znajdujący się u dołu okienka.

7.  W okienku zapory, zobacz wynikowa zasada sieci wirtualnej.

    ![Zobacz nowej reguły, w okienku zapory.][image-portal-firewall-vnet-result-rule-30-png]






<a name="anchor-how-to-links-60h" />

## <a name="related-articles"></a>Pokrewne artykuły:

- [Utwórz punkt końcowy usługi sieci wirtualnej, a następnie reguły sieci wirtualnej dla bazy danych SQL Azure przy użyciu programu PowerShell][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Punkty końcowe usługi sieci wirtualnej platformy Azure][vm-virtual-network-service-endpoints-overview-649d]
- [Reguły zapory poziomu serwera i bazy danych na poziomie bazy danych SQL Azure][sql-db-firewall-rules-config-715d]

Funkcja punktów końcowych usługi Microsoft Azure Virtual Network i reguły sieci wirtualnej funkcji bazy danych SQL Azure, zarówno stały się dostępne w późne września 2017 r.





<!-- Link references, to images. -->

[image-portal-firewall-vnet-add-existing-10-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png

[image-portal-firewall-create-update-vnet-rule-20-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png

[image-portal-firewall-vnet-result-rule-30-png]: media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png



<!-- Link references, to text, Within this same Github repo. -->

[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[expressroute-indexmd-744v]: ../expressroute/index.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[sql-db-firewall-rules-config-715d]: sql-database-firewall-configure.md

[sql-database-develop-error-messages-419g]: sql-database-develop-error-messages.md

[sql-db-vnet-service-endpoint-rule-powershell-md-52d]: sql-database-vnet-service-endpoint-rule-powershell.md

[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]: sql-database-vnet-service-endpoint-rule-powershell.md#a-verify-subnet-is-endpoint-ps-100

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[vm-virtual-network-service-endpoints-overview-649d]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.md



<!-- Link references, to text, Outside this Github repo (HTTP). -->

[http-azure-portal-link-ref-477t]: https://portal.azure.com/




<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON

- Azure CLI

- ARM templates
-->

