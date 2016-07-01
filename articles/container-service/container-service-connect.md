<properties
   pageTitle="Łączenie z klastrem usługi kontenera platformy Azure | Microsoft Azure"
   description="Łączenie z klastrem usługi kontenera platformy Azure przy użyciu tunelu SSH."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>


# Łączenie z klastrem usługi kontenera platformy Azure

Klastry DC/OS i Swarm wdrażane przez usługę kontenera platformy Azure uwidaczniają punkty końcowe REST. Te punkty końcowe nie są jednak otwarte dla użytkowników zewnętrznych. Aby zarządzać tymi punktami końcowymi, należy utworzyć tunel Secure Shell (SSH). Po ustanowieniu tunelu SSH możesz uruchamiać polecenia względem punktów końcowych klastra i wyświetlać interfejs użytkownika klastra za pośrednictwem przeglądarki we własnym systemie. Ten dokument zawiera opis kroków tworzenia tunelu SSH w systemach Linux, OS X i Windows.

>[AZURE.NOTE] Sesję SSH możesz utworzyć przy użyciu systemu zarządzania klastrem. Nie jest to jednak zalecane. Praca bezpośrednio w systemie zarządzania niesie ze sobą ryzyko przypadkowego wprowadzenia zmian konfiguracji.   

## Tworzenie tunelu SSH w systemie Linux lub OS X

Pierwszym krokiem tworzenia tunelu SSH w systemie Linux lub OS X jest zlokalizowanie publicznej nazwy DNS serwerów głównych ze zrównoważonym obciążeniem. W tym celu rozwiń grupę zasobów, aby wyświetlić poszczególne zasoby. Znajdź i wybierz publiczny adres IP serwera głównego. Spowoduje to otwarcie bloku z informacjami o publicznym adresie IP, w tym o nazwie DNS. Zapisz tę nazwę do późniejszego użycia. <br />


![Publiczna nazwa DNS](media/pubdns.png)

Teraz otwórz powłokę i uruchom poniższe polecenie, gdzie:

**PORT** to port punktu końcowego, który chcesz udostępnić. W przypadku klastra Swarm jest to port 2375. W przypadku klastra DC/OS jest to port 80.  
**USERNAME** (nazwa_użytkownika) to nazwa użytkownika podana podczas wdrażania klastra.  
**DNSPREFIX** (prefiks_DNS) to prefiks DNS podany podczas wdrażania klastra.  
**REGION** to region, w którym znajduje się grupa zasobów.  
**PATH_TO_PRIVATE_KEY** (ścieżka_do_klucza_prywatnego) [OPCJONALNIE] to ścieżka do klucza prywatnego odpowiadająca kluczowi prywatnemu podanemu podczas tworzenia usługi kontenera. Użyj tej opcji z flagą -i.

```bash
# ssh sample

ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Port połączenia SSH to 2200, a nie standardowy port 22.

## Tunel DC/OS

Aby otworzyć tunel do punktów końcowych powiązanych z klastrem DC/OS, wykonaj polecenie podobne do następującego:

```bash
# ssh sample

sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Teraz możesz uzyskiwać dostęp do punktów końcowych powiązanych z klastrem DC/OS w następujących lokalizacjach:

- DC/OS: `http://localhost/`
- Marathon: `http://localhost/marathon`
- Mesos: `http://localhost/mesos`

Podobnie za pomocą tego tunelu możesz uzyskiwać dostęp do interfejsów API REST dla poszczególnych aplikacji.

## Tunel Swarm

Aby otworzyć tunel do punktu końcowego Swarm, wykonaj polecenie podobne do następującego:

```bash
# ssh sample

ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Teraz możesz ustawić w poniższy sposób zmienną środowiskową DOCKER_HOST i nadal w zwykły sposób używać interfejsu wiersza polecenia Docker.

```bash
export DOCKER_HOST=:2375
```

## Tworzenie tunelu SSH w systemie Windows

Istnieje wiele opcji tworzenia tuneli SSH w systemie Windows. W tym dokumencie opisano sposób wykonania tej czynności przy użyciu programu PuTTY.

Pobierz program PuTTY do systemu Windows i uruchom aplikację.

Wprowadź nazwę hosta złożoną z nazwy użytkownika administratora klastra i publicznej nazwy DNS pierwszego serwera głównego w klastrze. **Nazwa hosta** będzie wyglądać następująco: `adminuser@PublicDNS`. W polu **Port** wprowadź wartość 2200.

![Konfiguracja programu PuTTY 1](media/putty1.png)

Wybierz pozycje `SSH` i `Authentication`. Dodaj plik klucza prywatnego na potrzeby uwierzytelniania.

![Konfiguracja programu PuTTY 2](media/putty2.png)

Wybierz pozycję `Tunnels` i skonfiguruj następujące przekazane porty:
- **Port źródłowy:** — zgodnie z własnymi preferencjami: użyj portu 80 w przypadku opcji DC/OS lub portu 2375 w przypadku opcji Swarm.
- **Miejsce docelowe:** — użyj wartości localhost:80 w przypadku opcji DC/OS lub wartości localhost:2375 w przypadku opcji Swarm.

Poniższy przykład został skonfigurowany na potrzeby opcji DC/OS, ale będzie wyglądać podobnie w przypadku rozwiązania Docker Swarm.

>[AZURE.NOTE] W przypadku tworzenia tego tunelu nie można używać portu 80.

![Konfiguracja programu PuTTY 3](media/putty3.png)

Po zakończeniu zapisz konfigurację połączenia, a następnie nawiąż połączenie z sesją programu PuTTY. Po nawiązaniu połączenia konfiguracja portów będzie dostępna w dzienniku zdarzeń programu PuTTY.

![Dziennik zdarzeń programu PuTTY](media/putty4.png)

Po skonfigurowaniu tunelu dla opcji DC/OS można uzyskiwać dostęp do powiązanego punktu końcowego w następujących lokalizacjach:

- DC/OS: `http://localhost/`
- Marathon: `http://localhost/marathon`
- Mesos: `http://localhost/mesos`

Po skonfigurowaniu tunelu dla rozwiązania Docker Swarm można uzyskiwać dostęp do klastra Swarm za pośrednictwem interfejsu wiersza polecenia programu Docker. Najpierw należy skonfigurować zmienną środowiskową systemu Windows o nazwie `DOCKER_HOST` i wartości ` :2375`.

## Następne kroki

Wdrażanie kontenerów i zarządzanie nimi przy użyciu rozwiązania DC/OS lub Swarm.

[Praca z usługą kontenera Azure i klastrem DC/OS](container-service-mesos-marathon-rest.md)
[Praca z usługą kontenera Azure i klastrem Docker Swarm](container-service-docker-swarm.md)



<!--HONumber=Jun16_HO2-->


