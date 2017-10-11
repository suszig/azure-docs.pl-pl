---
title: "Tworzenie kopii zapasowej obciążeń programu DPM do klasycznego portalu Azure | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do tworzenia kopii zapasowej serwerów DPM za pomocą usługi Kopia zapasowa Azure"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: System Center Data Protection Manager, programu data protection manager, kopii zapasowych programu dpm
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;giridham;markgal
ms.openlocfilehash: a9a516cfdfaf4b95c4f0121a66e90f6e71206e9f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Przygotowywanie do tworzenia kopii zapasowych obciążeń na platformie Azure przy użyciu programu DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Serwer kopii zapasowej systemu Azure (klasyczne)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasyczne)](backup-azure-dpm-introduction-classic.md)
>
>

Ten artykuł zawiera wprowadzenie do używania kopia zapasowa Microsoft Azure w celu ochrony programu System Center Data Protection Manager (DPM) serwerów i obciążeń. Przeczytaj go, będzie zrozumieć:

* Jak działa kopii zapasowej serwera Azure DPM
* Wymagania wstępne dotyczące uzyskania płynnego środowisko tworzenia kopii zapasowej
* Napotkano błędy typowe i sposób postępowania z nimi
* Obsługiwane scenariusze

System Center DPM tworzy kopie zapasowe danych plików i aplikacji. Dane kopii zapasowej programu DPM można przechowywane na taśmie na dysku, lub kopię zapasową na platformie Azure w usłudze Kopia zapasowa Microsoft Azure. Program DPM współdziała z usługi Kopia zapasowa Azure w następujący sposób:

* **Program DPM wdrożony jako fizycznego serwera lub lokalnej maszyny wirtualnej** — Jeśli program DPM wdrożony jako serwera fizycznego lub lokalnej maszyny wirtualnej funkcji Hyper-V można tworzyć kopie zapasowe danych do magazynu usługi Kopia zapasowa Azure oprócz dysków i taśm kopii zapasowych.
* **Program DPM wdrożony jako maszyna wirtualna platformy Azure** — z programu System Center 2012 R2 z aktualizacją Update 3 programu DPM można wdrożyć jako maszynę wirtualną platformy Azure. Jeśli program DPM wdrożony jako maszyna wirtualna platformy Azure, które można tworzyć kopie zapasowe danych na dyskach platformy Azure dołączonych do maszyny wirtualnej platformy Azure programu DPM lub przez tworzenie kopii go do magazynu usługi Kopia zapasowa Azure można odciążyć magazyn danych.

## <a name="why-backup-your-dpm-servers"></a>Dlaczego warto utworzyć kopię zapasową serwerów DPM?
Korzyści biznesowe za pomocą usługi Kopia zapasowa Azure do wykonywania kopii zapasowych serwerów programu DPM obejmują:

* Lokalnego wdrożenia programu DPM można użyć kopii zapasowej systemu Azure zamiast wdrożenia długoterminowych na taśmie.
* W przypadku wdrożeń programu DPM na platformie Azure kopia zapasowa Azure umożliwia odciążyć magazyn z dysku platformy Azure, co umożliwia skalowanie w górę starsze dane są przechowywane w usłudze Kopia zapasowa Azure i nowych danych na dysku.

## <a name="how-does-dpm-server-backup-work"></a>Jak działa kopii zapasowej serwera DPM
Aby utworzyć kopię zapasową maszyny wirtualnej, najpierw w momencie migawki danych jest wymagana. Usługa Azure Backup Inicjuje zadania tworzenia kopii zapasowej w zaplanowanym terminie, a następnie wyzwala zapasowy numer wewnętrzny, aby utworzyć migawkę. Zapasowy numer wewnętrzny koordynuje w usłudze VSS gościa w celu osiągnięcia spójności i wywołuje interfejs API migawki obiektu blob usługi Azure Storage, po przekroczeniu spójności. Można to zrobić, aby uzyskać spójny migawek dysków maszyny wirtualnej bez konieczności zamykają ją.

Po podjęciu migawki, dane są przesyłane przez usługę kopia zapasowa Azure do magazynu kopii zapasowych. Usługa odpowiada on za identyfikacji i transferowanie tylko bloków, które zostały zmienione od ostatniej kopii zapasowej efektywne tworzenie magazynu kopii zapasowych i sieci. Po ukończeniu transferu danych migawki zostaną usunięte i utworzenia punktu odzyskiwania. Tego punktu odzyskiwania można wyświetlić w klasycznym portalu Azure.

> [!NOTE]
> Dla maszyn wirtualnych systemu Linux tylko plików kopii zapasowej spójnej jest możliwe.
>
>

## <a name="prerequisites"></a>Wymagania wstępne
Przygotuj kopia zapasowa Azure, aby utworzyć kopię zapasową danych programu DPM w następujący sposób:

