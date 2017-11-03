---
title: "Wprowadzenie do zasad zabezpieczeń Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat zasad zabezpieczeń Centrum zabezpieczeń Azure i jego kluczowych możliwości."
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 95ef2099cb16bcfd550ce2799428f1a16031f535
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/13/2017
---
# <a name="security-policies-overview"></a>Omówienie zasad zabezpieczeń
Ten dokument zawiera omówienie zasad zabezpieczeń w Centrum zabezpieczeń.

## <a name="what-are-security-policies"></a>Czym są zasady zabezpieczeń?
Zasady zabezpieczeń definiują pożądaną konfigurację Twoich obciążeń oraz pomagają zapewnić zgodność z wymaganiami dotyczącymi zabezpieczeń określonymi przez firmę lub przepisy. W usłudze Security Center możesz zdefiniować zasady dla swoich subskrypcji platformy Azure, które mogą być dopasowane do typu obciążenia oraz poufności danych. Na przykład aplikacje używające danymi podlegającymi ochronie jak identyfikowalne dane osobowe mogą wymagać wyższego poziomu zabezpieczeń niż innych obciążeń. 

Zasady usługi Security Center zawierają następujące składniki:

- Funkcji zbierania danych: agent inicjowania obsługi administracyjnej i [zbierania danych](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) ustawienia.
- Zasady zabezpieczeń: Określa, które kontrolki są monitorowane i zalecane przez Centrum zabezpieczeń (Edytuj [zasady zabezpieczeń](https://docs.microsoft.com/en-us/azure/security-center/security-center-policies) w Centrum zabezpieczeń, lub użyj [zasadami Azure](security-center-azure-policy.md), w podglądzie ograniczony, aby utworzyć nowy definicje, zdefiniować dodatkowe zasady i przypisanie zasad w grupach zarządzania).
- Wiadomości e-mail z powiadomieniami: kontakty zabezpieczeń i [powiadomień e-mail](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) ustawienia.
- Warstwy cenowej: wolnego lub standard [cennik wybór](https://docs.microsoft.com/azure/security-center/security-center-pricing), która określa zakres funkcji Centrum zabezpieczeń, które są dostępne dla zasobów w zakresie (można określić dla subskrypcji, grupy zasobów i obszarów roboczych). 


## <a name="who-can-edit-security-policies"></a>Kto może edytować zasady zabezpieczeń?
Centrum zabezpieczeń używa opartej na rolach kontroli dostępu (RBAC), który zawiera wbudowane role, które można przypisać do użytkowników, grup i usług Azure. Gdy użytkownik otwiera usługę Security Center, widzi tylko informacje dotyczące zasobów, do których ma dostęp. Oznacza to, że użytkownik ma przypisaną rolę właściciela, współautora lub czytelnika subskrypcji albo grupy zasobów, do której należy zasób. Oprócz tych ról istnieją dwie określone role usługi Security Center:

- Czytnik zabezpieczeń: użytkownika, który należy do tej roli jest możliwość wyświetlania praw do Centrum zabezpieczeń, który zawiera zalecenia, alertów, zasad i kondycji, ale nie będzie mogła wprowadzać zmiany.
- Administrator zabezpieczeń: takie same jak czytnika zabezpieczeń, ale można także zaktualizować zasady zabezpieczeń odrzucić zalecenia i alerty.


## <a name="next-steps"></a>Następne kroki
W tym dokumencie przedstawiono informacje dotyczące zasad zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — informacje o sposobie konfigurowania zasad zabezpieczeń dla subskrypcji platformy Azure i grup zasobów.
* [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md) — informacje na temat monitorowania kondycji zasobów platformy Azure.
* [Reagowanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure i zarządzanie nimi](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak reagowania na alerty zabezpieczeń i zarządzania nimi.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md) — informacje na temat monitorowania stanu kondycji rozwiązań partnerskich.
- [Bezpieczeństwo danych w Centrum zabezpieczeń Azure](security-center-data-security.md) — Dowiedz się, jak dane są zarządzane i w Centrum zabezpieczeń.
* [Azure Security Center — często zadawane pytania](security-center-faq.md) — odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Uzyskaj najnowsze informacje zabezpieczeń platformy Azure i informacje.


