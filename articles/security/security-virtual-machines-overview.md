---
title: "Funkcje zabezpieczeń platformy Azure używana z maszyn wirtualnych platformy Azure | Dokumentacja firmy Microsoft"
description: " Przegląd podstawowe funkcje zabezpieczeń platformy Azure, które mogą być używane z maszyn wirtualnych platformy Azure. Maszyny wirtualne platformy Azure zapewniają elastyczność wirtualizacji bez konieczności kupowania i utrzymywania fizycznego sprzętu, na którym działa maszyna wirtualna. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: terrylan
ms.openlocfilehash: f1fb7f876c7dc010c03f01a4f6698ddc18da1100
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-virtual-machines-security-overview"></a>Omówienie zabezpieczeń usługi Azure maszyny wirtualne
Usługa Azure Virtual Machines umożliwia elastyczne wdrażanie szerokiego zakresu rozwiązań obliczeniowych. Dzięki obsłudze rozwiązań Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM i SAP oraz usługi BizTalk Services na platformie Azure możesz wdrożyć dowolne obciążenie w dowolnym języku i w prawie każdym systemie operacyjnym.

Maszyna wirtualna Azure umożliwia swobodne korzystanie z wirtualizacji bez konieczności kupowania i utrzymywania fizycznego sprzętu potrzebnego do działania maszyny wirtualnej.  Możesz skompilować i wdrożyć aplikacje za zapewnienie, że dane są chronione i bezpieczne w naszych centrach danych w bardzo bezpieczny.

Przy użyciu platformy Azure można tworzyć rozwiązania z rozszerzonymi zabezpieczeniami, zgodne który:

* Ochrona maszyn wirtualnych przed wirusami i złośliwym oprogramowaniem
* Szyfrowanie poufnych danych
* Zabezpieczanie ruchu sieciowego
* Identyfikowanie i wykrywanie zagrożeń
* Spełnianie wymagań dotyczących zgodności

Celem tego artykułu jest zapewnienie omówienie podstawowe funkcje zabezpieczeń platformy Azure, które mogą być używane z maszyn wirtualnych. Firma Microsoft udostępnia również łącza do artykułów, które zapewniają szczegółowe informacje dotyczące każdej funkcji, aby dowiedzieć się więcej.  

Podstawowe funkcje zabezpieczeń maszyny wirtualnej Azure mają być uwzględnione w tym artykule:

* Oprogramowanie chroniące przed złośliwym kodem
* Sprzętowy moduł zabezpieczeń
* Szyfrowanie dysków maszyny wirtualnej
* Kopia zapasowa maszyny wirtualnej
* Azure Site Recovery
* Sieci wirtualne
* Zarządzanie zasadami zabezpieczeń i raportowanie
* Zgodność

## <a name="antimalware"></a>Oprogramowanie chroniące przed złośliwym kodem
Przy użyciu platformy Azure ochrony przed złośliwym oprogramowaniem z dostawcami zabezpieczeń, takich jak Microsoft, firmy Symantec, Trend Micro i Kaspersky służy do ochrony maszyn wirtualnych z złośliwych plików, adware i innymi zagrożeniami. W sekcji Dowiedz się więcej poniżej, aby znaleźć artykuły dotyczące partnera rozwiązania.

Antimalware firmy Microsoft dla usług Azure Cloud Services i maszyn wirtualnych jest możliwość ochrony w czasie rzeczywistym, która pomaga w identyfikacji i usuwania wirusy, programy szpiegujące lub inne złośliwe oprogramowanie.  Microsoft Antimalware udostępnia można skonfigurować alerty, gdy wiadomo, że złośliwego lub niechcianego oprogramowania próbuje się zainstalować lub uruchomić w systemie Azure.

