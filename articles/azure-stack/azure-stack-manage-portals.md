---
title: Korzystanie z portalu administratora w stosie Azure | Dokumentacja firmy Microsoft
description: "Jako operator stosu Azure jak korzystać z portalu administratora."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 34d9d207225327758d535a11c870019f3db31cb0
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2018
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Korzystanie z portalu administratora w stosie Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Istnieją dwa portale w stosie Azure; portal administratora i portalu użytkownika (czasem nazywane *dzierżawy* portalu). Jako operator stosu Azure mogą korzystać z portalu administratora, bieżące zarządzanie i operacje stosu Azure. 

## <a name="access-the-administrator-portal"></a>Uzyskiwanie dostępu do portalu administratora

Środowisko deweloperskie zestawu, należy najpierw upewnić się, że można [połączyć się z hostem zestawu programowanie](azure-stack-connect-azure-stack.md) za pomocą połączenia pulpitu zdalnego lub za pośrednictwem wirtualnej sieci prywatnej (VPN).

Aby uzyskać dostęp do portalu administratora, przejdź do adresu URL portalu i zaloguj się przy użyciu poświadczeń operator stosu Azure. Zintegrowany system portalu, do którego jest zależny od adresu URL na podstawie nazwa regionu i zewnętrznych pełną nazwę domeny (FQDN) wdrożenia stosu Azure.

| Środowisko | Adres URL portalu administratora |   
| -- | -- | 
| Zestaw deweloperski| https://adminportal.local.azurestack.external  |
| Systemy zintegrowane | https://adminportal.&lt;*region*&gt;.&lt;*FQDN*&gt; | 
| | |

 ![Portal administratora](media/azure-stack-manage-portals/image1.png)

W portalu administratora należy wykonać czynności takich jak:

* Zarządzanie infrastrukturą (w tym kondycji systemu, aktualizacje, pojemność itp.)
* Wypełnij witryny marketplace
* Tworzenie planów i oferty
* Utwórz subskrypcje dla użytkowników

W **samouczek Szybki Start** kafelka, istnieją linki do dokumentacji online dla najbardziej typowych zadań.
 
Ma możliwości dla operatora utworzyć zasoby, takie jak maszyn wirtualnych, sieci wirtualnych i kont magazynu w portalu administratora, ale powinien [Zaloguj się do portalu użytkowników](user/azure-stack-use-portal.md) do utworzenia i przetestowania zasobów. ( **Utwórz maszynę wirtualną** łącza na kafelku samouczek Szybki Start jest tworzenie maszyny wirtualnej w portalu administratora, ale ta procedura jest tylko do zweryfikowania stosu Azure po początkowym wdrożeniu.)

## <a name="subscription-behavior"></a>Zachowanie subskrypcji
 
Istnieje tylko jedna subskrypcja, które są dostępne w portalu administratora. Ta subskrypcja jest *domyślny dostawca subskrypcji*. Nie można dodać żadnych innych subskrypcji do użycia w portalu administratora.

Jako operator stosu Azure można dodać subskrypcji dla użytkowników (w tym siebie) z portalu administratora. Użytkownicy (w tym siebie) dostępu i używania te subskrypcje z aplikacji portal użytkowników. Portal użytkowników nie zapewnia dostęp do wszystkich funkcji administracyjnych lub operacyjnych portalu administratora.

Portale administratora i użytkownika bazują na oddzielnego wystąpienia usługi Azure Resource Manager. Z powodu separacji Menedżera zasobów subskrypcji nie przechodzą portali. Na przykład jeśli jako operator stosu Azure loguje się do portalu użytkowników, nie masz dostępu do subskrypcji domyślny dostawca. W związku z tym nie ma dostępu do żadnych funkcji administracyjnych. Można utworzyć subskrypcji dla siebie z ofertami publicznego, ale są traktowane jako użytkownik dzierżawy.

  >[!NOTE]
  W środowisku programistycznym zestawu Jeśli użytkownik należy do tego samego katalogu dzierżawy jako operator stosu Azure ich nie są blokowane logowanie do portalu administratora. Jednak ich nie może uzyskać dostępu do żadnych funkcji administracyjnych. Ponadto w portalu administratora nie mogą dodać oferty subskrypcji lub dostępu, które są udostępniane użytkownikom w portalu użytkowników.

## <a name="administrator-portal-tips"></a>Wskazówki dotyczące portalu administratora

### <a name="customize-the-dashboard"></a>Dostosowywać pulpit nawigacyjny

Pulpit nawigacyjny zawiera zestaw domyślnych kafelków. Możesz kliknąć **edycji pulpitu nawigacyjnego** do zmiany domyślnego pulpitu nawigacyjnego, albo kliknij przycisk **nowego pulpitu nawigacyjnego** dodawać niestandardowe pulpity nawigacyjne. Można łatwo dodać Kafelki na pulpicie nawigacyjnym. Na przykład kliknięcie **nowy**, kliknij prawym przyciskiem myszy **oferuje + plany**, a następnie kliknij przycisk **Przypnij do pulpitu nawigacyjnego**.

### <a name="quick-access-to-online-documentation"></a>Szybki dostęp do dokumentacji online

Aby uzyskać dostęp do dokumentacji operator stosu Azure, kliknij ikonę Pomoc i obsługa techniczna (znak zapytania) w prawym górnym rogu portalu administratora, a następnie kliknij przycisk **Pomoc i obsługa techniczna**.

### <a name="quick-access-to-help-and-support"></a>Szybki dostęp do pomocy i obsługi technicznej

Jeśli kliknij ikonę Pomoc i obsługa techniczna (znak zapytania) w prawym górnym rogu portalu administratora, a następnie kliknij przycisk **nowy obsługuje żądania**, to jest jedną z następujących czynności:

- Jeśli używasz zintegrowanego systemu, Akcja ta otwiera witrynę, której można bezpośrednio otworzyć bilet pomocy technicznej z usług obsługi klienta firmy Microsoft (CSS). Zapoznaj się z sekcją "Where uzyskać pomoc techniczną" [podstawy administracji stosu Azure](azure-stack-manage-basics.md) zrozumienie, kiedy należy przejść za pomocą techniczną firmy Microsoft lub działem pomocy technicznej dostawcy sprzętu producenta sprzętu (OEM).
- Jeśli używasz zestaw deweloperski, ta akcja powoduje otwarcie witryny fora stosu Azure bezpośrednio. Fora te są regularnie monitorowane. Ponieważ zestaw deweloperski środowiska do oceny, nie jest oficjalną obsługiwane oferowane przez firmy Microsoft CSS.

## <a name="next-steps"></a>Kolejne kroki

- [Zarządzanie regionu Azure stosu](azure-stack-region-management.md)
