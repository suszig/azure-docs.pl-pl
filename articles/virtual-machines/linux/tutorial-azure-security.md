---
title: "Maszyny wirtualne w Centrum zabezpieczeń Azure i systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zabezpieczeń dla maszyny wirtualnej systemu Linux platformy Azure z Centrum zabezpieczeń Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/07/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: dbba39f5b9f18aaca6449e08aa584224fc2126d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Monitorowanie zabezpieczeń maszyny wirtualnej przy użyciu Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure ułatwia wgląd we Azure zasobu rozwiązania w zakresie zabezpieczeń. Centrum zabezpieczeń zapewnia zintegrowane zabezpieczenia monitorowania. Można wykryć zagrożenia, które w przeciwnym razie mogłyby pozostać niezauważone. Z tego samouczka, dowiesz się o Centrum zabezpieczeń Azure i jak:
 
> [!div class="checklist"]
> * Konfigurowanie zbierania danych
> * Ustawianie zasad zabezpieczeń
> * Wyświetl i rozwiąż problemy z konfiguracją kondycji
> * Przejrzyj wykrytych zagrożeń  

## <a name="security-center-overview"></a>Omówienie Centrum zabezpieczeń

Centrum zabezpieczeń identyfikuje potencjalne problemy z konfiguracją maszyny wirtualnej (VM), a Ustawianie zagrożenia bezpieczeństwa. Mogą one obejmować maszyn wirtualnych, których brakuje grup zabezpieczeń sieci niezaszyfrowane dyski i atakami siłowymi protokołu RDP (Remote Desktop). Informacje są wyświetlane na pulpicie nawigacyjnym Centrum zabezpieczeń na wykresach łatwe do odczytania.

Aby uzyskać dostępu do pulpitu nawigacyjnego Centrum zabezpieczeń w portalu Azure, w menu wybierz **Centrum zabezpieczeń**. Na pulpicie nawigacyjnym można sprawdzić kondycję zabezpieczeń w środowisku platformy Azure, Znajdź liczbę zalecanych rozwiązań i wyświetlić bieżący stan alertów zagrożeń. Można rozwinąć każdego wysokiego poziomu wykresie, aby zobaczyć więcej szczegółów.

![Pulpit nawigacyjny Centrum zabezpieczeń](./media/tutorial-azure-security/asc-dash.png)

Centrum zabezpieczeń wykracza poza podano zalecenia dotyczące problemów wykrytych danych odnajdywania. Na przykład jeśli maszyna wirtualna została wdrożona bez dołączonego sieciowej grupy zabezpieczeń, Centrum zabezpieczeń wyświetla zalecenia, korygowania prostych kroków, które można wykonać. Otrzymasz automatycznego korygowania bez opuszczania kontekstu Centrum zabezpieczeń.  

