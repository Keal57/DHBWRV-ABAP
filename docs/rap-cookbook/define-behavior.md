---
title: 7. Verhalten festlegen
description: ""
sidebar_position: 70
---

- Die Behavior Definition `ZXX_R_TRAVEL` erstellen
- Eine Verhaltensimplementierung für die Behavior Definition `ZXX_R_TRAVEL` generieren lassen
- Die BO Projection View `ZXX_C_Travel` um Angaben zum Provider Contract erweitern
- Die Behavior Projection `ZXX_C_TRAVEL` erstellen

## Behavior Definition `ZXX_R_TRAVEL`

```sql showLineNumbers
//highlight-start
managed implementation in class ZXX_bp_travel unique;
strict ( 2 );

define behavior for ZXX_R_Travel alias Travel
persistent table ZXX_travel_a
lock master
authorization master ( instance )
//etag master <field_name>
{
  create;
  update;
  delete;

  association _Bookings { create; }

  field ( readonly, numbering : managed ) TravelUuid;

  mapping for ZXX_travel_a corresponding
  {
    AgencyId = agency_id;
    BeginDate = begin_date;
    BookingFee = booking_fee;
    CreatedAt = created_at;
    CreatedBy = created_by;
    CurrencyCode = currency_code;
    CustomerId = customer_id;
    Description = description;
    EndDate = end_date;
    LastChangedAt = last_changed_at;
    LastChangedBy = last_changed_by;
    Status = status;
    TotalPrice = total_price;
    TravelId = travel_id;
    TravelUuid = travel_uuid;
  }
}

define behavior for ZXX_R_Booking alias Booking
persistent table ZXX_booking_a
lock dependent by _Travel
authorization dependent by _Travel
//etag master <field_name>
{
  update;
  delete;

  association _Travel;

  field ( readonly, numbering : managed ) BookingUuid;
  field ( readonly ) TravelUuid;

  mapping for ZXX_booking_a corresponding
  {
    BookingDate = booking_Date;
    BookingId = booking_id;
    BookingUuid = booking_uuid;
    CarrierId = carrier_id;
    ConnectionId = connection_id;
    CurrencyCode = currency_code;
    FlightDate = flight_date;
    FlightPrice = flight_price;
    TravelUuid = Travel_uuid;
  }
}
//highlight-end
```

## Verhaltensimplementierung `ZXX_BP_TRAVEL`

### Global Class `ZXX_BP_TRAVEL`

```abap title="ZXX_BP_TRAVEL.abap" showLineNumbers
//highlight-start
CLASS ZXX_bp_travel DEFINITION PUBLIC ABSTRACT FINAL FOR BEHAVIOR OF ZXX_r_travel.
  PROTECTED SECTION.

  PRIVATE SECTION.
ENDCLASS.

CLASS ZXX_bp_travel IMPLEMENTATION.
ENDCLASS.
//highlight-end
```

### Local Type `LHC_TRAVEL`

```abap title="ZXX_BP_TRAVEL.abap" shwoLineNumbers
//highlight-start
CLASS lhc_travel DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR Travel RESULT result.
ENDCLASS.

CLASS lhc_travel IMPLEMENTATION.
  METHOD get_instance_authorizations.
  ENDMETHOD.
ENDCLASS.
//highlight-end
```

## BO Projection View `ZXX_C_Travel`

```sql showLineNumbers
@EndUserText.label: 'Travel'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Search.searchable: true
@Metadata.allowExtensions: true
define root view entity ZXX_C_Travel
//highlight-start
  provider contract transactional_query
//highlight-end
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
      LastChangedAt

      /* Associations */
      _Bookings : redirected to composition child ZXX_C_Booking
}
```

## Behavior Projection `ZXX_C_TRAVEL`

```sql showLineNumbers
//highlight-start
projection;
strict ( 2 );

define behavior for ZXX_C_Travel alias Travel
{
  use create;
  use update;
  use delete;

  use association _Bookings { create; }
}

define behavior for ZXX_C_Booking alias Booking
{
  use update;
  use delete;

  use association _Travel;
}
//highlight-end
```
