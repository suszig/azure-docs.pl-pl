

## <a name="deployment-considerations"></a>Zagadnienia dotyczące wdrażania
* **Subskrypcja platformy Azure** — Aby wdrożyć więcej niż kilka wystąpień obliczeniowych, należy wziąć pod uwagę z subskrypcji lub inne opcje zakupu. Jeśli używasz [bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/), możesz użyć ograniczonej liczby rdzeni obliczeniowych platformy Azure.

* **Cennik i dostępności** -rozmiarów tych maszyn wirtualnych są oferowane tylko w standardowej warstwie cenowej. Sprawdź [produkty dostępne według regionu] (https://azure.microsoft.com/regions/services/) dostępności w regionach platformy Azure. 
* **Limit przydziału rdzeni** — może być konieczne zwiększyć limit przydziału rdzeni w subskrypcji platformy Azure z wartości domyślnej. Subskrypcją również może ograniczyć liczbę rdzeni, którą można wdrożyć w niektórych rodzin rozmiar maszyny Wirtualnej, w tym serię H. Aby zażądać zwiększenia limitu przydziału [otwarcia żądania pomocy technicznej online klienta](../articles/azure-supportability/how-to-create-azure-support-request.md) bez dodatkowych opłat. (Domyślne limity może się różnić w zależności od kategorii subskrypcji).
  
  > [!NOTE]
  > Jeśli masz wymagana pojemność na dużą skalę, skontaktuj się z pomocą techniczną platformy Azure. Przydziały Azure są środki ogranicza nie gwarantuje pojemności. Niezależnie od limitu przydziału naliczane są tylko opłaty dla rdzeni użycie.
  > 
  > 
* **Sieć wirtualna** — moduł Azure [sieci wirtualnej](https://azure.microsoft.com/documentation/services/virtual-network/) nie trzeba używać wystąpień obliczeniowych. Jednak w przypadku wielu wdrożeń należy co najmniej oparte na chmurze sieci wirtualnej platformy Azure lub połączenie lokacja lokacja Jeśli musisz uzyskiwać dostęp do zasobów lokalnych. W razie potrzeby utwórz nową sieć wirtualną, aby wdrożyć wystąpienia. Dodawanie obliczeniowych maszyn wirtualnych do sieci wirtualnej w grupie koligacji nie jest obsługiwane.
* **Zmiana rozmiaru** — ze względu na ich specjalne sprzętu można rozmiaru wystąpień obliczeniowych w ramach tej samej rodziny rozmiaru (serii H lub obliczeniowych A-series). Na przykład można zmienić tylko wirtualna H-series, z jedną rozmiar z serii H do innego. Ponadto zmiana rozmiaru od rozmiaru nie obliczeniowych do rozmiaru obliczeniowych nie jest obsługiwane.  
