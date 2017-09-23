---
title: "Zarządzanie kontem usługi Azure Automation | Microsoft Docs"
description: "W tym artykule opisano sposób zarządzania konfiguracją konta usługi Automation obejmującą np. odnawianie certyfikatów, usuwanie i błędną konfigurację."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/13/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 41efdbcacede74bac038342688362ff480cadc7e
ms.contentlocale: pl-pl
ms.lasthandoff: 04/15/2017

---

# <a name="manage-azure-automation-account"></a>Zarządzanie kontem usługi Azure Automation
W pewnym momencie przed wygaśnięciem ważności konta usługi Automation należy odnowić certyfikat. Jeśli uważasz, że bezpieczeństwo konta Uruchom jako zostało naruszone, możesz je usunąć i utworzyć ponownie. W tej sekcji opisano kroki umożliwiające wykonanie tych operacji.

## <a name="self-signed-certificate-renewal"></a>Odnawianie certyfikatu z podpisem własnym
Certyfikat z podpisem własnym utworzony dla konta Uruchom jako wygasa rok od daty utworzenia. Można go odnowić w dowolnym momencie przed wygaśnięciem jego ważności. Po odnowieniu bieżący ważny certyfikat zostanie zachowany w celu zapewnienia, że ta zmiana nie wpłynie negatywnie na żadne umieszczone w kolejce lub aktywnie działające elementy runbook, które uwierzytelniają się przy użyciu konta Uruchom jako. Certyfikat pozostanie ważny aż do daty wygaśnięcia.

> [!NOTE]
> Jeśli konto Uruchom jako usługi Automation skonfigurowano do użycia certyfikatu wystawionego przez urząd certyfikacji przedsiębiorstwa, w przypadku użycia tej opcji certyfikat przedsiębiorstwa zostanie zastąpiony certyfikatem z podpisem własnym.

Aby odnowić certyfikat, wykonaj następujące czynności:

1. W witrynie Azure Portal otwórz konto usługi Automation.

2. W bloku **Konto usługi Automation** w okienku **Właściwości konta** w obszarze **Ustawienia konta** wybierz pozycję **Konta Uruchom jako**.

    ![Okienko właściwości konta usługi Automation](media/automation-manage-account/automation-account-properties-pane.png)
3. W bloku właściwości **Konta Uruchom jako** wybierz konto Uruchom jako albo klasyczne konto Uruchom jako, dla którego chcesz odnowić certyfikat.

4. W bloku **Właściwości** wybranego konta kliknij pozycję **Odnów certyfikat**.

    ![Odnawianie certyfikatu konta Uruchom jako](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. W trakcie odnawiania certyfikatu postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Usuwanie konta Uruchom jako lub klasycznego konta Uruchom jako
W tej sekcji opisano sposób usuwania i ponownego tworzenia konta Uruchom jako lub klasycznego konta Uruchom jako. Konto usługi Automation jest zachowywane podczas wykonywania tej akcji. Po usunięciu konta Uruchom jako lub klasycznego konta Uruchom jako możesz je ponownie utworzyć w witrynie Azure Portal.

1. W witrynie Azure Portal otwórz konto usługi Automation.

2. W bloku **Konto usługi Automation** w okienku właściwości konta wybierz pozycję **Konta Uruchom jako**.

3. W bloku właściwości **Konta Uruchom jako** wybierz konto Uruchom jako albo klasyczne konto Uruchom jako, które chcesz usunąć. Następnie w bloku **Właściwości** wybranego konta kliknij pozycję **Usuń**.

 ![Usuwanie konta Uruchom jako](media/automation-manage-account/automation-account-delete-runas.png)

4. W trakcie usuwania konta postęp można śledzić po wybraniu z menu opcji **Powiadomienia**.

5. Po usunięciu konta możesz je ponownie utworzyć w bloku właściwości **Konta Uruchom jako**, wybierając opcję tworzenia **Konto Uruchom jako platformy Azure**.

 ![Ponowne tworzenie konta Uruchom jako usługi Automation](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Błąd konfiguracji
Niektóre elementy konfiguracji niezbędne do poprawnego działania konta Uruchom jako lub klasycznego konta Uruchom jako zostały usunięte lub nie zostały utworzone prawidłowo podczas początkowej konfiguracji. Te elementy to na przykład:

* Zasób certyfikatu
* Zasób połączenia
* Konto Uruchom jako zostało usunięte z roli współautora
* Nazwa główna usługi lub aplikacji w usłudze Azure AD

W poprzednim i innych przypadkach błędnej konfiguracji konto usługi Automation wykrywa te zmiany i wyświetla stan *Niekompletne* w bloku właściwości **Konta Uruchom jako** dla konta.

![Stan Niekompletne dla konfiguracji konta Uruchom jako](media/automation-manage-account/automation-account-runas-incomplete-config.png)

Po wybraniu konta Uruchom jako w okienku **Właściwości** konta zostanie wyświetlony następujący komunikat o błędzie:

![Komunikat ostrzegawczy Niekompletne dla konfiguracji konta Uruchom jako](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

Te problemy związane z kontem Uruchom jako można szybko rozwiązać, usuwając konto i tworząc je ponownie.

## <a name="next-steps"></a>Następne kroki
* Więcej informacji na temat nazw głównych usługi można znaleźć w temacie [Obiekty aplikacji i obiekty nazwę głównej usługi](../active-directory/active-directory-application-objects.md).
* Więcej informacji na temat kontroli dostępu opartej na rolach w usłudze Automatyzacja platformy Azure można znaleźć w temacie [Kontrola dostępu oparta na rolach w usłudze Automatyzacja platformy Azure](automation-role-based-access-control.md).
* Aby uzyskać więcej informacji na temat certyfikatów i usług Azure, zobacz [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md) (Omówienie certyfikatów usług Azure Cloud Services).
