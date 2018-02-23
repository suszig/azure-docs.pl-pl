---
title: "Ochrona danych osobowych z funkcjami zabezpieczeń sieci platformy Azure | Dokumentacja firmy Microsoft"
description: "Ochrona danych osobowych przy użyciu funkcji zabezpieczeń sieci platformy Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 2510a4aed34b6a156cc9a9da6215de5956aac023
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a>Ochrona danych osobowych z funkcjami zabezpieczeń sieci: Brama aplikacji w usłudze Azure i grup zabezpieczeń sieci

Ten artykuł zawiera informacje i procedury, które pomogą Ci użyć bramy aplikacji Azure i grup zabezpieczeń sieci do ochrony danych osobowych.

Ważnym elementem strategii zabezpieczeń wielowarstwowy, aby chronić prywatność danych osobowych jest ochrona przed wspólnej luki w zabezpieczeniach luki w zabezpieczeniach takich jak iniekcja kodu SQL lub skryptów między witrynami. Utrzymywanie ruchu sieciowego niechciane poza Azure sieci wirtualnej pomaga w ochronie przed potencjalne naruszenie zabezpieczeń danych poufnych i Microsoft Azure oferuje narzędzia ułatwiające ochronę danych przed atakami.

## <a name="scenario"></a>Scenariusz

Firma rejs dużych, siedzibą w Stanach Zjednoczonych rozwija operacjach oferowanie trasy w DS, Adriatyku i Bałtyckiego mórz, jak również brytyjskich. Do realizacji tych działań ustawił mniejszych rejs wiersze na podstawie we Włoszech, Niemczech, Dania i Zjednoczone Królestwo

Przez firmę Microsoft Azure do przechowywania danych firmowych w chmurze i uruchamianie aplikacji na maszynach wirtualnych, które przetwarzają i dostęp do tych danych. Te dane obejmują dane osobowe, takich jak nazwy, adresy, numery telefonów i informacje o karcie kredytowej z jej klientów globalnych. Zawiera także tradycyjnych informacje kadr, takie jak adresy, numery telefonów, numery identyfikacyjne podatku i inne informacje dotyczące pracowników firmy we wszystkich lokalizacjach. Wiersz rejs zachowuje również dużej bazy danych elementów członkowskich programu osób trzecich i lojalność zawierający informacje osobiste, aby śledzić relacje z bieżących i starszych klientów.

Pracownikom firmy dostępu do sieci w oddziałach firmy i podróży agentów na całym świecie mają dostęp do niektórych zasobów firmy i aplikacji sieci web hostowanych na maszynach wirtualnych Azure umożliwia interakcję z nią.

## <a name="problem-statement"></a>Opis problemu

Firmy muszą chronić prywatność klientów i danych osobistych pracowników z atakami, które wykorzystują luki w zabezpieczeniach oprogramowania do uruchamiania złośliwego kodu, który może ujawniać dane osobowe przechowywane lub używane przez aplikacje oparte na chmurze firmy.

## <a name="company-goal"></a>Celem firmy

Cel firmy, aby upewnić się, że osoby nieupoważnione nie może uzyskać dostępu firmowych sieciach wirtualnych platformy Azure oraz aplikacji i danych, które znajdują się przez wykorzystywanie znanych luk. 

## <a name="solutions"></a>Rozwiązania

Microsoft Azure zapewnia mechanizmy zabezpieczeń, aby zapobiec wprowadzania sieci wirtualnych Azure niepożądanego ruchu. Kontrola ruchu przychodzącego i wychodzącego tradycyjnie jest wykonywane przez zapory. W Azure można użyć bramy aplikacji z zapory aplikacji sieci Web i grup zabezpieczeń sieci (NSG), które działają jako proste rozproszonej zapory. Te narzędzia umożliwiają wykrycie i zablokowanie ruchu sieciowego niepożądane.

### <a name="application-gatewayweb-application-firewall"></a>Aplikacja bramy/Zapora aplikacji sieci Web

[Zapory aplikacji sieci Web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF) składnik [brama aplikacji w usłudze Azure](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) chroni aplikacje sieci web, które są coraz bardziej cele złośliwe ataki wykorzystujące wspólnej znanych luk w zabezpieczeniach. Scentralizowane zapory aplikacji sieci Web chroni przed atakami z sieci web i upraszcza zarządzanie zabezpieczeniami bez konieczności wprowadzania zmian w aplikacji.

