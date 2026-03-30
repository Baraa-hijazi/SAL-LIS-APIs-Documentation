<img src="download.svg" alt="Logo" width="160">

# SAL LIS APIs Documentation

**Base URL:** `https://uat-sal.gnteq.app`
**Authentication:** Bearer Token (JWT)
**Content-Type:** `application/json`

---

## Table of Contents

1. [Authentication](#1-authentication)
2. [Get Token (API Users)](#2-get-token-api-users)
3. [Create Shipment](#3-create-shipment)
4. [Get Shipment By Airwaybill](#4-get-shipment-by-airwaybill)
5. [Assign Shipment Event](#5-assign-shipment-event)
6. [Update Shipment Weight](#6-update-shipment-weight)
7. [Tracking](#7-tracking)
8. [Label Print](#8-label-print)
9. [Create Master Airwaybill (MAWB)](#9-create-master-airwaybill-mawb)
10. [Finalize MAWB](#10-finalize-mawb)
11. [COD Settlement](#11-cod-settlement)
12. [Fuse Shipment Tracking](#12-fuse-shipment-tracking)

---

## Authentication Flow

All endpoints (except Authentication and Get Token) require a Bearer token in the `Authorization` header. To obtain a token:

1. Call **Authentication** (for UI users) or **Get Token** (for API users)
2. Extract `token.access_token` from the response
3. Pass it as `Authorization: Bearer <access_token>` on all subsequent requests

---

## 1. Authentication

Authenticates a UI user and returns a JWT access token.

**Endpoint**
```
POST /api/identity/Authentication
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |

**Request Body**
```json
{
    "username": "admin",
    "email": "",
    "password": "P@ssw0rd"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `username` | string | Yes | The user's login username |
| `email` | string | No | User email (can be empty) |
| `password` | string | Yes | The user's password |

**Response** `200 OK`
```json
{
    "id": 1,
    "userName": "admin",
    "fullName": "Administrator",
    "email": "admin@gnteq.com",
    "phoneNumber": "0000000000",
    "isWeakPassword": false,
    "lockoutEnabled": false,
    "lockoutEnd": null,
    "accessFailedCount": 0,
    "userTypeCode": null,
    "token": {
        "access_token": "eyJhbGciOiJIUzI1NiIs...",
        "expires_in": 86400
    }
}
```

---

## 2. Get Token (API Users)

Authenticates an API user and returns a JWT access token. Preferred for programmatic/integration use.

**Endpoint**
```
POST /api/identity/Authentication/GetToken
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |

**Request Body**
```json
{
    "userName": "admin",
    "password": "P@ssw0rd"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `userName` | string | Yes | API user's username |
| `password` | string | Yes | API user's password |

**Response** `200 OK`

Same structure as Authentication endpoint. Extract `token.access_token` for use in subsequent requests.

---

## 3. Create Shipment

Creates a new shipment with shipper/consignee details, items, pieces, and weight information.

**Endpoint**
```
POST /api/gnconnect/Shipments
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "customerCode": "CC870610",
    "branchCode": "BR707983",
    "AirwaybillNumber": "",
    "shippingDateTime": "2026-03-05T10:48:04.334Z",
    "dueDate": "2026-03-05T10:48:04.334Z",
    "descriptionOfGoods": "Clothes and Accessories",
    "foreignHAWB": "",
    "numberOfPieces": "3",
    "cod": 0,
    "CodCurrnecy": "SAR",
    "totalCod": 0,
    "customsDeclaredValue": 150,
    "CustomsDeclaredValueCurrency": "SAR",
    "productType": "DLV",
    "dutyHandling": "DDP",
    "supplierCode": "SPL",
    "labelformat": "HTML",
    "LabelSize": "4*6",
    "IncludeOfficeDetails": true,
    "IncludeLabel": false,
    "consignee": {
        "consigneeContact": {
            "personName": "GNteq QA",
            "companyName": "GnTeq",
            "phoneNumber1": "00966500631952",
            "phoneNumber2": "00966500631952",
            "cellPhone": "00966500631952",
            "emailAddress": "home@gnteq.com",
            "type": "Residential",
            "civilId": "32156845"
        },
        "consigneeAddress": {
            "countryCode": "SA",
            "district": "",
            "city": "Riyadh",
            "line1": "Test",
            "line2": "",
            "line3": "",
            "postCode": "8994",
            "longitude": "",
            "latitude": "",
            "shortAddress": "",
            "locationCode1": "",
            "locationCode2": "",
            "locationCode3": ""
        }
    },
    "shipper": {
        "shipperAddress": {
            "countryCode": "GBR",
            "city": "London",
            "line1": "Testing Warehouse",
            "line2": "Testing Warehouse",
            "line3": "Testing Warehouse",
            "postCode": "1022",
            "longitude": "",
            "latitude": "",
            "locationCode1": "",
            "locationCode2": "",
            "locationCode3": ""
        },
        "shipperContact": {
            "personName": "GNteq QA",
            "companyName": "GNteq",
            "phoneNumber1": "00966500631952",
            "phoneNumber2": "00966500631952",
            "cellPhone": "",
            "emailAddress": "shipper@gnteq.com",
            "type": "Clothes"
        }
    },
    "items": [
        {
            "quantity": 1,
            "weight": { "unit": 1, "value": 0.5 },
            "customsValue": { "currencyCode": "SAR", "value": 100 },
            "comments": "QA",
            "reference": "QA reference",
            "commodityCode": "HHH QA",
            "goodsDescription": "Airpods",
            "countryOfOrigin": "GBR",
            "packageType": "Box",
            "containsDangerousGoods": false
        }
    ],
    "shipmentWeight": {
        "value": 0.5,
        "weightUnit": 1,
        "length": 0.5,
        "width": 0.5,
        "height": 0.5,
        "dimensionUnit": 1
    },
    "reference": {
        "shipperReference1": "Test",
        "shipperNote1": ""
    },
    "shipmentServicesCodes": [],
    "shipmentPieces": [
        {
            "pieceBarcode": "test10411",
            "pieceWeight": 10,
            "piecePrice": 15,
            "pieceDescription": "test"
        },
        {
            "pieceBarcode": "test10412",
            "pieceWeight": 10,
            "piecePrice": 15,
            "pieceDescription": "test"
        }
    ]
}
```

### Field Reference

**Top-level fields**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerCode` | string | Yes | Customer identifier (e.g., `CC870610`) |
| `branchCode` | string | Yes | Branch identifier (e.g., `BR707983`) |
| `AirwaybillNumber` | string | No | Leave empty for auto-generation |
| `shippingDateTime` | string (ISO 8601) | Yes | Shipping date/time |
| `dueDate` | string (ISO 8601) | Yes | Expected delivery date |
| `descriptionOfGoods` | string | Yes | Description of shipment contents |
| `foreignHAWB` | string | No | Foreign house airwaybill reference |
| `numberOfPieces` | string | Yes | Total number of pieces (must equal `shipmentPieces.length + 1`) |
| `cod` | number | No | Cash on delivery amount |
| `CodCurrnecy` | string | No | COD currency code (e.g., `SAR`) |
| `totalCod` | number | No | Total COD amount |
| `customsDeclaredValue` | number | Yes | Declared customs value |
| `CustomsDeclaredValueCurrency` | string | Yes | Currency for declared value |
| `productType` | string | Yes | Product type code (e.g., `DLV` for delivery) |
| `dutyHandling` | string | Yes | Duty handling type (`DDP` = Delivered Duty Paid) |
| `supplierCode` | string | Yes | Supplier code (e.g., `SPL`) |
| `labelformat` | string | No | Label format (`HTML` or `PDF`) |
| `LabelSize` | string | No | Label size (e.g., `4*6`) |
| `IncludeOfficeDetails` | boolean | No | Include office details in response |
| `IncludeLabel` | boolean | No | Include label data in response |

**consignee.consigneeContact**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `personName` | string | Yes | Consignee contact name |
| `companyName` | string | No | Company name |
| `phoneNumber1` | string | Yes | Primary phone |
| `phoneNumber2` | string | No | Secondary phone |
| `cellPhone` | string | No | Cell phone |
| `emailAddress` | string | No | Email address |
| `type` | string | No | Address type (`Residential`, `Commercial`) |
| `civilId` | string | No | Civil/national ID |

**consignee.consigneeAddress**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `countryCode` | string | Yes | ISO country code (e.g., `SA`) |
| `city` | string | Yes | City name |
| `line1` | string | Yes | Address line 1 |
| `line2` | string | No | Address line 2 |
| `line3` | string | No | Address line 3 |
| `postCode` | string | No | Postal code |
| `district` | string | No | District |
| `longitude` | string | No | GPS longitude |
| `latitude` | string | No | GPS latitude |
| `shortAddress` | string | No | Short/national address |

**shipper.shipperContact** — Same structure as `consigneeContact`

**shipper.shipperAddress** — Same structure as `consigneeAddress`

**items[] (array)**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `quantity` | integer | Yes | Item quantity |
| `weight.unit` | integer | Yes | Weight unit (1 = KG) |
| `weight.value` | number | Yes | Weight value |
| `customsValue.currencyCode` | string | Yes | Currency code |
| `customsValue.value` | number | Yes | Customs value |
| `goodsDescription` | string | Yes | Item description |
| `countryOfOrigin` | string | Yes | Country of origin code |
| `packageType` | string | No | Package type (e.g., `Box`) |
| `containsDangerousGoods` | boolean | No | Dangerous goods flag |
| `commodityCode` | string | No | HS/commodity code |

**shipmentWeight**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `value` | number | Yes | Total weight |
| `weightUnit` | integer | Yes | Weight unit (1 = KG) |
| `length` | number | No | Length |
| `width` | number | No | Width |
| `height` | number | No | Height |
| `dimensionUnit` | integer | No | Dimension unit (1 = CM) |

**shipmentPieces[] (array)**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `pieceBarcode` | string | Yes | Unique barcode for the piece |
| `pieceWeight` | number | Yes | Piece weight |
| `piecePrice` | number | No | Piece declared price |
| `pieceDescription` | string | No | Piece description |

**Response** `201 Created`
```json
{
    "status": "Success",
    "airwaybill": "GNTUPD0009104326",
    "airwaybillId": 0,
    "message": null,
    "shipmentLabel": null,
    "labelDownloadUrl": null,
    "officeId": null,
    "sortaionCenter": "Test 1",
    "supplier": null,
    "childBarcodes": null,
    "nqlClientId": null,
    "nqlPassword": null
}
```

> **Note:** Shipment persistence is asynchronous. Wait at least 5 seconds after creation before querying the shipment via other endpoints (GetByAirwaybill, LabelPrint, etc.).

---

## 4. Get Shipment By Airwaybill

Retrieves full shipment details by airwaybill number.

**Endpoint**
```
GET /api/gnconnect/Shipments/GetByAirwaybill/{airwaybillNumber}
```

**Headers**
| Key | Value |
|-----|-------|
| Authorization | Bearer `<access_token>` |

**Path Parameters**
| Parameter | Type | Description |
|-----------|------|-------------|
| `airwaybillNumber` | string | The airwaybill number (e.g., `GNTUPD0009104326`) |

**Response** `200 OK`
```json
{
    "id": 2270786,
    "customerCode": "CC870610",
    "branchCode": "BR707983",
    "airwaybillNumber": "GNTUPD0009104326",
    "shippingDateTime": "2026-03-05T00:00:00+00:00",
    "dueDate": "2026-03-05T00:00:00+00:00",
    "descriptionOfGoods": "Clothes and Accessories",
    "customsDeclaredValueCurrency": "SAR",
    "codCurrnecy": "SAR",
    "numberOfPieces": "3",
    "cod": 0,
    "totalCod": 0,
    "customsDeclaredValue": 150,
    "productType": "DLV",
    "dutyHandling": "DDP",
    "supplierCode": "SPL"
}
```

**Error Response** `404 Not Found`
```json
{
    "type": "https://tools.ietf.org/html/rfc9110#section-15.5.5",
    "title": "Not Found",
    "status": 404
}
```

---

## 5. Assign Shipment Event

Assigns a tracking event (e.g., "Delivered", "Picked Up") to one or more shipments.

**Endpoint**
```
POST /api/gnconnect/ShipmentEvents/
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "shipmentTrakingEvent": [
        {
            "shipmentId": 2270786,
            "airWayBill": "GNTUPD0009104326",
            "supplierCode": "SPL"
        }
    ],
    "eventCode": "GN012",
    "eventName": "Shipment Delivered",
    "userName": "admin",
    "notes": null,
    "actionDate": "2026-03-05T10:00:00.000Z",
    "eventCountry": "SAU",
    "eventCity": "ABHA",
    "weightValue": null,
    "weightUnit": null,
    "length": null,
    "width": null,
    "height": null,
    "dimensionUnit": null,
    "pieces": null,
    "signature": null
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `shipmentTrakingEvent` | array | Yes | Array of shipments to assign the event to |
| `shipmentTrakingEvent[].shipmentId` | integer | Yes | Shipment database ID |
| `shipmentTrakingEvent[].airWayBill` | string | Yes | Airwaybill number |
| `shipmentTrakingEvent[].supplierCode` | string | No | Supplier code |
| `eventCode` | string | Yes | Event code (e.g., `GN012`) |
| `eventName` | string | Yes | Event description |
| `userName` | string | Yes | User performing the action |
| `actionDate` | string (ISO 8601) | Yes | Event timestamp |
| `eventCountry` | string | No | Country where event occurred |
| `eventCity` | string | No | City where event occurred |

**Response** `201 Created`

Returns the event object as submitted.

---

## 6. Update Shipment Weight

Updates the weight and dimensions of an existing shipment.

**Endpoint**
```
POST /api/gnconnect/Shipments/UpdateShipmentWeight
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "orderNo": "GNTUPD0009104326",
    "machineNo": "Item",
    "weight": 1,
    "weightUnit": null,
    "length": 0,
    "width": 0,
    "height": 0,
    "dimensionUnit": 1
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `orderNo` | string | Yes | Airwaybill number of the shipment |
| `machineNo` | string | No | Machine/scanner identifier |
| `weight` | number | Yes | New weight value |
| `weightUnit` | integer/null | No | Weight unit |
| `length` | number | No | Length |
| `width` | number | No | Width |
| `height` | number | No | Height |
| `dimensionUnit` | integer | No | Dimension unit (1 = CM) |

**Response** `200 OK`
```json
{
    "airwaybillNumber": "GNTUPD0009104326",
    "chargeableWeight": {
        "value": 1,
        "unitofMeasure": "KG"
    }
}
```

**Error Response** `400 Bad Request`
```json
{
    "isSuccess": false,
    "errors": ["Failed - Invalid Shipment"]
}
```

---

## 7. Tracking

Retrieves tracking information for a shipment by airwaybill, reference, or customer/branch.

**Endpoint**
```
POST /api/gnconnect/Tracking
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "airwaybill": "GNTUPD0009104326",
    "reference": "",
    "customerCode": "CC870610",
    "branchCode": "BR707983",
    "shipperReference1": "",
    "shipperReference2": ""
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `airwaybill` | string | Yes* | Airwaybill number to track |
| `reference` | string | No | Alternative reference number |
| `customerCode` | string | No | Filter by customer code |
| `branchCode` | string | No | Filter by branch code |
| `shipperReference1` | string | No | Shipper reference 1 |
| `shipperReference2` | string | No | Shipper reference 2 |

*At least one search parameter (airwaybill, reference, or customer/branch) should be provided.

**Response** `200 OK`

Returns an array of tracking events. Empty array `[]` if no tracking data found.

---

## 8. Label Print

Generates shipping labels for one or more airwaybills.

**Endpoint**
```
POST /api/gnconnect/Shipments/LabelPrint
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "airwaybills": ["GNTUPD0009104326"],
    "labelFormat": "PDF",
    "labelSize": "4*6",
    "customerCode": "CC870610",
    "branchCode": "BR707983"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `airwaybills` | string[] | Yes | Array of airwaybill numbers |
| `labelFormat` | string | Yes | `PDF` or `HTML` |
| `labelSize` | string | Yes | Label dimensions (e.g., `4*6`) |
| `customerCode` | string | Yes | Customer code |
| `branchCode` | string | Yes | Branch code |

**Response** `200 OK`
```json
[
    {
        "airwaybillNumber": "GNTUPD0009104326",
        "shipmentLabel": "<base64 or HTML content>",
        "status": "Success",
        "labelDownloadUrl": null
    }
]
```

> **Note:** Wait at least 5 seconds after shipment creation before requesting labels.

---

## 9. Create Master Airwaybill (MAWB)

Creates a Master Airwaybill by grouping one or more shipments for air cargo transport.

**Endpoint**
```
POST /api/gnconnect/MasterAirWaybill/CreateMasterAirwaybillByShipmentsWeb
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "isFinalize": false,
    "code": "",
    "isApi": false,
    "shipmentAirwabillNumbers": ["GNTUPD0009104326"],
    "airLineNumber": "065",
    "airlineCode": "SV",
    "departureDate": "2026-03-05T10:00:00.000Z",
    "arrivalDate": "2026-03-05T10:00:00.000Z",
    "origin": "GBR",
    "nextStop": "SAU",
    "flightNumber": "SV110",
    "shippingTypeCode": "",
    "originAirportCode": "LHR",
    "nextStopAirportCode": "RUH",
    "mawbTotalWeight": 1,
    "mawbChargeableWeight": 1,
    "mawbTotalVolumetricWeight": 0,
    "mawbTotalPieces": 1,
    "mawbCargoValue": 0,
    "mawbType": ""
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `isFinalize` | boolean | Yes | Set `false` to create draft, `true` to create and finalize |
| `code` | string | No | MAWB code (leave empty for auto-generation) |
| `isApi` | boolean | No | Whether this is an API-initiated request |
| `shipmentAirwabillNumbers` | string[] | Yes | Array of shipment AWBs to include |
| `airLineNumber` | string | Yes | Airline number (e.g., `065` for Saudia) |
| `airlineCode` | string | Yes | IATA airline code (e.g., `SV`) |
| `departureDate` | string (ISO 8601) | Yes | Departure date/time |
| `arrivalDate` | string (ISO 8601) | Yes | Arrival date/time |
| `origin` | string | Yes | Origin country code |
| `nextStop` | string | Yes | Destination country code |
| `flightNumber` | string | Yes | Flight number |
| `originAirportCode` | string | Yes | IATA origin airport code (e.g., `LHR`) |
| `nextStopAirportCode` | string | Yes | IATA destination airport code (e.g., `RUH`) |
| `mawbTotalWeight` | number | Yes | Total weight |
| `mawbChargeableWeight` | number | Yes | Chargeable weight |
| `mawbTotalVolumetricWeight` | number | No | Total volumetric weight |
| `mawbTotalPieces` | integer | Yes | Total number of pieces |
| `mawbCargoValue` | number | No | Total cargo value |
| `mawbType` | string/null | No | MAWB type (send `""` or `null` for default) |

**Response** `200 OK`
```json
{
    "isFinalize": false,
    "validateConsignee": false,
    "mawbCode": "065-MAW9617005",
    "csvBytes": null,
    "consigneeValidationResult": null
}
```

---

## 10. Finalize MAWB

Finalizes a previously created Master Airwaybill, locking it for shipment.

**Endpoint**
```
POST /api/gnconnect/MasterAirWaybill/FinalizeMAWB
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "isFinalize": true,
    "mawbCode": "065-MAW9617005"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `isFinalize` | boolean | Yes | Must be `true` |
| `mawbCode` | string | Yes | MAWB code from Create MAWB response |

**Response** `200 OK`
```json
{
    "isFinalize": true,
    "validateConsignee": false,
    "mawbCode": "065-MAW9617005",
    "csvBytes": null,
    "consigneeValidationResult": null
}
```

---

## 11. COD Settlement

Marks a Cash on Delivery (COD) bill as settled/paid.

**Endpoint**
```
POST /api/fuse/Billing/CODSettlmentPaid
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "bill": {
        "bill_No": "1010632110415",
        "refNumber": "",
        "commission": 10,
        "commissionVAT": 0.5
    },
    "APIUserName": "admin",
    "APIPassword": "P@ssw0rd"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `bill.bill_No` | string | Yes | Bill/settlement number |
| `bill.refNumber` | string | No | Reference number |
| `bill.commission` | number | No | Commission amount |
| `bill.commissionVAT` | number | No | VAT on commission |
| `APIUserName` | string | Yes | API username for verification |
| `APIPassword` | string | Yes | API password for verification |

**Response** `200 OK`
```json
{
    "bill_NO": "1010632110415",
    "settlementBillList": [],
    "genericResult": {
        "errorCode": "STL01",
        "errorFriendlyMessage": "Reference Not Exist",
        "status": false,
        "systemErrorMessage": "Reference Not Exist"
    }
}
```

---

## 12. Fuse Shipment Tracking

Pushes tracking events from an external system (fuse integration) into the platform.

**Endpoint**
```
POST /api/fuse/Shipment/Tracking
```

**Headers**
| Key | Value |
|-----|-------|
| Content-Type | application/json-patch+json |
| Authorization | Bearer `<access_token>` |

**Request Body**
```json
{
    "messages": [
        {
            "ItemCode": "GNTUPD0009104326",
            "EventDescription_Ar": "تم تسليم الشحنة، شكرا لتعاملك معنا",
            "EventDescription_En": "Your shipment is delivered, Thank you for choosing us",
            "LastEventDate": "2026-03-05T20:22:26",
            "Main_channel": 2,
            "OFFICE_CODE": "220866",
            "DESTINATION_OFFICE": "",
            "Mobile": "0",
            "EVENT_CODE": "18",
            "MainEvent_Code": 5,
            "Id": 793078931
        }
    ]
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `messages` | array | Yes | Array of tracking events to push |
| `messages[].ItemCode` | string | Yes | Airwaybill or item code |
| `messages[].EventDescription_Ar` | string | No | Arabic event description |
| `messages[].EventDescription_En` | string | No | English event description |
| `messages[].LastEventDate` | string (ISO 8601) | Yes | Event timestamp |
| `messages[].Main_channel` | integer | No | Channel identifier |
| `messages[].OFFICE_CODE` | string | No | Office code |
| `messages[].DESTINATION_OFFICE` | string | No | Destination office |
| `messages[].Mobile` | string | No | Mobile number for notification |
| `messages[].EVENT_CODE` | string | Yes | Event code |
| `messages[].MainEvent_Code` | integer | Yes | Main event code |
| `messages[].Id` | integer | Yes | Unique event ID |

**Response** `200 OK`
```json
{
    "status": 0,
    "statusDescription": "Success"
}
```

---

## Error Responses

All endpoints may return errors in the following formats:

**Standard HTTP Error**
```json
{
    "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
    "title": "One or more validation errors occurred.",
    "status": 400,
    "errors": {
        "fieldName": ["Error message"]
    }
}
```

**Business Logic Error (Hidden in HTTP 200)**
```json
{
    "type": "Error description",
    "detail": "Detailed message",
    "title": "Exception",
    "status": 500,
    "code": "GN50"
}
```

> **Important:** Some endpoints return HTTP 200 with a `status: 500` in the response body. Always check the response body's `status` field in addition to the HTTP status code.

**Authentication Error** `401 Unauthorized`
```json
{
    "message": "Unauthorized"
}
```
