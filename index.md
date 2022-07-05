---
title: 联机托管说明
permalink: index.html
layout: home
ms.openlocfilehash: 8cc220d44fe56f19385b25675c29e391b610db8e
ms.sourcegitcommit: d2d374fffa4fcbf92b9c4bdc9c9ecc470152e033
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2021
ms.locfileid: "138099941"
---
## <a name="content-directory"></a>内容目录

下面列出了每个实验室的超链接。

## <a name="labs"></a>实验室

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| 模块 | 实验室 |
| --- | --- |
{% for activity in labs  %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}
