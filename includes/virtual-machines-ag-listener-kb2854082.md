Następnie wszystkie serwery w klastrze korzystający z systemu Windows Server 2008 R2 lub Windows Server 2012, należy sprawdzić, czy poprawka [KB2854082](http://support.microsoft.com/kb/2854082) jest instalowane na każdym z serwerów lokalnych lub maszynach wirtualnych platformy Azure, które są częścią klastra. Dowolnego serwera lub maszyny Wirtualnej w klastrze, ale nie znajduje się w grupie dostępności powinien również zawierać po zainstalowaniu tej poprawki.

W sesji pulpitu zdalnego dla każdego z węzłów klastra, Pobierz [KB2854082](http://support.microsoft.com/kb/2854082) do katalogu lokalnego. Następnie należy zainstalować poprawkę w każdym węźle klastra po kolei. Jeśli usługa klastrowania jest obecnie uruchomiona w węźle klastra, ponownym uruchomieniu serwera po zakończeniu instalacji.

> [!WARNING]
> Zatrzymanie usługi klastrowania lub ponowne uruchomienie serwera wpływa na kondycję kworum klastra i grupy dostępności, co może spowodować z klastrem w celu przejścia do trybu offline. Aby zachować wysoką dostępność klastra podczas instalacji, upewnij się, że:
> 
> * Klaster jest użytkowany kondycji optymalne kworum. 
> * Przed zainstalowaniem poprawki w każdym węźle, wszystkie węzły klastra są w trybie online.
> * Przed zainstalowaniem poprawki na jednym z węzłów w klastrze, Zezwalaj na instalację poprawek do uruchomienia do ukończenia na jednym węźle, w tym pełni ponowne uruchomienie serwera.
> 
> 