1. **Utwórz magazyn kopii zapasowych**. Jeśli nie utworzono magazyn kopii zapasowych w ramach subskrypcji, zobacz wersji portalu Azure w tym artykule — [przygotować tworzenie kopii zapasowej obciążeń na platformie Azure za pomocą programu DPM](backup-azure-dpm-introduction.md).

  > [!IMPORTANT]
  > Począwszy od marca 2017 r. nie można już tworzyć magazynów kopii zapasowych za pomocą klasycznego portalu.
  > Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> Po 15 października 2017 r. nie będzie można tworzyć magazynów kopii zapasowych za pomocą programu PowerShell. **Do 1 listopada 2017 r.**:
  >- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
  >- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
  >

2. **Pobierz poświadczenia magazynu** — w kopii zapasowej systemu Azure, Przekaż utworzony certyfikat zarządzania do magazynu.
3. **Zainstaluj agenta kopii zapasowej Azure i Zarejestruj serwer** — z usługi Kopia zapasowa Azure, zainstaluj agenta na każdym serwerze DPM i zarejestrować serwer programu DPM w magazynie kopii zapasowych.

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a>Wymagania (i ograniczenia)
* Program DPM może działać jako serwer fizyczny lub maszyny wirtualnej funkcji Hyper-V zainstalowanych w programie System Center 2012 SP1 lub System Center 2012 R2. Mogą również działać jako maszyna wirtualna platformy Azure z programu System Center 2012 R2 z co najmniej programu DPM 2012 R2 Update Rollup 3 lub maszyny wirtualnej systemu Windows w środowisku programu VMWare na nim uruchomione w programie System Center 2012 R2 z pakietem zbiorczym aktualizacji 5.
* Jeśli używasz programu DPM z programu System Center 2012 z dodatkiem SP1, należy zainstalować pakiet zbiorczy aktualizacji 2 dla programu System Center Data Protection Manager z dodatkiem SP1. Jest to wymagane, aby można było zainstalować agenta kopii zapasowej Azure.
* Serwer programu DPM powinien mieć środowiska Windows PowerShell i .net Framework 4.5 zainstalowane.
* Program DPM można wykonać kopię zapasową większości obciążeń kopia zapasowa Azure. Pełną listę, co ma obsługiwane Zobacz Kopia zapasowa Azure obsługuje poniższe elementy.
* Nie można odzyskać danych przechowywanych w usłudze Kopia zapasowa Azure przy użyciu opcji "Kopiuj na taśmę".
* Konieczne będzie konto platformy Azure z włączoną funkcją kopia zapasowa Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Przeczytaj informacje o [cennikiem usługi Kopia zapasowa Azure](https://azure.microsoft.com/pricing/details/backup/).
* Za pomocą usługi Kopia zapasowa Azure wymaga agenta kopii zapasowej Azure można zainstalować na serwerach, które chcesz utworzyć kopię zapasową. Każdy serwer musi mieć co najmniej 10% rozmiaru danych, która jest tworzona kopia zapasowa, dostępne jako wolnego miejsca w magazynie lokalnym. Na przykład tworzenie kopii zapasowej 100 GB danych wymaga co najmniej 10 GB wolnego miejsca w lokalizacji pliki tymczasowe. W trakcie co najmniej 10% 15% wolnego miejsca używanego do lokalizacji pamięci podręcznej jest zalecane.
* Dane będą przechowywane w magazynie usługi Azure storage. Nie ma żadnego limitu ilości danych, które użytkownik może wykonać kopię zapasową kopia zapasowa Azure magazynu, ale rozmiar źródła danych (na przykład maszyny wirtualnej lub bazy danych) nie powinna przekraczać 54,400 GB.

Tworzenie kopii zapasowych Azure przypadku są obsługiwane następujące typy plików:

* Zaszyfrowane (tylko pełne kopie zapasowe)
* Skompresowane (obsługą przyrostowych kopii zapasowych)
* Rozrzedzone (obsługą przyrostowych kopii zapasowych)
* Skompresowane i rozrzedzone (traktowane jako rozrzedzone)

I te nie są obsługiwane:

* Serwery w systemach plików z uwzględnieniem wielkości liter nie są obsługiwane.
* Twarde linki (pomijane)
* (Pomijane) punkty ponownej analizy
* Zaszyfrowane i skompresowane (pomijane)
* Zaszyfrowane i rozrzedzone (pomijane)
* Skompresowany strumień
* Rozrzedzony strumień

> [!NOTE]
> Od w System Center 2012 DPM z dodatkiem SP1 lub nowszym, można tworzyć kopie zapasowe dużych obciążeń chronionych przez program DPM na platformie Azure przy użyciu kopii zapasowej Microsoft Azure.
>
>
