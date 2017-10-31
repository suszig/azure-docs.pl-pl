---
title: "Ustawianie zasad zabezpieczeń w usłudze Azure Security Center | Microsoft Docs"
description: "Ten dokument zawiera informacje pomocne podczas konfigurowania zasad zabezpieczeń w Centrum zabezpieczeń Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 1cebb6edecd13c6ab32c6854bfd6fe908c1f71f4
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/13/2017
---
# <a name="set-security-policies-in-security-center"></a>Ustawianie zasad zabezpieczeń w usłudze Security Center
Ten dokument zawiera informacje i niezbędne instrukcje pomocne podczas konfigurowania zasad zabezpieczeń w usłudze Security Center. 


## <a name="how-security-policies-work"></a>Jak działają zasady zabezpieczeń
Usługa Security Center automatycznie tworzy domyślne zasady zabezpieczeń dla każdej Twojej subskrypcji platformy Azure. W usłudze Security Center możesz edytować zasady i monitorować ich zgodność. 

> [!NOTE]
> Teraz można rozszerzyć zasady zabezpieczeń w usłudze Security Center za pomocą usługi Azure Policy dostępnej w ograniczonej wersji zapoznawczej. Kliknij [tutaj](http://aka.ms/getpolicy), aby przetestować wersję zapoznawczą lub zapoznaj się z dokumentacją [tutaj](security-center-azure-policy.md).

Na przykład zasoby używane do celów projektowania lub testowania mogą mieć inne wymagania dotyczące zabezpieczeń niż te, które są używane przez aplikacje produkcyjne. Aplikacje z danymi podlegającymi ochronie (takimi jak dane osobowe) mogą wymagać wyższego poziomu zabezpieczeń. Zasady zabezpieczeń włączone w usłudze Azure Security Center regulują zalecenia dotyczące zabezpieczeń i monitorowania, które ułatwiają znalezienie potencjalnych luk i uniknięcie zagrożeń. Przeczytaj artykuł [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md), aby uzyskać więcej informacji na temat sposobu określenia, która opcja jest bardziej odpowiednia dla Ciebie.

## <a name="edit-security-policies"></a>Edytowanie zasad zabezpieczeń
W usłudze Security Center możesz edytować domyślne zasady zabezpieczeń dla każdej Twojej subskrypcji platformy Azure. Aby zmodyfikować zasady zabezpieczeń, musisz być właścicielem, współautorem lub administratorem zabezpieczeń subskrypcji. Zaloguj się w witrynie Azure Portal i postępuj zgodnie z poniższymi krokami, aby skonfigurować zasady zabezpieczeń w usłudze Security Center: 

1.  Na pulpicie nawigacyjnym usługi **Security Center** w obszarze **Ogólne** kliknij pozycję **Zasady zabezpieczeń**.
2.  Wybierz subskrypcję, dla której chcesz włączyć zasady zabezpieczeń.
3.  W sekcji **SKŁADNIKI ZASAD** kliknij pozycję **Zasady zabezpieczeń**.
4.  Są to domyślne zasady przypisane przez usługę Security Center. Możesz wyłączyć lub włączyć dostępne zalecenia dotyczące zabezpieczeń.
5.  Po zakończeniu edycji kliknij pozycję **Zapisz**.

## <a name="available-security-policy-options"></a>Dostępne opcje zasad zabezpieczeń

Użyj poniższej tabeli jako źródła informacji, aby zrozumieć do czego służą poszczególne opcje:

| Zasady | Gdy ustawienie jest włączone |
| --- | --- |
| Aktualizacje systemu |Codziennie pobiera listę dostępnych aktualizacji zabezpieczeń i aktualizacji krytycznych z usługi Windows Update lub Windows Server Update Services. Pobierana lista zależy od usługi, która jest skonfigurowana dla tej maszyny wirtualnej. Zaleca się zastosowanie brakujących aktualizacji. W systemach Linux korzysta z systemu zarządzania pakietami udostępnionego wraz z dystrybucją, aby ustalić, dla których pakietów są dostępne aktualizacje. Sprawdzane są również aktualizacje zabezpieczeń i aktualizacje krytyczne z maszyn wirtualnych usługi [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md). |
| Luki w zabezpieczeniach systemu operacyjnego |Analizuje codziennie konfigurację systemu operacyjnego w celu określenia problemów, które mogą uczynić maszynę wirtualną podatną na ataki. Zaleca także dokonanie zmian w konfiguracji w celu usunięcia tych luk w zabezpieczeniach. Więcej informacji na temat określonych monitorowanych ustawień konfiguracyjnych znajduje się na [liście zalecanych linii bazowych](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (Obecnie system Windows Server 2016 nie jest w pełni obsługiwany). |
| Ochrona punktów końcowych |Zaleca ochronę punktów końcowych, które mają zostać aprowizowane dla wszystkich maszyn wirtualnych systemu Windows, aby ułatwić identyfikację oraz usunięcie wirusów, programów szpiegujących oraz innego złośliwego oprogramowania. |
| Szyfrowanie dysków |Zaleca włączenie szyfrowania dysków na wszystkich maszynach wirtualnych w celu zwiększenia ochrony magazynowanych danych. |
| Grupy zabezpieczeń sieci |Zaleca się, aby [sieciowe grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md) były skonfigurowane do kontrolowania ruchu przychodzącego i wychodzącego do maszyn wirtualnych z publicznymi punktami końcowymi. Grupy zabezpieczeń sieci skonfigurowane dla podsieci są dziedziczone przez wszystkie interfejsy sieciowe maszyny wirtualnej, chyba że określono inaczej. Oprócz sprawdzania, czy grupa zabezpieczeń sieci została skonfigurowana, ta opcja również ocenia reguły zabezpieczeń ruchu przychodzącego w celu określenia, czy istnieją takie, które zezwalają na ruch przychodzący. |
| Zapora aplikacji sieci Web |Zaleca się aprowizowanie zapory aplikacji sieci Web na maszynach wirtualnych, dla których spełniony jest dowolny z następujących warunków: </br></br>Używany jest [Publiczny adres IP na poziomie wystąpienia](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) i konfiguracja reguł zabezpieczeń ruchu przychodzącego dla skojarzonej grupy zabezpieczeń sieci umożliwia dostęp do portu 80/443.</br></br>Używany jest adres IP ze zrównoważonym obciążeniem, a konfiguracja skojarzonych reguł równoważenia obciążenia i reguł translatora adresów sieciowych (NAT) dla ruchu przychodzącego umożliwia dostęp do portu 80/443. Aby uzyskać więcej informacji, zobacz artykuł [Azure Resource Manager support for Load Balancer](../load-balancer/load-balancer-arm.md) (Obsługa usługi Azure Resource Manager dla modułu równoważenia obciążenia). |
| Zapora nowej generacji |Powoduje rozszerzenie ochrony sieci poza grupy zabezpieczeń sieci, które są wbudowane w platformę Azure. Usługa Security Center wykryje wdrożenia, dla których zaleca się zaporę nowej generacji, i pozwoli na aprowizację urządzenia wirtualnego. |
| Inspekcja SQL i wykrywanie zagrożeń |Zaleca się, by inspekcja dostępu do bazy danych Azure była włączona w celu zapewnienia zgodności, umożliwienia zaawansowanego wykrywania zagrożeń i na potrzeby analizy. |
| Szyfrowanie SQL |Zaleca się, aby funkcja szyfrowania nieaktywnych danych była włączona dla usługi Azure SQL Database, powiązanych kopii zapasowych i plików dziennika transakcji. Dzięki temu nawet w przypadku włamania się do danych, nie będzie można ich odczytać. |
| Ocena luk w zabezpieczeniach |Zaleca się zainstalowanie na maszynie wirtualnej rozwiązania do oceny luk w zabezpieczeniach. |
| Szyfrowanie w usłudze Storage |Obecnie ta funkcja jest dostępna dla plików i obiektów blob Azure. Po włączeniu szyfrowania w usłudze Storage szyfrowane będą tylko nowe dane, a wszystkie pliki istniejące już na tym koncie magazynu pozostaną niezaszyfrowane. |
| Dostęp do sieci JIT |Gdy zostanie włączona funkcja „dokładnie na czas”, usługa Security Center zablokuje ruch przychodzący do maszyn wirtualnych platformy Azure poprzez utworzenie reguły sieciowej grupy zabezpieczeń. Należy wybrać porty na maszynie wirtualnej, do których ruch przychodzący powinien zostać zablokowany. Aby uzyskać więcej informacji, zobacz [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (Zarządzanie dostępem maszyny wirtualnej przy użyciu funkcji „dokładnie na czas”). |


## <a name="next-step"></a>Następny krok
W tym dokumencie przedstawiono sposób konfigurowania zasad zabezpieczeń w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md). Informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md). Informacje na temat sposobu zarządzania alertami zabezpieczeń i reagowania na nie.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md). Informacje na temat sposobu monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.
