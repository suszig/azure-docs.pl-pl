---
title: "Rozwiązywanie problemów z wdrażaniem OpenShift na platformie Azure | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z wdrażaniem OpenShift na platformie Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: ea3664870480f6ed170972a5f52940dc4a852219
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshooting-openshift-deployment-in-azure"></a>Rozwiązywanie problemów z wdrażaniem OpenShift na platformie Azure

Jeśli klaster OpenShift nie pomyślnie wdrożone, dostępne są niektóre rozwiązywania problemów dotyczących zadań, które można wykonać, aby zawęzić problem. Wyświetl stan wdrożenia i porównać poniższą listę kodów zakończenia.

- Kod zakończenia 3: Red Hat subskrypcji nazwę użytkownika / hasło lub identyfikator organizacji / klucz aktywacji jest niepoprawny
- Kod zakończenia 4: Identyfikatora Red Hat puli jest nieprawidłowy lub nie dostępnych żadnych uprawnień
- Kod zakończenia 5: Nie można aprowizować woluminu puli elastycznej Docker
- Kod zakończenia 6: Instalacja OpenShift klastra nie powiodła się.
- Kod zakończenia 7: Instalacji klastra OpenShift zakończyło się pomyślnie, ale konfiguracji dostawcy chmury Azure nie powiodło się - config głównego na węzeł główny problem
- Kod zakończenia 8: Klastra OpenShift instalacja zakończyła się pomyślnie, ale konfiguracja dostawcy chmury Azure nie powiodła się - config węzła na węzeł główny problem
- Kod zakończenia 9: Klastra OpenShift instalacja zakończyła się pomyślnie, ale konfiguracja dostawcy chmury Azure nie powiodła się - config węzła na problem Infra lub węzła aplikacji
- Kod zakończenia: 10: Klastra OpenShift instalacji zakończyło się pomyślnie, ale konfiguracja dostawcy chmury Azure nie powiodło się — Korygowanie wzorca węzłów lub nie można ustawić jako unschedulable wzorca
- Kodu zakończenia 11: Nie można wdrożyć metryki
- Kod zakończenia 12: Rejestrowanie wdrażanie nie powiodło się

Kody zakończenia 7-10 OpenShift klastra został zainstalowany, ale nie konfiguracji dostawcy chmury Azure. Można SSH do węzła głównego (źródło) lub węzeł bastionu (kontenera platformy) i z SSH występują na każdym z węzłów w klastrze i rozwiązać problemy.

Typowe przyczyny błędów z 7 kody zakończenia — 9 jest nazwy głównej usługi nie ma odpowiednich uprawnień do subskrypcji lub grupy zasobów. Jeśli w rzeczywistości jest to problem, następnie przypisz odpowiednie uprawnienia i ręcznie uruchom ponownie skrypt, który nie powiodło się wszystkie kolejne skrypty.
Pamiętaj ponownie uruchomić usługę, która nie powiodło się (na przykład systemctl ponowne uruchomienie atomic-openshift-node.service) przed wykonaniem skryptów ponownie.

Dla dalszego rozwiązywania problemów, SSH w węźle głównym na porcie 2200 (źródło) lub węzeł bastionu na port 22 (kontenera Platform). Musisz być głównym (sudo su-), a następnie przejdź do następującego katalogu: /var/lib/waagent/custom-script/download

Folder o nazwie "0" i "1" powinna zostać wyświetlona. W każdym z tych folderów Zobacz dwa pliki i stderr stdout. Można sprawdzić za pomocą tych plików, aby określić, w którym wystąpił błąd.