Microsoft Antimalware to rozwiązanie jednego agenta dla aplikacji i dzierżawy w środowiskach, przeznaczony do pracy w tle bez udziału człowieka. Można wdrożyć ochrony oparte na potrzeby obciążeń aplikacji, z albo podstawowe secure domyślnie lub Zaawansowane Konfiguracja niestandardowa, w tym monitorowania ochrony przed złośliwym oprogramowaniem.

Podczas wdrażania i włączyć Microsoft Antimalware dostępne są następujące funkcje podstawowe:

* Ochrona w czasie rzeczywistym - monitorów działania usług w chmurze i maszyn wirtualnych do wykrywania i blokowania złośliwego oprogramowania wykonywania.
* Okresowo zaplanowane skanowanie — wykonuje skanowanie docelowe do wykrywania złośliwego oprogramowania, włącznie z aktywnie działającymi programy.
* Korygowania złośliwego oprogramowania — automatycznie pobiera wykrytego złośliwego oprogramowania, takich jak usuwanie lub poddawania złośliwych plików i czyszczenie wpisy rejestru złośliwe działania.
* Aktualizacje podpisu — automatycznie instaluje najnowsze podpisy ochrony (definicje wirusów), aby zapewnić ochronę jest aktualny z wstępnie określoną częstotliwością.
* Aktualizacje aparatu ochrony przed złośliwym kodem — automatycznie aktualizuje aparat Antimalware firmy Microsoft.
* Aktualizacje platformy ochrony przed złośliwym kodem — automatycznie aktualizuje platformy Microsoft Antimalware.
* Active protection - raporty do metadanych telemetrii Azure o wykrytych zagrożeń i podejrzane zasobów w celu zapewnienia szybkiego odpowiedzi i umożliwia dostarczanie w czasie rzeczywistym synchroniczne podpisu za pomocą Microsoft Active Protection System (MAPS).
* Przykłady raportowania — zapewnia i raporty przykłady usługi Microsoft Antimalware ułatwia uściślić usługi i włączyć rozwiązywania problemów.
* Wykluczenia — pozwala na konfigurowanie niektórych plików procesów, aplikacji i Administratorzy usługi i dyski, aby wykluczyć je z ochrony i skanowanie w poszukiwaniu wydajności i innych powodów.
* Zbieranie zdarzeń ochrony przed złośliwym kodem — rejestruje kondycję usługi ochrony przed złośliwym kodem, podejrzanych działań i korygowania akcje wykonywane w dzienniku zdarzeń systemu operacyjnego i zbiera je do konta usługi Azure Storage klienta.

Dowiedz się więcej: Aby dowiedzieć się więcej na temat ochrony przed złośliwym oprogramowaniem w celu ochrony maszyn wirtualnych, zobacz:

