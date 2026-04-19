# Pharmacy API

Backend API untuk manajemen data apotek menggunakan CodeIgniter 4.

## Database Overview

- `products` → master data
- `sales` → header transaksi
- `sale_items` → detail item transaksi
- `stock_movements` → histori (IN/OUT)
- `stocks` → stok saat ini

## Database Schema

1. `products` (Master Data Barang)

```
products
-------------------------------
id (PK)
code (unique)
name
item_type
price_hna
unit
is_active
created_at
updated_at
```

Fungsi: data utama semua obat/barang

2. `sales` (Header Transaksi)

```
sales
-------------------------------
id (PK)
invoice_number
prescription_number
episode_number
medical_record_number
patient_name
doctor_name
payer_name
transaction_date
total_price
created_at
updated_at
```

Fungsi: data utama transaksi penjualan

3. `sale_items` (Detail Transaksi)

```
sale_items
-------------------------------
id (PK)
sale_id (FK → sales.id)
product_id (FK → products.id)
qty
price
subtotal
created_at
updated_at
```

Fungsi: item-item dalam satu transaksi

4. `stocks` (Stok Saat Ini)

```
stocks
-------------------------------
id (PK)
product_id (FK → products.id)
qty_available
expired_date
batch_number
created_at
updated_at
```

Fungsi: kondisi stok real-time (bisa multiple per batch)

5. `stock_movements` (Kartu Stok / Histori)

```
stock_movements
-------------------------------
id (PK)
product_id (FK → products.id)
transaction_id (nullable)
transaction_type (IN / OUT / ADJUSTMENT)
transaction_date
description
qty
balance
created_at
```

Fungsi: semua pergerakan stok (log lengkap)

## Relationships

```
products
   │
   ├── stocks
   │       (1 product → banyak batch)
   │
   ├── stock_movements
   │       (histori semua transaksi)
   │
   └── sale_items
            │
            └── sales
```
