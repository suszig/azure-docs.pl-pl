---
title: "Usługi Azure AD w wersji 2 dla systemu Android pobieranie rozpoczęte — Instalatora | Dokumentacja firmy Microsoft"
description: "Jak uzyskać token dostępu i wywołania interfejsu API programu Microsoft Graph lub interfejsów API, które wymagają tokenów dostępu z usługą Azure Active Directory v2 końcowego region systemu Android"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: a43d7e30a6f4176afba27f0de2c2c116df741080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
## <a name="set-up-your-project"></a>Konfigurowanie projektu

> Preferowane jest zamiast tego Pobierz ten przykładowy projekt programu Android Studio? [Pobieranie projektu](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) i przejść [kroku konfiguracji](#create-an-application-express) skonfigurować przykładowy kod przed wykonaniem.


### <a name="create-a-new-project"></a>Tworzenie nowego projektu 
1.  Otwórz program Android Studio, przejdź do:`File` > `New` > `New Project`
2.  Nazwa aplikacji, a następnie kliknij przycisk`Next`
3.  Upewnij się wybrać *21 interfejsu API lub nowszej (Android 5.0)* i kliknij przycisk`Next`
4.  Pozostaw `Empty Activity`, kliknij przycisk `Next`, następnie`Finish`


### <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Dodaj do projektu biblioteki uwierzytelniania firmy Microsoft (MSAL)
1.  W programie Android Studio przejdź do:`Gradle Scripts` > `build.gradle (Module: app)`
2.  Skopiuj i wklej następujący kod w obszarze `Dependencies`:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>O tym pakiecie

Powyżej pakiet instaluje biblioteki uwierzytelniania firmy Microsoft (MSAL). MSAL obsługuje pobieranie, buforowanie i odświeżanie tokenów użytkownika, które umożliwiają dostęp do interfejsów API chronione przez punkt końcowy w wersji 2 usługi Azure Active Directory.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>Tworzenie aplikacji interfejsu użytkownika

1.  Otwórz: `activity_main.xml` w obszarze`res` > `layout`
2.  Zmian układu działania z `android.support.constraint.ConstraintLayout` lub innych do`LinearLayout`
3.  Dodaj `android:orientation="vertical"` właściwości `LinearLayout` węzła
4.  Skopiuj i wklej następujący kod do `LinearLayout` węzła, zastępując bieżący zawartości:

```xml
<TextView
    android:text="Welcome, "
    android:textColor="#3f3f3f"
    android:textSize="50px"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="15dp"
    android:id="@+id/welcome"
    android:visibility="invisible"/>

<Button
    android:id="@+id/callGraph"
    android:text="Call Microsoft Graph"
    android:textColor="#FFFFFF"
    android:background="#00a1f1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="200dp"
    android:textAllCaps="false" />

<TextView
    android:text="Getting Graph Data..."
    android:textColor="#3f3f3f"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:id="@+id/graphData"
    android:visibility="invisible"/>

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="0dip"
    android:layout_weight="1"
    android:gravity="center|bottom"
    android:orientation="vertical" >

    <Button
        android:text="Sign Out"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="15dp"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:textAllCaps="false"
        android:id="@+id/clearCache"
        android:visibility="invisible" />
</LinearLayout>
```