Azure zapory aplikacji sieci Web adresów różnych kategorii ataku, takich jak iniekcja kodu SQL, skryptów krzyżowych, protokołu HTTP naruszeń i anomalie, robotów, przeszukiwarek, skanerów, Omówiliśmy aplikacji, HTTP "odmowa usługi", i inne typowe ataki, takie jak uruchomienie polecenia, HTTP żądania przemytu, dzielenie odpowiedzi HTTP i ataków dołączenie pliku zdalnego. 

Można utworzyć bramy aplikacji z zapory aplikacji sieci Web, lub dodawanie zapory aplikacji sieci Web do istniejącej bramy aplikacji. W obu przypadkach brama aplikacji w usłudze Azure wymaga jego własnej podsieci.

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a>Jak utworzyć bramę aplikacji z zapory aplikacji sieci Web? 

Aby utworzyć nową bramę aplikacji za pomocą zapory aplikacji sieci Web jest włączone, wykonaj następujące czynności:

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
2. Wybierz **Utwórz zasób** > **sieci** > **brama aplikacji w**.
5. W **podstawy** bloku, zostanie wyświetlone, wprowadź wartości dla pola: nazwa, warstwy (standardowy lub zapory aplikacji sieci Web), rozmiar jednostki SKU (mały, średni lub duży) wystąpienia count (2 wysokiej dostępności), subskrypcji, lokalizacji i grupy zasobów.
6. W **ustawienia** bloku, który jest wyświetlany w obszarze **sieci wirtualnej**, kliknij przycisk **wybierz sieć wirtualną**. W tym kroku zostanie otwarta wprowadź wybierz bloku sieci wirtualnej.

7. Kliknij przycisk **Utwórz nowy** otworzyć **Utwórz sieć wirtualną** bloku.

8. Wprowadź następujące wartości: nazwa, przestrzeń adresów, nazwy podsieci, zakres adresów podsieci. Kliknij przycisk **OK**.

9. Na **ustawienia** bloku w obszarze **konfiguracji IP frontonu**, wybierz typ adresu IP.

10. Kliknij przycisk **wybierz publiczny adres IP,** następnie **Utwórz nowe.**

11. Zaakceptuj wartość domyślną, a następnie kliknij przycisk **OK.**

12. Na **ustawienia** bloku w obszarze **konfiguracji odbiornika**, wybierz opcję Użyj protokołu HTTP lub HTTPS, w obszarze **protokołu**. Aby używać protokołu HTTPS, wymagany jest certyfikat.

13. Skonfigurować specyficznych ustawień zapory aplikacji sieci Web: **zapory stan** (**włączone**) i **trybie zapory** (**zapobiegania**). Jeśli wybierzesz **wykrywania** jak tryb, ruch jest tylko rejestrowane.

14. Przegląd **Podsumowanie** i kliknij przycisk **OK**. Brama aplikacji jest teraz w kolejce, a utworzona.

Po utworzeniu bramy aplikacji może łączyć się z nim w portalu i kontynuować konfigurację bramy aplikacji.

