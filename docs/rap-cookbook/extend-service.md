---
title: 5. Business Service erweitern
description: ""
sidebar_position: 50
---

- Die BO Projection View `ZXX_C_Booking` inklusive einer Assoziation zur BO Projection View `ZXX_C_Travel` erstellen
- Die BO Projection View `ZXX_C_Travel` um eine Assoziation zur BO Projection View `ZXX_C_Booking` erweitern
- Die Service Definition `ZXX_UI_TRAVEL` um die BO Projection View `ZXX_C_Booking` erweitern

## BO Projection View `ZXX_C_Booking`

```sql showLineNumbers
//highlight-start
@EndUserText.label: 'Booking'
@AccessControl.authorizationCheck: #NOT_REQUIRED
define view entity ZXX_C_Booking
  as projection on ZXX_R_Booking
{
  key BookingUuid,
      TravelUuid,
      BookingId,
      BookingDate,
      CarrierId,
      ConnectionId,
      FlightDate,
      FlightPrice,
      CurrencyCode,

      /* Associations */
      _Travel : redirected to parent ZXX_C_Travel
}
//highlight-end
```

## BO Projection View `ZXX_C_Travel`

```sql showLineNumbers
@EndUserText.label: 'Travel'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Search.searchable: true
@Metadata.allowExtensions: true
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
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.7
      Description,
      Status,

      /* Administrative Data */
      CreatedBy,
      CreatedAt,
      LastChangedBy,
      LastChangedAt,

//highlight-start
      /* Associations */
      _Bookings : redirected to composition child ZXX_C_Booking
//highlight-end
}
```

## Service Definition `ZXX_UI_TRAVEL`

```sql showLineNumbers
@EndUserText.label: 'Travel'
define service ZXX_UI_TRAVEL {
  expose ZXX_C_Travel as Travel;
//highlight-start
  expose ZXX_C_Booking as Booking;
//highlight-end
}
```
