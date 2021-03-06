# API OF INTEGRATION

- [API OF INTEGRATION](#api-of-integration)
  - [Introduction of document](#introduction-of-document)
  - [Changelog](#changelog)
  - [Preparation before integration](#preparation-before-integration)
  - [Steps of getting ads](#steps-of-getting-ads)
  - [Instruction](#instruction)
    - [Request URL](#request-url)
    - [Communication Mode and Encoding](#communication-mode-and-encoding)
    - [Request Header](#request-header)
    - [Request](#request)
      - [APP Information](#app-information)
      - [Device Information](#device-information)
        - [Screen Information](#screen-information)
        - [Geo Information](#geo-information)
      - [User Information](#user-information)
      - [Ad Information](#ad-information)
        - [Native Information](#native-information)
          - [Asset Information](#asset-information)
    - [Response Information](#response-information)
      - [Ad Information](#ad-information-1)
        - [Native Information](#native-information-1)
        - [Asset Information](#asset-information-1)
          - [Image Information](#image-information)
          - [Title Information](#title-information)
          - [Data Information](#data-information)
        - [Link Information](#link-information)
      - [ATTACHMENT](#attachment)
        - [STORE](#store)
        - [CATEGORY](#category)

## Introduction of document

This document is only for developers who integrate with ZPLAY Ads using the API.

## Changelog

| Version | Modifier  | Time       | Description |
| ------- | --------- | ---------- | ----------- |
| v1.0    | ZPALY Ads | 2018.09.20 | Creat       |

## Preparation before integration

Please get 'Account ID' and 'Token' from your account manager.

## Steps of getting ads

1. When the user visit the APP of developer, developer should send a request to ZPLAY Ads.
2. ZPLAY Ads return a response to developer, both request and response should obey the format agreed by this document.
3. Developer renders the ads according specification of this document

## Instruction

### Request URL

When you send a request, send a HTTP POST to the following address: pa-engine.zplayads.com/v1/api/ads

### Communication Mode and Encoding

Communication protocol: HTTP
Method: POST
Data format: UTF-8

### Request Header

| http header information | instruction                                                                                                                                                                                                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| X-Forwarded-For         | Include the real request address of the client, e.g. “8.8.8.8”. If integrated via server, please pass client address because server address will be blocked and regarded as fraud traffic.                                                  |
| User-Agent              | User Agent of mobile device，e.g. “Mozilla/5.0 (iPad; CPU OS 5_0 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9A334 Safari/7534.48.3”. Non-real User-Agent from server will be regarded as problematic traffic. |

### Request

| Parameter       | Type               | mandatory | Description                                                                                                                            |
| --------------- | ------------------ | --------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| version             | string             | Y         | Protocol version, current version is 1.0                                                                                               |
| developer_token | string             | Y         | Developer token, offered by ZPLAY Ads account manager                                                                                  |
| need_https      | int                | N         | For material's link or tracking url link, whether the prefix is https. 0 as default. 0: don’t need https, 1: need https for all links. |
| support_function | boolean            | Y         | Whether support close event and install event in third and forth part of [Check_list](/check_list_en.md); 0: not support, 1: support; Publisher should handle both close event and install event if does not support these two events we provide |
| app             | object             | Y         | APP information                                                                                                                        |
| device          | object             | Y         | Device information                                                                                                                     |
| user            | object             | N         | User information                                                                                                                       |
| ads             | ad array of object | Y         | Ad information                                                                                                                         |

#### APP Information

| Parameter | Type   | mandatory | Description                                                                                                                                                                                                            |
| --------- | ------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| app_id    | string | Y         | APP ID, generated by ZPLAYAds after you registered the app on [ZPLAY Ads](https://www.zplayads.com)                                                                                                                    |
| app_name  | string | N         | APP name, please make sure this value is same with the name that you have registered on[ZPLAY Ads 平台](https://www.zplayads.com) in advance                                                                           |
| bundle_id | string | Y         | PackageName in Android，such as "com.zplayads.demo"；Bundle ID in iOS, such as "com.zplayads.demo"                                                                                                                             |
| version   | string | N         | application version                                                                                                                                                                                                    |
| cat       | string | N         | application category, such as"Action"，category refer to [CATEGORY](#CATEGORY). please make sure this value is same with the category that you have registered on[ZPLAY Ads 平台](https://www.zplayads.com) in advance |

#### Device Information

| Parameter       | Type    | mandatory | Description                                                                                                                                                                                                                                                                                                                                                                              |
| --------------- | ------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| model           | string  | Y         | Device model                                                                                                                                                                                                                                                                                                                                                                             |
| manufacturer    | string  | N         | Manufacturer, such as "Samsung"                                                                                                                                                                                                                                                                                                                                                          |
| brand           | string  | N         | Brand, such as "MI4"                                                                                                                                                                                                                                                                                                                                                                     |
| plmn            | string  | N         | Public land mobile network (PLMN), as defined in telecommunications regulation, is a network that is established and operated by an administration or by a recognized operating agency (ROA) for the specific purpose of providing land mobile telecommunications services to the public, such as "46000"                                                                                |
| device_type     | string  | Y         | Device type, "phone", "tablet"                                                                                                                                                                                                                                                                                                                                                           |
| adt             | boolean | N         | Whether to allow targeted user by tracking user behavior, 0: not allowed, 1: allowed, default is 1                                                                                                                                                                                                                                                                                       |
| connection_type | string  | Y         | Connection type, empty means unknown, the value is wifi, 4g, 3g, 2g, ethernet，cell_unknown                                                                                                                                                                                                                                                                                              |
| carrier         | int     | Y         | Carrier, 0:China Mobile，1:Telecom，3:Unicom, 4:unknown                                                                                                                                                                                                                                                                                                                                  |
| orientation     | int     | N         | Device orientation, 0: landscape, 1: portrait                                                                                                                                                                                                                                                                                                                                            |
| mac             | string  | N         | Media access control address, is a unique identifier assigned to a network interface controller (NIC) for communications at the data link layer of a network segment. MAC addresses are used as a network address for most IEEE 802 network technologies, including Ethernet and Wi-Fi. In this context, MAC addresses are used in the medium access control protocol sublayer. Md5 Hash |
| imei            | string  | N         | International Mobile Equipment Identity, is a number, usually unique, to identify 3GPP and iDEN mobile phones, as well as some satellite phones. (sending meid(mobile equipment identifier) if the mobile is CDMA2000). Md5 Hash                                                                                                                                                         |
| android_id      | string  | N         | Android ID is an unique ID to each device. It is used to identify your device for market downloads, Md5 Hash, no value in Android device will affect ad fill.                                                                                                                                                                                                                            |
| android_adid    | string  | N         | AAID(Google Advertising ID)                                                                                                                                                                                                                                                                                                                                                              |
| idfa            | string  | Y         | IDFA( Identifier for Advertising)                                                                                                                                                                                                                                                                                                                                                        |
| idfv            | string  | N         | identifierForVendor, please view [more info](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)                                                                                                                                                                                                                                                       |
| openudid        | string  | N         | openudid                                                                                                                                                                                                                                                                                                                                                                                 |
| language        | string  | Y         | Device language                                                                                                                                                                                                                                                                                                                                                                          |
| os_type         | string  | Y         | Operation system, "iOS", " Android"                                                                                                                                                                                                                                                                                                                                                      |
| os_version      | string  | Y         | Operation system version, such as 11.4.1，12.0，7.1.0. Please note: the main version of iOS are limited in 9.x， 10.x， 11.x， 12.x； the main version of Android are limited in 5.x， 6.x， 7.x， 8.x， 9.x;                                                                                                                                                                                       |
| screen          | object  | Y         | Device screen info                                                                                                                                                                                                                                                                                                                                                                       |
| geo             | object  | N         | Device geo info                                                                                                                                                                                                                                                                                                                                                                          |

##### Screen Information

| Parameter | Type  | Mandatory | Description                                                                    |
| --------- | ----- | --------- | ------------------------------------------------------------------------------ |
| width     | int   | Y         | Landscape resolution, unit: pixel                                              |
| height    | int   | Y         | Portrait resolution, unit: pixel                                               |
| dpi       | int   | N         | Pixel density，unit: pixel numbers per inch                                    |
| pxratio   | float | N         | Physical pixel density, e.g. 1 on iPhone 3, 2 on iPhone 4, 3 on iPhone 6s Plus |

##### Geo Information

| Parameter       | Type   | Mandatory | Description                    |
| --------------- | ------ | --------- | ------------------------------ |
| lat             | double | Y         | Latitude                       |
| lon             | double | Y         | Longitude                      |
| horizontal_accu | int    | N         | Horizon accurate, unit: meter  |
| vertical_accu   | int    | N         | Vertical accurate, unit: meter |

#### User Information

| Parameter | Type   | Mandatory | Description                                      |
| --------- | ------ | --------- | ------------------------------------------------ |
| id        | string | N         | User id                                          |
| gender    | int    | N         | Gender, 0: female, 1: male, 2: other, 3: unknown |
| age       | int    | N         | Age                                              |
| keywords  | array  | N         | Keyword interested by user                       |

#### Ad Information

| Parameter  | Type   | Mandatory | Description                                                                                                                        |
| ---------- | ------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| unit_type  | int    | Y         | ad unit type, 0: banner, 1: interstitial, 2: splash, 3: native, 4: rewarded video, unit_type should be same with your ad unit type |
| ad_unit_id | string | Y         | Ad unit id, generated by ZPLAYAds after you registered your ad unit on [ZPLAY Ads](https://www.zplayads.com)                       |
| native     | object | N         | Native information, it's mandatory when unit_type is 3; it should not fill when unit_type isn't 3                                  |

##### Native Information

| Parameter | Type           | Mandatory | Description                                                                                                                                               |
| --------- | -------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| layout    | int            | N         | Native types，1: content wall, 2: app wall, 3: news stream， 4: chat list，5:scroll ads，6:content stream，7:matrix, only content stream is supported now |
| assets    | array of asset | Y         | Native assets，currently there’re five elements: Title(data), Icon(img), Large image (img), Description (data) and score(data)                            |

###### Asset Information

| Parameter | Type   | Mandatory | Description                                                             |
| --------- | ------ | --------- | ----------------------------------------------------------------------- |
| id        | int    | Y         | Element id                                                              |
| required  | int    | N         | Whether the element is required, 1: required, 0: optional, default is 0 |
| title     | object | N         | Title element, content is text                                          |
| image     | object | N         | Title element, content is image                                         |
| data      | object | N         | Other element data                                                      |

> One asset only includes one element of image，title and data

**Image Information**

| Parameter | Type | Mandatory | Description                                                                                                                                                                              |
| --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type      | int  | Y         | Image element type, 1: icon, 2: Logo, 3: main picture, 4: "play game without downloading" button, when user click the button, please load playable_ads_html in response.ads into webview. "play game without downloading" button should be included in request information |
| width     | int  | Y         | Image width，unit: pixel.                                                                                                                                                                |
| height    | int  | Y         | Image height，unit: pixel.                                                                                                                                                               |

**Title Information**

| Parameter | Type | Mandatory | Description     |
| --------- | ---- | --------- | --------------- |
| len       | int  | Y         | Length of title |

**Data Information**

| Parameter | Type | Mandatory | Description                                                                                                                                                                                                                                                                                                                                                                                              |
| --------- | ---- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type      | int  | Y         | Type of data, 1: Sponsor name, should include brand name, 2: description, 3: score, 4: number of likes, 5: number of downloads, 6: price of product, 7: price of sales, always display with price of product together and show user the discount, 8: phone number, 9: address, 10: second description, 11: link that show to user, 12: click-to-action button, 1001: video URL, 1002: number of comments |
| len       | int  | Y         | Length of data                                                                                                                                                                                                                                                                                                                                                                                           |

### Response Information

| Parameter | Type             | Mandatory | Description                                                                 |
| --------- | ---------------- | --------- | --------------------------------------------------------------------------- |
| result    | int              | Y         | Response result, 0: success, less than 0: fail                              |
| msg       | string           | N         | The reason for failure if response result is fail, such as "Internet error" |
| ads       | array of objects | N         | No data if response result is fail                                          |
| cur       | string           | N         | Ad currency, such as "CNY" or "USD". If the price in Ad Information is null, currency is null; If the price in Ad Information is not null, currency is not null, refer to [ISO-4217 alpha codes](https://en.wikipedia.org/wiki/ISO_4217)                         |

#### Ad Information

| Parameter         | Type   | Mandatory | Description                                                                                                                                                                                                                        |
| ----------------- | ------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                | string | Y         | Ad id                                                                                                                                                                                                                              |
| ad_unit_id        | string | Y         | Ad unit id, it's the same with ad_unit_id in request                                                                                                                                                                               |
| app_bundle        | string | Y         | APP packageName for Android, Bundle ID for iOS, please listen to install event, open build-in APP Store or Google Play                                                                                                             |
| playable_ads_html | string | Y         | Playable ad HTML snippet, make sure to load it with in-app webview                                                                                                                                                                 |
| target_url        | string | Y         | Target url to download APP, which will jump to when user click ad                                                                                                                                                                  |
| target_url_type   | int    | Y         | Type of actions when user click ad, 1: open the url within webview in-app, 2: open the url within system browser, 3: open map, 4: open dial, 5: play video, 6: download App, make sure to open App Store or Google Play in the app |
| price             | float  | N         | Ad price, empty means 0, unit: cent                                                                                   |
| native            | object | N         | Native object, it will return native object if unit_type is native                                                                                                                                                                 |

##### Native Information

| Parameter   | Type           | Mandatory | Description                                                                                                                             |
| ----------- | -------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| assets      | array of asset | Y         | Native ad element list, five type elements supported currently, tile (data), icon (img), picture (img), description (data), score(data) |
| imp_tracker | array          | N         | Impression tracking URL array, a 1-pixel picture need to be returned for tracking URL                                                   |
| link        | object         | Y         | Target link, this is a default link object of native ads. Use this object when link object does not included in assets                  |

##### Asset Information

| Parameter | Type   | Mandatory | Description                                                                   |
| --------- | ------ | --------- | ----------------------------------------------------------------------------- |
| id        | int    | Y         | Ad element ID                                                                 |
| required  | int    | N         | Whether the element must be displayed, 1: required, 0: optional, default is 0 |
| title     | object | N         | Title element, content is text                                                |
| image     | object | N         | Image element, content is picture                                             |
| data      | object | N         | Other element data                                                            |
| link      | object | N         | Target link to download APP                                                   |

###### Image Information

| Parameter | Type   | Mandatory | Description               |
| --------- | ------ | --------- | ------------------------- |
| url       | string | Y         | Image url                 |
| width     | int    | N         | Image width，unit: pixel  |
| height    | int    | N         | Image height，unit: pixel |

###### Title Information

| Parameter | Type   | Mandatory | Description   |
| --------- | ------ | --------- | ------------- |
| text      | string | Y         | Title content |

###### Data Information

| Parameter | Type   | Mandatory | Description  |
| --------- | ------ | --------- | ------------ |
| label     | string | N         | Data name    |
| value     | string | Y         | Data content |

##### Link Information

| Parameter       | Type   | Mandatory | Description                                                                                                                                                                                                                        |
| --------------- | ------ | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| target_url      | string | Y         | Target link to download APP                                                                                                                                                                                                        |
| app_bundle      | string | Y         | APP packageName for Android, Bundle ID for iOS, please listen to install event, open build-in APP Store or Google Play, for install event, refer to [Check_list](check_list_en.md)                                                 |
| click_tracker   | array  | N         | click tracker                                                                                                                                                                                                                      |
| target_url_type | int    | Y         | Type of actions when user click ad, 1: open the url within webview in-app, 2: open the url within system browser, 3: open map, 4: open dial, 5: play video, 6: download App, make sure to open App Store or Google Play in the app |

#### ATTACHMENT

##### STORE

##### CATEGORY

|  ID  |  中文  |  英文  |
|  -  |  -  |  -  |
|  B7405162-B110-E039-0796-7D9A766E8B27  |  新闻  |  News  |
|  F682D42C-A3F1-F854-A4F1-9B95EAF82ED9  |  图书  |  Library  |
|  79CBC4E4-1C81-3D00-C90B-741E89AC294C  |  社交  |  Social  |
|  880E064C-CCB1-013B-BFD1-37AD5DEEE528  |  生活  |  Life  |
|  B37E28A3-F0D5-ECA9-419F-A87F73E899A8  |  财务  |  Finance  |
|  F2EFEBD9-290B-362B-E799-69DC3990A60C  |  娱乐  |  Entertain  |
|  1F97B2D3-9FF6-3108-027B-679B6D387E22  |  教育  |  Education  |
|  200E54DC-CFF5-EDC4-A4CD-89CE5C747C88  |  旅行  |  Travel  |
|  397242EE-2865-9420-1EF2-C71C9BD9703B  |  导航  |  Navigation  |
|  664C6DA9-D35B-A37E-37B0-CA64128C5041  |  商业  |  Business  |
|  5FBC0108-47D5-3014-1315-8416379D6AF4  |  工具  |  Tool  |
|  BEA4FCCE-2037-1596-EF75-C034DF44F69C  |  效率  |  Efficiency  |
|  7741B885-EC32-9F76-C200-B01E0AB6C9F7  |  健康健美  |  Fitness  |
|  D6641394-E658-9857-0E82-C30BA86798B5  |  摄影与录像  |  Camera  |
|  E777608D-BB45-8A2D-F64A-ECD433553BE4  |  商品指南  |  Product Guide  |
|  C15D7FA6-1AF5-76C2-19F0-71C33EEE533F  |  美食佳饮  |  Food and Drink  |
|  A295D31D-47D9-FCB9-BF38-F5C958DFDD5C  |  参考  |  Reference  |
|  993667EF-0B50-33AF-627A-B2BDFC18F149  |  报刊杂志  |  Magazine  |
|  A42AAEF7-8992-E19D-4C53-00FE406CE607  |  体育  |  Sports  |
|  C5431997-0943-5B5B-5693-F75D66481766  |  天气  |  Weather  |
|  4609676E-BA15-B635-CE6C-022CCB7E34EB  |  医疗  |  Medical Treatment  |
|  C271F038-85D6-2DD0-0AC9-8D35CA8DE316  |  音乐  |  Music  |
|  E4C887C3-8F3F-B896-70A3-AAB325FA6768  |  视频  |  Video  |
|  26039567-EDE2-2D97-BE16-477FAF639041  |  购物  |  Shopping  |
|  F015DDC1-8475-3138-37FB-D824608C16AF  |  交通  |  Traffic  |
|  AD9C1D4C-272D-7E6B-BB64-B770A8A5B9B0  |  动作游戏  |  Action Game  |
|  AED099F1-2B7E-9A52-9F2E-D4F7CFE500AA  |  益智解谜  |  Puzzle Game  |
|  7AD7776C-20D1-7A2A-9388-0D67A44FF399  |  卡牌游戏  |  Card Game  |
|  36FBA1A1-C5E7-D9F6-9170-9BA0DA13CBF9  |  休闲游戏  |  Casual Game  |
|  1CF3914A-E099-92A5-0ECF-D3C05A177253  |  冒险游戏  |  Adventure Game  |
|  40E3C001-ADF6-A950-6E73-5DB66B0648E3  |  角色扮演游戏  |  Role-playing Game  |
|  7DA00174-66E2-5119-6355-331049316283  |  策略游戏  |  Strategy Game  |
|  F3C58318-6A2E-223E-C3D5-5936C48196B1  |  街机游戏  |  Arcade Game  |
|  AEDADC6C-562B-751F-E137-907A605BAA6C  |  儿童游戏  |  Kids Game  |
|  34879F45-896A-FFA9-582E-D576345F1D9A  |  竞速游戏  |  Racing Game  |
|  C91C130E-3166-FB45-8951-25DC86850C12  |  聚会游戏  |  Family Game  |
|  E13A497E-BCFA-1E66-05E4-31B716765B5B  |  模拟游戏  |  Simulation Game  |
|  CEDC63B5-9A57-DFD0-75CB-1289DEB7578A  |  体育游戏  |  Sports Game  |
|  48EF79CA-B384-F7A8-736A-423E8D62A023  |  文字游戏  |  Word Game  |
|  7F43E937-69EA-5C4A-954B-E52B10094225  |  问答游戏  |  Trivia Game  |
|  59B07A55-A40B-0D29-BF2D-CF29AFED540E  |  音乐游戏  |  Music Game  |
|  EC5336A4-A434-BDDA-4246-6F50DA2EF63E  |  桌面游戏  |  Board Game  |
|  57728DCB-9EC5-DC08-B999-3F9DDDF66083  |  赌场游戏  |  Casino Game  |
|  CB45DD59-EF28-EC0D-423B-2ED10F54C8D7  |  教育游戏  |  Education Game  |


