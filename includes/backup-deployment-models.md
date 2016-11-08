Usługa Azure Backup ma dwa typy magazynów — magazyn kopii zapasowych i magazyn usługi Recovery Services. Najpierw powstał magazyn kopii zapasowych. Następnie wprowadzono magazyn usługi Recovery Services, aby zapewnić obsługę rozszerzonych wdrożeń usługi Resource Manager. Firma Microsoft zaleca używanie wdrożeń usługi Resource Manager, chyba że wymagane jest wdrożenie klasyczne.

| **Wdrożenie** | **Portal** | **Magazyn** |
| --- | --- | --- |
| Wdrożenie klasyczne |[Wdrożenie klasyczne](https://manage.windowsazure.com) |Tworzenie kopii zapasowych |
| Resource Manager |[Azure](https://portal.azure.com) |Recovery Services |

> [!NOTE]
> Magazyny kopii zapasowych nie mogą chronić rozwiązań wdrożonych przy użyciu usługi Resource Manager. Jednak przy użyciu magazynu usługi Recovery Services można chronić serwery i maszyny wirtualne wdrożone w modelu klasycznym.  
> 
> 

<!--HONumber=Sep16_HO3-->


