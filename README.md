# school-fund-bos-analysis
Analisis dan rekapitulasi data keuangan Dana BOS sekolah menggunakan SQL &amp; BigQuery.
# Analisis & Rekapitulasi Keuangan Dana BOS Sekolah (BigQuery & SQL)

## Deskripsi Proyek
Proyek portofolio ini mensimulasikan proses pengelolaan, pembersihan, dan analisis data keuangan **Bantuan Operasional Sekolah (BOS)**. Menggunakan **Google BigQuery** dan **SQL**, proyek ini dirancang untuk mengubah data transaksi keuangan yang mentah menjadi laporan rekapitulasi yang rapi, transparan, serta siap digunakan untuk pengambilan keputusan.

---

## Permasalahan (Business Problem)
Pengelolaan dana BOS instansi sering kali menghadapi tantangan seperti:
* Pencatatan transaksi yang terfragmentasi dan tidak seragam.
* Kesulitan melacak alokasi anggaran per kategori (misal: Kegiatan Belajar, Pemeliharaan, Administrasi).
* Keterlambatan dalam menyusun laporan rekapitulasi bulanan/tahunan secara akurat.

---

## Tech Stack & Tools
* **Database / Data Warehouse:** Google BigQuery
* **Query Language:** Standard SQL (Aggregations, Joins, CTEs)
* **Version Control:** Git & GitHub

---

## Tahapan Analisis & Query SQL
Berikut adalah contoh skrip SQL yang digunakan untuk mengolah data di BigQuery:

### 1. Data Cleaning & Standardization
Membersihkan nilai transaksi dan menyeragamkan format data:
```sql
SELECT 
  TRIM(transaction_id) AS transaction_id,
  PARSE_DATE('%Y-%m-%d', transaction_date) AS clean_date,
  LOWER(category) AS category,
  CAST(amount AS INT64) AS clean_amount
FROM `your_project.bos_dataset.raw_transactions`
WHERE amount IS NOT NULL;
SELECT 
  FORMAT_DATE('%Y-%m', clean_date) AS month_period,
  category,
  SUM(clean_amount) AS total_spent,
  COUNT(transaction_id) AS total_transactions
FROM `your_project.bos_dataset.clean_transactions`
GROUP BY month_period, category
ORDER BY month_period DESC, total_spent DESC;
