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
<p>このレポートには、さまざまなリージョンと不可能カ所の地域間で移動するユーザーのサインイン間の時間から実施された、2 回のサインイン ユーザーから成功したサインインが含まれています。 考えられる原因は次のとおりです。</p><ul><li>ユーザーが他のユーザーとパスワードを共有している</li><li>ユーザーがリモート デスクトップを使用してサインインのための web ブラウザーを起動するには</li><li>ハッカーが別の国からユーザーのアカウントにサインインした</li><li>ユーザーが VPN またはプロキシを使用している</li><li>ユーザーが同時に複数のデバイス (デスクトップと携帯電話など) からサインインしており、携帯電話の IP アドレスが異常である</li></ul><p>このレポートの結果には表示成功したサインイン イベント、それらのサインイン間の時間、地域間から実施されたサインインと推定移動時間領域を使用します。</p><p>示される移動時間は単なる推定値であり、各場所間の実際の移動時間とは異なる可能性があります。</p>


![複数の地域からのサインイン](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)


