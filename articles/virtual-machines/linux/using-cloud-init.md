---
title: "Omówienie chmury inicjowania obsługi maszyn wirtualnych systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Omówienie funkcji init chmurze na platformie Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 3670676032eb71a5339bb1219cb794366b912147
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-in-azure"></a>Init chmury umożliwia dostosowywanie Maszynę wirtualną systemu Linux na platformie Azure
W tym artykule przedstawiono sposób użycia [init chmury](https://cloudinit.readthedocs.io) do skonfigurowania maszyny wirtualnej (VM) lub maszyny wirtualnej zestawach skali (VMSS) na inicjowanie obsługi administracyjnej czas na platformie Azure. Skrypty te init chmury są uruchamiane po pierwszym uruchomieniu komputera po zasoby zostały udostępnione przez platformę Azure.  

## <a name="cloud-init-overview"></a>Init chmury — omówienie
[Init chmury](https://cloudinit.readthedocs.io) jest powszechnie używaną podejście, aby dostosować Maszynę wirtualną systemu Linux, ponieważ jest on uruchamiany po raz pierwszy. Init chmury można użyć, aby zainstalować pakiety i zapisywać pliki, lub aby skonfigurować użytkowników i zabezpieczeń. Ponieważ init chmury jest wywoływana podczas początkowego procesu rozruchu, nie są żadne dodatkowe kroki lub agentów wymaganych do zastosowania konfiguracji.  Aby uzyskać więcej informacji na temat sposobu poprawnie sformatowana Twojej `#cloud-config` plików, zobacz [witryna dokumentacji usługi chmury init](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config`fiels są plikami tekstowymi zakodowane w formacie base64.

Init chmury działa także w dystrybucji. Na przykład nie używaj **instalacji stanie get** lub **yum zainstalować** do zainstalowania pakietu. Zamiast tego można zdefiniować listę pakietów do zainstalowania. Init chmury automatycznie używa narzędzia do zarządzania natywnego pakietu dla distro, którą wybierzesz.

 Obecnie pracujemy z partnerami potwierdzony distro systemu Linux w celu dostępnych obrazów włączone inicjowania chmury w portalu Azure marketplace. Te obrazy spowoduje, że wdrożeń chmury init i konfiguracje współpracuje z maszynami wirtualnymi i zestawy skalowania maszyny Wirtualnej (VMSS). W poniższej tabeli przedstawiono bieżącej dostępności obrazów init chmury, włączone na platformie Azure:

| Wydawca | Oferta | SKU | Wersja | gotowe init chmury
|:--- |:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |16.04 LTS |najnowsza |tak | 
|Canonical |UbuntuServer |14.04.5-LTS |najnowsza |tak |
|CoreOS |CoreOS |Stable |najnowsza |tak |
|OpenLogic |CentOS |7-CI |najnowsza |wersja zapoznawcza |
|RedHat |RHEL |7-RAW-CI |najnowsza |wersja zapoznawcza |

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Jaka jest różnica między init chmury i agenta systemu Linux (WALA)?
WALA jest używany do obsługi administracyjnej i skonfigurować maszyny wirtualne i obsługi rozszerzeń Azure agenta specyficzne dla platformy Azure. Firma Microsoft są udoskonalanie zadanie konfigurowania maszyn wirtualnych do korzystania z chmury init zamiast agenta systemu Linux w celu umożliwienia istniejących klientów init chmury za pomocą ich bieżący skryptów init chmury.  Jeśli dokonano już inwestycji w chmurze init skrypty do konfigurowania serwerów z systemem Linux są **są wymagane nie dodatkowe ustawienia** je włączyć. 

Jeśli nie ma przełącznika wiersza polecenia AzureCLI `--custom-data` na inicjowanie obsługi administracyjnej czasu WALA wykonuje minimalnego obsługi parametrów wymaganych do obsługi administracyjnej maszyny Wirtualnej i ukończyć wdrażanie przy użyciu ustawień domyślnych maszyny Wirtualnej.  Jeśli odwołanie init chmury `--custom-data` przełącznika, niezależnie od znajduje się w danych niestandardowych (poszczególnych ustawień lub skryptu pełne) zastępuje WALA zdefiniowane wartości domyślne. 

Konfiguracje WALA maszyn wirtualnych jest ograniczone do pracy w ramach maksymalny czas inicjowania obsługi administracyjnej maszyny Wirtualnej.  Konfiguracje init chmury stosowane do maszyn wirtualnych bez ograniczeń czasowych i nie powoduje wdrożenia niepowodzenie przez przekroczeniem limitu czasu. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Wdrażanie inicjowania chmury włączone maszyny wirtualnej
Wdrażanie włączone inicjowania chmury maszyny wirtualnej jest tak proste, jak odwołujące się do dystrybucji chmury inicjowaniem włączona podczas wdrażania.  Maintainers dystrybucji systemu Linux musiał wybrać włączyć i integrowanie init chmury podstawowej Azure obrazów opublikowanych. Po potwierdzeniu chmury inicjowaniem włączone jest obraz, który chcesz wdrożyć, można użyć AzureCLI do wdrożenia obrazu. 

Pierwszym etapem wdrożenia tego obrazu jest utworzenie grupy zasobów z [Tworzenie grupy az](/cli/azure/group#create) polecenia. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Następnym krokiem jest utworzenie pliku w bieżącym powłoki, o nazwie *init.txt chmury* i wklej następującą konfigurację. Na przykład można utworzyć pliku w powłoce chmury nie na komputerze lokalnym. Można użyć dowolnego edytora, którego chcesz. Wprowadź `sensible-editor cloud-init.txt` do tworzenia pliku i wyświetlić listę dostępnych edytory. Wybierz #1, aby użyć **nano** edytora. Upewnij się, że poprawnie skopiować pliku całego init chmury szczególnie pierwszy wiersz:

```yaml
#cloud-config
package_upgrade: true
packages:
  -httpd
```
Naciśnij klawisz `ctrl-X` aby wyjść z pliku, wpisz `y` zapisać plik i naciśnij przycisk `enter` aby potwierdzić nazwę pliku na wyjściu.

Ostatnim krokiem jest utworzenie maszyny Wirtualnej z [tworzenia maszyny wirtualnej az](/cli/azure/vm#az_vm_create) polecenia. 

Poniższy przykład tworzy Maszynę wirtualną o nazwie *centos74* i tworzy kluczy SSH, jeśli nie już istnieją w domyślnej lokalizacji klucza. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`.  Użyj `--custom-data` parametr do przekazania w pliku config init chmury. Podaj pełną ścieżkę do *init.txt chmury* konfiguracji, jeśli plik został zapisany poza istnieje katalog roboczy. Poniższy przykład tworzy Maszynę wirtualną o nazwie *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Po utworzeniu maszyny Wirtualnej, interfejsu wiersza polecenia Azure zawiera informacje specyficzne dla danego wdrożenia. Zwróć uwagę na element `publicIpAddress`. Ten adres jest używany na potrzeby uzyskiwania dostępu do maszyny wirtualnej.  Trwa trochę czasu, można utworzyć maszyny Wirtualnej, pakietów do zainstalowania i aplikacji, aby rozpocząć. Istnieją zadania w tle, które nadal działać po interfejsu wiersza polecenia Azure powrót do wiersza polecenia. Można SSH do maszyny Wirtualnej i sprawdź dzienniki z inicjowaniem chmury przy użyciu kroków opisanych w części Rozwiązywanie problemów. 

## <a name="troubleshooting-cloud-init"></a>Rozwiązywanie problemów z inicjowaniem chmury
Po zainicjowano maszyny Wirtualnej, init chmurze zostanie uruchomiony za pośrednictwem wszystkich modułów i skryptów są zdefiniowane w `--custom-data` w celu skonfigurowania maszyny Wirtualnej.  Należy sprawdzić poprawność błędów lub pominięć z konfiguracji, należy wyszukać nazwę modułu (`disk_setup` lub `runcmd` na przykład) w dzienniku chmury init - znajduje się w **/var/log/cloud-init.log**.

> [!NOTE]
> Nie każdy błąd modułu powoduje krytyczny init chmury ogólny błąd konfiguracji. Na przykład za pomocą `runcmd` moduł, jeśli skrypt zakończy się niepowodzeniem, init chmury nadal zgłasza inicjowania obsługi administracyjnej zakończyło się pomyślnie, ponieważ moduł runcmd wykonany.

Więcej informacji o chmurze init rejestrowania, zapoznaj się [dokumentacji init chmury](http://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Następne kroki
Przykłady init chmury zmian konfiguracji można znaleźć w następujących dokumentach:
 
- [Dodaj dodatkowe użytkownika w systemie Linux na maszynie Wirtualnej](cloudinit-add-user.md)
- [Uruchom Menedżera pakietów, aby zaktualizować istniejące pakiety po pierwszym uruchomieniu komputera](cloudinit-update-vm.md)
- [Zmień lokalną nazwą hosta maszyny Wirtualnej](cloudinit-update-vm-hostname.md) 
- [Zainstaluj pakiet aplikacji, zaktualizuj pliki konfiguracji i wstrzyknąć kluczy](tutorial-automate-vm-deployment.md)