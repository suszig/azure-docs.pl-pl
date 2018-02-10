---
title: "Plik dołączania"
description: "Plik dołączania"
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 2/7/2018
ms.author: trinadhk
ms.custom: include file
ms.openlocfilehash: 7ca5b34961b4d0e3d4fcecb8175e3e0901d7049d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
Następujące ograniczenia mają zastosowanie do usługi Kopia zapasowa Azure.

| Identyfikator limit | Limit domyślny |
| --- | --- |
| Liczba serwerów/maszyny, które mogą być rejestrowane przed każdym magazynu |50 dla systemu Windows Server/klienta/SCDPM <br/> 200 dla maszyn wirtualnych IaaS |
| Rozmiar źródła danych dla danych przechowywanych w magazynie usługi Azure storage |Maksymalna liczba 54400 GB<sup>1</sup> |
| Liczba magazynów kopii zapasowych, które mogą zostać utworzone w każdej subskrypcji platformy Azure |25 Magazyny usług odzyskiwania dla regionu |
| Liczba kopii zapasowej mogą być planowane na dzień |3 dziennie dla systemu Windows Server/Client <br/> 2 dziennie SCDPM <br/> Raz dziennie dla maszyn wirtualnych IaaS |
| Dyskach danych dołączonych do maszyny wirtualnej platformy Azure do utworzenia kopii zapasowej |16 |
| Rozmiar dysku danych poszczególnych dołączony do maszyny wirtualnej platformy Azure do utworzenia kopii zapasowej| 1024 GB <sup>2</sup>|

* <sup>1</sup>limitu 54400 GB nie ma zastosowania do kopii zapasowej maszyny Wirtualnej IaaS.
* <sup>2</sup> mamy [prywatnej wersji zapoznawczej](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a?redir=0) do obsługi dysków maksymalnie 4 TB. 