![Brama aplikacji w utworzony](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-to-an-existing-application"></a>Jak dodać zapory aplikacji sieci Web do istniejącej aplikacji?

Aby uaktualnić istniejącą bramę aplikacji do obsługi w trybie zapobiegania zapory aplikacji sieci Web, wykonaj następujące czynności:

1. W okienku **Ulubione** witryny Azure Portal kliknij pozycję **Wszystkie zasoby**.

2. Kliknij istniejącą bramę aplikacji w **wszystkie zasoby** bloku. 
>[!NOTE]
Uwaga: Jeśli subskrypcja już ma kilka zasobów, można wprowadzić nazwę filtru według nazwy... aby łatwo uzyskać dostęp do strefy DNS.
3. Kliknij przycisk **zapory aplikacji sieci Web** i zaktualizuj ustawienia bramy aplikacji: **Uaktualnij do planu zapory aplikacji sieci Web** (zaznaczone), **zapory stan** (włączone), **trybie zapory** (zapobiegania). Należy również skonfigurować zestaw reguł, a następnie skonfiguruj wyłączone reguły.

Aby uzyskać szczegółowe informacje na temat tworzenia nową bramę aplikacji z zapory aplikacji sieci Web i jak dodawanie zapory aplikacji sieci Web do istniejącej bramy aplikacji, zobacz [Utwórz bramę aplikacji z zapory aplikacji sieci web za pomocą portalu.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)

### <a name="network-security-groups"></a>Grupy zabezpieczeń sieci

A [sieciowej grupy zabezpieczeń](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) zawiera listę reguł zabezpieczeń, które akceptować lub odrzucać ruch sieciowy do zasoby podłączone do [sieci wirtualnych Azure](https://docs.microsoft.com/azure/virtual-network/) (VNet). Grupy NSG można skojarzyć podsieci lub poszczególnych maszyn wirtualnych. Jeśli sieciowa grupa zabezpieczeń jest skojarzona z podsiecią, te reguły są stosowane do wszystkich zasobów połączonych z tą podsiecią. Ruch można ograniczyć jeszcze bardziej, kojarząc sieciową grupę zabezpieczeń również z maszyną wirtualną lub kartą sieciową.

Grupy NSG zawierają czterech właściwości: nazwa, Region, grupy zasobów i zasady.
>[!Note]
Chociaż sieciowa grupa zabezpieczeń istnieje w grupie zasobów, może być skojarzona z zasobami w dowolnej grupie zasobów, jeśli tylko zasób jest częścią tego samego regionu świadczenia usługi Azure co sieciowa grupa zabezpieczeń.

Reguły NSG obejmują dziewięć właściwości: nazwa, protokół (TCP, UDP lub \*, w tym protokołu ICMP, a także protokołów UDP i TCP), zakres portów, zakres portów docelowych źródła, adres źródłowy prefiksu, prefiks adresu docelowego, kierunek (przychodzący lub wychodzący) (priorytet od 100 do 4096) oraz dostęp typu (zezwalania lub odmowy). Wszystkie grupy NSG zawierają zestaw reguł domyślnych, które mogą być usunięte lub zastąpione przez tworzone zasady.

#### <a name="how-do-i-implement-nsgs"></a>Jak wdrożyć grup NSG?

Wdrożeniem grup NSG wymaga planowania i istnieje kilka kwestii, które należy wziąć pod uwagę. Obejmują one limity liczby grup NSG dla każdej subskrypcji i reguł na grupę NSG; Projekt sieci wirtualnej i podsieci, reguły specjalne, ruch protokołu ICMP, izolacji warstwy z podsieci i moduły równoważenia obciążenia.

Aby uzyskać więcej pomocy w planowanie i wdrażanie grup NSG, a przykładowy scenariusz wdrażania, zobacz [filtrowania ruchu sieciowego z grup zabezpieczeń sieci.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)

#### <a name="how-do-i-create-rules-in-an-nsg"></a>Jak utworzyć zasady w grupie NSG?

Aby utworzyć reguły ruchu przychodzącego w istniejącej grupy NSG, wykonaj następujące czynności:

1. Kliknij przycisk **wszystkie usługi**, a następnie **sieciowej grupy zabezpieczeń**.

2. Na liście grup NSG, kliknij **frontonu NSG**, a następnie **reguły zabezpieczeń dla ruchu przychodzącego.**

3. Na liście reguł zabezpieczeń dla ruchu przychodzącego kliknij **Dodaj.**

4. Wprowadź wartości w następujących polach: nazwa, priorytet, źródło, protokół, źródła należeć do zakresu, miejsce docelowe docelowy zakres portów i akcji.

Nowa reguła będą wyświetlane w grupie NSG po kilku sekundach.

![reguły zabezpieczeń sieci](media/protect-netsec/inbound-security.png)

Aby uzyskać dodatkowe instrukcje dotyczące sposobu tworzenia grup NSG w podsieci, tworzenie reguł i skojarzeniu grupy NSG z podsiecią frontonu i zaplecza, zobacz [Utwórz grupy zabezpieczeń sieci przy użyciu portalu Azure.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)

## <a name="next-steps"></a>Kolejne kroki

[Zabezpieczenia sieci platformy Azure](https://azure.microsoft.com/blog/azure-network-security/)

[Azure Network Security Best Practices (Najlepsze rozwiązania z zakresu zabezpieczeń sieci platformy Azure)](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[Uzyskaj informacje na temat grupy zabezpieczeń sieci](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[Zapora aplikacji sieci Web (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
