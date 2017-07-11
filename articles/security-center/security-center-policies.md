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
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 2593e6846c897644017083b49ad4ba8219696c6c
ms.contentlocale: pl-pl
ms.lasthandoff: 06/17/2017


---
<a id="set-security-policies-in-azure-security-center" class="xliff"></a>

# Ustawianie zasad zabezpieczeń w usłudze Azure Security Center
Ten dokument zawiera informacje i niezbędne instrukcje pomocne podczas konfigurowania zasad zabezpieczeń w usłudze Security Center.

>[!NOTE] 
>Począwszy od początku czerwca 2017 roku, usługa Security Center będzie używać programu Microsoft Monitoring Agent do gromadzenia i przechowywania danych. Aby dowiedzieć się więcej, zobacz [Migracja platformy usługi Azure Security Center](security-center-platform-migration.md). Informacje przedstawione w tym artykule reprezentują funkcję Security Center po przejściu do programu Microsoft Monitoring Agent.
>

<a id="what-are-security-policies" class="xliff"></a>

## Czym są zasady zabezpieczeń?
Zasady zabezpieczeń określają zestaw mechanizmów kontrolnych, które są zalecane dla zasobów w określonej subskrypcji. W usłudze Security Center można zdefiniować zasady dla subskrypcji platformy Azure zgodnie z potrzebami zabezpieczeń firmy i typem aplikacji oraz poufnością danych w poszczególnych subskrypcjach.

Na przykład zasoby używane do celów projektowania lub testowania mogą mieć inne wymagania dotyczące zabezpieczeń niż te, które są używane przez aplikacje produkcyjne. Aplikacje z danymi podlegającymi ochronie (takimi jak dane osobowe) mogą wymagać wyższego poziomu zabezpieczeń. Zasady zabezpieczeń włączone w usłudze Azure Security Center regulują zalecenia dotyczące zabezpieczeń i monitorowania, które ułatwiają znalezienie potencjalnych luk i uniknięcie zagrożeń. Przeczytaj artykuł [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md), aby uzyskać więcej informacji na temat sposobu określenia, która opcja jest bardziej odpowiednia dla Ciebie.

<a id="set-security-policies" class="xliff"></a>

## Ustawianie zasad zabezpieczeń
Zasady zabezpieczeń można skonfigurować dla każdej subskrypcji. Aby zmodyfikować zasady zabezpieczeń, musisz być właścicielem lub współautorem subskrypcji. Zaloguj się w witrynie Azure Portal i postępuj zgodnie z poniższymi krokami, aby skonfigurować zasady zabezpieczeń w usłudze Security Center:

