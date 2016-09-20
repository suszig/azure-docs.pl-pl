<properties
    pageTitle="Używanie programu Outlook w usłudze Azure RemoteApp | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować program Outlook i używać go w usłudze Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# Używanie programu Microsoft Outlook w usłudze Azure RemoteApp

> [AZURE.IMPORTANT]
> Usługa Azure RemoteApp nie jest już obsługiwana. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).

Usługa Azure RemoteApp obsługuje program Microsoft Outlook usługi O365. Dowiedz się więcej na temat [działania pakietu Office w usłudze Azure RemoteApp](remoteapp-officesubscription.md). Istnieje kilka zalecanych ustawień dla programu Outlook używanego w usłudze Azure RemoteApp.

## Tryb buforowany
Tryb buforowany jest zalecaną konfiguracją w przypadku korzystania z programu Outlook w usłudze Azure RemoteApp. Po skonfigurowaniu konta programu Outlook 2013 do używania trybu buforowanej wymiany program Outlook 2013 współpracuje z lokalną kopią należącej do użytkownika skrzynki pocztowej programu Microsoft Exchange, która jest przechowywana w pliku danych w trybie offline (pliku OST) na komputerze użytkownika wraz z książką adresową trybu offline (OAB). Buforowana skrzynka pocztowa i książka adresowa offline są okresowo aktualizowane danymi z usługi O365. Przeczytaj więcej na temat [różnic między trybem buforowanym a trybem online](https://technet.microsoft.com/library/jj683103.aspx).

Użytkownik może wybrać **tryb buforowanej wymiany** lub **tryb online** podczas konfigurowania konta lub zmieniania jego ustawień. Można także wdrożyć jeden z tych trybów przy użyciu narzędzia dostosowywania pakietu Office (OCT) lub zasad grupy.  

Przeczytaj [instrukcje krok po kroku dotyczące włączania trybu buforowanego](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## Wyszukiwanie
Wyszukiwanie w programie Outlook w usłudze Azure RemoteApp ma pewne ograniczenia. Usługa Azure RemoteApp używa puli maszyn wirtualnych na potrzeby sesji użytkownika. Indeksowanie wyszukiwania zależy od identyfikatora komputera, który jest różny na różnych maszynach wirtualnych. Użytkownicy logujący się do usługi Azure RemoteApp mogą być za każdym razem przekierowywani do nowej maszyny wirtualnej. Oznacza to, że jeśli pozwolimy na wyszukiwanie lokalne, indeksator będzie uruchamiany przy każdej zmianie identyfikatora komputera (użyciu innej maszyny wirtualnej). W zależności od rozmiaru pliku OST działanie indeksatora może zajmować dużo czasu i zużywać zasoby potrzebne dla innych aplikacji. Wyszukiwanie nie tylko jest powolne, ale może nie dawać wyników. Ten problem można obejść, używając profilu konta w trybie online, ale mogłoby to spowodować obniżenie ogólnej wydajności z powodu braku lokalnej pamięci podręcznej (aby uzyskać więcej informacji o różnicach między trybem online i trybem pamięci podręcznej, zobacz powyższy link). Niestety, nie można wyłączyć indeksowanego/lokalnego wyszukiwania ani włączyć wyszukiwania w trybie online jako domyślnego w programie Outlook 2013.

W programie Outlook 2016 rozwiązano ten problem w trybie pamięci podręcznej dzięki realizacji nowego środowiska wyszukiwania usług dla skrzynek pocztowych hostowanych w programie Exchange 2016 (lub w usłudze Office 365). Ta metoda korzysta z wyników wyszukiwania usług zamiast lokalnej pamięci podręcznej (OST). W programie Outlook można przywrócić używanie lokalnego indeksatora wyszukiwania w niektórych scenariuszach, ale większość operacji wyszukiwania będzie korzystać z tej nowej funkcji wyszukiwania usług. Zalecamy, aby podczas korzystania z usługi Azure RemoteApp używać programu Outlook 2016, jeśli wyszukiwanie poczty jest bardzo ważne.



<!--HONumber=sep16_HO1-->


