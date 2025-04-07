
# 🧾 API Documentation - User Authentication

Base URL: `https://sara545.pythonanywhere.com/user/`

---

## 📌 Endpoints

---

## 📝 1. Register (تسجيل مستخدم جديد)

**Endpoint:** `POST /register/`

**Content-Type:** `multipart/form-data`

### 🔸 Request Body:

| Field                | Type      | Required | Description                                       |
|---------------------|-----------|----------|---------------------------------------------------|
| first_name          | string    | ✅       | اسم المستخدم الأول                               |
| last_name           | string    | ✅       | اسم المستخدم الأخير                              |
| email               | string    | ✅       | بريد إلكتروني فريد                              |
| phone               | string    | ✅       | رقم الهاتف                                       |
| governorate         | string    | ✅       | المحافظة                                          |
| city                | string    | ✅       | المدينة                                           |
| user_type           | string    | ✅       | نوع المستخدم (worker, contractor, equipment_owner, material_supplier) |
| password            | string    | ✅       | كلمة المرور                                      |
| password_confirmation | string  | ✅       | تأكيد كلمة المرور                                 |
| profile_image       | file      | ✅       | صورة الملف الشخصي                                |

> **ملاحظة:** هذا الطلب يجب أن يكون `multipart/form-data` لإرسال صورة.

---

### ✅ Success Response (201 Created):
```json
{
  "email": "example@email.com",
  "user_type": "worker",
  "first_name": "محمد",
  "last_name": "علي",
  "profile_image": "/media/profiles/image.jpg",
  "message": "User created successfully"
}
```

---

### ❌ Error Responses (400 Bad Request):

```json
{
  "password": ["Passwords must match"]
}
```

أو

```json
{
  "email": ["This field must be unique."]
}
```

---

## 🔐 2. Login (تسجيل الدخول)

**Endpoint:** `POST /login/`

**Content-Type:** `application/json`

### 🔸 Request Body:

| Field     | Type   | Required | Description          |
|-----------|--------|----------|----------------------|
| email     | string | ✅       | البريد الإلكتروني     |
| password  | string | ✅       | كلمة المرور           |

---

### ✅ Success Response (200 OK):

```json
{
  "token": "JWT_TOKEN_HERE",
  "user_type": "worker"
}
```

---

### ❌ Error Responses (400 Bad Request):

```json
{
  "non_field_errors": ["البريد الإلكتروني غير مسجل"]
}
```

أو

```json
{
  "non_field_errors": ["كلمة السر غير صحيحة"]
}
```

---

## 👥 User Types (أنواع المستخدمين)

| Value              | الوصف              |
|--------------------|--------------------|
| `worker`           | عامل               |
| `contractor`       | مقاول              |
| `equipment_owner`  | صاحب معدات         |
| `material_supplier`| مورد مواد بناء     |

---

## 🌐 Base URL

🔗 `https://sara545.pythonanywhere.com/user/`

---

## ✅ Notes

- الرجاء التأكد من رفع صورة عند تسجيل مستخدم جديد.
- التوكن الذي يتم إرجاعه من تسجيل الدخول هو JWT، ويُستخدم في التفويض لاحقًا.
- كل الطلبات يجب أن تكون إلى روابط تبدأ بـ `/user/`.
