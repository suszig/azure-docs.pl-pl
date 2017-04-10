---
title: "Używanie programu Outlook w usłudze Azure RemoteApp | Microsoft Docs"
description: "Dowiedz się, jak skonfigurować program Outlook i używać go w usłudze Azure RemoteApp | Microsoft Azure"
services: remoteapp
documentationcenter: 
author: pavithir
manager: mbaldwin
ms.assetid: cb2a498f-9539-4522-a874-542114926a61
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: a6d4fbdf0e552f50673092183e893841ec0c5aa4
ms.lasthandoff: 03/31/2017


---
# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Używanie programu Microsoft Outlook w usłudze Azure RemoteApp
> [!IMPORTANT]
> Usługa Azure RemoteApp nie będzie obsługiwana od 31 sierpnia 2017 r. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Usługa Azure RemoteApp obsługuje program Microsoft Outlook usługi O365. Dowiedz się więcej na temat [działania pakietu Office w usłudze Azure RemoteApp](remoteapp-officesubscription.md). Istnieje kilka zalecanych ustawień dla programu Outlook używanego w usłudze Azure RemoteApp.

## <a name="cached-mode"></a>Tryb buforowany
Tryb buforowany jest zalecaną konfiguracją w przypadku korzystania z programu Outlook w usłudze Azure RemoteApp. Po skonfigurowaniu konta programu Outlook 2013 do używania trybu buforowanej wymiany program Outlook 2013 współpracuje z lokalną kopią należącej do użytkownika skrzynki pocztowej programu Microsoft Exchange, która jest przechowywana w pliku danych w trybie offline (pliku OST) na komputerze użytkownika wraz z książką adresową trybu offline (OAB). Buforowana skrzynka pocztowa i książka adresowa offline są okresowo aktualizowane danymi z usługi O365. Przeczytaj więcej na temat [różnic między trybem buforowanym a trybem online](https://technet.microsoft.com/library/jj683103.aspx).

Użytkownik może wybrać **tryb buforowanej wymiany** lub **tryb online** podczas konfigurowania konta lub zmieniania jego ustawień. Można także wdrożyć jeden z tych trybów przy użyciu narzędzia dostosowywania pakietu Office (OCT) lub zasad grupy.  

Przeczytaj [instrukcje krok po kroku dotyczące włączania trybu buforowanego](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Wyszukiwanie
Wyszukiwanie w programie Outlook w usłudze Azure RemoteApp ma pewne ograniczenia. Usługa Azure RemoteApp używa puli maszyn wirtualnych na potrzeby sesji użytkownika. Indeksowanie wyszukiwania zależy od identyfikatora komputera, który jest różny na różnych maszynach wirtualnych. Użytkownicy logujący się do usługi Azure RemoteApp mogą być za każdym razem przekierowywani do nowej maszyny wirtualnej. Oznacza to, że jeśli pozwolimy na wyszukiwanie lokalne, indeksator będzie uruchamiany przy każdej zmianie identyfikatora komputera (użyciu innej maszyny wirtualnej). W zależności od rozmiaru pliku OST działanie indeksatora może zajmować dużo czasu i zużywać zasoby potrzebne dla innych aplikacji. Wyszukiwanie nie tylko jest powolne, ale może nie dawać wyników. Ten problem można obejść, używając profilu konta w trybie online, ale mogłoby to spowodować obniżenie ogólnej wydajności z powodu braku lokalnej pamięci podręcznej (aby uzyskać więcej informacji o różnicach między trybem online i trybem pamięci podręcznej, zobacz powyższy link). Niestety, nie można wyłączyć indeksowanego/lokalnego wyszukiwania ani włączyć wyszukiwania w trybie online jako domyślnego w programie Outlook 2013.


