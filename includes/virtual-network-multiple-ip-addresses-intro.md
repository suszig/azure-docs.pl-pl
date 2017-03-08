> [!div class="op_single_selector"]
> * [Portal](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [Interfejs wiersza polecenia 2.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> > * [Interfejs wiersza polecenia 1.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli-nodejs.md)
> * [Szablon](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Do maszyny wirtualnej platformy Azure jest dołączony co najmniej jeden interfejs sieciowy (karta sieciowa). Każda karta sieciowa może mieć przypisany jeden lub wiele statycznych lub dynamicznych publicznych i prywatnych adresów IP. Przypisywanie wielu adresów IP do maszyny wirtualnej włącza następujące możliwości:

* Hostowanie wielu witryn sieci Web lub usług z różnymi adresami IP i certyfikatami SSL na jednym serwerze.
* Może służyć jako sieciowe urządzenie wirtualne, takie jak zapora lub moduł równoważenia obciążenia.
* Możliwość dodania dowolnego z prywatnych adresów IP dla dowolnej z kart sieciowych do puli zaplecza usługi Azure Load Balancer. W przeszłości tylko podstawowy adres IP podstawowej karty sieciowej można było dodać do puli zaplecza. Aby dowiedzieć się więcej na temat sposobu równoważenia obciążenia dla wielu konfiguracji adresów IP, zapoznaj się z artykułem [Load balancing multiple IP configurations](../articles/load-balancer/load-balancer-multiple-ip.md) (Równoważenie obciążenia dla wielu konfiguracji adresów IP).

Z każdą kartą sieciową dołączoną do maszyny wirtualnej jest skojarzona co najmniej jedna konfiguracja adresu IP. Każdej konfiguracji jest przypisany jeden statyczny lub dynamiczny prywatny adres IP. Każda konfiguracja może mieć również powiązany jeden zasób publicznego adresu IP. Zasób publicznego adresu IP ma przypisany dynamiczny lub statyczny publiczny adres IP. Aby dowiedzieć się więcej o adresach IP na platformie Azure, zapoznaj się z artykułem [Adresy IP na platformie Azure](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md). Do każdej karty sieciowej można przypisać maksymalnie 250 prywatnych adresów IP. Do każdej karty sieciowej możesz przypisać wiele publicznych adresów IP, jednak istnieją limity dotyczące liczby publicznych adresów IP, których można używać w subskrypcji platformy Azure. Aby uzyskać szczegółowe informacje, zobacz artykuł dotyczący [limitów platformy Azure](../articles/azure-subscription-service-limits.md#networking-limits).
