---
title: 9. Textelemente implementieren
description: ""
sidebar_position: 90
---

- Die Interface View `ZXX_I_CustomerText` erstellen
- Die BO Base View `ZXX_R_Travel` um ein transientes Feld für Kundentexte und eine Annotation für Textelemente erweitern
- Die BO Projection View `ZXX_C_Travel` um ein transientes Feld für Kundentexte erweitern

## Interface View `ZXX_I_CustomerText`

```sql showLineNumbers
//highlight-start
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Textelement for Customer'
define view entity ZXX_I_CustomerText
as select from /dmo/customer
{
  key customer_id                                 as CustomerId,
      first_name                                  as FirstName,
      last_name                                   as LastName,

      /* Transient Data */
      concat_with_space(first_name, last_name, 1) as Name

}
//highlight-end
```

## BO Base View `ZXX_R_Travel`

```sql showLineNumbers
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Travel'
define root view entity ZXX_R_Travel
  as select from ZXX_travel_a
  composition [0..*] of ZXX_R_Booking      as _Bookings
//highlight-start
  association [1..1] to ZXX_I_CustomerText as _CustomerText on $projection.CustomerId = _CustomerText.CustomerId
//highlight-end
{
  key travel_uuid        as TravelUuid,
      travel_id          as TravelId,
      agency_id          as AgencyId,
//highlight-start
      @ObjectModel.text.element: ['CustomerName']
//highlight-end
      customer_id        as CustomerId,
      begin_date         as BeginDate,
      end_date           as EndDate,
      @Semantics.amount.currencyCode: 'CurrencyCode'
      booking_fee        as BookingFee,
      @Semantics.amount.currencyCode: 'CurrencyCode'
      total_price        as TotalPrice,
      currency_code      as CurrencyCode,
      description        as Description,
      status             as Status,

      /* Administrative Data */
      created_by         as CreatedBy,
      created_at         as CreatedAt,
      last_changed_by    as LastChangedBy,
      last_changed_at    as LastChangedAt,

//highlight-start
      /* Transient Data */
      _CustomerText.Name as CustomerName,
//highlight-end

      /* Associations */
      _Bookings
}
```

## BO Projection View `ZXX_C_Travel`

```sql showLineNumbers
@EndUserText.label: 'Travel'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Search.searchable: true
@Metadata.allowExtensions: true
define root view entity ZXX_C_Travel
  provider contract transactional_query
  as projection on ZXX_R_Travel
{
  key TravelUuid,
      TravelId,
      @Consumption.valueHelpDefinition: [{ entity: { name: '/DMO/I_Agency_StdVH', element: 'AgencyID' } }]
      AgencyId,
      @Consumption.valueHelpDefinition: [{ entity: { name: 'ZXX_I_CustomerVH', element: 'CustomerId' } }]
      CustomerId,
      BeginDate,
      EndDate,
      BookingFee,
      TotalPrice,
      @Consumption.valueHelpDefinition: [{ entity: { name: 'I_CurrencyStdVH', element: 'Currency' } }]
      CurrencyCode,
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.7
      Description,
      @Consumption.valueHelpDefinition: [{ entity: { name: 'ZXX_I_StatusVH', element: 'Status' } }]
      Status,

      /* Administrative Data */
      CreatedBy,
      CreatedAt,
      LastChangedBy,
      LastChangedAt,

//highlight-start
      /* Transient Data */
      CustomerName,
//highlight-end

      /* Associations */
      _Bookings : redirected to composition child ZXX_C_Booking
}
```
