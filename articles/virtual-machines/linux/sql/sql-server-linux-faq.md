---
title: "Program SQL Server na maszynach wirtualnych systemu Linux platformy Azure — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące programem SQL Server na maszynach wirtualnych systemu Linux platformy Azure."
services: virtual-machines-linux
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 10/05/2017
ms.author: jroth
ms.openlocfilehash: a001ae116e33e0b7be4431b0bc4c8bb319f4e801
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Często zadawane pytania dotyczące programu SQL Server na maszynach wirtualnych Azure systemu Linux

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

W tym temacie przedstawiono odpowiedzi na niektóre często zadawane pytania dotyczące uruchamiania [programu SQL Server na maszynach wirtualnych Azure Linux](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> Ten temat koncentruje się na kwestie związane z programem SQL Server na maszynach wirtualnych systemu Linux. Jeśli używasz programu SQL Server na maszynach wirtualnych systemu Windows, temacie [systemu Windows — często zadawane pytania](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Często zadawane pytania

1. **Jak utworzyć maszynę wirtualną systemu Linux platformy Azure z programem SQL Server?**

   Najlepszym rozwiązaniem jest tworzenie maszyny wirtualnej systemu Linux, który zawiera program SQL Server. Samouczek dotyczący rejestracji na platformie Azure i tworzenia maszyny Wirtualnej SQL z portalu, zobacz [Aprowizowanie maszyny wirtualnej systemu Linux programu SQL Server, w portalu Azure](provision-sql-server-linux-virtual-machine.md). Istnieje również opcja ręcznego instalowania programu SQL Server na maszynie Wirtualnej w trybie za darmo licencjonowanej (deweloperem lub Express) lub przez ponowne użycie licencji usługi lokalnej. Jeśli użycie własnej licencji, musisz mieć [przenośność licencji za pośrednictwem programu Software Assurance na platformie Azure](https://azure.microsoft.com/pricing/license-mobility).

1. **Jak uaktualnić do nowej wersji/wydania programu SQL Server w maszynie Wirtualnej platformy Azure?**

   Obecnie nie są bez uaktualnienia w miejscu programu SQL Server w maszynie Wirtualnej platformy Azure. Utworzenie nowej maszyny wirtualnej platformy Azure z żądanej wersji programu SQL Server/wydania, a następnie przeprowadzić migrację bazy danych do nowego serwera przy użyciu [standardowych danych migracji techniki](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview).

1. **Jak zainstalować mój licencjonowanej kopii programu SQL Server na maszynie Wirtualnej platformy Azure?**

   Najpierw należy utworzyć maszyny wirtualnej tylko do systemu operacyjnego Linux. Następnie uruchom [kroki instalacji programu SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) dla dystrybucji systemu Linux. Jeśli nie instalujesz jedną z pełni licencjonowanej wersji programu SQL Server, również musi mieć licencję programu SQL Server i [przenośność licencji za pośrednictwem programu Software Assurance na platformie Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Dla programu SQL Server są obrazy maszyny wirtualnej systemu Linux Bring-Your-właścicielem-License (BYOL)?**

   W tej chwili brak nie BYOL Linux obrazów maszyny wirtualnej dla programu SQL Server. Jednak można ręcznie zainstalować program SQL Server na maszynie Wirtualnej systemu Linux — tylko do zgodnie z opisem w pytania Wstecz.

1. **Czy można zmienić maszyny Wirtualnej, aby użyć własnej licencji programu SQL Server, jeśli została utworzona z jednego z obrazów z galerii?**

   Nie. Nie można zmienić na przy użyciu własnej licencji Licencjonowanie płatności na minutę. Należy utworzyć nową maszynę Wirtualną systemu Linux, instalowania programu SQL Server i migrację danych. Zobacz na powyższe pytanie, aby uzyskać więcej informacji na temat przełączania własnej licencji.

1. **Co powiązanych pakietów programu SQL Server są również instalowane?**

   Aby wyświetlić pakiety programu SQL Server, które są instalowane domyślnie na maszynach wirtualnych systemu Linux serwera SQL, zobacz [zainstalowane pakiety](sql-server-linux-virtual-machines-overview.md#packages).

1. **Rozwiązania wysokiej dostępności programu SQL Server są obsługiwane na maszynach wirtualnych Azure?**

   Nie w tej chwili. Zawsze włączone grupy dostępności i klaster pracy awaryjnej wymagają klastrowania rozwiązania w systemie Linux, takich jak rozrusznik. Obsługiwane dystrybucje systemu Linux dla programu SQL Server nie obsługują ich dodatki wysokiej dostępności w chmurze.

1. **Dlaczego nie można udostępnić RHEL lub maszyna wirtualna SLES SQL Server subskrypcji platformy Azure, która jest objęta limitem wydatków?**

   Maszyny wirtualne RHEL i SLES wymagają subskrypcji z limitu wydatków i skojarzone z subskrypcją formę płatności zweryfikowano (zazwyczaj karty kredytowej). Jeśli dostarczasz RHEL lub SLES maszyny Wirtualnej bez usunięcia limitu wydatków, subskrypcja zostanie wyłączone i zatrzymanie wszystkich maszyn wirtualnych/usług. Jeśli zostanie uruchomione w tym stanie, aby ponownie włączyć subskrypcji [usunąć limit wydatków](https://account.windowsazure.com/subscriptions). Zostaną przywrócone z pozostałych środków dla bieżącego cyklu rozliczeniowego, ale RHEL lub wirtualna SLES przeciążenia obrazu zostanie umieszczona przed karty kredytowej Jeśli zdecydujesz się na ponowne uruchomienie i kontynuować jego uruchomieniem.

## <a name="resources"></a>Zasoby

**Maszyny wirtualne systemu Linux**:

* [Omówienie programu SQL Server na maszynie Wirtualnej systemu Linux](sql-server-linux-virtual-machines-overview.md)
* [Udostępnić Maszynę wirtualną systemu Linux serwera SQL](provision-sql-server-linux-virtual-machine.md)
* [Serwer SQL w dokumentacji systemu Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Maszyny wirtualne systemu Windows**:

* [Omówienie programu SQL Server w systemie Windows maszyny Wirtualnej](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Udostępnianie maszyn wirtualnych systemu Windows serwera SQL](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Często zadawane pytania (system Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)