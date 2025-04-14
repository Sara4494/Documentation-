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

**Endpoint:**  
`POST construction/create/`

**Requires Authentication:** ✅  
Authorization: `Token <your_token>`

### 🎯 الوصف
يتيح هذا الاندبوينت لصاحب مواد البناء (`construction_owner`) إضافة مادة بناء جديدة إلى النظام.

---

### 🛠 Headers

| Key           | Value               |
|---------------|---------------------|
| Authorization | Token <your_token>  |
| Content-Type  | multipart/form-data |

---

### 📩 البيانات المطلوبة (Form Data)

| الحقل        | النوع      | مطلوب؟ | الوصف                              |
|--------------|------------|--------|-------------------------------------|
| price        | decimal    | ✅     | سعر مادة البناء (مثلاً: 2500.50)    |
| description  | string     | ✅     | وصف لمادة البناء                    |
| image        | image file | ✅     | صورة لمادة البناء                   |
| category     | integer    | ✅     | ID الخاص بفئة مادة البناء (CategoryConstruction) |

**🔒 ملاحظة:**  
- الـ owner لا يتم إدخاله من العميل، بل يتم تعيينه تلقائيًا من المستخدم المسجل حاليًا.

---

### 📤 مثال على الطلب (Postman)

**POST** `construction/create/`

**Headers:**
```http
Authorization: Token 13a2d7fbb5d64075a80e3e0f5e98d8ab
```

**Body (form-data):**
```
price: 3000.00
description: رمل ناعم للتشطيب
image: <اختيار صورة>
category: 2
```

---

### ✅ الاستجابة الناجحة

```json
{
  "id": 5,
  "price": "3000.00",
  "description": "رمل ناعم للتشطيب",
  "image": "http://localhost:8000/media/construction_imeges/filename.jpg",
  "category": 2
}
```

---

### ❌ الأخطاء المتوقعة

| الحالة | الكود | الرسالة                                      |
|--------|------|-----------------------------------------------|
| غير مسموح | 403  | `{"error": "Unauthorized"}`                |
| غير صالح | 400  | تفاصيل الحقول الغير صالحة                  |

---


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

 
 

# Order API Documentation

## 🛠 إنشاء طلب (Create Order)
**POST** `/orders/create/`

### Headers
```
Authorization: Token <your_token>
Content-Type: application/json
```

### Request Body
```json
{
  "item_type": "worker | equipment | construction",
  "item_id": 1
}
```

### Response (مثال)
```json
{
  "message": "تم إرسال الطلب بنجاح ✅",
  "order_id": 12,
  "item_type": "worker",
  "category": "سباك",
  "price": "250.00",
  "created_at": "2025-04-14T22:10:15.123Z"
}
```

---

## 🛒 الطلبات التي قمت بها (My Purchases)
**GET** `/orders/my-purchases/`

### Headers
```
Authorization: Token <your_token>
```

### Response
```json
[
  {
    "id": 12,
    "item_type": "worker",
    "item_id": 1,
    "category": "سباك",
    "image": "http://example.com/media/orders/profile.jpg",
    "price": "250.00",
    "created_at": "2025-04-14T22:10:15.123Z",
    "buyer_name": "أحمد علي"
  },
  ...
]
```

---

## 📥 الطلبات الواردة إليك (Incoming Orders)
**GET** `/orders/incoming/`

### Headers
```
Authorization: Token <your_token>
```

### Response
نفس شكل الاستجابة كما في "my-purchases".

---

## 📦 ملاحظات
- `item_type`: يمكن أن يكون `worker`, `equipment`, أو `construction`
- النظام تلقائيًا يملأ البيانات (`price`, `category`, `image`) حسب نوع المنتج أو العامل.
- في حالة `worker`، يتم جلب الصورة والسعر من حساب العامل (CustomUser).


## Enumerations

### User Types
```javascript
[
    {"value": "worker", "label": "عامل"},
    {"value": "contractor", "label": "مقاول"},
    {"value": "equipment_owner", "label": "صاحب معدات"}
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
