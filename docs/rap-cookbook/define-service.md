---
title: 2. Business Service festlegen
description: ""
sidebar_position: 20
---

- Die BO Projection View `ZXX_C_Travel` erstellen
- Die Service Definition `ZXX_UI_TRAVEL` erstellen
- Das Service Binding `ZXX_UI_TRAVEL_V2` erstellen

## BO Projection View `ZXX_C_Travel`

```sql showLineNumbers
//highlight-start
@EndUserText.label: 'Travel'
@AccessControl.authorizationCheck: #NOT_REQUIRED
define root view entity ZXX_C_Travel
  as projection on ZXX_R_Travel
{
  key TravelUuid,
      TravelId,
      AgencyId,
      CustomerId,
      BeginDate,
      EndDate,
      BookingFee,
      TotalPrice,
      CurrencyCode,
      Description,
      Status,

      /* Administrative Data */
      CreatedBy,
      CreatedAt,
      LastChangedBy,
      LastChangedAt
}
//highlight-end
```

## Service Definition `ZXX_UI_TRAVEL`

```sql showLineNumbers
//highlight-start
@EndUserText.label: 'Travel'
define service ZXX_UI_TRAVEL {
  expose ZXX_C_Travel as Travel;
}
//highlight-end
```

## Service Binding `ZXX_UI_TRAVEL_V2`

- Service Definition: ZXX_UI_TRAVEL
- Binding Type: OData V2 - UI
