---
title: "Problemów dotyczących łączności i sieci dla Microsoft Azure Cloud Services często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera listę często zadawanych pytań dotyczących łączności i sieci dla usługi w chmurze Microsoft Azure."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: genli
ms.openlocfilehash: d86f3ec043c504c9d79b18f1f0b4c9cf0adb115b
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problemów dotyczących łączności i sieci dla usług Azure Cloud Services: często zadawane pytania (FAQ)

Ten artykuł zawiera często zadawane pytania dotyczące problemów dotyczących łączności i sieci dla [usługi w chmurze Microsoft Azure](https://azure.microsoft.com/services/cloud-services). Można również znaleźć [strony rozmiar maszyny Wirtualnej usługi w chmurze](cloud-services-sizes-specs.md) dla informacji o rozmiarze.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>I nie można zastrzec adresu IP w usłudze chmury wielu wirtualnych adresów IP
Najpierw upewnij się, że wystąpienie maszyny wirtualnej, który próbujesz zastrzec adresu IP dla jest włączona. Drugie upewnij się, że używasz zastrzeżonych adresów IP w przypadku wdrożeń zarówno tymczasowych i produkcyjnych. **Nie** zmiany ustawień podczas uaktualniania jest wdrożenie.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Jak Pulpit zdalny gdy grupa NSG?
Dodaj reguły do grupy NSG, które zezwalają na ruch na portach **3389** i **20000**.  Pulpit zdalny używa portu **3389**.  Wystąpienia usługi chmury jest równoważone, więc nie można bezpośrednio kontrolować którego wystąpienia, aby nawiązać połączenie.  *RemoteForwarder* i *RemoteAccess* agentów zarządzania na ruch RDP i umożliwić klientom wysyłanie plików cookie protokołu RDP i określ poszczególne wystąpienia do nawiązania połączenia.  *RemoteForwarder* i *RemoteAccess* agentów wymagają tego portu **20000** być otwarte, które mogą być zablokowane, jeśli masz grupy NSG.

## <a name="can-i-ping-a-cloud-service"></a>Czy można wywołać usługi w chmurze?

Nie, nie za pomocą Normalny "ping" / protokołu ICMP. Protokół ICMP nie jest dozwolone przy użyciu usługi równoważenia obciążenia Azure.

Aby przetestować połączenie, zaleca się wykonanie polecenia ping portu. Gdy Ping.exe korzysta z protokołu ICMP, innych narzędzi, takich jak narzędzia PSPing, Nmap i telnet umożliwiają testowanie łączności z określonym porcie TCP.

Aby uzyskać więcej informacji, zobacz [użyj polecenia ping portu zamiast protokołu ICMP do testowania łączności maszyny Wirtualnej Azure](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>Jak zapobiec odbieranie tysięcy trafień z nieznanych adresów IP, które wskazują jakiegoś ze strony złośliwego oprogramowania do usługi w chmurze?
Azure implementuje zabezpieczeń sieci wielowarstwowych do ochrony jego usług platformy atakami rozproszone (DDoS) typu "odmowa usługi". System obrony przed atakami DDoS Azure jest częścią systemu Azure ciągłego monitorowania procesu, który stale zwiększona testy penetracji. Ten system obrony przed atakami DDoS zaprojektowano w celu wytrzymać nie tylko atakami z zewnątrz, ale także od innych dzierżawców Azure. Aby uzyskać więcej szczegółów, zobacz [zabezpieczenia sieci Microsoft Azure](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Można również utworzyć zadanie uruchamiania selektywnego blokowania niektórych z określonych adresów IP. Aby uzyskać więcej informacji, zobacz [zablokować określonego adresu IP](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Próba RDP do mojego wystąpienia usługi chmury, jest wyświetlany komunikat, "konto użytkownika wygasło."
Po obejściu datę wygaśnięcia, który jest skonfigurowany w ustawieniach protokołu RDP, może wystąpić komunikat o błędzie "upłynął to konto użytkownika". Data wygaśnięcia można zmienić z portalu, wykonaj następujące czynności:
1. Zaloguj się do [portalu Azure](https://portal.azure.com), przejdź do usługi w chmurze i wybierz **pulpitu zdalnego** kartę.
2. Wybierz **produkcji** lub **przemieszczania** miejsca wdrożenia.
3. Zmień datę "Wygasa", a następnie Zapisz konfigurację.

Teraz można dla protokołu RDP na komputerze.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Dlaczego jest modułu równoważenia obciążenia nie równoważenia ruch równomiernie?
Aby uzyskać informacje o jak wewnętrzne działania usługi równoważenia obciążenia, zobacz [nowy tryb dystrybucji modułu równoważenia obciążenia w Azure](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Krotka 5 jest używany algorytm dystrybucji (źródłowy adres IP, port źródłowy, docelowy adres IP, port docelowy protokół typu) skrót do mapowania ruchu na dostępne serwery. Zapewnia on lepkości tylko w ramach sesji transportu. Pakiety w tej samej sesji TCP lub UDP, zostanie przekierowany do tego samego wystąpienia adres IP (DIP) datacenter za punkt końcowy ze zrównoważonym obciążeniem. Jeśli klient zamyka i ponowne otwarcie połączenia lub uruchamia nową sesję z tym samym źródłowy adres IP, port źródłowy zmiany i powoduje, że ruch komunikować się z innym punktem końcowym DIP.

## <a name="how-can-i-redirect-the-incoming-traffic-to-my-default-url-of-cloud-service-to-a-custom-url"></a>Jak przekierować ruch przychodzący do mojego domyślny adres URL z usługi w chmurze niestandardowy adres URL 

Adres URL przepisywania moduł programu IIS można użyć do przekierowywania ruchu przychodzącego do domyślnego adresu URL dla usługi w chmurze (na przykład \*. cloudapp.net) do niektórych niestandardowe DNS nazwa lub adres URL. Ponieważ moduł ponowne zapisywanie adresów URL jest domyślnie włączone na role sieci Web i jego reguły są konfigurowane w pliku web.config aplikacji, zawsze będzie dostępna na maszynie Wirtualnej niezależnie od ponownego uruchomienia/reimages. Aby uzyskać więcej informacji, zobacz:

- [Tworzenie reguły ponownego zapisywania dla adresu URL przepisywania moduł](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Jak usunąć domyślne łącze](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-the-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Jak można I blok/wyłączanie ruch przychodzący do domyślnego adresu URL usługi w chmurze? 

Ruch przychodzący może uniemożliwić do domyślnego adresu URL/nazwa usługi w chmurze (na przykład \*. cloudapp.net), ustawiając nagłówek hosta na niestandardową nazwę DNS (na przykład www. MyCloudService.com) w obszarze Konfiguracja powiązania witryny w pliku definicji (*.csdef) usługi chmury opisany poniżej: 
 

    <?xml version="1.0" encoding="utf-8"?> 
    <ServiceDefinition name="AzureCloudServicesDemo" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6"> 
      <WebRole name="MyWebRole" vmsize="Small"> 
        <Sites> 
          <Site name="Web"> 
            <Bindings> 
              <Binding name="Endpoint1" endpointName="Endpoint1" hostHeader="www.MyCloudService.com" /> 
            </Bindings> 
          </Site> 
        </Sites> 
        <Endpoints> 
          <InputEndpoint name="Endpoint1" protocol="http" port="80" /> 
        </Endpoints> 
        <ConfigurationSettings> 
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" /> 
        </ConfigurationSettings> 
      </WebRole> 
    </ServiceDefinition> 
 
Jak to powiązanie nagłówka hosta jest wymuszane za pośrednictwem pliku csdef, usługi tylko będą dostępne za pośrednictwem niestandardową nazwę "www.MyCloudService.com", natomiast wszystkich przychodzących żądań "*. cloudapp.net" domeny zawsze nie powiedzie się. Trwa powiedział, jeśli jednak używać niestandardowych sondowania Programowego lub wewnętrzny moduł równoważenia obciążenia w usłudze, blokuje domyślnego adresu URL/nazwa usługi może zakłóca działanie sondowania. 

## <a name="how-to-make-sure-the-public-facing-ip-address-of-a-cloud-service-aka-vip-never-changes-so-that-it-could-be-customarily-whitelisted-by-few-specific-clients"></a>Jak upewnij się, że publiczny adres IP połączonej usługi w chmurze (czyli VIP) nie zmienia tak, aby może to być zwyczajowo białej przez kilka określonych klientów?

Listę dozwolonych podobnej adres IP usługi w chmurze zalecane jest, że masz zastrzeżony adres IP skojarzone z nim w inny sposób, który wirtualny adres IP podał Azure będzie można cofnąć alokacji z Twojej subskrypcji po usunięciu wdrożenia. Należy pamiętać, że do pomyślnego operacja wymiany adresów VIP będzie potrzebny poszczególnych zastrzeżonych adresów IP dla zarówno produkcyjne i przejściowe miejsc, w przypadku braku które wymiany operacja zakończy się niepowodzeniem. Wykonaj te artykuły do zarezerwowania adresu IP i powiązać ją z usługami w chmurze:  
 
- [Rezerwacja adresu IP istniejącej usługi w chmurze](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Skojarzyć zastrzeżonego adresu IP do usługi w chmurze przy użyciu pliku konfiguracji usługi](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Ile masz więcej niż jedno wystąpienie dla poszczególnych ról kojarzenie RIP z usługi w chmurze nie powinny powodować żadnych przestojów. Można też dozwolonych zakres adresów IP centrum danych Azure. Wszystkie zakresy IP platformy Azure można znaleźć [tutaj](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Ten plik zawiera zakresy adresów IP (w tym zakresy zasobów obliczeniowych, bazy danych SQL i magazynu) używane w centrach danych platformy Microsoft Azure. Co tydzień jest publikowany zaktualizowany plik odzwierciedlający aktualnie wdrożone zakresy i wszystkie nadchodzące zmiany w zakresach adresów IP. Nowe zakresy pojawiające się w pliku nie będą używane w centrach danych przez co najmniej tydzień. Nowy plik xml należy pobierać co tydzień i wykonywać niezbędne zmiany w witrynie, aby prawidłowo identyfikować usługi uruchomione na platformie Azure. Użytkownicy usługi ExpressRoute mogą zwrócić uwagę na to, że wcześniej ten plik aktualizował anonsowania BGP platformy Azure w pierwszym tygodniu każdego miesiąca. 

## <a name="how-can-i-use-azure-resource-manager-vnets-with-cloud-services"></a>Jak używać sieci wirtualnych platformy Azure Resource Manager z usługami w chmurze? 

Usługi w chmurze nie może być umieszczona w sieci wirtualnych Menedżera zasobów Azure, ale mogą zostać połączone sieci wirtualnych Azure Resource Manager i klasycznych sieci wirtualnych za pomocą komunikacji równorzędnej. Aby uzyskać więcej informacji, zobacz [równorzędna sieci wirtualnej](../virtual-network/virtual-network-peering-overview.md).