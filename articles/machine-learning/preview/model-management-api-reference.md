---
title: "Tworzenie obrazu Docker dla modelu zarządzania w usłudze Azure Machine Learning | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano kroki dotyczące tworzenia obrazu Docker dla usługi sieci web."
services: machine-learning
author: chhavib
ms.author: chhavib
manager: neerajkh
editor: jasonwhowell
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 134971e4a663baefa4e1051f087038d3debcb969
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| model | treść | Ładunek, który służy do rejestrowania modelu. | Yes | [Model](#model) |


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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| name | query | Nazwa obiektu. | Nie | ciąg |
| Tag | query | Tag modelu. | Nie | ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator obiektu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| manifestRequest | treść | Ładunek, który służy do rejestrowania manifestu. | Yes | [Manifest](#manifest) |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| modelId | query | Identyfikator modelu. | Nie | ciąg |
| ManifestName | query | Nazwa manifestu. | Nie | ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator obiektu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| imageRequest | treść | Ładunek, który jest używany do utworzenia obrazu. | Yes | [ImageRequest](#imagerequest) |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| Identyfikator manifestu | query | Identyfikator manifestu. | Nie | ciąg |
| ManifestName | query | Nazwa manifestu. | Nie | ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator obrazu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| żądania obsługi | treść | Ładunek, który służy do tworzenia usługi. | Yes | [ServiceCreateRequest](#servicecreaterequest) |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| ServiceName | query | Nazwa usługi. | Nie | ciąg |
| modelId | query | Nazwa modelu. | Nie | ciąg |
| modelName | query | Identyfikator modelu. | Nie | ciąg |
| Identyfikator manifestu | query | Identyfikator manifestu. | Nie | ciąg |
| ManifestName | query | Nazwa manifestu. | Nie | ciąg |
| imageId | query | Identyfikator obrazu. | Nie | ciąg |
| computeResourceId | query | Machine Learning obliczeń identyfikator zasobu. | Nie | ciąg |
| liczba | query | Liczba elementów do pobrania na stronie. | Nie | ciąg |
| $skipToken | query | Token kontynuacji do pobrania następnej strony. | Nie | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator obiektu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator obiektu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| serviceUpdateRequest | treść | Ładunek, który jest używany do zaktualizowania istniejącej usługi. | Yes |  [ServiceUpdateRequest](#serviceupdaterequest) |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator obiektu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator usługi. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator usługi. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| regenerateKeyRequest | treść | Ładunek, który jest używany do zaktualizowania istniejącej usługi. | Yes | [ServiceRegenerateKeyRequest](#serviceregeneratekeyrequest) |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |
| serviceId | query | Identyfikator usługi. | Nie | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator wdrożenia. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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
| Name (Nazwa) | Lokalizacja | Opis | Wymagane | Schemat
|--------------------|--------------------|--------------------|--------------------|--------------------|
| subscriptionId | ścieżka | Identyfikator subskrypcji platformy Azure. | Yes | ciąg |
| Grupy zasobów o nazwie | ścieżka | Nazwa grupy zasobów, w którym znajduje się konto zarządzania modelu. | Yes | ciąg |
| Nazwa konta | ścieżka | Nazwa konta zarządzania modelu. | Yes | ciąg |
| id | ścieżka | Identyfikator operacji. | Yes | ciąg |
| wersja interfejsu API | query | Wersja interfejsu API do używania dostawcy zasobów Microsoft.Machine.Learning. | Yes | ciąg |
| Autoryzacja | nagłówek | Token autoryzacji. Powinien być podobny do ciągu "Bearer XXXXXX". | Yes | ciąg |

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


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator zasobu.|ciąg|
|**mimeType**  <br>*opcjonalne*|Typ MIME zawartości modelu. Aby uzyskać więcej informacji o typie MIME, zobacz [listę typów nośników IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|ciąg|
|**Rozpakowywanie**  <br>*opcjonalne*|Wskazuje, gdzie musimy rozpakuj zawartość podczas tworzenia obrazu Docker.|wartość logiczna|
|**adres URL**  <br>*opcjonalne*|Adres URL lokalizacji zasobów.|ciąg|


<a name="asyncoperationstate"></a>
### <a name="asyncoperationstate"></a>AsyncOperationState
Stan operacji asynchronicznej.

*Typ*: wyliczenia (NotStarted, uruchamianie, odwołania, zakończyło się pomyślnie, nie powiodła się)


<a name="asyncoperationstatus"></a>
### <a name="asyncoperationstatus"></a>AsyncOperationStatus
Stan operacji.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**createdTime**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas utworzenia operacji asynchronicznych (UTC).|ciąg (daty i godziny)|
|**wartość endTime**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas zakończenia operacji asynchronicznych (UTC).|ciąg (daty i godziny)|
|**Błąd**  <br>*opcjonalne*||[Errorresponse języka](#errorresponse)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator operacji asynchronicznej|ciąg|
|**Typ operacji**  <br>*opcjonalne*|Typ operacji asynchronicznej.|wyliczenia (obraz, usługa)|
|**resourceLocation**  <br>*opcjonalne*|Zasób tworzony lub aktualizowany przez operacji asynchronicznej.|ciąg|
|**Stan**  <br>*opcjonalne*||[AsyncOperationState](#asyncoperationstate)|


<a name="authkeys"></a>
### <a name="authkeys"></a>AuthKeys
Klucze uwierzytelniania dla usługi.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**primaryKey**  <br>*opcjonalne*|Klucz podstawowy.|ciąg|
|**Klucz pomocniczy**  <br>*opcjonalne*|Klucz pomocniczy.|ciąg|


<a name="autoscaler"></a>
### <a name="autoscaler"></a>AutoScaler
Ustawienia dla autoscaler.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**autoscaleEnabled**  <br>*opcjonalne*|Włącz lub wyłącz autoscaler.|wartość logiczna|
|**maxReplicas**  <br>*opcjonalne*|Maksymalna liczba replik pod do skalowania.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**minReplicas**  <br>*opcjonalne*|Minimalna liczba replik pod można skalować do.  <br>**Minimalna wartość**:`0`|liczba całkowita|
|**refreshPeriodInSeconds**  <br>*opcjonalne*|Odśwież czasu dla wyzwalacza Skalowanie automatyczne.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**targetUtilization**  <br>*opcjonalne*|Procent użycia, które wyzwala Skalowanie automatyczne.  <br>**Minimalna wartość**:`0`  <br>**Maksymalna wartość**:`100`|liczba całkowita|


<a name="computeresource"></a>
### <a name="computeresource"></a>ComputeResource
Uczenie maszynowe zasobów obliczeniowych.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator zasobu.|ciąg|
|**Typ**  <br>*opcjonalne*|Typ zasobu.|wyliczenia (klaster)|


<a name="containerresourcereservation"></a>
### <a name="containerresourcereservation"></a>ContainerResourceReservation
Konfiguracja zarezerwować zasobów dla kontenera w klastrze.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**Procesor CPU**  <br>*opcjonalne*|Określa zastrzeżenie procesora CPU. Format Kubernetes: zobacz [znaczenie Procesora](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu).|ciąg|
|**pamięci**  <br>*opcjonalne*|Określa zastrzeżenie pamięci. Format Kubernetes: zobacz [znaczenie pamięci](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-memory).|ciąg|


<a name="deployment"></a>
### <a name="deployment"></a>Wdrożenie
Wystąpienia wdrożenia usługi Azure Machine Learning.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**createdAt**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas utworzenia wdrożenia (UTC).|ciąg (daty i godziny)|
|**expiredAt**  <br>*opcjonalne*  <br>*tylko do odczytu*|Ważność wdrożenia czasu (UTC).|ciąg (daty i godziny)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator wdrożenia.|ciąg|
|**imageId**  <br>*opcjonalne*|Identyfikator obrazu skojarzonego z tym wdrożeniem.|ciąg|
|**serviceName**  <br>*opcjonalne*|Nazwa usługi.|ciąg|
|**Stan**  <br>*opcjonalne*|Bieżący stan wdrożenia.|ciąg|


<a name="deploymentlist"></a>
### <a name="deploymentlist"></a>DeploymentList
Tablica obiektów wdrożenia.

*Typ*: <[wdrożenia](#deployment)> tablicy


<a name="errordetail"></a>
### <a name="errordetail"></a>ErrorDetail
Model, szczegóły błędu usługi zarządzania.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**Kod**  <br>*Wymagane*|Kod błędu.|ciąg|
|**Komunikat**  <br>*Wymagane*|Komunikat o błędzie.|ciąg|


<a name="errorresponse"></a>
### <a name="errorresponse"></a>Errorresponse języka
Obiekt błąd usługi zarządzania w modelu.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**Kod**  <br>*Wymagane*|Kod błędu.|ciąg|
|**Szczegóły**  <br>*opcjonalne*|Tablica obiektów szczegółów błędu.|<[ErrorDetail](#errordetail)> tablicy|
|**Komunikat**  <br>*Wymagane*|Komunikat o błędzie.|ciąg|
|**statusCode**  <br>*opcjonalne*|Kod stanu HTTP.|liczba całkowita|


<a name="image"></a>
### <a name="image"></a>Image (Obraz)
Obraz usługi Azure Machine Learning.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**computeResourceId**  <br>*opcjonalne*|Identyfikator środowiska utworzone w zasób obliczeniowy uczenia maszynowego.|ciąg|
|**createdTime**  <br>*opcjonalne*|Czas utworzenia obrazu (UTC).|ciąg (daty i godziny)|
|**creationState**  <br>*opcjonalne*||[AsyncOperationState](#asyncoperationstate)|
|**Opis elementu**  <br>*opcjonalne*|Opis obrazu.|ciąg|
|**Błąd**  <br>*opcjonalne*||[Errorresponse języka](#errorresponse)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator obrazu.|ciąg|
|**imageBuildLogUri**  <br>*opcjonalne*|Identyfikator URI przekazanych dzienników z kompilacji obrazu.|ciąg|
|**Element imageLocation**  <br>*opcjonalne*|Ciąg lokalizacji Azure rejestru kontenera utworzony obraz.|ciąg|
|**imageType**  <br>*opcjonalne*||[ImageType](#imagetype)|
|**Manifest**  <br>*opcjonalne*||[Manifest](#manifest)|
|**Nazwa**  <br>*opcjonalne*|Nazwa obrazu.|ciąg|
|**Wersja**  <br>*opcjonalne*|Wersja obrazu ustawiony przez usługę zarządzania modelu.|liczba całkowita|


<a name="imagerequest"></a>
### <a name="imagerequest"></a>imageRequest
Żądanie utworzenia obrazu usługi Azure Machine Learning.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**computeResourceId**  <br>*Wymagane*|Identyfikator środowiska utworzone w zasób obliczeniowy uczenia maszynowego.|ciąg|
|**Opis elementu**  <br>*opcjonalne*|Opis obrazu.|ciąg|
|**imageType**  <br>*Wymagane*||[ImageType](#imagetype)|
|**Identyfikator manifestu**  <br>*Wymagane*|Identyfikator manifestu, w którym zostanie utworzony obraz.|ciąg|
|**Nazwa**  <br>*Wymagane*|Nazwa obrazu.|ciąg|


<a name="imagetype"></a>
### <a name="imagetype"></a>Typ obrazu
Określa typ obrazu.

*Typ*: wyliczenia (Docker)


<a name="manifest"></a>
### <a name="manifest"></a>Manifest
Uczenie maszynowe Azure manifestu.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**zasoby**  <br>*Wymagane*|Lista zasobów.|<[Zasobów](#asset)> tablicy|
|**createdTime**  <br>*opcjonalne*  <br>*tylko do odczytu*|Manifest godzina utworzenia (UTC).|ciąg (daty i godziny)|
|**Opis elementu**  <br>*opcjonalne*|Manifest tekst opisu.|ciąg|
|**driverProgram**  <br>*Wymagane*|Sterownik programu manifestu.|ciąg|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator manifestu.|ciąg|
|**modelIds**  <br>*opcjonalne*|Lista identyfikatorów modelu w zarejestrowany modeli. Żądanie zakończy się niepowodzeniem, jeśli dowolnego dołączone modelu nie zostały zarejestrowane.|<string>Tablica|
|**modelType**  <br>*opcjonalne*|Określa, że modele są już zarejestrowane w usłudze zarządzania modelu.|wyliczenia (zarejestrowane)|
|**Nazwa**  <br>*Wymagane*|Nazwa manifestu.|ciąg|
|**targetRuntime**  <br>*Wymagane*||[TargetRuntime](#targetruntime)|
|**Wersja**  <br>*opcjonalne*  <br>*tylko do odczytu*|Wersja manifestu przypisany przez usługę zarządzania modelu.|liczba całkowita|
|**webserviceType**  <br>*opcjonalne*|Określa typ żądanej usługi sieci web, która zostanie utworzona w manifeście.|wyliczenia (w czasie rzeczywistym)|


<a name="model"></a>
### <a name="model"></a>Model
Wystąpienie modelu uczenia maszynowego Azure.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**createdAt**  <br>*opcjonalne*  <br>*tylko do odczytu*|Czas utworzenia modelu (UTC).|ciąg (daty i godziny)|
|**Opis elementu**  <br>*opcjonalne*|Opis modelu.|ciąg|
|**Identyfikator**  <br>*opcjonalne*  <br>*tylko do odczytu*|Identyfikator modelu.|ciąg|
|**mimeType**  <br>*Wymagane*|Typ MIME zawartości modelu. Aby uzyskać więcej informacji o typie MIME, zobacz [listę typów nośników IANA](https://www.iana.org/assignments/media-types/media-types.xhtml).|ciąg|
|**Nazwa**  <br>*Wymagane*|Nazwa modelu.|ciąg|
|**tagów**  <br>*opcjonalne*|Lista znaczników modelu.|<string>Tablica|
|**Rozpakowywanie**  <br>*opcjonalne*|Wskazuje, czy musimy Rozpakuj modelu podczas tworzenia obrazu Docker.|wartość logiczna|
|**adres URL**  <br>*Wymagane*|Adres URL modelu. Zazwyczaj testujemy adres URL sygnatury dostępu współdzielonego w tym miejscu.|ciąg|
|**Wersja**  <br>*opcjonalne*  <br>*tylko do odczytu*|Wersja modelu przypisany przez usługę zarządzania modelu.|liczba całkowita|


<a name="modeldatacollection"></a>
### <a name="modeldatacollection"></a>ModelDataCollection
Informacje o modelu danych kolekcji.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**eventHubEnabled**  <br>*opcjonalne*|Włącz Centrum zdarzeń dla usługi.|wartość logiczna|
|**storageEnabled**  <br>*opcjonalne*|Włącz magazyn dla usługi.|wartość logiczna|


<a name="paginatedimagelist"></a>
### <a name="paginatedimagelist"></a>PaginatedImageList
Na liście obrazów.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów modelu.|<[Obraz](#image)> tablicy|


<a name="paginatedmanifestlist"></a>
### <a name="paginatedmanifestlist"></a>PaginatedManifestList
Lista podzieloną manifestów.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów manifestu.|<[Manifest](#manifest)> tablicy|


<a name="paginatedmodellist"></a>
### <a name="paginatedmodellist"></a>PaginatedModelList
Na listę modeli.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów modelu.|<[Model](#model)> tablicy|


<a name="paginatedservicelist"></a>
### <a name="paginatedservicelist"></a>PaginatedServiceList
Na liście usług.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**nextLink**  <br>*opcjonalne*|Łącze kontynuacji (bezwzględny identyfikator URI) do następnej strony wyników na liście.|ciąg|
|**wartość**  <br>*opcjonalne*|Tablica obiektów usługi.|<[ServiceResponse](#serviceresponse)> tablicy|


<a name="servicecreaterequest"></a>
### <a name="servicecreaterequest"></a>ServiceCreateRequest
Żądanie utworzenia usługi.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**appInsightsEnabled**  <br>*opcjonalne*|Włącz usługę application insights dla usługi.|wartość logiczna|
|**autoScaler**  <br>*opcjonalne*||[AutoScaler](#autoscaler)|
|**computeResource**  <br>*Wymagane*||[ComputeResource](#computeresource)|
|**containerResourceReservation**  <br>*opcjonalne*||[ContainerResourceReservation](#containerresourcereservation)|
|**Klasa dataCollection**  <br>*opcjonalne*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*Wymagane*|Obraz, aby utworzyć usługę.|ciąg|
|**maxConcurrentRequestsPerContainer**  <br>*opcjonalne*|Maksymalna liczba równoczesnych żądań.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**Nazwa**  <br>*Wymagane*|Nazwa usługi.|ciąg|
|**numReplicas**  <br>*opcjonalne*|Liczba replik pod uruchomiony w dowolnym momencie. Nie można określić, czy Autoscaler jest włączone.  <br>**Minimalna wartość**:`0`|liczba całkowita|


<a name="serviceregeneratekeyrequest"></a>
### <a name="serviceregeneratekeyrequest"></a>ServiceRegenerateKeyRequest
Żądanie ponownego generowania klucza dla usługi.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**Właściwość keyType**  <br>*opcjonalne*|Określa klucz, do którego można ponownie wygenerować.|wyliczenia (podstawowe, pomocniczy)|


<a name="serviceresponse"></a>
### <a name="serviceresponse"></a>ServiceResponse
Szczegółowy stan usługi.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**createdAt**  <br>*opcjonalne*|Tworzenie usługi godzina (UTC).|ciąg (daty i godziny)|
|**Identyfikator**  <br>*opcjonalne*|Identyfikator usługi.|ciąg|
|**Obraz**  <br>*opcjonalne*||[Obraz](#image)|
|**Manifest**  <br>*opcjonalne*||[Manifest](#manifest)|
|**modele**  <br>*opcjonalne*|Lista modeli.|<[Model](#model)> tablicy|
|**Nazwa**  <br>*opcjonalne*|Nazwa usługi.|ciąg|
|**scoringUri**  <br>*opcjonalne*|Identyfikator URI dla oceniania usługi.|ciąg|
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


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**appInsightsEnabled**  <br>*opcjonalne*|Włącz usługę application insights dla usługi.|wartość logiczna|
|**autoScaler**  <br>*opcjonalne*||[AutoScaler](#autoscaler)|
|**containerResourceReservation**  <br>*opcjonalne*||[ContainerResourceReservation](#containerresourcereservation)|
|**Klasa dataCollection**  <br>*opcjonalne*||[ModelDataCollection](#modeldatacollection)|
|**imageId**  <br>*opcjonalne*|Obraz, aby utworzyć usługę.|ciąg|
|**maxConcurrentRequestsPerContainer**  <br>*opcjonalne*|Maksymalna liczba równoczesnych żądań.  <br>**Minimalna wartość**:`1`|liczba całkowita|
|**numReplicas**  <br>*opcjonalne*|Liczba replik pod uruchomiony w dowolnym momencie. Nie można określić, czy Autoscaler jest włączone.  <br>**Minimalna wartość**:`0`|liczba całkowita|


<a name="targetruntime"></a>
### <a name="targetruntime"></a>TargetRuntime
Typ docelowego środowiska uruchomieniowego.


|Name (Nazwa)|Opis|Schemat|
|---|---|---|
|**właściwości**  <br>*Wymagane*||< ciąg, ciąg > mapy|
|**runtimeType**  <br>*Wymagane*|Określa środowisko uruchomieniowe.|wyliczenia (SparkPython, Python)|

