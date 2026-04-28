# Tugas 6 - Middleware & API Routing

**Nama:** Fatkur Rohman Irham  
**Kampus:** Politeknik Negeri Banyuwangi  

Proyek ini merupakan kelanjutan dari sistem API Book Sales. Fokus pada tugas ini adalah menerapkan **Middleware** untuk membatasi hak akses pada operasi-operasi tertentu agar lebih aman dan sesuai aturan bisnis.

## Instruksi Tugas

1. **Atur Routing dengan Middleware:**
   - Endpoint **Read All (Index)** dan **Show (Detail)** untuk tabel `Author` dan `Genre` bersifat **publik** (dapat diakses oleh siapa saja tanpa autentikasi).
   - Endpoint **Create, Update, dan Destroy** hanya boleh diakses oleh pengguna yang memiliki role **Admin**.
2. **Testing:**
   - Gunakan Postman untuk menguji skenario Public Access vs Admin-only Access.
3. **Pengumpulan:**
   - Push repository ke GitHub.
   - Mengumpulkan Link Repository.
   - Mengumpulkan file `routes/api.php`.

---

# Hasil Pengerjaan Tugas

## Dokumentasi Pengujian Endpoint (POSTMAN)

### Bagian 1: Endpoint Public (Tanpa Autentikasi)
*Pengujian ini dilakukan tanpa mengirimkan Bearer Token (belum login)*

#### A. Tabel Genre
**1. Read All Genre (GET)**
<img width="710" height="384" alt="image" src="https://github.com/user-attachments/assets/02aa4e92-5979-4e8d-985c-a9d916386fb5" />

**2. Show Detail Genre (GET)**
<img width="716" height="378" alt="image" src="https://github.com/user-attachments/assets/11ba76c0-7d30-4386-b4f1-975b6bc02fa1" />


#### B. Tabel Author
**1. Read All Author (GET)**
<img width="719" height="404" alt="image" src="https://github.com/user-attachments/assets/5528efdf-3814-499d-87b4-32aaf4d6947d" />


**2. Show Detail Author (GET)**
<img width="718" height="395" alt="image" src="https://github.com/user-attachments/assets/7c4f16e8-ddd9-4307-8ccc-b96d08b7f074" />


---

### Bagian 2: Akses Tertolak (Akses Admin tanpa Token / Unauthorized)
*Pengujian ini menunjukkan ketika user (atau orang asing) mencoba mengakses rute terproteksi tanpa token admin*

**1. Gagal Create Genre (POST)**
<img width="722" height="437" alt="image" src="https://github.com/user-attachments/assets/99811385-f197-4a12-9244-b8fe4f5062c0" />

**2. Gagal Update genre (PUT / PATCH)**
<img width="711" height="442" alt="image" src="https://github.com/user-attachments/assets/f276079c-f5b5-46b5-998e-dde496198659" />

**3. Gagal Destroy Genre (DELETE)**
<img width="700" height="428" alt="image" src="https://github.com/user-attachments/assets/087a2b69-7e19-46f4-910c-1f093e4350e8" />

---

### Bagian 3: Akses Admin Berhasil (Menggunakan Token Admin)
*Pengujian ini dilakukan setelah Admin Login dan memasukkan Bearer Token pada Header Postman*

#### A. Operasi oleh Admin pada Tabel Genre
**1. Create Genre oleh Admin (POST)**
<img width="718" height="391" alt="image" src="https://github.com/user-attachments/assets/b5f19afa-9d13-48d5-a607-e62981cb01cd" />


**2. Update Genre oleh Admin (PUT / PATCH)**
<img width="714" height="407" alt="image" src="https://github.com/user-attachments/assets/444ae7b6-60a5-4663-a4c5-b0c6e1ca3072" />


**3. Destroy Genre oleh Admin (DELETE)**

<img width="719" height="332" alt="image" src="https://github.com/user-attachments/assets/208b13e2-c4a2-4868-a9f0-e958eebb6955" />

#### B. Operasi oleh Admin pada Tabel Author
**1. Create Author oleh Admin (POST)**
<img width="715" height="404" alt="image" src="https://github.com/user-attachments/assets/1a653423-6e1a-4e4a-9df9-46a660db7d79" />


**2. Update Author oleh Admin (PUT / PATCH)**
<img width="725" height="415" alt="image" src="https://github.com/user-attachments/assets/8a0e1651-18a2-4381-993a-e6777ab38686" />


**3. Destroy Author oleh Admin (DELETE)**
<img width="716" height="362" alt="image" src="https://github.com/user-attachments/assets/714119b0-c2a1-4060-81dc-3d09f728b5ea" />


---

### Bagian 4: Cuplikan Kode Implementasi Middleware (`routes/api.php`)


```php
route::post('register',[AuthController::class,'register']);
Route::post('login',[AuthController::class,'login']);
Route::post('logout',[AuthController::class,'logout'])->middleware('auth:api');
route::middleware('auth:api')->group(function () {

route::middleware('role:admin')->group(function () {
    Route::apiResource('/books', BookController::class)->only('store','update','destroy');
    Route::apiResource('/author', AuthorController::class)->only('store','update','destroy');
    Route::apiResource('/genre', GenreController::class)->only('store','update','destroy');
});});
Route::apiResource('/books', BookController::class)->only('index','show');
Route::apiResource('/author', AuthorController::class)->only('index','show');
Route::apiResource('/genre',GenreController::class)->only('index','show');
```
