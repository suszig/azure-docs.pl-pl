---
title: "Opis połączeń wychodzących na platformie Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak Azure umożliwia maszyny wirtualne do komunikowania się z publicznej usługi internetowe."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: d02960017b8793eccc2990a17e3d854991e877b6
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
# <a name="understanding-outbound-connections-in-azure"></a>Informacje o połączeniach wychodzących na platformie Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Maszyna wirtualna (VM) na platformie Azure mogą komunikować się z punktami końcowymi poza platformą Azure w publicznej przestrzeni adresów IP. Gdy maszyna wirtualna inicjuje przepływu wychodzącego do miejsca docelowego w publicznej przestrzeni adresów IP, Azure mapuje prywatnego adresu IP maszyny Wirtualnej do publicznego adresu IP i zezwala na ruch zwracany nawiązać połączenie z maszyną Wirtualną.

Platforma Azure udostępnia trzy różnych metod na osiągnięcie łączność wychodząca. Każda metoda ma swoją własną możliwości i ograniczeń. Przejrzyj każdej metody dokładnie, aby wybrać, które spełni wymagania organizacji.

| Scenariusz | Metoda | Uwaga |
| --- | --- | --- |
| Maszyna wirtualna autonomiczne (Brak usługi równoważenia obciążenia, żaden adres publiczny adres IP na poziomie wystąpienia) |Domyślne SNAT |Azure kojarzy publicznego adresu IP dla SNAT |
| Maszyna wirtualna z równoważeniem obciążenia (nie wystąpienia poziomu publicznego adresu IP na maszynie Wirtualnej) |SNAT przy użyciu usługi równoważenia obciążenia |Azure używa publicznego adresu IP usługi równoważenia obciążenia dla SNAT |
| Maszyna wirtualna o wystąpieniu poziomu publicznego adresu IP (z lub bez modułu równoważenia obciążenia) |SNAT nie jest używany. |Azure używa publicznego adresu IP, przypisane do maszyny Wirtualnej |

