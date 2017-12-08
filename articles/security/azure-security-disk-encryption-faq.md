---
title: "Szyfrowanie dysków Azure — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące programu Microsoft Azure dysku szyfrowanie dla systemu Windows i maszyn wirtualnych systemu Linux IaaS."
services: security
documentationcenter: na
author: deventiwari
manager: avibm
editor: yuridio
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw
ms.openlocfilehash: 2ccadfdec0e653264671f5a9a38d4541b0fc4e69
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-disk-encryption-faq"></a>Szyfrowanie dysków Azure — często zadawane pytania

Ten artykuł zawiera odpowiedzi na często zadawane pytania (FAQ) dotyczące Azure dysku szyfrowanie dla systemu Windows i maszyn wirtualnych systemu Linux IaaS. Aby uzyskać więcej informacji na temat tej usługi, zobacz [Azure dysku szyfrowanie dla systemu Windows i maszyn wirtualnych systemu Linux IaaS](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Pytania ogólne
**Pytanie:** w ogólnodostępnej (GA) w przypadku szyfrowania dysków Azure?

**Odpowiedź:** Azure dysku szyfrowanie dla systemu Windows i maszyn wirtualnych systemu Linux IaaS jest ogólnie dostępności we wszystkich regionach publicznej Azure.

**Pytanie:** napotyka użytkownika są dostępne w przypadku szyfrowania dysków Azure?

**Odpowiedź:** GA szyfrowania dysków Azure obsługuje szablonów usługi Azure Resource Manager, programu Azure PowerShell i interfejsu wiersza polecenia Azure. Zapewnia dużą elastyczność. Możliwe są trzy różne umożliwiających szyfrowanie dysku dla maszyn wirtualnych IaaS. Aby uzyskać więcej informacji na środowisko użytkownika i wskazówki krok po kroku, które są dostępne w szyfrowania dysków Azure Zobacz szyfrowania dysków Azure wdrożeń i środowisk.

**Pytanie:** ile kosztuje szyfrowania dysków Azure?

**Odpowiedź:** bezpłatnie szyfrowania dysków maszyny Wirtualnej przy użyciu szyfrowania dysków Azure nie istnieje.

**Pytanie:** warstwy maszyny wirtualnej, które szyfrowania dysków Azure obsługuje?

**Odpowiedź:** szyfrowania dysków Azure jest dostępna na maszynach wirtualnych warstwy standardowa, łącznie z [A, D DS, G, GS i F](https://azure.microsoft.com/pricing/details/virtual-machines/) serii maszyn wirtualnych IaaS. Jest również dostępna dla maszyn wirtualnych z magazyn w warstwie premium. Nie jest dostępne w warstwie podstawowa maszyn wirtualnych.

**Pytanie:** dystrybucje systemu Linux co to jest obsługa szyfrowania dysków Azure?

**Odpowiedź:** szyfrowania dysków Azure jest obsługiwane w następujących dystrybucje serwera systemu Linux i wersji:

| Dystrybucja systemu Linux | Wersja | Typ woluminu obsługiwany w przypadku szyfrowania|
| --- | --- |--- |
| Ubuntu | 16.04 — CODZIENNIE LTS | Dysk systemu operacyjnego i danych |
| Ubuntu | 14.04.5-DAILY-LTS | Dysk systemu operacyjnego i danych |
| RHEL | 7.3 | Dysk systemu operacyjnego i danych |
| RHEL | 7.2 | Dysk systemu operacyjnego i danych |
| RHEL | 6.8 | Dysk systemu operacyjnego i danych |
| RHEL | 6.7 | Dysk z danymi |
| CentOS | 7.3 | Dysk systemu operacyjnego i danych |
| CentOS | 7.2n | Dysk systemu operacyjnego i danych |
| CentOS | 6.8 | Dysk systemu operacyjnego i danych |
| CentOS | 7.1 | Dysk z danymi |
| CentOS | 7.0 | Dysk z danymi |
| CentOS | 6.7 | Dysk z danymi |
| CentOS | 6.6 | Dysk z danymi |
| CentOS | 6.5 | Dysk z danymi |
| openSUSE | 13.2 | Dysk z danymi |
| SLES | 12 Z DODATKIEM SP1 | Dysk z danymi |
| SLES | Priorytet: 12-z dodatkiem SP1 | Dysk z danymi |
| SLES | HPC 12 | Dysk z danymi |
| SLES | Priorytet: 11-SP4 | Dysk z danymi |
| SLES | 11 Z DODATKIEM SP4 | Dysk z danymi |

**Pytanie:** jak umożliwić, za pomocą szyfrowania dysków Azure?

**A:** na początek przeczytaj [Azure dysku szyfrowanie dla systemu Windows i maszyn wirtualnych systemu Linux IaaS](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) oficjalny dokument.

**Pytanie:** można zaszyfrować woluminów zarówno rozruchu i danych za pomocą szyfrowania dysków Azure?

**Odpowiedź:** tak, można zaszyfrować woluminy rozruchowe i danych systemu Windows i maszyn wirtualnych systemu Linux IaaS. Dla maszyn wirtualnych systemu Windows nie można zaszyfrować dane bez pierwszy szyfrowania woluminu systemu operacyjnego. Dla maszyn wirtualnych systemu Linux można zaszyfrować ilość danych, bez konieczności najpierw szyfrowania woluminu systemu operacyjnego. Po zaszyfrowanych woluminu systemu operacyjnego Linux wyłączenie szyfrowania na woluminie systemu operacyjnego dla maszyn wirtualnych systemu Linux IaaS nie jest obsługiwane.

**Pytanie:** jest szyfrowania dysków Azure umożliwiają Przełącz możliwości klucza (BYOK)?

**Odpowiedź:** tak, możesz podać własne klucze szyfrowania kluczy. Te klucze są chronione w usłudze Azure Key Vault, która jest magazynu kluczy do szyfrowania dysków Azure. Aby uzyskać więcej informacji o kluczach szyfrowania klucza obsługi scenariuszy, zobacz scenariusze wdrażania szyfrowania dysków Azure i środowiska.

**Pytanie:** można użyć klucza szyfrowania klucza utworzone Azure?

**Odpowiedź:** tak, można użyć usługi Azure Key Vault, aby wygenerować klucz szyfrowania do użycia szyfrowania dysków Azure. Te klucze są chronione w usłudze Azure Key Vault, która jest magazynu kluczy do szyfrowania dysków Azure. Aby uzyskać więcej informacji o scenariuszach Obsługa kluczy szyfrowania Zobacz szyfrowania dysków Azure wdrożeń i środowisk.

**Pytanie:** można za pomocą usługi zarządzania kluczami lokalnymi lub modułu HSM do ochrony kluczy szyfrowania?

**Odpowiedź:** lokalną usługą zarządzania kluczami lub HSM nie można użyć do ochrony kluczy szyfrowania z szyfrowania dysków Azure. Usługa Azure Key Vault służy tylko do ochrony kluczy szyfrowania. Aby uzyskać więcej informacji o scenariuszach Obsługa kluczy szyfrowania Zobacz szyfrowania dysków Azure wdrożeń i środowisk.

**Pytanie:** jakie są wymagania wstępne, aby skonfigurować szyfrowanie dysków Azure?

**Odpowiedź:** jest skrypt programu PowerShell wymagań wstępnych. Za pomocą tego skryptu można utworzyć aplikację usługi Azure Active Directory, Utwórz nowy magazyn kluczy lub skonfigurować istniejący magazyn kluczy na dostęp do szyfrowania dysku włączyć szyfrowanie i ochronę kluczy tajnych i kluczy. Aby uzyskać więcej informacji o scenariuszach Obsługa kluczy szyfrowania Zobacz wymagań wstępnych szyfrowania dysków Azure i scenariusze wdrażania i środowiska.

**Pytanie:** gdzie można uzyskać więcej informacji na temat sposobu konfigurowania szyfrowania dysków Azure przy użyciu programu PowerShell?

**Odpowiedź:** mamy niektóre dużą artykuły, w jaki sposób można wykonywać podstawowe zadania szyfrowania dysków Azure, a także bardziej zaawansowanych scenariuszy. Do podstawowych zadań, zobacz [Eksploruj szyfrowania dysków Azure przy użyciu programu Azure PowerShell — część 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Aby uzyskać bardziej zaawansowanych scenariuszy, zobacz [Eksploruj szyfrowania dysków Azure przy użyciu programu Azure PowerShell — część 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

**Pytanie:** jakiej wersji programu Azure PowerShell szyfrowania dysków Azure obsługuje?

**Odpowiedź:** używać najnowszej wersji zestawu SDK platformy Azure PowerShell do konfigurowania szyfrowania dysków Azure. Pobierz najnowszą wersję [programu Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Szyfrowanie dysków Azure jest *nie* obsługiwane przez zestaw Azure SDK w wersji 1.1.0.

> [!NOTE]
> Rozszerzenie podglądu szyfrowania dysku systemu Linux platformy Azure jest przestarzały. Aby uzyskać więcej informacji, zobacz [rozszerzenie Podgląd szyfrowania dysku wycofano Azure dla maszyn wirtualnych systemu Linux IaaS](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

**Pytanie:** I zastosować szyfrowanie dysków Azure na mój niestandardowego obrazu systemu Linux?

**Odpowiedź:** szyfrowania dysków Azure nie można zastosować do niestandardowego obrazu systemu Linux. Firma Microsoft obsługuje tylko obrazy Linux galerii obsługiwanych dystrybucji wcześniej o nazwie. Niestandardowe obrazy systemu Linux nie jest obecnie obsługiwana.

**Pytanie:** I aktualizacje można stosować do Red Hat Maszynę wirtualną systemu Linux używającej aktualizacji yum?

**Odpowiedź:** tak, można wykonać operacji aktualizacji lub poprawka maszyny Wirtualnej systemu Linux Red Hat. Aby uzyskać więcej informacji, zobacz [stosowania aktualizacji do zaszyfrowanej IaaS Red Hat maszyny Wirtualnej platformy Azure przy użyciu aktualizacji yum](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

**Pytanie:** co to jest przepływ pracy szyfrowania dysków Azure zalecany dla systemu Linux?

**Odpowiedź:** poniższy przepływ pracy jest zalecane, aby uzyskać najlepsze rezultaty w systemie Linux:
* Rozpocznij od obrazu niezmodyfikowanego galerii standardowych odpowiadającej żądanej distro systemu operacyjnego i wersji
* Utwórz kopię zapasową zainstalowanych dyskach, które będą szyfrowane.  Umożliwia odzyskiwanie w razie awarii, na przykład jeśli maszyna wirtualna jest ponowny rozruch szyfrowania została ukończona.
* Szyfrowanie (może zająć wiele godzin lub nawet przez kilka dni w zależności od właściwości maszyny wirtualnej i rozmiar wszystkich dysków dołączonych danych)
* Dostosowywanie, a w razie potrzeby dodaj oprogramowania do obrazu.

Jeśli ten przepływ pracy nie jest możliwe, opierając się na [szyfrowanie usługi Magazyn](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption) (SSE) w magazynie platformy warstwy konto może być zamiast pełne szyfrowanie dysków za pomocą dm-crypt.

**Pytanie:** co to jest dysk "Wolumin Bek" lub "/ mnt/azure_bek_disk"?

**Odpowiedź:** "Bek wolumin" dla systemu Windows lub "/ mnt/azure_bek_disk" dla systemu Linux jest woluminem danych lokalnych, służący do bezpiecznego przechowywania kluczy szyfrowania dla zaszyfrowanych IaaS maszynach wirtualnych platformy Azure.
> [!NOTE]
> Nie należy usuwać ani edytować zawartość w tym dysku. Odinstalowuje dysku, ponieważ obecności klucza szyfrowania jest potrzebny dla wszystkich operacji szyfrowania na Maszynie wirtualnej IaaS.

**Pytanie:** I gdzie mogą aby zadać pytania lub wyrazić swoją opinię?

**Odpowiedź:** można zadawać pytania i wyrazić swoją opinię na [forum szyfrowania dysków Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Następne kroki
W tym dokumencie przedstawiono więcej informacji na temat najczęściej zadawane pytania dotyczące szyfrowania dysków Azure. Aby uzyskać więcej informacji na temat tej usługi i jego możliwości zobacz następujące artykuły:

- [Zastosuj szyfrowanie dysków w Centrum zabezpieczeń Azure](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Szyfrowanie maszyny wirtualnej platformy Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Szyfrowanie przechowywanych danych Azure](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