* [Ochrony przed złośliwym oprogramowaniem firmy Microsoft dla usług w chmurze Azure i maszyn wirtualnych](azure-security-antimalware.md)
* [Wdrażanie rozwiązań do ochrony przed złośliwym kodem na maszynach wirtualnych platformy Azure](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Jak zainstalować i skonfigurować Trend Micro głębokie zabezpieczeń jako usługa na maszynie Wirtualnej systemu Windows](../virtual-machines/windows/classic/install-trend.md)
* [Jak zainstalować i skonfigurować Symantec Endpoint Protection na maszynie Wirtualnej systemu Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Rozwiązań zabezpieczeń w portalu Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Zabezpieczenia sprzętowe modułu
Zabezpieczenia uwierzytelniania i szyfrowania może zostać poprawione skracając klucza zabezpieczeń. Można uprościć zarządzanie i bezpieczeństwo kluczy i kluczy tajnych krytyczne przez zapisanie ich w usłudze Azure Key Vault. Usługa Key Vault oferuje możliwość przechowywania kluczy w sprzętowych modułach zabezpieczeń z certyfikatami poświadczającymi zgodność ze standardami FIPS 140-2 (poziom 2). Klucze szyfrowania programu SQL Server do utworzenia kopii zapasowej lub [przezroczystego szyfrowania danych](https://msdn.microsoft.com/library/bb934049.aspx) wszystkie można przechowywać w magazynie kluczy z kluczy ani kluczy tajnych z aplikacji. Zarządzać uprawnień i dostępu do tych elementów chronionych za pomocą [usługi Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Więcej informacji:

* [Co to jest usługa Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Wprowadzenie do usługi Azure Key Vault](../key-vault/key-vault-get-started.md)
* [Blog Azure Key Vault](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Szyfrowanie dysków maszyny wirtualnej
Szyfrowanie dysków Azure to nowa funkcja umożliwia zaszyfrowanie dysków systemu Windows i Linux Azure Virtual Machine. Szyfrowanie dysków Azure korzysta ze standardu branżowego [funkcji BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkcji systemu Windows i [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkcji systemu Linux w celu zapewnienia szyfrowania woluminów systemu operacyjnego i dysków z danymi.

Rozwiązanie jest zintegrowany z usługą Azure Key Vault ułatwia kontrolowanie i zarządzanie nimi klucze szyfrowania dysku i kluczy tajnych w ramach subskrypcji magazynu kluczy, zapewniając, że wszystkie dane na dyskach maszyny wirtualnej są szyfrowane, gdy w magazynie Azure.

Więcej informacji:

* [Szyfrowanie dysków Azure dla systemu Windows i maszyn wirtualnych systemu Linux IaaS](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Szyfrowanie dysków Azure dla maszyn wirtualnych systemu Windows i Linux](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Szyfrowanie maszyny wirtualnej](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Kopia zapasowa maszyny wirtualnej
Azure Backup to skalowalne rozwiązanie chroniące dane aplikacji, które nie wymaga żadnych inwestycji kapitałowych i cechujące się minimalnymi kosztami operacyjnymi. Błędy aplikacji mogą powodować uszkodzenia danych, a błędy użytkowników — usterki aplikacji. Kopia zapasowa Azure maszynach wirtualnych z systemem Windows i Linux są chronione.

Więcej informacji:

* [Co to jest Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Ścieżka szkoleniowa kopii zapasowej systemu Azure](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Usługa Kopia zapasowa Azure — często zadawane pytania](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Ważną częścią strategii BCDR organizacji stanowi poznanie sposobu na utrzymanie działających obciążeń i aplikacji firmowych podczas zaplanowanych przerw i nieplanowanych awarii. Usługa Azure Site Recovery pomaga organizowania replikacji, trybu failover i odzyskiwania obciążeń i aplikacji, tak aby były dostępne z lokalizacji dodatkowej jeśli spadnie do lokalizacji głównej.

Usługa Site Recovery:

* **Upraszcza strategii BCDR** — Usługa Site Recovery ułatwia obsługę replikacji, trybu failover i odzyskiwania wielu firmowych obciążeń i aplikacji z jednej lokalizacji. Usługa Site Recovery organizuje replikację i tryb failover, ale nie przechwytuje danych aplikacji ani nie zbiera żadnych informacji ich dotyczących.
* **Zapewnia elastyczne replikacji** — przy użyciu usługi Site Recovery może replikować obciążenia uruchomione na maszynach wirtualnych funkcji Hyper-V, maszyny wirtualne VMware i serwery fizyczne z systemem Windows lub Linux.
* **Obsługuje tryb failover i odzyskiwania** — Usługa Site Recovery zapewnia testowy tryb failover do obsługi testowanie odzyskiwania po awarii bez wywierania wpływu na środowiska produkcyjne. Możesz również uruchomić planowane tryby failover (brak utraty danych) w przypadku przewidywanych przerw w działaniu lub nieplanowane tryby failover (minimalna utrata danych, zależna od częstotliwości replikacji) w przypadku nieoczekiwanych awarii. Po przejściu do trybu failover można powrócić do lokacji głównych. Usługa Site Recovery zapewnia plany odzyskiwania, które mogą uwzględniać skrypty i skoroszyty automatyzacji platformy Azure. Dzięki nim możesz dostosować tryb failover i odzyskiwanie dla aplikacji wielowarstwowych.
* **Eliminuje dodatkowego centrum danych** — można replikować do lokacji dodatkowej lokalnymi lub na platformie Azure. Za pomocą platformy Azure jako miejsca docelowego dla odzyskiwania po awarii eliminuje koszty i z utrzymywaniem lokacji dodatkowej. Replikowane dane są przechowywane w usłudze Azure Storage.
* **Integruje się z istniejącymi technologiami BCDR** — Usługa Site Recovery współpracuje z innymi funkcjami BCDR aplikacji. Na przykład można użyć usługi Site Recovery do ochrony wewnętrznej programu SQL Server obciążeń firmowych. W tym macierzystą obsługę AlwaysOn programu SQL Server zarządzać trybem failover grup dostępności.

Więcej informacji:

* [Co to jest Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Jak działa usługa Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Jakie obciążenia są chronione przez usługę Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Sieci wirtualne
Maszyny wirtualne muszą łączność sieciową. W celu spełnienia tego wymagania, platforma Azure wymaga maszyn wirtualnych, które będą podłączone do sieci wirtualnej platformy Azure. Sieci wirtualnej platformy Azure jest konstrukcją logiczną, rozszerzający fizycznej Azure sieci szkieletowej. Każdy logicznej sieci wirtualnej platformy Azure jest odizolowana od wszystkich innych sieciach wirtualnych platformy Azure. Izolacja pomaga upewnić się, że ruch sieciowy we wdrożeniach nie jest dostępna dla innych klientów firmy Microsoft Azure.

Więcej informacji:

* [Przegląd zabezpieczeń sieci platformy Azure](security-network-overview.md)
* [Omówienie sieci wirtualnej](../virtual-network/virtual-networks-overview.md)
* [Funkcje sieci i powiązań dla scenariuszy dla przedsiębiorstw](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Zarządzanie zasadami zabezpieczeń i raportowanie
Centrum zabezpieczeń Azure ułatwia zapobieganie, wykrywania i reagowania na zagrożenia i zapewnia zwiększyć widoczność i kontrolę nad, zabezpieczeń zasobów platformy Azure. Zapewnia zintegrowane monitorowanie zabezpieczeń i zarządzanie zasadami subskrypcji platformy Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie mogłyby pozostać niezauważone, a także współpracuje z szerokim ekosystemem rozwiązań z zakresu zabezpieczeń.

Centrum zabezpieczeń Azure ułatwia optymalizacji i monitorowania zabezpieczeń maszyny wirtualnej:

* Zapewnianie maszyny wirtualnej [zalecenia dotyczące zabezpieczeń](../security-center/security-center-recommendations.md) takich jak stosowanie aktualizacji systemu skonfigurować listy ACL punktów końcowych, włączanie ochrony przed złośliwym kodem, Włącz grup zabezpieczeń sieci i zastosować szyfrowanie dysku.
* Monitorowanie stanu maszyn wirtualnych

Więcej informacji:

* [Wprowadzenie do Centrum zabezpieczeń Azure](../security-center/security-center-intro.md)
* [Centrum zabezpieczeń Azure — często zadawane pytania](../security-center/security-center-faq.md)
* [Planowanie Centrum zabezpieczeń Azure i operacje](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Zgodność
Maszyny wirtualne platformy Azure jest certyfikowany do FISMA, FedRAMP HIPAA, PCI DSS poziom 1 i innych programów zgodności kluczy. Certyfikat ułatwia dla aplikacji platformy Azure spełnić wymagania zgodności i dla Twojej firmy w celu rozwiązania szeroką gamę krajowych i międzynarodowych przepisów wymagania.

Więcej informacji:

* [Centrum zaufania Microsoft: zgodności](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Zaufanych chmury: Microsoft Azure zabezpieczeń, prywatności i zgodności](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
