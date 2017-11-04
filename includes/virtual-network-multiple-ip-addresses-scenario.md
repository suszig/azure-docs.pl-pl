## <a name="scenario"></a>Scenariusz
Maszynę Wirtualną z jednej karty Sieciowej jest utworzony i podłączony do sieci wirtualnej. Maszyna wirtualna wymaga trzech różnych *prywatnej* IP adresy i dwa *publicznego* adresów IP. Adresy IP są przypisywane do następujące konfiguracje adresów IP:

* **IPConfig-1:** przypisuje *statycznych* prywatnego adresu IP i *statycznych* publicznego adresu IP.
* **Polecenie IPConfig-2:** przypisuje *statycznych* prywatnego adresu IP i *statycznych* publicznego adresu IP.
* **Polecenie IPConfig-3:** przypisuje *statycznych* prywatnego adresu IP i żadnego publicznego adresu IP.
  
    ![Wiele adresów IP](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

Konfiguracje adresów IP są powiązane z kartą sieciową, gdy karta sieciowa jest tworzony i karty Sieciowej jest dołączony do maszyny Wirtualnej po utworzeniu maszyny Wirtualnej. Typy adresy IP używane dla tego scenariusza są ilustracyjną. Można przypisać niezależnie od adresu i przypisywania typów IP wymagane.

> [!NOTE]
> Chociaż kroki opisane w tym artykule przypisuje wszystkie konfiguracje adresów IP do jednej karty Sieciowej, można także przypisać wielu konfiguracji adresów IP do dowolnej karty interfejsu Sieciowego na maszynie Wirtualnej z wieloma kartami Sieciowymi. Aby dowiedzieć się, jak utworzyć Maszynę wirtualną z wieloma kartami sieciowymi, przeczytaj [tworzenia maszyn wirtualnych z wieloma kartami sieciowymi](../articles/virtual-machines/windows/multiple-nics.md) artykułu.