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
ms.date: 09/26/2017
ms.author: yurid
ms.openlocfilehash: 67564e930310433bf4d51f7642bdd7ebf7e8e600
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Ustawianie zasad zabezpieczeń w usłudze Azure Security Center
Ten dokument zawiera informacje i niezbędne instrukcje pomocne podczas konfigurowania zasad zabezpieczeń w usłudze Security Center.


## <a name="how-security-policies-work"></a>Jak działają zasady zabezpieczeń?
Usługa Security Center automatycznie tworzy domyślne zasady zabezpieczeń dla każdej Twojej subskrypcji platformy Azure. Możesz edytować zasady w usłudze Security Center lub użyć usługi [Azure Policy](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) do utworzenia nowych definicji, zdefiniowania dodatkowych zasad i przypisania zasad w grupach zarządzania (które mogą reprezentować całą organizację, jednostki biznesowe w niej itp.) oraz monitorowania zgodności z tymi zasadami w tych zakresach.

> [!NOTE]
> Usługa Azure Policy jest dostępna w ograniczonej wersji zapoznawczej. Kliknij [tutaj](https://aka.ms/getpolicy), aby dołączyć. Aby uzyskać więcej informacji na temat usługi Azure Policy, przeczytaj artykuł [Create and manage policies to enforce compliance (Tworzenie zasad i zarządzanie nimi w celu wymuszenia zgodności)](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy).

## <a name="how-to-change-security-policies-in-security-center"></a>Jak zmienić zasady zabezpieczeń w usłudze Security Center?
W usłudze Security Center możesz edytować domyślne zasady zabezpieczeń dla każdej Twojej subskrypcji platformy Azure. Aby zmodyfikować zasady zabezpieczeń, musisz być właścicielem, współautorem lub administratorem zabezpieczeń tej subskrypcji lub zawierającej ją grupy zarządzania. Zaloguj się w witrynie Azure Portal i postępuj zgodnie z poniższymi krokami, aby wyświetlić swoje zasady zabezpieczeń w usłudze Security Center:

1. Na pulpicie nawigacyjnym usługi **Security Center** w obszarze **Ogólne** kliknij pozycję **Zasady zabezpieczeń**.
2. Wybierz subskrypcję, dla której chcesz włączyć zasady zabezpieczeń.

    ![Zarządzanie zasadami](./media/security-center-policies/security-center-policies-fig10.png)

3. W sekcji **SKŁADNIKI ZASAD** kliknij pozycję **Zasady zabezpieczeń**.

    ![Składniki zasad](./media/security-center-policies/security-center-policies-fig12.png)

4. Są to domyślne zasady przypisane do usługi Security Center przy użyciu usługi Azure Policy. Możesz usunąć elementy znajdujące się w obszarze **ZASADY I PARAMETRY** lub możesz dodać inne definicje zasad znajdujące się w obszarze **DOSTĘPNE OPCJE**. W tym celu po prostu kliknij znak plus obok nazwy definicji.

    ![Definicje zasad](./media/security-center-policies/security-center-policies-fig11.png)

5. Jeśli potrzebujesz bardziej szczegółowych informacji na temat zasad, kliknij je, a zostanie otwarta inna strona zawierająca szczegóły i kod JSON, który ma strukturę [definicji zasad(https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy#policy-definition-structure):

    ![JSON](./media/security-center-policies/security-center-policies-fig13.png)

6. Po zakończeniu edycji kliknij pozycję **Zapisz**.

## <a name="see-also"></a>Zobacz też
W tym dokumencie przedstawiono konfigurowanie zasad zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

* [Przewodnik planowania i obsługi usługi Azure Security Center](security-center-planning-and-operations-guide.md). Informacje na temat planowania i zagadnień projektowych podczas wdrażania usługi Azure Security Center.
* [Monitorowanie kondycji zabezpieczeń w usłudze Azure Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.
* [Reagowanie na alerty zabezpieczeń i zarządzanie nimi w usłudze Azure Security Center](security-center-managing-and-responding-alerts.md). Informacje na temat sposobu zarządzania alertami zabezpieczeń i reagowania na nie.
* [Monitorowanie rozwiązań partnerskich w usłudze Azure Security Center](security-center-partner-solutions.md). Informacje na temat sposobu monitorowania stanu kondycji rozwiązań partnerskich.
* [Azure Security Center — często zadawane pytania](security-center-faq.md). Odpowiedzi na często zadawane pytania dotyczące korzystania z usługi.
* [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/). Wpisy na blogu dotyczące zabezpieczeń i zgodności platformy Azure.
