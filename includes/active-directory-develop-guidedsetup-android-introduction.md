---
title: インクルード ファイル
description: インクルード ファイル
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: a1cd25012461ae8bb445dcb1de8fe5be49e04760
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/24/2018
ms.locfileid: "47060452"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Android アプリケーションからユーザーにサインインし、Microsoft Graph を呼び出す

このチュートリアルでは、Android アプリケーションを構築し、それを Microsoft ID プラットフォームに統合する方法を学習します。 具体的には、このアプリケーションは、ユーザーのサインイン、Microsoft Graph API を呼び出すアクセス トークンの取得、および Microsoft Graph API への基本的な要求を行います。  

このガイドを完了すると、アプリケーションは、個人用の Microsoft アカウント (outlook.com、live.com など) と、Azure Active Directory を使用する会社や組織の職場または学校アカウントのサインインを受け入れるようになります。 

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>このガイドで生成されたサンプル アプリの動作
![このサンプルのしくみ](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

このサンプルのアプリケーションは、ユーザーにサインインし、ユーザーに代わってデータを取得します。  このデータは、承認を要求し、Microsoft ID プラットフォームによっても保護されるリモート API (ここでは Microsoft Graph API) を介してアクセスされます。 

具体的には次のとおりです。 
* アプリケーションは、ユーザーにサインインする Web ページを開きます。
* アプリケーションには、Microsoft Graph API 用のアクセス トークンが発行されます。
* アクセス トークンは、Web API への HTTP 要求に含められます。
* Microsoft Graph の応答を処理します。 

このサンプルでは、Android 用の Microsoft 認証ライブラリ (MSAL) を使用して、Auth を使用した調整と支援を行います。MSAL は自動的にトークンを更新し、デバイス上の他のアプリとの間で SSO を提供し、アカウントの管理を助け、条件付きアクセスのほとんどを処理します。 

## <a name="prerequisites"></a>前提条件
* このガイド付きセットアップでは、Android Studio 3.0 を使用します。 
* Android 21 以降が必要です (25 以降を推奨)。
* このバージョンの Android 向け MSAL には、Google Chrome またはユーザー設定のタブを使用している Web ブラウザーが必要です。

## <a name="library"></a>ライブラリ

このガイドでは、次の認証ライブラリを使用します。

|ライブラリ|説明|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|
