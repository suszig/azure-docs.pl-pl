---
title: "Azure Site Recovery Rozwiązywanie problemów z VMware do platformy Azure | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie błędów podczas replikowania maszyn wirtualnych platformy Azure"
services: site-recovery
documentationcenter: 
author: asgang
manager: srinathv
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/28/2017
ms.author: asgang
ms.openlocfilehash: b7b03442ba815c86e5defa1018b66f56c0b379df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Rozwiązywanie problemów dotyczących instalacji wypychanej usługi mobilności

W tym artykule opisano typowych problemów, które mogą dostęp podczas próby instalacji usługi mobilności Azure Site Recovery na serwerze źródłowym, aby włączyć ochronę.

## <a name="error-78007---the-requested-operation-could-not-be-completed"></a>Błąd 78007 — nie można ukończyć żądanej operacji
Ten błąd może zostać wygenerowany przez usługę kilka przyczyn. Wybierz odpowiednie błąd dostawcy, aby dalsze poszukiwanie rozwiązania problemu.

* [Błąd 95103](#error-95103---protection-could-not-be-enabled-ep0854) 
* [Błąd 95105](#error-95105---protection-could-not-be-enabled-ep0856) 
* [Błąd 95107](#error-95107---protection-could-not-be-enabled-ep0858) 
* [Błąd 95108](#error-95108---protection-could-not-be-enabled-ep0859) 
* [Błąd 95117](#error-95117---protection-could-not-be-enabled-ep0865) 
* [Błąd 95213](#error-95213---protection-could-not-be-enabled-ep0874) 
* [Błąd 95224](#error-95224---protection-could-not-be-enabled-ep0883) 
* [Błąd 95265](#error-95265---protection-could-not-be-enabled-ep0902) 


## <a name="error-95105---protection-could-not-be-enabled-ep0856"></a>Błąd 95105 - ochrony nie można włączyć (EP0856)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95105 </br>**Komunikat o błędzie:** instalacji wypychanej usługi mobilności na maszynie źródłowej nie powiodła się z kodem błędu **EP0856**. <br> Albo **udostępnianie plików i drukarek** nie jest dozwolony w źródle komputera lub brak są sieci występują problemy z łącznością między serwerem przetwarzania i maszyną źródłową.| **Udostępnianie plików i drukarek** nie jest włączone. | Zezwalaj na **udostępnianie plików i drukarek** na maszynie źródłowej w Zaporze systemu Windows. Na maszynie źródłowej w obszarze **zapory systemu Windows** > **Zezwalaj aplikacji lub funkcji za pośrednictwem zapory**, wybierz pozycję **udostępnianie plików i drukarek we wszystkich profilach**. </br> Ponadto należy sprawdzić następujące wymagania wstępne do pomyślnego zakończenia instalacji wypychanej.<br> Przeczytaj więcej na temat [Rozwiązywanie problemów z WMI isssues](#troubleshoot-wmi-issues)


## <a name="error-95107---protection-could-not-be-enabled-ep0858"></a>Błąd 95107 - ochrony nie można włączyć (EP0858)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95107 </br>**Komunikat o błędzie:** instalacji wypychanej usługi mobilności na maszynie źródłowej nie powiodła się z kodem błędu **EP0858**. <br> Albo udostępnionych poświadczeń do zainstalowania usługi mobilności są niepoprawne lub konto użytkownika ma niewystarczające uprawnienia. | Poświadczenia użytkownika do zainstalowania usługi mobilności na maszynie źródłowej są niepoprawne. | Upewnij się, poświadczenia użytkownika podane dla maszyny źródłowej na serwerze konfiguracji są poprawne. <br> Aby Dodawanie/edytowanie poświadczeń użytkownika, przejdź do serwera konfiguracji i wybierz **Cspsconfigtool** > **Zarządzaj kontem**. </br> Dodatkowo sprawdź następujące [wymagania wstępne](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) do pomyślnego zakończenia instalacji wypychanej.


## <a name="error-95117---protection-could-not-be-enabled-ep0865"></a>Błąd 95117 - ochrony nie można włączyć (EP0865)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95117 </br>**Komunikat o błędzie:** instalacji wypychanej usługi mobilności na maszynie źródłowej nie powiodła się z kodem błędu **EP0865**. <br> Maszyna źródłowa nie jest uruchomiona albo istnieją problemy z łącznością sieciową między serwerem przetwarzania i maszyną źródłową. | Problemy z łącznością sieciową między serwerem przetwarzania i na serwerze źródłowym. | Sprawdź łączność między serwerem przetwarzania i na serwerze źródłowym. </br> Dodatkowo sprawdź następujące [wymagania wstępne](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) do pomyślnego zakończenia instalacji wypychanej.|

## <a name="error-95103---protection-could-not-be-enabled-ep0854"></a>Błąd 95103 - ochrony nie można włączyć (EP0854)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95103 </br>**Komunikat o błędzie:** instalacji wypychanej usługi mobilności na maszynie źródłowej nie powiodła się z kodem błędu **EP0854**. <br> Instrumentacja zarządzania Windows (WMI) nie jest dozwolona na maszynie źródłowej, albo istnieją problemy z łącznością sieciową między serwerem przetwarzania i maszyną źródłową.| Usługa WMI jest zablokowana w Zaporze systemu Windows. | Usługa WMI w Zaporze systemu Windows. W obszarze **zapory systemu Windows** > **Zezwalaj aplikacji lub funkcji za pośrednictwem zapory**, wybierz pozycję **WMI dla wszystkich profilów**. </br> Dodatkowo sprawdź następujące [wymagania wstępne](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) do pomyślnego zakończenia instalacji wypychanej.|

## <a name="error-95213---protection-could-not-be-enabled-ep0874"></a>Błąd 95213 - ochrony nie można włączyć (EP0874)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95213 </br>**Komunikat o błędzie:** można zainstalować usługi mobilności na maszynie źródłowej % SourceIP; nie powiodło się z kodem błędu **EP0874**. <br> | Wersja systemu operacyjnego na maszynie źródłowej nie jest obsługiwana. <br>| Upewnij się, że maszyna źródłowa jest obsługiwana wersja systemu operacyjnego. Odczyt [macierz obsługi](https://aka.ms/asr-os-support). </br> Dodatkowo sprawdź następujące [wymagania wstępne](https://aka.ms/pushinstallerror) do pomyślnego zakończenia instalacji wypychanej.| 


## <a name="error-95108---protection-could-not-be-enabled-ep0859"></a>Błąd 95108 - ochrony nie można włączyć (EP0859)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95108 </br>**Komunikat o błędzie:** instalacji wypychanej usługi mobilności na maszynie źródłowej nie powiodła się z kodem błędu **EP0859**. <br>| Poświadczenia podane do zainstalowania usługi mobilności jest niepoprawna albo konto użytkownika ma niewystarczające uprawnienia <br>| Upewnij się, że podane poświadczenia są **głównego** poświadczeń konta. Aby [Dodawanie/edytowanie poświadczeń użytkownika](site-recovery-vmware-to-azure-manage-configuration-server.md#modify-user-accounts-and-passwords), przejdź do konfiguracji serwera i kliknij ikonę "Cspsconfigtool" skrót na pulpicie. Kliknij pozycję "Zarządzanie kontem" Aby Dodawanie/edytowanie poświadczeń.|

## <a name="error-95265---protection-could-not-be-enabled-ep0902"></a>Błąd 95265 - ochrony nie można włączyć (EP0902)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95265 </br>**Komunikat o błędzie:** instalacji wypychanej usługi mobilności na maszynie źródłowej zakończyło się pomyślnie, ale maszyna źródłowa wymaga ponownego uruchomienia dla niektórych systemu zmiany zaczęły obowiązywać. <br>| Starsza wersja usługi mobilności została już zainstalowana na serwerze.| Replikacja maszyny wirtualnej nadal bezproblemowo.<br> Ponownego rozruchu serwera podczas następnego okno obsługi w taki sposób, aby uzyskać korzyści nowe ulepszenia w usługi mobilności.|


## <a name="error-95224---protection-could-not-be-enabled-ep0883"></a>Błąd 95224 - ochrony nie można włączyć (EP0883)

**Kod błędu:** | **Możliwe przyczyny** | **Zalecenia dotyczące błędu**
--- | --- | ---
95224 </br>**Komunikat o błędzie:** można przeprowadzić instalacji wypychanej usługi mobilności na maszynie źródłowej % SourceIP; nie powiodło się z kodem błędu EP0883. System oczekuje na aktualizację / ponowne uruchomienie z poprzedniej instalacji.| System nie uruchomiono ponownie odinstalowywanie starsze/niezgodności wersji usługi mobilności.| Upewnij się, że nie ma wersji usługi mobilności istnieje na serwerze. <br> Uruchom ponownie serwer i ponownie uruchom zadanie włączania ochrony|

## <a name="resource-to-troubleshoot-push-installation-problems"></a>Zasób, aby rozwiązać problemy z instalacją wypychaną

#### <a name="troubleshoot-file-and-print-sharing-issues"></a>Rozwiązywanie problemów udostępnianie plików i drukarek
*  [Włącz lub wyłącz udostępnianie plików przy użyciu zasad grupy](https://technet.microsoft.com/en-us/library/cc754359(v=ws.10).aspx)
* [Włączanie pliku i udostępniania przez zaporę systemu Windows drukarek](https://technet.microsoft.com/en-us/library/ff633412(v=ws.10).aspx)

#### <a name="troubleshoot-wmi-issues"></a>Rozwiązywanie problemów z WMI
* [Testowanie podstawowe usługi WMI](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [Rozwiązywanie problemów z WMI](https://msdn.microsoft.com/en-us/library/aa394603(v=vs.85).aspx)
* [Rozwiązywanie problemów z usługi WMI i skryptów usługi WMI](https://technet.microsoft.com/en-us/library/ff406382.aspx#H22)

## <a name="next-steps"></a>Następne kroki
- [Włączanie replikacji maszyn wirtualnych VMware](vmware-walkthrough-enable-replication.md)
