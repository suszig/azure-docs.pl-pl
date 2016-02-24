<properties
    pageTitle="複数の地域からのサインイン"
    description="2 回のサインインが異なるリージョンから実行されたと思われ、サインイン間の時間からユーザーがそれらのリージョン間を移動することは不可能と判断される場合に、そのユーザーを示すレポート。"
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2015"
    ms.author="saah;kenhoff"/>

# 複数の地域からのサインイン
<p>このレポートには、さまざまなリージョンと不可能カ所の地域間で移動するユーザーのサインイン間の時間から実施された、2 回のサインイン ユーザーから成功したサインインが含まれています。 考えられる原因は次のとおりです。</p><ul><li>ユーザーが他のユーザーと、パスワードを共有します。</li><li>ユーザーがリモート デスクトップを使用してサインインのための web ブラウザーを起動するには</li><li>ハッカーにサインインのユーザーのアカウント別の国から</li><li>ユーザーが VPN 接続やプロキシを使用します。</li><li>ユーザーがサインインしている複数のデバイスからデスクトップや携帯電話など、同時に、携帯電話の IP アドレスは、通常は.</li></ul><p>このレポートの結果には表示成功したサインイン イベント、それらのサインイン間の時間、地域間から実施されたサインインと推定移動時間領域を使用します。</p><p>旅行の時間は推定値であり、場所の間での実際の移動時間と異なる場合があります。</p>


![複数の地域からのサインイン](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