1. Kliknij kafelek **Zasady** na pulpicie nawigacyjnym usługi Security Center.
2. W otwartym bloku Zasady zabezpieczeń wybierz subskrypcję, dla której chcesz włączyć zasady zabezpieczeń.

    ![Definiowanie zasad](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Zostanie otwarty blok **Zasady zabezpieczeń** dla wybranej subskrypcji z zestawem opcji. Opcje dostępne w tym bloku to:

   * **Zasady zapobiegania**: ta opcja umożliwia konfigurowanie zasad dla subskrypcji lub grupy zasobów.  
   * **Powiadomienie e-mail**: ta opcja umożliwia konfigurowanie powiadomienia e-mail, które jest wysyłane po pierwszym wystąpieniu alertu w ciągu dnia i w przypadku alertów o wysokiej ważności. Preferencje poczty e-mail można konfigurować tylko dla zasad dotyczących subskrypcji. Więcej informacji dotyczących sposobu konfigurowania powiadomień e-mail można znaleźć w artykule [Provide security contact details in Azure Security Center](security-center-provide-security-contact-details.md) (Wprowadzanie danych kontaktowych na potrzeby zabezpieczeń w usłudze Azure Security Center).
   * **Warstwa cenowa**: użyj tej opcji, aby uaktualnić wybór warstwy cenowej. Zobacz [cennik usługi Security Center](security-center-pricing.md), aby dowiedzieć się więcej na temat opcji cen.
4. Upewnij się, że opcja **Zbieraj dane z maszyn wirtualnych** jest włączona (**Wł.**). Ta opcja umożliwia automatyczne zbieranie dzienników dla istniejących i nowych zasobów przy użyciu programu Microsoft Monitoring Agent — ten sam agent jest używany przez pakiet Operations Management Suite i usługę Log Analytics. Dane zbierane z tego agenta będą przechowywane we wszystkich istniejących obszarach roboczych usługi Log Analytics skojarzonych z subskrypcją platformy Azure lub w nowych obszarach roboczych, uwzględniając lokalizację geograficzną maszyny wirtualnej.

5. W bloku **Zasady zabezpieczeń** kliknij pozycję **Zasady zapobiegania**, aby wyświetlić dostępne opcje. Kliknij pozycję **Włącz**, aby włączyć zalecenia dotyczące zabezpieczeń istotne dla tej subskrypcji.

    ![Wybieranie zasad zabezpieczeń](./media/security-center-policies/security-center-policies-fig4-newUI.png)

Użyj poniższej tabeli jako źródła informacji, aby zrozumieć do czego służą poszczególne opcje:

| Zasady | Gdy ustawienie jest włączone |
| --- | --- |
| Aktualizacje systemu |Codziennie pobiera listę dostępnych aktualizacji zabezpieczeń i aktualizacji krytycznych z usługi Windows Update lub Windows Server Update Services. Pobierana lista zależy od usługi, która jest skonfigurowana dla tej maszyny wirtualnej. Zaleca się zastosowanie brakujących aktualizacji. W systemach Linux korzysta z systemu zarządzania pakietami udostępnionego wraz z dystrybucją, aby ustalić, dla których pakietów są dostępne aktualizacje. Sprawdzane są również aktualizacje zabezpieczeń i aktualizacje krytyczne z maszyn wirtualnych usługi [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md). |
| Luki w zabezpieczeniach systemu operacyjnego |Analizuje codziennie konfigurację systemu operacyjnego w celu określenia problemów, które mogą uczynić maszynę wirtualną podatną na ataki. Zaleca także dokonanie zmian w konfiguracji w celu usunięcia tych luk w zabezpieczeniach. Więcej informacji na temat określonych monitorowanych ustawień konfiguracyjnych znajduje się na [liście zalecanych linii bazowych](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (Obecnie system Windows Server 2016 nie jest w pełni obsługiwany). |
| Ochrona punktów końcowych |Zaleca ochronę punktów końcowych, które mają zostać aprowizowane dla wszystkich maszyn wirtualnych systemu Windows, aby ułatwić identyfikację oraz usunięcie wirusów, programów szpiegujących oraz innego złośliwego oprogramowania. |
| Szyfrowanie dysków |Zaleca włączenie szyfrowania dysków na wszystkich maszynach wirtualnych w celu zwiększenia ochrony magazynowanych danych. |
| Grupy zabezpieczeń sieci |Zaleca się, aby [sieciowe grupy zabezpieczeń](../virtual-network/virtual-networks-nsg.md) były skonfigurowane do kontrolowania ruchu przychodzącego i wychodzącego do maszyn wirtualnych z publicznymi punktami końcowymi. Grupy zabezpieczeń sieci skonfigurowane dla podsieci będą dziedziczone przez wszystkie interfejsy sieciowe maszyny wirtualnej, chyba że określono inaczej. Oprócz sprawdzania, czy grupa zabezpieczeń sieci została skonfigurowana, ta opcja również ocenia reguły zabezpieczeń ruchu przychodzącego w celu określenia, czy istnieją takie, które zezwalają na ruch przychodzący. |
| Zapora aplikacji sieci Web |Zaleca się aprowizowanie zapory aplikacji sieci Web na maszynach wirtualnych, dla których spełniony jest dowolny z następujących warunków: </br></br>Używany jest [Publiczny adres IP na poziomie wystąpienia](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) i konfiguracja reguł zabezpieczeń ruchu przychodzącego dla skojarzonej grupy zabezpieczeń sieci umożliwia dostęp do portu 80/443.</br></br>Używany jest adres IP ze zrównoważonym obciążeniem, a konfiguracja skojarzonych reguł równoważenia obciążenia i reguł translatora adresów sieciowych (NAT) dla ruchu przychodzącego umożliwia dostęp do portu 80/443. Aby uzyskać więcej informacji, zobacz artykuł [Azure Resource Manager support for Load Balancer](../load-balancer/load-balancer-arm.md) (Obsługa usługi Azure Resource Manager dla modułu równoważenia obciążenia). |
| Zapora nowej generacji |Powoduje rozszerzenie ochrony sieci poza grupy zabezpieczeń sieci, które są wbudowane w platformę Azure. Usługa Security Center wykryje wdrożenia, dla których zaleca się zaporę nowej generacji, i pozwoli na aprowizację urządzenia wirtualnego. |
| Inspekcja SQL i wykrywanie zagrożeń |Zaleca się, by inspekcja dostępu do bazy danych Azure była włączona w celu zapewnienia zgodności, umożliwienia zaawansowanego wykrywania zagrożeń i na potrzeby analizy. |
| Szyfrowanie SQL |Zaleca się, aby funkcja szyfrowania nieaktywnych danych była włączona dla usługi Azure SQL Database, powiązanych kopii zapasowych i plików dziennika transakcji. Dzięki temu nawet w przypadku włamania się do danych, nie będzie można ich odczytać. |
| Ocena luk w zabezpieczeniach |Zaleca się zainstalowanie na maszynie wirtualnej rozwiązania do oceny luk w zabezpieczeniach. |
| Szyfrowanie w usłudze Storage |Obecnie ta funkcja jest dostępna dla plików i obiektów blob Azure. Należy pamiętać, że po włączeniu szyfrowania w usłudze Storage szyfrowane będą tylko nowe dane, a wszystkie pliki istniejące już na tym koncie magazynu pozostaną niezaszyfrowane. |

Po skonfigurowaniu wszystkich opcji kliknij przycisk **OK** w bloku **Zasady zabezpieczeń**, który zawiera zalecenia, a następnie kliknij pozycję **Zapisz** w bloku **Zasady zabezpieczeń**, który zawiera ustawienia początkowe.

> [!NOTE]
> Warstwa cenowa ma nadal zastosowanie względem poziomu grupy zasobów. Więcej informacji zawiera strona [Cennik](https://azure.microsoft.com/pricing/details/security-center/).
>
>

<a id="see-also" class="xliff"></a>

## Zobacz też
W tym dokumencie przedstawiono konfigurowanie zasad zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md). Informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md). Informacje na temat sposobu zarządzania alertami zabezpieczeń i reagowania na nie.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md). Informacje na temat sposobu monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.

