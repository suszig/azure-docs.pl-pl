---
title: "Tworzenie obrazu Docker dla modelu zarządzania w usłudze Azure Machine Learning | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano kroki dotyczące tworzenia obrazu Docker dla usługi sieci web."
services: machine-learning
author: chhavib
ms.author: chhavib
manager: neerajkh
editor: jasonwhowell
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 03e51ab298a08386f0094d6d0290aa1ec85d337f
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="azure-machine-learning-model-management-account-api-reference"></a>Odwołanie do usługi Azure Machine Learning Model zarządzania konta API

Aby uzyskać informacje dotyczące konfigurowania środowiska wdrażania, zobacz [ustawienia konta zarządzania modelu](deployment-setup-configuration.md).

Azure Machine Learning Model zarządzania konta API zaimplementowano następujące operacje:

- Rejestracja modelu
- Tworzenie manifestu
- Tworzenie obrazu docker
- Tworzenie usługi sieci Web

Ten obraz służy do tworzenia usługi sieci web albo lokalnie lub na zdalny klaster usługi kontenera platformy Azure lub innego środowiska obsługiwane Docker wybranych przez użytkownika.

## <a name="prerequisites"></a>Wymagania wstępne
Upewnij się, że można wykonać żadnych kroków instalacji [zainstalować i utworzyć szybkiego startu](quickstart-installation.md) dokumentu.

Poniżej przedstawiono wymagania przed kontynuowaniem:
1. Model inicjowania obsługi administracyjnej kont zarządzania
2. Tworzenie środowiska wdrażania i zarządzania nimi modeli
3. Model uczenia maszynowego

### <a name="azure-ad-token"></a>Token usługi Azure AD
Podczas korzystania z interfejsu wiersza polecenia Azure, zaloguj się przy użyciu `az login`. Interfejsu wiersza polecenia używa tokenu programu Azure Active Directory (Azure AD) z pliku .azure. Jeśli chcesz użyć interfejsów API, masz następujące opcje.

##### <a name="acquire-the-azure-ad-token-manually"></a>Ręcznie uzyskać tokenu usługi Azure AD
Można użyć `az login` i pobrać najnowsze tokenu z pliku .azure w katalogu macierzystego.

##### <a name="acquire-the-azure-ad-token-programmatically"></a>Programowo uzyskać tokenu usługi Azure AD
```
az ad sp create-for-rbac --scopes /subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroupName> --role Contributor --years <length of time> --name <MyservicePrincipalContributor>
```
Po utworzeniu nazwy głównej usługi, należy zapisać dane wyjściowe. Teraz można go używać, aby uzyskać token z usługi Azure AD:

```cs
 private static async Task<string> AcquireTokenAsync(string clientId, string password, string authority, string resource)
{
        var creds = new ClientCredential(clientId, password);
        var context = new AuthenticationContext(authority);
        var token = await context.AcquireTokenAsync(resource, creds).ConfigureAwait(false);
        return token.AccessToken;
}
```
Token jest umieszczany w nagłówku autoryzacji do wywołania interfejsu API.


## <a name="register-a-model"></a>Zarejestruj modelu

W kroku rejestracji modelu rejestruje modelu uczenia maszynowego z utworzonego konta usługi Azure Management modelu. Rejestracja umożliwia śledzenie modeli i ich wersje, które są przypisane w chwili rejestracji. Użytkownik podaje nazwę modelu. Generuje kolejnych rejestracji modeli z tą samą nazwą nowej wersji i identyfikatora.

### <a name="request"></a>Żądanie

| Metoda | Identyfikator URI żądania |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modeli 
 |
