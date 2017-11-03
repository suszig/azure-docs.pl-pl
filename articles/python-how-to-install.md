---
title: "Instalowanie języka Python i zestawu SDK - Azure"
description: "Dowiedz się, jak zainstalować Python i zestawu SDK do korzystania z usługi Azure."
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.openlocfilehash: e69fff29be5b12c3c0004b4101eba69c7da87d3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="installing-python-and-the-sdk"></a>Instalowanie języka Python i zestawu SDK
Python jest łatwo skonfigurować w systemie Windows i jest wstępnie zainstalowane dla komputerów Mac, Linux, a [Bash dla systemu Windows](https://msdn.microsoft.com/commandline/wsl/about). Ten przewodnik przeprowadzi Cię przez instalacji i uzyskiwanie komputerze rozpocząć korzystanie z platformy Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Co to jest w języku Python SDK platformy Azure?
Zestaw Azure SDK for Python zawiera składniki, które umożliwiają tworzenie, wdrażanie i zarządzanie aplikacjami języka Python dla platformy Azure. W szczególności zestaw Azure SDK for Python obejmuje następujące funkcje:

* **Biblioteki zarządzania**. Te biblioteki klas udostępniają interfejs zarządzania zasobami platformy Azure, takich jak konta magazynu, maszyn wirtualnych.
* **Biblioteki wykonawcze**. Te biblioteki klas zapewniają interfejs do uzyskiwania dostępu do funkcji platformy Azure, takich jak magazyn i usługi magistrali.

## <a name="which-python-and-which-version-to-use"></a>Które Python i wersji do użycia
Istnieje kilka odmian Python tłumaczy — przykłady to między innymi:

* Języka CPython - standard i najczęściej używane interpreter języka Python
* PyPy — szybkie, zgodne alternatywnej implementacji języka CPython
* IronPython - interpreter języka Python, uruchamianego na .net/CLR
* Jython - interpreter języka Python, który działa na maszynie wirtualnej Java

**Języka CPython** v2.7 lub v3.3 + PyPy 5.4.0 przetestowane i obsługiwane w przypadku zestawu Azure SDK dla języka Python.

## <a name="where-to-get-python"></a>Skąd uzyskać Python?
Istnieje kilka sposobów, aby uzyskać języka CPython:

* Bezpośrednio z [www.python.org][www.python.org]
* Z zaufanych distro, takich jak [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] lub [www.activestate.com][www.activestate.com]
* Tworzenie źródła!

W przypadku braku konkretna potrzeba użycia firma Microsoft zaleca dwóch pierwszych opcji.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Instalacja zestawu SDK systemu Windows, Linux i MacOS (tylko w przypadku bibliotek klienta)
Jeśli masz już zainstalowany języka Python, należy zainstalować pakiet biblioteki klienta w istniejących Python 2.7 lub środowiska Python 3.3 + przez osoby pip. Spowoduje to pobranie pakietów z [indeksu pakietów języka Python] [ Python Package Index] (PyPI).

Prawa administratora może być konieczne:

* Linux i MacOS, użyj `sudo` polecenia: `sudo pip install azure-mgmt-compute`.
* System Windows: Otwórz z wiersza polecenia/programu PowerShell jako administrator

Można zainstalować oddzielnie każdej biblioteki dla poszczególnych usług Azure:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Pakiety w wersji zapoznawczej mogą być instalowane za pomocą `--pre` flagi:

```console
   $ pip install --pre azure-mgmt-compute # installs only the latest Compute Management library
```

Można także zainstalować zestaw Azure bibliotek w jednym wierszu przy użyciu `azure` meta-package. Ponieważ nie wszystkie pakiety w tym pakiecie meta są publikowane jako stabilny jeszcze `azure` meta pakiet jest dostępny w wersji zapoznawczej.
Jednak pakietami podstawowymi, z perspektywy jakości/kompletności kodu jest uznawana za "stabilna" w tym momencie

* jego jest oficjalnie oznaczony jako takie w synchronizacji z innymi językami tak szybko, jak to możliwe.
  Firma Microsoft nie zaplanowano wszelkie dodatkowe istotne zmiany do tego czasu.

Ponieważ chodzi o wersji zapoznawczej, należy użyć `--pre` flagi:

```console
   $ pip install --pre azure
```

lub bezpośrednio

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Pobieranie kolejne pakiety
[Indeksu pakietów języka Python] [ Python Package Index] (PyPI) zawiera bogaty wybór bibliotek języka Python.  Jeśli wybrano opcję zainstalowania Distro, konieczne będzie już najbardziej interesujące usługi bits dla różnych scenariuszy z projektowanie witryn sieci web do przetwarzania danych technicznych.

## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio
[Narzędzia Python Tools for Visual Studio][narzędzi Python Tools for Visual Studio] (PTVS) jest wolny OSS dodatku firmy Microsoft, która zmieni się VS w pełni funkcjonalnymi IDE języka Python:

![How-do-install-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Przy użyciu narzędzi PTVS jest opcjonalne, ale jest zalecane, ponieważ umożliwia obsługę języka Python i rozwiązanie projektu sieci Web, debugowanie, profilowanie, okna interaktywnego, edytowanie szablonu i Intellisense.

PTVS również ułatwia wdrażanie do systemu Microsoft Azure z obsługą wdrażania na [usługi w chmurze](cloud-services/cloud-services-python-ptvs.md) i [witryn sieci Web](app-service/app-service-web-overview.md).

PTVS współpracuje z istniejącej instalacji programu Visual Studio 2013, 2015 lub 2017 r.  Dokumentacja, pliki do pobrania i dyskusji dla [narzędzi Python Tools for Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python scenariuszy Azure dla systemów Linux i MacOS
Dla systemu Linux lub MacOS, główne Azure scenariusze, które są obsługiwane:

1. Korzystanie z usług Azure za pomocą biblioteki klienta dla języka Python
2. Uruchamianie aplikacji w Maszynę wirtualną systemu Linux
3. Tworzenie i publikowanie witryn sieci Web platformy Azure przy użyciu narzędzia Git

Pierwszy scenariusz umożliwia tworzenie aplikacji sieci web sformatowanego, które korzystają z możliwości Azure PaaS takich jak [magazynu obiektów blob](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [kolejki magazynu](storage/queues/storage-python-how-to-use-queue-storage.md), [tabeli magazynu](cosmos-db/table-storage-how-to-use-python.md) itp., za pomocą Pythonic otoki dla interfejsów API REST usługi Azure. Te działają tak samo w systemie Windows, Mac i Linux.  Te biblioteki klienta można używać z komputera lokalnego rozwoju lub maszyny Wirtualnej systemu Linux działających na platformie Azure.

W scenariuszu maszyny Wirtualnej wystarczy uruchomić maszyny Wirtualnej systemu Linux wybranych przez użytkownika (Ubuntu, CentOS, Suse) i uruchom lub zarządzać co Ci się podoba.  Na przykład można uruchomić [IPython] [ IPython] REPL/Notes w z systemem Windows lub Mac/Linux komputera, a następnie wskaż przeglądarkę Linux lub maszyny Wirtualnej wiele procesów systemu Windows uruchomiony aparat IPython na platformie Azure.

Aby uzyskać informacje na temat sposobu konfigurowania maszyny Wirtualnej systemu Linux, zobacz [tworzenia maszyny wirtualnej systemem Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) samouczka.

Za pomocą narzędzia Git, mogą tworzyć aplikacji sieci web języka Python i opublikować go do witryny sieci Web platformy Azure z dowolnego systemu operacyjnego.  Po naciśnięciu repozytorium Azure automatycznie tworzy środowisko wirtualne i pip instaluje wymagane pakiety.

Aby uzyskać więcej informacji na za pomocą dowolnej architektury WSGI zgodne, zobacz [Konfigurowanie Python z witryny sieci Web Azure](app-service/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>Dodatkowe oprogramowanie i zasoby:
* [Zestaw Azure SDK dla języka Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Zestaw Azure SDK dla języka Python GitHub](https://github.com/Azure/azure-sdk-for-python)
* [Oficjalna przykładów dla platformy Azure dla języka Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Kontynuacja Analytics Python dystrybucji][Continuum Analytics Python Distribution]
* [Dystrybucję oprogramowania Python Enthought][Enthought Python Distribution]
* [Dystrybucję oprogramowania Python ActiveState][ActiveState Python Distribution]
* [SciPy — pakiet bibliotek naukowych Python][SciPy - A suite of Scientific Python libraries]
* [NumPy — Biblioteka numeryczne dla języka Python][NumPy - A numerics library for Python]
* [Projekt Django — framework/system CMS dojrzałe sieci web][Django Project - A mature web framework/CMS]
* [IPython — zaawansowane REPL/Notes, dla języka Python][IPython - an advanced REPL/Notebook for Python]
* [Python Tools for Visual Studio w witrynie GitHub][Python Tools for Visual Studio on GitHub]
* [Centrum deweloperów języka Python](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[narzędzi Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
