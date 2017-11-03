---
title: "Dodawanie certyfikatu do magazynu urzędu certyfikacji Java | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać certyfikat urzędu certyfikacji certyfikatu do magazynu certyfikatów (cacerts) Java urzędu certyfikacji dla usługi usługi Twilio lub usługi Azure Service Bus."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: b6e1a305e19415ab1c4b4c208dac98ad1e2689c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Dodawanie certyfikatu do magazynu certyfikatów urzędu certyfikacji Java
Poniższe kroki pokazują, jak dodać certyfikat urzędu certyfikacji certyfikatu do magazynu certyfikatów (cacerts) Java urzędu certyfikacji. Przykład używana jest wymagane przez usługę usługi Twilio certyfikatu urzędu certyfikacji. Informacje podane w dalszej części tematu opisano sposób instalowania certyfikatu urzędu certyfikacji dla usługi Azure Service Bus. 

Można dodać certyfikatu urzędu certyfikacji przed pakowanie Twojej JDK i dodanie go do projektu platformy Azure keytool **approot** folderze, lub można uruchomić zadanie uruchamiania Azure używającej keytool, aby dodać certyfikat. W tym przykładzie przyjęto założenie, że spowoduje dodanie certyfikatu urzędu certyfikacji przed JDK trwa zip. Ponadto określonego certyfikatu urzędu certyfikacji będzie używany w tym przykładzie, ale kroki uzyskiwania innego certyfikatu urzędu certyfikacji i importowania go do magazynu cacerts mogą być podobne.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Aby dodać certyfikat do magazynu cacerts
1. W wierszu polecenia administratora skonfigurowanej do Twojej JDK **jdk\jre\lib\security** folderu, uruchom następujące polecenie, aby sprawdzić, jakie certyfikaty są zainstalowane:
   
    `keytool -list -keystore cacerts`
   
    Zostanie wyświetlony monit o podanie hasła magazynu. Domyślne hasło jest **changeit**. (Jeśli chcesz zmienić hasło, zobacz dokumentację keytool w <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) W tym przykładzie założono, że certyfikat z MD5 odcisków palców 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nie ma na liście, a chcesz zaimportować go (tego certyfikatu określonego wymagane przez usługę interfejsu API usługi Twilio).
2. Uzyskiwanie certyfikatu z listy certyfikatów znajduje się na [certyfikaty główne GeoTrust](http://www.geotrust.com/resources/root-certificates/). Kliknij prawym przyciskiem myszy łącze certyfikat o numerze seryjnym 35:DE:F4:CF i zapisać go do **jdk\jre\lib\security** folderu. Do celów tego przykładu, została zapisana w pliku o nazwie **firmy Equifax\_bezpiecznego\_certyfikatu\_Authority.cer**.
3. Zaimportuj certyfikat przy użyciu następującego polecenia:
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    Po wyświetleniu monitu o zaufanie tego certyfikatu, jeśli certyfikat ma MD5 odcisk palca 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, Odpowiedz, wpisując **y**.
4. Uruchom następujące polecenie, aby upewnić się, że certyfikat urzędu certyfikacji został pomyślnie zaimportowany:
   
    `keytool -list -keystore cacerts`
5. Zestaw JDK zip i dodaj go do projektu platformy Azure **approot** folderu.

Aby uzyskać informacje o keytool, zobacz <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Certyfikaty główne Azure
Należy traktować jako zaufany certyfikat główny firmy CyberTrust Baltimore aplikacji korzystających z usług Azure (np. usługi Azure Service Bus). (Począwszy od 15 kwietnia 2013 Azure rozpoczynająca się migrowania GTE CyberTrust Root globalne do Baltimore CyberTrust Root. Ta migracja trwało kilka miesięcy do ukończenia).

Baltimore certyfikatu może być zainstalowany w magazynie cacerts więc Pamiętaj, aby uruchomić **keytool — lista** polecenie, aby dowiedzieć się, jeśli już istnieje.

Jeśli konieczne jest dodanie Baltimore CyberTrust Root, ma numer seryjny 02:00:00:b9 i c d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 odcisk palca SHA1: 78:db:28:52:ca:e4:74. Można go pobrać z <https://cacert.omniroot.com/bc2025.crt>, zapisanych w lokalnym pliku z rozszerzeniem **.cer**, a następnie zaimportowane przy użyciu **keytool** zgodnie z powyższym.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat certyfikaty główne używane przez platformę Azure, zobacz [migracji certyfikatu głównego Azure](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Aby uzyskać więcej informacji na temat języka Java, zobacz [Azure dla deweloperów języka Java](/java/azure).

