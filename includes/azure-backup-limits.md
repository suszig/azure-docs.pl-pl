---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 2/7/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: b345283f87c446ff3b583b0c5dd8a4be222303ae
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
Następujące ograniczenia mają zastosowanie do usługi Kopia zapasowa Azure.

| Identyfikator limit | Limit domyślny |
| --- | --- |
| Liczba serwerów/maszyny, które mogą być rejestrowane przed każdym magazynu |50 dla systemu Windows Server/klienta/SCDPM <br/> 200 dla maszyn wirtualnych IaaS |
| Rozmiar źródła danych dla danych przechowywanych w magazynie usługi Azure storage |Maksymalna liczba 54400 GB<sup>1</sup> |
| Liczba magazynów kopii zapasowych, które mogą zostać utworzone w każdej subskrypcji platformy Azure |25 Magazyny usług odzyskiwania dla regionu |
| Liczba kopii zapasowej mogą być planowane na dzień |3 dziennie dla systemu Windows Server/Client <br/> 2 dziennie SCDPM <br/> Raz dziennie dla maszyn wirtualnych IaaS |
| Dyskach danych dołączonych do maszyny wirtualnej platformy Azure do utworzenia kopii zapasowej |16 |
| Rozmiar dysku danych poszczególnych dołączony do maszyny wirtualnej platformy Azure do utworzenia kopii zapasowej| 4095 GB <sup>2</sup>|

* <sup>1</sup>limitu 54400 GB nie ma zastosowania do kopii zapasowej maszyny Wirtualnej IaaS.
 

