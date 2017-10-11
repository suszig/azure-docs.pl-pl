Należy koniecznie należy pamiętać, że istnieją dwa sposoby konfigurowania odbiornik grupy dostępności na platformie Azure. Możliwości różnią się w typie modułu równoważenia obciążenia Azure, używanego podczas tworzenia odbiornika. W poniższej tabeli opisano różnice:

| Typ usługi równoważenia obciążenia | Wdrażanie | Zastosowania: |
| --- | --- | --- |
| **Zewnętrzne** |Używa *publiczny wirtualny adres IP* usługi w chmurze, który jest hostem maszyny wirtualnej (VM). |Należy uzyskać dostępu do odbiornika z spoza sieci wirtualnej, łącznie z Internetu. |
| **Wewnętrzny** |Używa *wewnętrznego modułu równoważenia obciążenia* za pomocą prywatnego adresu dla odbiornika. |Można uzyskać dostępu do odbiornika tylko w obrębie tej samej sieci wirtualnej. Ten dostęp obejmuje sieci VPN typu lokacja lokacja w scenariuszach hybrydowych. |

> [!IMPORTANT]
> Dla odbiornika, który korzysta z usługi w chmurze publicznej VIP (zewnętrznej usługi równoważenia obciążenia), tak długo, jak klient odbiornik i bazy danych znajdują się w tym samym regionie Azure, nie będą naliczane opłaty za wyjście. W przeciwnym razie żadnych danych zwrócony przez odbiornik jest uznawany za wyjście i jest rozliczana stawkami normalne transferu danych. 
> 
> 

ILB można skonfigurować tylko w sieciach wirtualnych z zakresu regionalnego. Istniejących sieci wirtualnych, które zostały skonfigurowane dla grupy koligacji nie można użyć ILB. Aby uzyskać więcej informacji, zobacz [Omówienie usługi równoważenia obciążenia wewnętrznego](../articles/load-balancer/load-balancer-internal-overview.md).

