<properties
   pageTitle="Instalowanie interfejsu wiersza polecenia DC/OS | Microsoft Azure"
   description="Instalowanie interfejsu wiersza polecenia DC/OS."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontenery, mikrousługi, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>


>[AZURE.NOTE] Te informacje dotyczą pracy z klastrami usługi ACS opartymi na kontrolerach domeny/systemach operacyjnych. Nie trzeba wykonywać tych czynności w przypadku klastrów usługi ACS opartych na aplikacji Swarm.

Najpierw [nawiąż połączenie z klastrem usługi ACS opartym na kontrolerze domeny/systemie operacyjnym](../articles/container-service/container-service-connect.md). Następnie możesz zainstalować interfejs wiersza polecenia DC/OS na komputerze klienckim przy użyciu poniższych poleceń:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Jeśli używasz starszej wersji środowiska Python, mogą zostać wyświetlone ostrzeżenia „InsecurePlatformWarnings”. Możesz je bezpiecznie zignorować.

Aby rozpocząć pracę bez ponownego uruchamiania powłoki, uruchom polecenie:

```bash
source ~/.bashrc
```

Ta czynność nie będzie konieczna w przypadku uruchomienia nowych powłok.

Teraz można potwierdzić, że interfejs wiersza polecenia został zainstalowany:

```bash
dcos --help
```


<!--HONumber=Sep16_HO3-->


