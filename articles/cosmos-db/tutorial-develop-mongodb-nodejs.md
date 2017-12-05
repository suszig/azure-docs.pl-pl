---
title: "Samouczek bazy danych MongoDB, usługi Angular i języka Node dla platformy Azure | Microsoft Doc"
description: "Dowiedz się, jak utworzyć aplikację bazy danych MongoDB w usłudze Angular i języku Node dla usługi Azure Cosmos DB przy użyciu tych samych interfejsów API, co używane w usłudze MongoDB, oglądając tę serię samouczków opartą na filmach wideo."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 37bc34c470190f3acb1f0f2e9bed5e0a4918405b
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db"></a>Tworzenie aplikacji bazy danych MongoDB przy użyciu usług Angular i Azure Cosmos DB 

Ten wieloczęściowy samouczek pokazuje, jak utworzyć nową aplikację [interfejsu API bazy danych MongoDB](mongodb-introduction.md) za pomocą programu Express, usługi Angular i języka Node.js (stos MEAN), a następnie podłączyć ją do bazy danych usługi Azure Cosmos DB. Usługa Azure Cosmos DB obsługuje połączenia klienta bazy danych MongoDB, dlatego można używać usługi Azure Cosmos DB zamiast bazy danych MongoDB, ale stosując ten sam kod już używany w dla aplikacji bazy danych MongoDB. Zyskuje się w ten sposób dodatkowe korzyści związane z usługą Azure Cosmos DB, takie jak proste wdrażanie w chmurze, skalowanie, dane replikowane globalne, obsługa wielu modeli oraz niezwykle szybkie operacje odczytu i zapisu. 

Azure Cosmos DB to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft. Dzięki wykorzystaniu dystrybucji globalnej i możliwości skalowania poziomego opartego na usłudze Azure Cosmos DB można szybko tworzyć i za pomocą zapytań badać bazy danych dokumentów, par klucz/wartość i grafów. 

W tym wieloczęściowym samouczku opisano następujące zadania:

> [!div class="checklist"]
> * [Tworzenie aplikacji Node.js Express za pomocą interfejsu wiersza polecenia usługi Angular](tutorial-develop-mongodb-nodejs-part2.md)
> * [Tworzenie interfejsu użytkownika przy użyciu usługi Angular](tutorial-develop-mongodb-nodejs-part3.md)
> * [Tworzenie konta usługi Azure Cosmos DB przy użyciu interfejsu wiersza polecenia platformy Azure](tutorial-develop-mongodb-nodejs-part4.md) 
> * [Łączenie z usługą Azure Cosmos DB przy użyciu wtyczki Mongoose](tutorial-develop-mongodb-nodejs-part5.md)
> * [Dodawanie funkcji Post, Put i Delete do aplikacji](tutorial-develop-mongodb-nodejs-part6.md)

Chcesz skompilować tę samą aplikację na platformie React? Zobacz [samouczek platformy React w postaci serii filmów wideo](tutorial-develop-mongodb-react.md).

## <a name="video-walkthrough"></a>Przewodnik wideo

> [!VIDEO https://www.youtube.com/embed/vlZRP0mDabM]

## <a name="finished-project"></a>Zakończony projekt 

Ten samouczek przedstawia instrukcje tworzenia aplikacji krok po kroku. Jeśli chcesz pobrać gotowy projekt, ukończoną aplikację znajdziesz w [repozytorium angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) w witrynie GitHub.

## <a name="next-steps"></a>Następne kroki

W tej części samouczka zostały wykonane następujące zadania:

> [!div class="checklist"]
> * Zapoznanie się z omówieniem kroków wymaganych do utworzenia aplikacji MEAN.js przy użyciu usługi Azure Cosmos DB. 

Możesz przejść do następnej części samouczka, aby utworzyć aplikację Node.js Express.

> [!div class="nextstepaction"]
> [Tworzenie aplikacji Node.js Express za pomocą interfejsu wiersza polecenia usługi Angular](tutorial-develop-mongodb-nodejs-part2.md)
