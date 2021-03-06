---
title: Azure Service Bus の認証と承認 | Microsoft Docs
description: Shared Access Signature (SAS) 認証を使用して、アプリケーションを Service Bus に対して認証します。
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2018
ms.author: spelluru
ms.openlocfilehash: 726e414566622d28817c34c92bd45653677d590f
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392722"
---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus の認証と承認

アプリケーションは、Shared Access Signature (SAS) トークン認証を使用して Azure Service Bus リソースに対するアクセス権を取得します。 SAS では、アプリケーションから Service Bus にトークンが提示されます。このトークンはトークン発行者と Service Bus の両方に既知の対称キーで署名されています (したがって、"共有")。そのキーは、特定のアクセス権 (メッセージを受信/リッスンまたは送信するアクセス許可など) を付与するルールと直接関連付けられています。 SAS ルールは、名前空間または直接エンティティ (キュー、トピックなど) に対して構成され、詳細にアクセスを制御することができます。

SAS トークンは、Service Bus クライアントから直接生成するか、クライアントが対話する中間トークン発行エンドポイントから生成できます。 たとえば、システムはクライアントに対して、Active Directory 承認で保護されている Web サービス エンドポイントを呼び出し、ID とシステム アクセス権を証明するように要求する場合があります。この場合、Web サービスから適切な Service Bus トークンが返されます。 この SAS トークンは、Azure SDK に含まれている Service Bus トークン プロバイダーを使用して簡単に生成できます。 

> [!IMPORTANT]
> Azure Active Directory Access Control (Access Control Service または ACS とも呼ばれます) を Service Bus と一緒に使用している場合、この方法のサポートは制限されるようになったため、SAS を使用するようにアプリケーションを移行することをお勧めします。 詳細については、[こちらのブログ記事と](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)と[こちらの記事](service-bus-migrate-acs-sas.md)を参照してください。

## <a name="shared-access-signature-authentication"></a>Shared Access Signature 認証

[SAS 認証](service-bus-sas.md)により、特定の権限で Service Bus リソースにアクセスできるようになります。 Service Bus の SAS 認証には、Service Bus リソースに対する関連した権限を使用した暗号化キーの構成が伴います。 これにより、クライアントは SAS トークンを提示してリソースへのアクセス権を取得できます。このトークンは、アクセスされるリソース URI と、構成されたキーで署名された有効期限から成ります。

SAS のキーは Service Bus 名前空間で構成できます。 このキーは、その名前空間内のすべてのメッセージング エンティティに適用されます。 Service Bus のキューとトピックでキーを構成することもできます。 SAS は [Azure リレー](../service-bus-relay/relay-authentication-and-authorization.md)でもサポートされます。

SAS を使用するには、名前空間、キュー、トピックで [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) オブジェクトを構成します。 この規則は次の要素で構成されます。

* *KeyName*: ルールを識別します。
* *PrimaryKey*: SAS トークンの署名または検証に使用される暗号化キー。
* *SecondaryKey*: SAS トークンの署名または検証に使用される暗号化キー。
* *Rights*: 付与されている**リッスン**、**送信、**、または**管理**権限を表します。

名前空間レベルで構成された承認規則では、対応するキーを使用して署名されたトークンによって、クライアントの名前空間内のすべてのエンティティへのアクセス権を付与できます。 Service Bus の名前空間、キュー、またはトピックでは、このような承認規則を最大 12 個構成できます。 既定では、すべての権限を持つ [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) は、最初にプロビジョニングするときに、各名前空間用に構成されます。

エンティティにアクセスするには、クライアントには、特定の [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)を使用して生成された SAS トークンが必要です。 SAS トークンの生成には、承認規則に関連付けられた暗号化キーによって、アクセスが要求されるリソース URI と、有効期限で構成されるリソース文字列の HMAC-SHA256 を使用します。

Service Bus の SAS 認証サポートは、Azure .NET SDK バージョン 2.0 以降に含まれています。 SAS には、 [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)のサポートが含まれています。 接続文字列をパラメーターとして受け取るすべての API では、SAS 接続文字列がサポートされています。

## <a name="next-steps"></a>次の手順

- SAS の詳細については、「[Shared Access Signature による Service Bus の認証](service-bus-sas.md)」を引き続きお読みください。
- [Azure Active Directory Access Control (ACS) から Shared Access Signature 承認に移行](service-bus-migrate-acs-sas.md)する方法。
- [ACS 対応名前空間への変更](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)。
- Azure リレーの認証と承認についての対応する情報は、「[Azure Relay authentication and authorization (Azure リレーの認証と承認)](../service-bus-relay/relay-authentication-and-authorization.md)」を参照してください。 