Jeśli nie chcesz, aby maszyny Wirtualnej do komunikowania się z punktami końcowymi poza platformą Azure w publicznej przestrzeni adresów IP, można użyć grup zabezpieczeń sieci (NSG) w celu zablokowania dostępu. Za pomocą grup NSG omówiono bardziej szczegółowo w [zapobieganie łączności publicznej w zakresie](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Autonomiczny maszynę Wirtualną za pomocą żaden adres publiczny adres IP na poziomie wystąpienia

W tym scenariuszu maszyna wirtualna nie jest częścią puli usługi równoważenia obciążenia Azure i nie ma adresu wystąpienia poziomu publicznego adresu IP (ILPIP) przypisane do niej. Gdy maszyna wirtualna tworzy przepływu wychodzącego, Azure tłumaczy prywatnej źródłowy adres IP przepływu wychodzącego do źródłowego publicznego adresu IP. Publiczny adres IP używany dla tego przepływu ruchu wychodzącego nie można skonfigurować, nie będą uwzględniane względem subskrypcji publicznego adresu IP limit zasobów. Platforma Azure korzysta źródła sieci adresu tłumaczenia (SNAT) do tej funkcji. Porty efemeryczne publicznego adresu IP są używane do rozróżniania poszczególnych przepływów pochodzi przez maszynę Wirtualną. Podczas tworzenia przepływów SNAT dynamicznie przydziela porty efemeryczne. W tym kontekście tymczasowych portów używanych do SNAT są określane jako porty SNAT.

Porty SNAT są ograniczone zasób, który może być wyczerpany. Należy zrozumieć, jak są używane. Jeden port SNAT jest używany na przepływ do pojedynczego docelowy adres IP. Każdy przepływ zajmują jednego portu SNAT wiele przepływów do tego samego adresu IP i portu. Dzięki temu, że strumienie są unikatowe, jeśli pochodzi z tego samego publicznego adresu IP do tego samego adresu IP i portu. Wiele przepływów, każdy z nich do innego adresu IP i port, współużytkować jeden port SNAT. Docelowy adres IP i port zapewnia unikatowość przepływów.

Można użyć [analizy dzienników dla usługi równoważenia obciążenia](load-balancer-monitor-log.md) i [alertu dzienniki zdarzeń, aby monitorować SNAT portu wiadomości wyczerpania](load-balancer-monitor-log.md#alert-event-log) do monitorowania kondycji połączenia wychodzące. Gdy wyczerpania zasobów portu SNAT przepływów wychodzących niepowodzeniem, dopóki SNAT porty są wydawane przez przepływów. Moduł równoważenia obciążenia używa limit czasu bezczynności 4-minutowy odzyskiwanie SNAT portów.  Przegląd [maszyny Wirtualnej o wystąpieniu poziomu publicznego adresu IP (z lub bez modułu równoważenia obciążenia)](#vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer) następujących sekcji, jak również jako [wyczerpania Zarządzanie SNAT](#snatexhaust).

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Równoważeniem obciążenia maszyny Wirtualnej z żaden adres publiczny adres IP na poziomie wystąpienia

W tym scenariuszu maszyna wirtualna jest częścią puli usługi równoważenia obciążenia Azure.  Maszyna wirtualna nie ma publicznego adresu IP przypisane do niej. Zasób usługi równoważenia obciążenia musi być skonfigurowany z regułą, aby połączyć publicznego frontonu IP z puli zaplecza.  Jeśli nie wykonasz tej konfiguracji, zachowanie jest zgodnie z opisem w poprzedniej sekcji, aby uzyskać [autonomiczny maszynę Wirtualną za pomocą wystąpienia poziomu publiczny adres IP nie](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

Gdy maszyna wirtualna z równoważeniem obciążenia tworzy przepływu wychodzącego, Azure tłumaczy prywatnej źródłowy adres IP przepływu wychodzącego do publicznego adresu IP publiczny moduł równoważenia obciążenia serwera sieci Web. Platforma Azure korzysta źródła sieci adresu tłumaczenia (SNAT) do tej funkcji. Porty efemeryczne publicznego adresu IP usługi równoważenia obciążenia są używane do rozróżniania poszczególnych przepływów pochodzi przez maszynę Wirtualną. SNAT dynamicznie przydziela porty efemeryczne, podczas tworzenia przepływów wychodzących. W tym kontekście tymczasowych portów używanych do SNAT są określane jako porty SNAT.

Porty SNAT są ograniczone zasób, który może być wyczerpany. Należy zrozumieć, jak są używane. Jeden port SNAT jest używany na przepływ pojedynczego adresu IP i portu. Każdy przepływ zajmują jednego portu SNAT wiele przepływów do tego samego adresu IP i portu. Dzięki temu, że strumienie są unikatowe, jeśli pochodzi z tego samego publicznego adresu IP do tego samego adresu IP i portu. Wiele przepływów, każdy z nich do innego adresu IP i port, współużytkować jeden port SNAT. Docelowy adres IP i port zapewnia unikatowość przepływów.

Można użyć [analizy dzienników dla usługi równoważenia obciążenia](load-balancer-monitor-log.md) i [alertu dzienniki zdarzeń, aby monitorować SNAT portu wiadomości wyczerpania](load-balancer-monitor-log.md#alert-event-log) do monitorowania kondycji połączenia wychodzące. Gdy wyczerpania zasobów portu SNAT przepływów wychodzących niepowodzeniem, dopóki SNAT porty są wydawane przez przepływów. Moduł równoważenia obciążenia używa limit czasu bezczynności 4-minutowy odzyskiwanie SNAT portów.  Przejrzyj następującą sekcję oraz [wyczerpania Zarządzanie SNAT](#snatexhaust).

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Maszyna wirtualna o wystąpieniu poziomu publicznego adresu IP (z lub bez modułu równoważenia obciążenia)

W tym scenariuszu maszyna wirtualna ma wystąpienie poziomu publicznego adresu IP (ILPIP) przypisane do niej. Nie ma znaczenia, czy maszyna wirtualna jest równoważone, czy nie. W przypadku ILPIP źródła sieci adresu tłumaczenia (SNAT) nie jest używany. Maszyna wirtualna używa ILPIP dla wszystkich przepływów wychodzących. Jeśli wystąpią wyczerpania SNAT aplikacji inicjuje wiele przepływów wychodzących, należy rozważyć przypisywanie ILPIP złagodzić ograniczenia SNAT.

## <a name="discovering-the-public-ip-used-by-a-given-vm"></a>Odnajdywanie publiczny adres IP używany przez danego maszyny Wirtualnej

Istnieje wiele sposobów, aby określić źródłowy publiczny adres IP połączeń wychodzących. OpenDNS zapewnia usługę, które mogą być prezentowane z publicznego adresu IP maszyny Wirtualnej. Używanie polecenia nslookup, możesz wysłać zapytanie DNS dla nazwy myip.opendns.com OpenDNS program rozpoznawania nazw. Usługa zwraca źródłowy adres IP użyty do wysłania zapytania. Podczas wykonywania następującego zapytania z maszyny Wirtualnej, odpowiedź jest publiczny adres IP używany dla tej maszyny Wirtualnej.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Zapobieganie łączności publicznej w zakresie

Czasami jest niepożądanych dla maszyny Wirtualnej, aby można było utworzyć przepływ wychodzący może być wymagane do zarządzania, które miejsc docelowych jest osiągalna z przepływów wychodzących lub które miejsc docelowych może rozpocząć przepływów przychodzących. W takim przypadku należy użyć [grup zabezpieczeń sieci (NSG)](../virtual-network/virtual-networks-nsg.md) do zarządzania miejsc docelowych, które maszyny Wirtualnej można również uzyskać dostęp publiczny lokalizację docelową zainicjować przepływów przychodzących. Po zastosowaniu grupy NSG z maszyną wirtualną z równoważeniem obciążenia, należy zwrócić uwagę na [domyślne znaczniki](../virtual-network/virtual-networks-nsg.md#default-tags) i [domyślne zasady](../virtual-network/virtual-networks-nsg.md#default-rules).

Należy się upewnić, czy maszyny Wirtualnej może odbierać żądań sondy kondycji z modułu równoważenia obciążenia Azure. Jeśli grupy NSG blokuje żądania sondy kondycji z AZURE_LOADBALANCER domyślny znacznik, sondy kondycji Twojej maszyny Wirtualnej nie powiedzie się i maszyny Wirtualnej jest oznaczony w dół. Moduł równoważenia obciążenia zatrzymuje wysyłanie nowych przepływów z tą maszyną Wirtualną.

## <a name="snatexhaust"></a>Zarządzanie wyczerpania SNAT

Porty efemeryczne używane na potrzeby SNAT są zużywalnymi zasobami, zgodnie z opisem w [autonomiczny maszynę Wirtualną za pomocą żaden adres publiczny adres IP na poziomie wystąpienia](#standalone-vm-with-no-instance-level-public-ip-address) i [równoważeniem obciążenia maszyny Wirtualnej z żaden adres publiczny adres IP na poziomie wystąpienia](#standalone-vm-with-no-instance-level-public-ip-address).

Jeśli znasz są inicjowanie liczbę wychodzących połączeń TCP lub UDP do tego samego adresu IP i portu, obserwować niepowodzeniem połączenia wychodzące lub zalecana przy pomocy technicznej są wyczerpujące porty SNAT, dostępnych jest kilka ogólne środki zaradcze.  Przejrzyj te opcje i zdecydować, co to jest najlepsze w przypadku danego scenariusza.  Jest możliwe jeden lub więcej może ułatwić zarządzanie tego scenariusza.

### <a name="modify-application-to-reuse-connections"></a>Modyfikowanie aplikacji do ponownego użycia połączenia 
Można zmniejszyć zapotrzebowanie na porty efemeryczne używane w przypadku SNAT ponowne użycie połączenia w aplikacji.  Jest to szczególnie istotne dla protokołów, takich jak HTTP/1.1, gdzie ponownego użycia połączenia jest ustawieniem domyślnym.  I innych protokołów przy użyciu protokołu HTTP jako transportu (np. REST) mogą korzystać z kolei.  Ponowne użycie zawsze jest lepszym rozwiązaniem niż poszczególnych, atomic połączeń TCP dla każdego żądania i powoduje więcej wydajności, bardzo wydajny transakcji protokołu TCP.

### <a name="modify-application-to-use-connection-pooling"></a>Modyfikowanie aplikacji do korzystania z puli połączeń
Można wdrożyć połączenia buforowanie schemat w aplikacji, które żądania są dystrybuowane wewnętrznie przez ustalony zbiór połączeń (każdego ponowne użycie w miarę możliwości).  Tego ograniczenia liczby tymczasowych porty używane i tworzy środowisku bardziej przewidywalne.  To również zwiększyć przepływność żądania, zezwalając wiele równoczesnych operacji, gdy blokuje pojedynczego połączenia w przypadku odpowiedzi operacji.  Pula połączeń może już istnieć w ramach używanej do tworzenia aplikacji lub ustawienia konfiguracji dla aplikacji.  To można używać razem z ponownego użycia połączenia i wiele żądań używanie stały, przewidywalnych liczby portów do tego samego adresu IP i portu podczas również korzystających z bardzo skuteczne stosowanie transakcji TCP zmniejszenia opóźnienia i zasobów wykorzystanie.

### <a name="modify-application-to-use-less-aggressive-retry-logic"></a>Modyfikowanie aplikacji do korzystania z mniej aktywnego Logika ponawiania
Po wyczerpaniu porty efemeryczne używane na potrzeby SNAT lub występują błędy aplikacji, bez zanikania i wycofywania wyczerpania Przyczyna logiki wystąpienia lub utrwalić ponowi próbę życie aktywnego lub atak metodą. Można zmniejszyć zapotrzebowanie na porty efemeryczne przy użyciu mniej agresywne logiki ponawiania próby.   Porty efemeryczne ma 4-minutowy limit czasu bezczynności (nie można dostosować), a jeśli ponowne próby są zbyt agresywnie, wyczerpanie ma możliwość wyczyszczone samodzielnie.  W związku z tym biorąc pod uwagę, jak i jak często aplikacja ponowi próbę transakcji jest krytyczne projektu.

### <a name="assign-an-instance-level-public-ip-to-each-vm"></a>Przypisz publicznego adresu IP wystąpienia poziomu do każdej maszyny Wirtualnej
Spowoduje to zmianę Twojego scenariusza [poziomie wystąpienia publicznego adresu IP do maszyny Wirtualnej](#vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer).  Wszystkie porty efemeryczne publicznego adresu IP używane dla każdej maszyny Wirtualnej są dostępne dla maszyny Wirtualnej (w przeciwieństwie do scenariuszy, w którym porty efemeryczne publicznego adresu IP są udostępniane wszystkie maszyny Wirtualnej skojarzone z puli zaplecza odpowiednich).  Brak możliwości wziąć pod uwagę, takie jak dodatkowych kosztów adresów IP i potencjalnego wpływu listę dozwolonych podobnej dużą liczbę pojedyncze adresy IP.

## <a name="limitations"></a>Ograniczenia

Jeśli [wielu adresów IP (publicznymi) są skojarzone z modułem równoważenia obciążenia](load-balancer-multivip-overview.md), wszystkie te publicznego adresu IP, adresy są kandydatem do przepływów wychodzących.

Azure używa nieobsługiwanego algorytmu, aby określić liczbę dostępnych portów SNAT, zależnie od rozmiaru puli.  To nie konfiguruje się w tej chwili.

Połączenia wychodzące mieć 4-minutowy limit czasu bezczynności.  To nie jest zmieniane.

Należy pamiętać, że liczba dostępnych portów SNAT nie przekłada się bezpośrednio do liczby połączeń. Można znaleźć powyższej sekcji Szczegóły na kiedy i jak SNAT porty są przydzielane i [jak zarządzać zasobem wyczerpującymi](#snatexhaust).
