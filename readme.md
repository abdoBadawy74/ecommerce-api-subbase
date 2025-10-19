# 🧾 ECOMMERCE API DOCUMENTATION (Supabase)

**Base URL:**  
```
https://bsvwzsxojblzgcscsbsn.supabase.co/rest/v1
```

**Headers (يجب تضمينها في كل طلب):**
```http
apikey: <YOUR_ANON_KEY>
Authorization: Bearer <YOUR_ANON_KEY>
Content-Type: application/json
```

---

## 🧍‍♂️ 1. Authentication (Supabase Auth)

### 🔹 Sign Up
**POST** `/auth/v1/signup`

**Body:**
```json
{
  "email": "user@example.com",
  "password": "12345678"
}
```

### 🔹 Sign In
**POST** `/auth/v1/token?grant_type=password`

**Body:**
```json
{
  "email": "user@example.com",
  "password": "12345678"
}
```

---

## 🛍️ 2. Products

### 🔹 Get All Products
**GET** `/products`

**Response:**
```json
[
  {
    "id": "uuid",
    "name": "هاتف سامسونج Galaxy S24",
    "description": "هاتف ذكي بشاشة 6.5 إنش وكاميرا 50MP",
    "price": 24999.99,
    "image_url": "https://...",
    "stock": 15,
    "category": "Electronics"
  }
]
```

### 🔹 Create Product (Admin)
**POST** `/products`

**Body:**
```json
{
  "name": "Keyboard Logitech K380",
  "description": "Bluetooth keyboard",
  "price": 899.99,
  "image_url": "https://example.com/image.jpg",
  "stock": 20,
  "category": "Computers"
}
```

---

## 🛒 3. Cart

### 🔹 Get User Cart
**GET** `/cart?user_id=eq.<user_id>`

**Response:**
```json
[
  {
    "id": "uuid",
    "user_id": "uuid",
    "product_id": "uuid",
    "quantity": 2
  }
]
```

### 🔹 Add to Cart
**POST** `/cart`

**Body:**
```json
{
  "user_id": "uuid",
  "product_id": "uuid",
  "quantity": 1
}
```

---

## 📦 4. Orders

### 🔹 Get User Orders
**GET** `/orders?user_id=eq.<user_id>`

**Response:**
```json
[
  {
    "id": "uuid",
    "user_id": "uuid",
    "total_price": 1299.00,
    "status": "paid"
  }
]
```

### 🔹 Create Order
**POST** `/orders`

**Body:**
```json
{
  "user_id": "uuid",
  "total_price": 1299.00,
  "status": "pending"
}
```

---

## 📃 5. Order Items

### 🔹 Get Items in an Order
**GET** `/order_items?order_id=eq.<order_id>`

**Response:**
```json
[
  {
    "id": "uuid",
    "order_id": "uuid",
    "product_id": "uuid",
    "quantity": 2,
    "price": 24999.99
  }
]
```

---

# ⚙️ React Integration

## 📁 1. Create Supabase Client
```js
import { createClient } from '@supabase/supabase-js'

const SUPABASE_URL = 'https://bsvwzsxojblzgcscsbsn.supabase.co'
const SUPABASE_KEY = '<YOUR_ANON_KEY>'

export const supabase = createClient(SUPABASE_URL, SUPABASE_KEY)
```

## 🛍️ 2. Get Products
```js
import { supabase } from '../lib/supabaseClient'

export async function getProducts() {
  const { data, error } = await supabase.from('products').select('*')
  if (error) throw error
  return data
}
```

## 🛒 3. Add to Cart
```js
export async function addToCart(userId, productId, quantity = 1) {
  const { data, error } = await supabase
    .from('cart')
    .insert([{ user_id: userId, product_id: productId, quantity }])
  if (error) throw error
  return data
}
```

## 💻 4. Example React Component
```jsx
import React, { useEffect, useState } from 'react'
import { getProducts } from './services/products'

export default function ProductList() {
  const [products, setProducts] = useState([])

  useEffect(() => {
    getProducts().then(setProducts)
  }, [])

  return (
    <div className="grid grid-cols-3 gap-4 p-6">
      {products.map((p) => (
        <div key={p.id} className="border rounded-lg p-4 shadow">
          <img src={p.image_url} alt={p.name} className="w-full h-48 object-cover rounded" />
          <h2 className="font-bold mt-2">{p.name}</h2>
          <p className="text-gray-600">{p.price} EGP</p>
        </div>
      ))}
    </div>
  )
}
```

## 🧩 5. Environment Variables
```
REACT_APP_SUPABASE_URL=https://bsvwzsxojblzgcscsbsn.supabase.co
REACT_APP_SUPABASE_KEY="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImJzdnd6c3hvamJsemdjc2NzYnNuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjA4NDY2NDQsImV4cCI6MjA3NjQyMjY0NH0.grbGc1eQETDSB6f1AG0gyLeFTxs68U0HyB3v8i4AGFY"
```