### <a name="description"></a>Opis
Rejestruje modelu.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| model | treść | Ładunek, który służy do rejestrowania modelu. | Tak | [Model](#model) |


### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | OK. Rejestracja modelu zakończyło się pomyślnie. | [Model](#model) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="query-the-list-of-models-in-an-account"></a>Zapytanie listy modeli konta
### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / modeli 
 |
### <a name="description"></a>Opis
Wysyła zapytanie do listy modeli w ramach konta. Można filtrować listę wyników tagu i nazwę. Jeśli filtr nie zostanie przekazany, zapytanie Wyświetla listę wszystkich modeli w ramach konta. Jest podzielony na zwracanej liście strony, a liczba elementów w każdej z nich jest parametrem opcjonalnym.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| name | query | Nazwa obiektu. | Nie | Ciąg |
| Tag | query | Tag modelu. | Nie | Ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | Ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [PaginatedModelList](#paginatedmodellist) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="get-model-details"></a>Uzyskiwanie szczegółów modelu
### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /models/ {id}  
 |

### <a name="description"></a>Opis
Pobiera model według identyfikatora.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator obiektu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [Model](#model) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="register-a-manifest-with-the-registered-model-and-all-dependencies"></a>Zarejestruj manifestu zarejestrowanego modelu i wszystkie zależności

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / manifesty | 

### <a name="description"></a>Opis
Rejestruje manifestu z zarejestrowanego modelu i wszystkie jego zależności.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| manifestRequest | treść | Ładunek, który służy do rejestrowania manifestu. | Tak | [Manifest](#manifest) |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Manifestu rejestracja powiodła się. | [Manifest](#manifest) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="query-the-list-of-manifests-in-an-account"></a>Zapytanie listy manifestów konta

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / manifesty | 

### <a name="description"></a>Opis
Wysyła zapytanie do listy manifestów konta. Można filtrować listę wyników przez model identyfikator i nazwa manifestu. Jeśli filtr nie zostanie przekazany, zapytanie Wyświetla listę wszystkich manifestów w ramach konta. Jest podzielony na zwracanej liście strony, a liczba elementów w każdej z nich jest parametrem opcjonalnym.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| modelId | query | Identyfikator modelu. | Nie | Ciąg |
| ManifestName | query | Nazwa manifestu. | Nie | Ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | Ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [PaginatedManifestList](#paginatedmanifestlist) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="get-manifest-details"></a>Uzyskiwanie szczegółowych informacji manifestu

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /manifests/ {id} | 

### <a name="description"></a>Opis
Pobiera manifest według identyfikatora.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator obiektu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [Manifest](#manifest) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="create-an-image"></a>Tworzenie obrazu

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / obrazy | 

### <a name="description"></a>Opis
Tworzy obraz jako obraz Docker w rejestrze kontenera platformy Azure.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| imageRequest | treść | Ładunek, który jest używany do utworzenia obrazu. | Tak | [ImageRequest](#imagerequest) |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Nagłówki | Schemat |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Adres URL lokalizacji operacji asynchronicznej. Wywołanie GET Pokaż stan zadania tworzenia obrazu. | Operacja lokalizacji |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="query-the-list-of-images-in-an-account"></a>Listy obrazów w ramach konta kwerendy

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / obrazy | 

### <a name="description"></a>Opis
Wysyła zapytanie do listy obrazów w ramach konta. Można filtrować listę wyników według Identyfikatora manifestu i nazwę. Jeśli niepowodzenie bez filtru w zapytaniu przedstawiono wszystkie obrazy w ramach konta. Jest podzielony na zwracanej liście strony, a liczba elementów w każdej z nich jest parametrem opcjonalnym.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| Identyfikator manifestu | query | Identyfikator manifestu. | Nie | Ciąg |
| ManifestName | query | Nazwa manifestu. | Nie | Ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | Ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [PaginatedImageList](#paginatedimagelist) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="get-image-details"></a>Uzyskiwanie szczegółów obrazu

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /images/ {id} | 

### <a name="description"></a>Opis
Pobiera obraz według identyfikatora.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator obrazu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [Obraz](#image) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |


## <a name="create-a-service"></a>Tworzenie usługi

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / usługi | 

### <a name="description"></a>Opis
Tworzy usługę z obrazu.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| żądania obsługi | treść | Ładunek, który służy do tworzenia usługi. | Tak | [ServiceCreateRequest](#servicecreaterequest) |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Nagłówki | Schemat |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Adres URL lokalizacji operacji asynchronicznej. Wywołanie GET Pokaż stan zadania tworzenia usługi. | Operacja lokalizacji |
| 409 | Usługa o określonej nazwie już istnieje. |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="query-the-list-of-services-in-an-account"></a>Lista usług w ramach konta kwerendy

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / usługi | 

### <a name="description"></a>Opis
Wysyła zapytanie do listy usług dla konta. Można filtrować listę wyników Identyfikatora nazwy modelu, manifestu nazwa, identyfikator obrazu, nazwa usługi lub Machine Learning obliczeń z identyfikatorem zasobu. Jeśli filtr nie zostanie przekazany, zapytanie Wyświetla listę wszystkich usług w ramach konta. Jest podzielony na zwracanej liście strony, a liczba elementów w każdej z nich jest parametrem opcjonalnym.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| ServiceName | query | Nazwa usługi. | Nie | Ciąg |
| modelId | query | Nazwa modelu. | Nie | Ciąg |
| modelName | query | Identyfikator modelu. | Nie | Ciąg |
| Identyfikator manifestu | query | Identyfikator manifestu. | Nie | Ciąg |
| ManifestName | query | Nazwa manifestu. | Nie | Ciąg |
| imageId | query | Identyfikator obrazu. | Nie | Ciąg |
| computeResourceId | query | Machine Learning obliczeń identyfikator zasobu. | Nie | Ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | Ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [PaginatedServiceList](#paginatedservicelist) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse) |

## <a name="get-service-details"></a>Pobierz szczegóły usługi

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /services/ {id} | 

### <a name="description"></a>Opis
Pobiera usługę według identyfikatora.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator obiektu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [ServiceResponse](#serviceresponse) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)

## <a name="update-a-service"></a>Aktualizowanie usługi

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| PUT |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /services/ {id} | 

### <a name="description"></a>Opis
Aktualizuje istniejącą usługę.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator obiektu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| serviceUpdateRequest | treść | Ładunek, który jest używany do zaktualizowania istniejącej usługi. | Tak |  [ServiceUpdateRequest](#serviceupdaterequest) |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Nagłówki | Schemat |
|--------------------|--------------------|--------------------|--------------------|
| 202 | Adres URL lokalizacji operacji asynchronicznej. Wywołanie GET Pokaż stan zadania aktualizacji usługi. | Operacja lokalizacji |
| 404 | Usługa o określonym identyfikatorze nie istnieje. |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)

## <a name="delete-a-service"></a>Usuwanie usługi

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| DELETE |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /services/ {id} | 

### <a name="description"></a>Opis
Usuwa usługę.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator obiektu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. |  |
| 204 | Usługa o określonym identyfikatorze nie istnieje. |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)

## <a name="get-service-keys"></a>Pobieranie kluczy usługi

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /services/ {id} / klucze | 

### <a name="description"></a>Opis
Pobiera klucze usługi.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator usługi. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [AuthKeys](#authkeys)
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)

## <a name="regenerate-service-keys"></a>Ponowne generowanie kluczy usługi

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| POST |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /services/ {id} / klucze | 

### <a name="description"></a>Opis
Generuje ponownie klucze usługi i zwraca je.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator usługi. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| regenerateKeyRequest | treść | Ładunek, który jest używany do zaktualizowania istniejącej usługi. | Tak | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [AuthKeys](#authkeys)
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)

## <a name="query-the-list-of-deployments-in-an-account"></a>Zapytanie listy wdrożeń konta

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} /accounts/ {accountName} / wdrożenia | 

### <a name="description"></a>Opis
Wysyła zapytanie do listy wdrożeń w ramach konta. Można filtrować listę wyników według Identyfikatora usługi, która będzie zwracać tylko wdrożenia, które są tworzone dla określonej usługi. Jeśli niepowodzenie bez filtru w zapytaniu przedstawiono wszystkie wdrożenia w ramach konta.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |
| serviceId | query | Identyfikator usługi. | Nie | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [DeploymentList](#deploymentlist) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)

## <a name="get-deployment-details"></a>Pobierz szczegóły wdrożenia

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /deployments/ {id} | 

### <a name="description"></a>Opis
Pobiera wdrożenia według identyfikatora.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator wdrożenia. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [Wdrożenie](#deployment) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)

## <a name="get-operation-details"></a>Szczegóły operacji pobierania

### <a name="request"></a>Żądanie
| Metoda | Identyfikator URI żądania |
|------------|------------|
| GET |  /API/subscriptions / {subscriptionId} /resourceGroups/ {resourceGroupName} {accountName} /accounts/ /operations/ {id} | 

### <a name="description"></a>Opis
Pobiera stan operacji asynchronicznej według identyfikatora operacji.

### <a name="parameters"></a>Parametry
| Nazwa | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Tak | Ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Tak | Ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Tak | Ciąg |
| id | ścieżka | Identyfikator operacji. | Tak | Ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Tak | Ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Tak | Ciąg |

### <a name="responses"></a>Odpowiedzi
| Kod | Opis | Schemat |
|--------------------|--------------------|--------------------|
| 200 | Powodzenie. | [OperationStatus](#asyncoperationstatus) |
| domyślnie | Błąd odpowiedzi, która opisuje Dlaczego nie można wykonać operację. | [Errorresponse języka](#errorresponse)



<a name="definitions"></a>
## <a name="definitions"></a>Definicje

<a name="asset"></a>
### <a name="asset"></a>Zasób
Obiekt zasobów, który będzie wymagany podczas tworzenia obrazu Docker.


|Nazwa|Opis|Schemat|
|---|---|---|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator zasobu.|Ciąg|
|**mimeType**  <br>*opcjonalne*|Typ MIME zawartości modelu. Aby uzyskać więcej informacji o typie MIME, zobacz [listę typów nośników IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|Ciąg|
|**Rozpakowywanie**  <br>*opcjonalne*|Wskazuje, gdzie musimy rozpakuj zawartość podczas tworzenia obrazu Docker.|wartość logiczna|
|**adres URL**  <br>*opcjonalne*|Adres URL lokalizacji zasobów.|Ciąg|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Stan operacji asynchronicznej.

*Typ*: wyliczenia (NotStarted, uruchamianie, odwołania, zakończyło się pomyślnie, nie powiodła się)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
Stan operacji.


|Nazwa|Opis|Schemat|
|---|---|---|
|**createdTime**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas utworzenia operacji asynchronicznych (UTC).|ciąg (daty i godziny)|
|**wartość endTime**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas zakończenia operacji asynchronicznych (UTC).|ciąg (daty i godziny)|
|**Błąd**  <br>*opcjonalne*||[Errorresponse języka](#errorresponse)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator operacji asynchronicznej|Ciąg|
|**Typ operacji**  <br>*opcjonalne*|Typ operacji asynchronicznej.|wyliczenia (obraz, usługa)|
|**resourceLocation**  <br>*opcjonalne*|Zasób tworzony lub aktualizowany przez operacji asynchronicznej.|Ciąg|
|**Stan**  <br>*opcjonalne*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Klucze uwierzytelniania dla usługi.


|Nazwa|Opis|Schemat|
|---|---|---|
|**primaryKey**  <br>*opcjonalne*|Klucz podstawowy.|Ciąg|
|**Klucz pomocniczy**  <br>*opcjonalne*|Klucz pomocniczy.|Ciąg|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Ustawienia dla autoscaler.


|Nazwa|Opis|Schemat|
|---|---|---|
|**autoscaleEnabled**  <br>*opcjonalne*|Włącz lub wyłącz autoscaler.|wartość logiczna|
|**maxReplicas**  <br>*opcjonalne*|Maksymalna liczba replik pod do skalowania.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**minReplicas**  <br>*opcjonalne*|Minimalna liczba replik pod można skalować do.  <br>**Minimalna wartość**:`0`|liczba całkowita|
|**refreshPeriodInSeconds**  <br>*opcjonalne*|Odśwież czasu dla wyzwalacza Skalowanie automatyczne.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**targetUtilization**  <br>*opcjonalne*|Procent użycia, które wyzwala Skalowanie automatyczne.  <br>**Minimalna wartość**:`0`  <br>**Maksymalna wartość**:`100`|liczba całkowita|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Uczenie maszynowe zasobów obliczeniowych.


|Nazwa|Opis|Schemat|
|---|---|---|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator zasobu.|Ciąg|
|**Typ**  <br>*opcjonalne*|Typ zasobu.|wyliczenia (klaster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Konfiguracja zarezerwować zasobów dla kontenera w klastrze.


|Nazwa|Opis|Schemat|
|---|---|---|
|**Procesor CPU**  <br>*opcjonalne*|Określa zastrzeżenie procesora CPU. Format Kubernetes: zobacz [znaczenie Procesora](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|Ciąg|
|**pamięci**  <br>*opcjonalne*|Określa zastrzeżenie pamięci. Format Kubernetes: zobacz [znaczenie pamięci](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|Ciąg|


<a name="deployment"></a>
### <a name="deployment"></a>Wdrożenie
Wystąpienia wdrożenia usługi Azure Machine Learning.


|Nazwa|Opis|Schemat|
|---|---|---|
|**createdAt**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas utworzenia wdrożenia (UTC).|ciąg (daty i godziny)|
|**expiredAt**  <br>*opcjonalne*  <br>*tylko do odczytu*|Ważność wdrożenia czasu (UTC).|ciąg (daty i godziny)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator wdrożenia.|Ciąg|
|**imageId**  <br>*opcjonalne*|Identyfikator obrazu skojarzonego z tym wdrożeniem.|Ciąg|
|**serviceName**  <br>*opcjonalne*|Nazwa usługi.|Ciąg|
|**Stan**  <br>*opcjonalne*|Bieżący stan wdrożenia.|Ciąg|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Tablica obiektów wdrożenia.

*Typ*: <[wdrożenia](#deployment)> tablicy


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Model, szczegóły błędu usługi zarządzania.


|Nazwa|Opis|Schemat|
|---|---|---|
|**Kod**  <br>*Wymagane*|Kod błędu.|Ciąg|
|**Komunikat**  <br>*Wymagane*|Komunikat o błędzie.|Ciąg|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>Errorresponse języka
Obiekt błąd usługi zarządzania w modelu.


|Nazwa|Opis|Schemat|
|---|---|---|
|**Kod**  <br>*Wymagane*|Kod błędu.|Ciąg|
|**Szczegóły**  <br>*opcjonalne*|Tablica obiektów szczegółów błędu.|<[ErrorDetail](#errordetail)> tablicy|
|**Komunikat**  <br>*Wymagane*|Komunikat o błędzie.|Ciąg|
|**statusCode**  <br>*opcjonalne*|Kod stanu HTTP.|liczba całkowita|


<a name="image"></a>
### <a name="image"></a>Image (Obraz)
Obraz usługi Azure Machine Learning.


|Nazwa|Opis|Schemat|
|---|---|---|
|**computeResourceId**  <br>*opcjonalne*|Identyfikator środowiska utworzone w zasób obliczeniowy uczenia maszynowego.|Ciąg|
|**createdTime**  <br>*opcjonalne*|Czas utworzenia obrazu (UTC).|ciąg (daty i godziny)|
|**creationState**  <br>*opcjonalne*||[AsyncOperationState](#asyncoperationstate)|
|**Opis elementu**  <br>*opcjonalne*|Opis obrazu.|Ciąg|
|**Błąd**  <br>*opcjonalne*||[Errorresponse języka](#errorresponse)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator obrazu.|Ciąg|
|**imageBuildLogUri**  <br>*opcjonalne*|Identyfikator URI przekazanych dzienników z kompilacji obrazu.|Ciąg|
|**Element imageLocation**  <br>*opcjonalne*|Ciąg lokalizacji Azure rejestru kontenera utworzony obraz.|Ciąg|
|**imageType**  <br>*opcjonalne*||[ImageType](#imagetype)|
|**Manifest**  <br>*opcjonalne*||[Manifest](#manifest)|
|**Nazwa**  <br>*opcjonalne*|Nazwa obrazu.|Ciąg|
|**Wersja**  <br>*opcjonalne*|Wersja obrazu ustawiony przez usługę zarządzania modelu.|liczba całkowita|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Żądanie utworzenia obrazu usługi Azure Machine Learning.


|Nazwa|Opis|Schemat|
|---|---|---|
|**computeResourceId**  <br>*Wymagane*|Identyfikator środowiska utworzone w zasób obliczeniowy uczenia maszynowego.|Ciąg|
|**Opis elementu**  <br>*opcjonalne*|Opis obrazu.|Ciąg|
|**imageType**  <br>*Wymagane*||[ImageType](#imagetype)|
|**Identyfikator manifestu**  <br>*Wymagane*|Identyfikator manifestu, w którym zostanie utworzony obraz.|Ciąg|
|**Nazwa**  <br>*Wymagane*|Nazwa obrazu.|Ciąg|


<a name="imagetype"></a>
### <a name="imagetype"></a>Typ obrazu
Określa typ obrazu.

*Typ*: wyliczenia (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifest
Uczenie maszynowe Azure manifestu.


|Nazwa|Opis|Schemat|
|---|---|---|
|**zasoby**  <br>*Wymagane*|Lista zasobów.|<[Zasobów](#asset)> tablicy|
|**createdTime**  <br>*opcjonalne*  <br>*tylko do odczytu*|Manifest godzina utworzenia (UTC).|ciąg (daty i godziny)|
|**Opis elementu**  <br>*opcjonalne*|Manifest tekst opisu.|Ciąg|
|**driverProgram**  <br>*Wymagane*|Sterownik programu manifestu.|Ciąg|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator manifestu.|Ciąg|
|**modelIds**  <br>*opcjonalne*|Lista identyfikatorów modelu w zarejestrowany modeli. Żądanie zakończy się niepowodzeniem, jeśli dowolnego dołączone modelu nie zostały zarejestrowane.|<string>Tablica|
|**modelType**  <br>*opcjonalne*|Określa, że modele są już zarejestrowane w usłudze zarządzania modelu.|wyliczenia (zarejestrowane)|
|**Nazwa**  <br>*Wymagane*|Nazwa manifestu.|Ciąg|
|**targetRuntime**  <br>*Wymagane*||[TargetRuntime](#targetruntime)|
|**Wersja**  <br>*opcjonalne*  <br>*tylko do odczytu*|Wersja manifestu przypisany przez usługę zarządzania modelu.|liczba całkowita|
|**webserviceType**  <br>*opcjonalne*|Określa typ żądanej usługi sieci web, która zostanie utworzona w manifeście.|wyliczenia (w czasie rzeczywistym)|


<a name="model"></a>
### <a name="model"></a>Model
Wystąpienie modelu uczenia maszynowego Azure.


|Nazwa|Opis|Schemat|
|---|---|---|
|**createdAt**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas utworzenia modelu (UTC).|ciąg (daty i godziny)|
|**Opis elementu**  <br>*opcjonalne*|Opis modelu.|Ciąg|
|**Identyfikator**  <br>*opcjonalne*  <br>*tylko do odczytu*|Identyfikator modelu.|Ciąg|
|**mimeType**  <br>*Wymagane*|Typ MIME zawartości modelu. Aby uzyskać więcej informacji o typie MIME, zobacz [listę typów nośników IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|Ciąg|
|**Nazwa**  <br>*Wymagane*|Nazwa modelu.|Ciąg|
|**tagów**  <br>*opcjonalne*|Lista znaczników modelu.|<string>Tablica|
|**Rozpakowywanie**  <br>*opcjonalne*|Wskazuje, czy musimy Rozpakuj modelu podczas tworzenia obrazu Docker.|wartość logiczna|
|**adres URL**  <br>*Wymagane*|Adres URL modelu. Zazwyczaj testujemy adres URL sygnatury dostępu współdzielonego w tym miejscu.|Ciąg|
|**Wersja**  <br>*opcjonalne*  <br>*tylko do odczytu*|Wersja modelu przypisany przez usługę zarządzania modelu.|liczba całkowita|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Informacje o modelu danych kolekcji.


|Nazwa|Opis|Schemat|
|---|---|---|
|**eventHubEnabled**  <br>*opcjonalne*|Włącz Centrum zdarzeń dla usługi.|wartość logiczna|
|**storageEnabled**  <br>*opcjonalne*|Włącz magazyn dla usługi.|wartość logiczna|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Na liście obrazów.


|Nazwa|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|Ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów modelu.|<[Obraz](#image)> tablicy|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Lista podzieloną manifestów.


|Nazwa|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|Ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów manifestu.|<[Manifest](#manifest)> tablicy|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Na listę modeli.


|Nazwa|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|Ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów modelu.|<[Model](#model)> tablicy|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Na liście usług.


|Nazwa|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|Ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów usługi.|<[ServiceResponse](#serviceresponse)> tablicy|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Żądanie utworzenia usługi.


|Nazwa|Opis|Schemat|
|---|---|---|
|**appInsightsEnabled**  <br>*opcjonalne*|Włącz usługę application insights dla usługi.|wartość logiczna|
|**autoScaler**  <br>*opcjonalne*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Wymagane*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*opcjonalne*||[ContainerResourceReservation](#containerresourcereservation)|
|**Klasa dataCollection**  <br>*opcjonalne*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Wymagane*|Obraz, aby utworzyć usługę.|Ciąg|
|**maxConcurrentRequestsPerContainer**  <br>*opcjonalne*|Maksymalna liczba równoczesnych żądań.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**Nazwa**  <br>*Wymagane*|Nazwa usługi.|Ciąg|
|**numReplicas**  <br>*opcjonalne*|Liczba replik pod uruchomiony w dowolnym momencie. Nie można określić, czy Autoscaler jest włączone.  <br>**Minimalna wartość**:`0`|liczba całkowita|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Żądanie ponownego generowania klucza dla usługi.


|Nazwa|Opis|Schemat|
|---|---|---|
|**Właściwość keyType**  <br>*opcjonalne*|Określa klucz, do którego można ponownie wygenerować.|wyliczenia (podstawowe, pomocniczy)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Szczegółowy stan usługi.


|Nazwa|Opis|Schemat|
|---|---|---|
|**createdAt**  <br>*opcjonalne*|Tworzenie usługi godzina (UTC).|ciąg (daty i godziny)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator usługi.|Ciąg|
|**Obraz**  <br>*opcjonalne*||[Obraz](#image)|
|**Manifest**  <br>*opcjonalne*||[Manifest](#manifest)|
|**modele**  <br>*opcjonalne*|Lista modeli.|<[Model](#model)> tablicy|
|**Nazwa**  <br>*opcjonalne*|Nazwa usługi.|Ciąg|
|**scoringUri**  <br>*opcjonalne*|Identyfikator URI dla oceniania usługi.|Ciąg|
|**Stan**  <br>*opcjonalne*||[AsyncOperationState](#asyncoperationstate)|
|**updatedAt**  <br>*opcjonalne*|Data ostatniej aktualizacji (UTC).|ciąg (daty i godziny)|
|**appInsightsEnabled**  <br>*opcjonalne*|Włącz usługę application insights dla usługi.|wartość logiczna|
|**autoScaler**  <br>*opcjonalne*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Wymagane*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*opcjonalne*||[ContainerResourceReservation](#containerresourcereservation)|
|**Klasa dataCollection**  <br>*opcjonalne*||[ModelDataCollection](#modeldatacollection)|
|**maxConcurrentRequestsPerContainer**  <br>*opcjonalne*|Maksymalna liczba równoczesnych żądań.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**numReplicas**  <br>*opcjonalne*|Liczba replik pod uruchomiony w dowolnym momencie. Nie można określić, czy Autoscaler jest włączone.  <br>**Minimalna wartość**:`0`|liczba całkowita|
|**Błąd**  <br>*opcjonalne*||[Errorresponse języka](#errorresponse)|


<a name="serviceupdaterequest"></a>
### <a name="serviceupdaterequest"></a>serviceUpdateRequest
Żądanie aktualizacji usługi.


|Nazwa|Opis|Schemat|
|---|---|---|
|**appInsightsEnabled**  <br>*opcjonalne*|Włącz usługę application insights dla usługi.|wartość logiczna|
|**autoScaler**  <br>*opcjonalne*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*opcjonalne*||[ContainerResourceReservation](#containerresourcereservation)|
|**Klasa dataCollection**  <br>*opcjonalne*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*opcjonalne*|Obraz, aby utworzyć usługę.|Ciąg|
|**maxConcurrentRequestsPerContainer**  <br>*opcjonalne*|Maksymalna liczba równoczesnych żądań.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**numReplicas**  <br>*opcjonalne*|Liczba replik pod uruchomiony w dowolnym momencie. Nie można określić, czy Autoscaler jest włączone.  <br>**Minimalna wartość**:`0`|liczba całkowita|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Typ docelowego środowiska uruchomieniowego.


|Nazwa|Opis|Schemat|
|---|---|---|
|**właściwości**  <br>*Wymagane*||< ciąg, ciąg > mapy|
|**runtimeType**  <br>*Wymagane*|Określa środowisko uruchomieniowe.|wyliczenia (SparkPython, Python)|

