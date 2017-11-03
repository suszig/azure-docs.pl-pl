---
title: Kopia zapasowa i przywracanie programu SQL Server | Dokumentacja firmy Microsoft
description: "Zawiera opis zagadnień kopia zapasowa i przywracanie baz danych SQL Server uruchomiony na maszynach wirtualnych platformy Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: mikeray
ms.openlocfilehash: 65557938673c5442758396a47873be1016e0f71b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Tworzenie i przywracanie kopii zapasowych programu SQL Server na maszynach wirtualnych platformy Azure
## <a name="overview"></a>Omówienie
Magazyn Azure przechowuje 3 kopie każdego dysku maszyny Wirtualnej platformy Azure, aby zagwarantować ochronę przed utratą danych lub uszkodzenie danych fizycznych. W związku z tym w przeciwieństwie do lokalnego, nie musisz martwić się o tych. Jednak można powinien nadal kopie zapasowe baz danych programu SQL Server, aby zapewnić ochronę przed błędami aplikacji lub użytkownika (np. Wstawianie nieodpowiedniego lub usuwaniem tabel) oraz możliwość Przywracanie do punktu w czasie.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Dla programu SQL Server uruchomiony na maszynach wirtualnych Azure można używać natywnego wykonywania kopii zapasowych i przywracania techniki przy użyciu dołączonych dysków dla miejsca docelowego kopii zapasowej plików. Jednak istnieje limit liczby dysków, które można dołączyć do maszyny wirtualnej platformy Azure, na podstawie [rozmiar maszyny wirtualnej](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Istnieje również koszty Zarządzanie dyskami, aby wziąć pod uwagę.

Począwszy od programu SQL Server 2014, można utworzyć kopię zapasową i przywrócić do magazynu obiektów Blob Microsoft Azure. SQL Server 2016 udostępnia również rozszerzeń dla tej opcji. Ponadto plików bazy danych przechowywanych w magazynie obiektów Blob Microsoft Azure, programu SQL Server 2016 udostępnia opcję dla niemal natychmiastowe tworzenie kopii zapasowych i szybkiego przywracania przy użyciu migawek Azure. W tym artykule omówiono te opcje i dodatkowe informacje można znaleźć w folderze [programu SQL Server z kopii zapasowej i przywracania usługi magazynu obiektów Blob platformy Microsoft Azure](https://msdn.microsoft.com/library/jj919148.aspx).

> [!NOTE]
> Omówienie opcji tworzenia kopii zapasowych bardzo dużych baz danych, zobacz [Terabajtowe programu SQL Server bazy danych kopii zapasowej strategii dla maszyn wirtualnych Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).
> 
> 

Poniższe sekcje zawierają informacje specyficzne dla różnych wersji programu SQL Server obsługiwane przez maszynę wirtualną platformy Azure.

## <a name="sql-server-virtual-machines"></a>Maszyny wirtualne serwera SQL
Kiedy wystąpienia programu SQL Server jest uruchomiona na maszynie wirtualnej platformy Azure, plików bazy danych już znajdują się na dysku z danymi na platformie Azure. Te dyski na żywo w magazynie obiektów Blob Azure. Dlatego powodów tworzenia kopii zapasowej bazy danych i metodami zająć zmiany nieco. Rozważ następujące opcje. 

* Nie trzeba wykonywać kopie zapasowe bazy danych, aby zapewnić ochronę przed awariami sprzętu lub nośnika, ponieważ Microsoft Azure umożliwia ochronę w ramach usługi Microsoft Azure.
* Nadal trzeba wykonać kopie zapasowe bazy danych w celu zapewnienia ochrony przed błędy użytkownika lub archiwizacji, powodów przepisami lub celów administracyjnych.
* Plik kopii zapasowej można przechowywać bezpośrednio na platformie Azure. Aby uzyskać więcej informacji zobacz następujące sekcje, które zawierają wskazówki dotyczące różnych wersji programu SQL Server.

## <a name="sql-server-2016"></a>SQL Server 2016
Microsoft SQL Server 2016 obsługuje [kopia zapasowa i przywracanie z obiektami blob Azure](https://msdn.microsoft.com/library/jj919148.aspx) znaleziono funkcji w programie SQL Server 2014. Ale są również następujące ulepszenia:

| Rozszerzenie 2016 | Szczegóły |
| --- | --- |
| **Rozkładanie** |Podczas wykonywania kopii zapasowej do magazynu obiektów blob Microsoft Azure, programu SQL Server 2016 obsługuje tworzenie kopii zapasowych na wiele obiektów blob, aby umożliwić tworzenie kopii zapasowej dużych baz danych, maksymalnie 12,8 TB. |
| **Kopia zapasowa migawki** |Przy użyciu platformy Azure migawki kopii zapasowej migawki pliku programu SQL Server zapewnia niemal natychmiastowe tworzenie kopii zapasowych i szybkiego przywracania plików bazy danych przechowywane przy użyciu usługi magazynu obiektów Blob platformy Azure. Ta funkcja umożliwia uproszczenie kopii zapasowej i przywracanie zasad. Kopia zapasowa migawki pliku obsługuje również punktu w czasie przywracania. Aby uzyskać więcej informacji, zobacz [kopii zapasowych migawki plików bazy danych na platformie Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Zarządzane Planowanie tworzenia kopii zapasowej** |SQL Server zarządzanej kopii zapasowej Azure obsługuje teraz niestandardowe harmonogramy. Aby uzyskać więcej informacji, zobacz [programu SQL Server zarządzanej kopii zapasowej Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). |

Samouczek funkcji programu SQL Server 2016 przy użyciu magazynu obiektów Blob platformy Azure, zobacz [samouczek: Korzystanie z usługi magazynu obiektów Blob Microsoft Azure z bazami danych programu SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014
SQL Server 2014 zawiera następujące udoskonalenia:

1. **Kopia zapasowa i przywracanie Azure**:
   
   * *SQL Server Backup to URL* ma teraz obsługę programu SQL Server Management Studio. Opcję, aby utworzyć kopię zapasową na platformie Azure jest teraz dostępna, korzystając z zadania tworzenia kopii zapasowej lub przywracania lub kreatora w programie SQL Server Management Studio. Aby uzyskać więcej informacji, zobacz [kopii zapasowej serwera SQL do adresu URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
   * *Serwer zarządzany kopii zapasowej SQL Azure* ma nową funkcję, która umożliwia automatyczne zarządzanie kopii zapasowej. Jest to szczególnie przydatne w przypadku automatyzacji tworzenia kopii zapasowej zarządzania dla wystąpień programu SQL Server 2014 uruchomione na maszynie platformy Azure. Aby uzyskać więcej informacji, zobacz [programu SQL Server zarządzanej kopii zapasowej Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
   * *Automatyczna usługa Backup* udostępnia dodatkowe automatyzacji można automatycznie włączyć *programu SQL Server zarządzanej kopii zapasowej Azure* na wszystkich istniejących i nowych baz danych dla maszyny Wirtualnej programu SQL Server na platformie Azure. Aby uzyskać więcej informacji, zobacz [Automated Backup for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md) (Automatyczne tworzenie kopii zapasowych dla programu SQL Server w usłudze Azure Virtual Machines).
   * Przegląd wszystkich opcji dla programu SQL Server 2014 kopii zapasowej Azure, zobacz [programu SQL Server z kopii zapasowej i przywracania usługi magazynu obiektów Blob platformy Microsoft Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
2. **Szyfrowanie**: SQL Server 2014 obsługuje szyfrowanie danych podczas tworzenia kopii zapasowej. Obsługuje wiele algorytmów szyfrowania i użycie osf certyfikatu lub klucza asymetrycznego. Aby uzyskać więcej informacji, zobacz [szyfrowania kopii zapasowych](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012
Aby uzyskać szczegółowe informacje o kopii zapasowej serwera SQL i przywracania danych w programie SQL Server 2012, zobacz [kopii zapasowej i przywracanie z serwera bazy danych SQL (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Począwszy od programu SQL Server 2012 z dodatkiem SP1 Cumulative Update 2, tworzenia kopii zapasowych i przywracania usługi Magazyn obiektów Blob Azure. To rozszerzenie pozwala utworzyć kopię zapasową bazy danych programu SQL Server na serwerze SQL Server uruchomiony na maszynie wirtualnej platformy Azure lub lokalnego wystąpienia. Aby uzyskać więcej informacji, zobacz [programu SQL Server z kopii zapasowej i przywracania usługi magazynu obiektów Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Niektóre korzyści wynikające ze stosowania usługi magazynu obiektów Blob platformy Azure obejmują możliwość obejścia limit 16 dysków dołączonych dysków, łatwość zarządzania, dostępność bezpośredniego plik kopii zapasowej do innego wystąpienia programu SQL Server uruchomiony na maszynie wirtualnej platformy Azure lub lokalnego wystąpienia dla celów odzyskiwania po awarii lub migracji. Aby zapoznać się z pełną listą korzyści wynikające z używania usługi magazynu obiektów blob platformy Azure do przechowywania kopii zapasowych programu SQL Server, zobacz *korzyści* sekcji [programu SQL Server z kopii zapasowej i przywracania usługi magazynu obiektów Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Aby uzyskać zalecenia dotyczące najlepszych rozwiązań i informacje dotyczące rozwiązywania problemów, zobacz [kopii zapasowej i przywracanie najlepszych rozwiązań (usługi magazynu obiektów Blob Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008
Dla programu SQL Server z kopii zapasowej i przywracania danych w programie SQL Server 2008 R2, zobacz [wykonywanie kopii zapasowej i przywracanie baz danych programu SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Dla kopii zapasowej serwera SQL i przywracania danych w programie SQL Server 2008, zobacz [wykonywanie kopii zapasowej i przywracanie baz danych programu SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Następne kroki
Jeśli planowania wdrożenia programu SQL Server w maszynie Wirtualnej platformy Azure, możesz znaleźć wskazówki dotyczące inicjowania obsługi administracyjnej w następujących instrukcji: [inicjowania obsługi maszyny wirtualnej programu SQL Server na platformie Azure za pomocą Menedżera zasobów Azure](virtual-machines-windows-portal-sql-server-provision.md).

Chociaż kopii zapasowej i przywracania może służyć do migracji danych, istnieją potencjalnie łatwiejsze ścieżki migracji danych do programu SQL Server na maszynie Wirtualnej platformy Azure. Pełne omówienie opcji migracji i zalecenia, zobacz [Migrowanie bazy danych do programu SQL Server na maszynie Wirtualnej platformy Azure](virtual-machines-windows-migrate-sql.md).

Przejrzyj inne [zasobów uruchamiania programu SQL Server w usłudze Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