![Zalecenia](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Konfigurowanie zbierania danych

Przed uzyskaniem widoczność w konfiguracjach zabezpieczeń maszyny Wirtualnej, należy skonfigurować zbierania danych Centrum zabezpieczeń. Obejmuje to włączenie funkcji zbierania danych i tworzenie konta magazynu Azure do przechowywania zebranych danych. 

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń, kliknij przycisk **zasady zabezpieczeń**, a następnie wybierz subskrypcję. 
2. Aby uzyskać **zbierania danych**, wybierz pozycję **na**.
3. Aby utworzyć konto magazynu, wybierz **wybierz konto magazynu**. Następnie wybierz opcję **OK**.
4. Na **zasady zabezpieczeń** bloku, wybierz opcję **zapisać**. 

Agenta zbierania danych Centrum zabezpieczeń jest instalowany na wszystkich maszynach wirtualnych, a rozpoczęciem zbierania danych. 

## <a name="set-up-a-security-policy"></a>Skonfiguruj zasady zabezpieczeń

Zasady zabezpieczeń są używane do zdefiniowania elementów, dla których Centrum zabezpieczeń służy do zbierania danych i podano zalecenia. Zasady zabezpieczeń można stosować do różnych grup zasobów platformy Azure. Mimo że domyślnie zasobów platformy Azure są sprawdzane dla wszystkich elementów zasad, można wyłączyć elementy poszczególnych zasad dla wszystkich zasobów platformy Azure lub dla grupy zasobów. Aby uzyskać szczegółowe informacje na temat zasad zabezpieczeń Centrum zabezpieczeń, zobacz [ustawić zasady zabezpieczeń w Centrum zabezpieczeń Azure](../../security-center/security-center-policies.md). 

Aby skonfigurować zasady zabezpieczeń dla wszystkich zasobów systemu Azure:

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń, wybierz **zasady zabezpieczeń**, a następnie wybierz subskrypcję.
2. Wybierz **zasady zapobiegania**.
3. Włącz lub Wyłącz zasady elementów, które chcesz zastosować do wszystkich zasobów platformy Azure.
4. Po wybraniu ustawienia, wybierz **OK**.
5. Na **zasady zabezpieczeń** bloku, wybierz opcję **zapisać**. 

Aby skonfigurować zasady dla określonej grupy zasobów:

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń, wybierz **zasady zabezpieczeń**, a następnie wybierz grupę zasobów.
2. Wybierz **zasady zapobiegania**.
3. Włącz lub Wyłącz zasady elementów, które chcesz zastosować do grupy zasobów.
4. W obszarze **dziedziczenia**, wybierz pozycję **Unique**.
5. Po wybraniu ustawienia, wybierz **OK**.
6. Na **zasady zabezpieczeń** bloku, wybierz opcję **zapisać**.  

Ponadto można wyłączyć zbieranie danych dla określonej grupy zasobów na tej stronie.

W poniższym przykładzie utworzono unikatowe zasady dla grupy zasobów o nazwie *myResoureGroup*. W tej zasadzie dysku szyfrowanie i sieci web aplikacji zapory zalecenia są wyłączone.

![Unikatowe zasady](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Wyświetl kondycję konfiguracji maszyny Wirtualnej

Po utworzeniu włączone zbieranie danych i ustaw zasadę zabezpieczeń, alertów i zalecenia rozpoczyna się Centrum zabezpieczeń. Podczas wdrażania maszyn wirtualnych, zainstalowano agenta zbierania danych. Centrum zabezpieczeń jest następnie wypełniane przy użyciu danych dla nowych maszyn wirtualnych. Aby uzyskać szczegółowe informacje o kondycji konfiguracji maszyny Wirtualnej, zobacz [ochrony maszyn wirtualnych w Centrum zabezpieczeń](../../security-center/security-center-virtual-machine-recommendations.md). 

Ponieważ dane są zbierane, kondycja zasobów dla każdej maszyny Wirtualnej i powiązanych zasobów platformy Azure są agregowane. Informacje są wyświetlane na wykresie łatwe do odczytania. 

Aby wyświetlić kondycję zasobu:

1.  Na zabezpieczenia Centrum pulpitu nawigacyjnego, w obszarze **kondycja zabezpieczeń zasobów**, wybierz pozycję **obliczeniowe**. 
2.  Na **obliczeniowe** bloku, wybierz opcję **maszyn wirtualnych**. Ten widok zawiera podsumowanie stanu konfiguracji dla wszystkich maszyn wirtualnych.

![Obliczenia bazy danych kondycji](./media/tutorial-azure-security/compute-health.png)

Aby wyświetlić wszystkie zalecenia dotyczące maszyny Wirtualnej, wybierz maszynę Wirtualną. Zalecenia i korekty są opisane bardziej szczegółowo w następnej sekcji tego samouczka.

## <a name="remediate-configuration-issues"></a>Korygowanie problemów z konfiguracją

Po rozpoczęciu Centrum zabezpieczeń do wypełnienia z danymi konfiguracji zalecenia są wykonywane na podstawie skonfigurowanych zasad zabezpieczeń. Na przykład jeśli maszyna wirtualna została skonfigurowana bez skojarzonej sieciowej grupy zabezpieczeń, zalecenia dotyczące utworzyć. 

Aby wyświetlić listę wszystkich zaleceń: 

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń, wybierz **zalecenia**.
2. Wybierz określonego zalecenia. Zostanie wyświetlona lista wszystkich zasobów, dla którego stosuje zalecenia.
3. Aby zastosować zalecenie, wybierz określonego zasobu. 
4. Postępuj zgodnie z instrukcjami dotyczącymi czynności korygujące. 

W wielu przypadkach Centrum zabezpieczeń zapewnia można wykonać kroki, które można podjąć w celu rozwiązania zalecenie bez opuszczania Centrum zabezpieczeń. W poniższym przykładzie Centrum zabezpieczeń wykryje grupy zabezpieczeń sieci, która ma nieograniczony reguły ruchu przychodzącego. Na stronie zalecenia, można wybrać **edytowanie reguły ruchu przychodzącego** przycisku. Zostanie wyświetlony interfejs użytkownika, który jest wymagane do zmodyfikowania reguły. 

![Zalecenia](./media/tutorial-azure-security/remediation.png)

Zgodnie z zaleceniami zostaną rozwiązane, są one oznaczone jako rozwiązane. 

## <a name="view-detected-threats"></a>Wyświetl wykrytych zagrożeń

Oprócz zalecenia dotyczące konfiguracji zasobów Centrum zabezpieczeń wyświetla alertów wykrywania zagrożeń. Funkcja alerty zabezpieczeń agreguje dane zbierane z każdej maszyny Wirtualnej Azure dzienniki sieci i połączonych rozwiązań partnerskich do wykrywania zagrożeń względem zasobów platformy Azure. Aby uzyskać szczegółowe informacje o funkcji wykrywania zagrożeń Centrum zabezpieczeń, zobacz [funkcji wykrywania Centrum zabezpieczeń Azure](../../security-center/security-center-detection-capabilities.md).

Funkcja alerty zabezpieczeń wymaga Centrum zabezpieczeń warstwy cenowej zostać zwiększona z *wolne* do *standardowe*. 30-dniowej **bezpłatnej wersji próbnej** jest dostępne po przejściu do tego wyższej warstwy cenowej. 

Aby zmienić warstwę cenową:  

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń, kliknij przycisk **zasady zabezpieczeń**, a następnie wybierz subskrypcję.
2. Wybierz **warstwa cenowa**.
3. Wybierz nową warstwę, a następnie wybierz **wybierz**.
4. Na **zasady zabezpieczeń** bloku, wybierz opcję **zapisać**. 

Po zmianie warstwy cenowej, wykres alerty zabezpieczeń rozpoczyna się do wypełnienia jako zabezpieczeń, które zostały wykryte zagrożenia.

![Alerty zabezpieczeń](./media/tutorial-azure-security/security-alerts.png)

Wybierz alert, aby wyświetlić informacje. Na przykład można wyświetlić opis to zagrożenie, czas wykrycia wszystkie próby zagrożeń i zalecanych czynności naprawczych. W poniższym przykładzie ataków siłowych RDP wykryto z 294 nieudanych prób RDP. Zalecane rozwiązanie jest dostępne.

![Atak protokołu RDP](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku Konfigurowanie Centrum zabezpieczeń Azure, a następnie przejrzeć maszyn wirtualnych w Centrum zabezpieczeń. W tym samouczku omówiono:

> [!div class="checklist"]
> * Konfigurowanie zbierania danych
> * Ustawianie zasad zabezpieczeń
> * Wyświetl i rozwiąż problemy z konfiguracją kondycji
> * Przejrzyj wykrytych zagrożeń

Przejdź do następnego samouczkiem, aby dowiedzieć się więcej o tworzeniu potoku CI/CD z Wpięć, GitHub i Docker.

> [!div class="nextstepaction"]
> [Tworzenie elementu konfiguracji/CD infrastruktury z Wpięć, GitHub i Docker](tutorial-jenkins-github-docker-cicd.md)

