# API Documentation

## Base URL
`https://sara545.pythonanywhere.com`

## Authentication

### Register a new user
**Endpoint**: `POST /user/register/`

**Request Body**:
```json
{
    "first_name": "string",
    "last_name": "string",
    "email": "string",
    "phone": "string",
    "governorate": "string",
    "city": "string",
    "user_type": "worker|contractor|equipment_owner",
    "worker_specialization": "string (required if user_type=worker)",
    "profile_image": "file",
    "password": "string",
    "password_confirmation": "string",
    "price": "number (optional)"
}
```

**Response**:
```json
{
    "email": "string",
    "user_type": "string",
    "first_name": "string",
    "last_name": "string",
    "profile_image": "string (URL)",
    "token": "string",
    "message": "string",
    "worker_specialization": "string (if user_type=worker)"
}
```

### Login
**Endpoint**: `POST /user/login/`

**Request Body**:
```json
{
    "email": "string",
    "password": "string"
}
```

**Response**:
```json
{
    "token": "string",
    "user_type": "string",
    "email": "string"
}
```

### Get Profile Image
**Endpoint**: `GET /user/profile-image/`

**Headers**:
```
Authorization: Token <your_token>
```

**Response**:
```json
{
    "profile_image": "string (URL)"
}
```

## User Management

### List Workers
**Endpoint**: `GET /user/workers/`

**Response**:
```json
[
    {
        "first_name": "string",
        "last_name": "string",
        "price": "number",
        "governorate": "string",
        "city": "string",
        "worker_specialization": "string",
        "profile_image": "string (URL)"
    }
]
```

## Equipment Management

### List Equipment Categories
**Endpoint**: `GET /equipment/categories/`

**Response**:
```json
[
    {
        "id": "number",
        "name": "string",
        "image": "string (URL)"
    }
]
```

### Get Equipment by Category
**Endpoint**: `GET /equipment/categories/<category_id>/equipments/`

**Response**:
```json
[
    {
        "id": "number",
        "name": "string",
        "price": "number",
        "description": "string",
        "image": "string (URL)",
        "category": "number"
    }
]
```

### Create Equipment (Equipment Owners only)
**Endpoint**: `POST /equipment/equipments/create/`

**Headers**:
```
Authorization: Token <your_token>
```

**Request Body**:
```json
{
    "name": "string",
    "price": "number",
    "description": "string",
    "image": "file",
    "category": "number"
}
```

**Response**:
```json
{
    "id": "number",
    "name": "string",
    "price": "number",
    "description": "string",
    "image": "string (URL)",
    "category": "number"
}
```

## Construction Management

### List Construction Categories
**Endpoint**: `GET /construction/categories_construction/`

**Response**:
```json
[
    {
        "id": "number",
        "name": "string",
        "image": "string (URL)"
    }
]
```

### List All Construction Projects
**Endpoint**: `GET /construction/construction_list/`

**Response**:
```json
[
    {
        "id": "number",
        "name": "string",
        "price": "number",
        "description": "string",
        "image": "string (URL)",
        "category": "number"
    }
]
```

## 📦 إنشاء مادة بناء جديدة (Create Construction)

### Create Construction Material

- **Endpoint:** `POST /construction/create/`
- **Description:** Create a new construction material listing (only for authenticated construction owners).
- **Headers:** `Authorization: Token <token>`
- **Request Body (multipart-form):**
  ```
  {
    "price": 50.00,
    "description": "Portland cement bag",
    "image": <file>,
    "category": 1
  }
  ```
- **Response (201 Created):**
  ```json
  { "id": 5, "price": 50.00, "description": "Portland cement bag", "image": "/media/construction_images/5.png", "category": 1 }
  ``

## Models Reference

### User Model
```javascript
{
    "email": "string (unique)",
    "phone": "string",
    "governorate": "string",
    "city": "string",
    "user_type": "worker|contractor|equipment_owner",
    "worker_specialization": "string (optional)",
    "profile_image": "string (URL)",
    "first_name": "string",
    "last_name": "string",
    "price": "number (optional)"
}
```



### Equipment Model
```javascript
{
    "owner": "number (user ID)",
    "name": "string",
    "price": "number",
    "description": "string",
    "image": "string (URL)",
    "category": "number"
}
```

 
## Order Endpoints

### Create Order

- **Endpoint:** `POST orders/create/`
- **Description:** Place an order for a worker, equipment, or construction material.
- **Headers:** `Authorization: Token <token>`
- **Request Body (JSON):**
  ```json
  {
    "item_type": "worker",    // one of: worker, equipment, construction
    "item_id": 12             // ID of the item
  }
  ```
- **Response (201 Created):**
  ```json
  {
    "message": "تم إرسال الطلب بنجاح ",
    "order_id": 34,
    "item_type": "worker",
    "category": "عامل سباكة",
    "price": 150.00,
    "created_at": "2025-04-16T10:20:30Z"
  }
  ```

### List My Purchases

- **Endpoint:** `GET orders/my-purchases/`
- **Description:** Retrieve all orders made by the authenticated user.
- **Headers:** `Authorization: Token <token>`
- **Response (200 OK):**
  ```json
  [
    {
      "id": 34,
      "item_type": "worker",
      "item_id": 12,
      "category": "عامل سباكة",
      "image": "/media/profiles/worker12.jpg",
      "price": 150.00,
      "created_at": "2025-04-16T10:20:30Z",
      "buyer_name": "John Doe"
    },
    ...
  ]
  ```

### List Incoming Orders

- **Endpoint:** `GET orders/incoming/`
- **Description:** Retrieve all orders received by the authenticated user (as a seller).
- **Headers:** `Authorization: Token <token>`
- **Response (200 OK):**
  ```json
  [
    {
      "id": 35,
      "item_type": "equipment",
      "item_id": 10,
      "category": "Excavator",
      "image": "/media/equipment/10.png",
      "price": 200.00,
      "created_at": "2025-04-16T11:05:45Z",
      "buyer_name": "Jane Smith"
    },
    ...
  ]
  ```

---



## Enumerations

### User Types
```javascript
[
    {"value": "worker", "label": "عامل"},
    {"value": "contractor", "label": "مقاول"},
    {"value": "equipment_owner", "label": "صاحب معدات"}
    {"value": "construction_owner", "label": "مواد  بناء'"}

]
```

### Worker Specializations
```javascript
[
    {"value": "plumbing", "label": "عامل سباكة"},
    {"value": "carpentry", "label": "عامل نجارة"},
    {"value": "blacksmith", "label": "عامل حدادة"},
    {"value": "electrician", "label": "عامل كهرباء"},
    {"value": "plaster", "label": "عامل محارة"},
    {"value": "painter", "label": "عامل نقاشة"}
]
```
