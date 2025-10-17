# Laporan Praktikum 4 - Laravel Breeze Authentication

## Identitas Mahasiswa

-   **Nama**: Reno Fathullah
-   **NPM**: 4522210021
-   **Mata Kuliah**: Pemrograman Berbasis Web
-   **Materi**: Autentikasi Laravel Breeze

## Deskripsi Proyek

LaraPress adalah proyek Laravel yang mengimplementasikan sistem autentikasi menggunakan Laravel Breeze. Proyek ini mencakup instalasi, konfigurasi, dan modifikasi route untuk langsung menampilkan halaman login serta manajemen posts.

## Materi Praktikum

### 1. Instalasi Laravel Breeze

#### Langkah 1: Install Laravel Breeze via Composer

```bash
composer require laravel/breeze --dev
```

#### Langkah 2: Install Breeze Stack

```bash
php artisan breeze:install
```

Pilih stack yang diinginkan (Blade, React, Vue, atau API).

#### Langkah 3: Install Dependencies dan Compile Assets

```bash
npm install
npm run dev
```

#### Langkah 4: Migrate Database

```bash
php artisan migrate
```

### 2. Menjalankan Aplikasi

Jalankan aplikasi Laravel dengan perintah:

```bash
php artisan serve
```

Aplikasi akan berjalan di `http://localhost:8000`

### 3. Modifikasi Route untuk Langsung ke Halaman Login

Untuk membuat halaman utama langsung menampilkan halaman login, dilakukan modifikasi pada file `routes/web.php`:

**Implementasi yang digunakan:**

```php
Route::get('/', function () {
    return view('auth.login');
});
```

Dengan cara ini, saat user mengakses root URL (`/`), mereka akan langsung melihat halaman login tanpa perlu redirect.

**Alternatif lain yang bisa digunakan:**

```php
// Menggunakan redirect ke route login
Route::get('/', function () {
    return redirect()->route('login');
});

// Atau menggunakan redirect helper yang lebih singkat
Route::redirect('/', '/login');
```

### 4. Route yang Diimplementasikan

#### Route Publik

-   `/` - Menampilkan halaman login

#### Route Autentikasi (Protected)

-   `/dashboard` - Halaman dashboard (memerlukan autentikasi dan verifikasi email)
-   `/profile` - Menampilkan form edit profil
-   `/profile` (PATCH) - Update profil user
-   `/profile` (DELETE) - Hapus akun user

#### Route Posts Management (Protected)

-   `/posts/create` - Form create post baru
-   `/posts` (POST) - Menyimpan post baru
-   `/posts/{post}/edit` - Form edit post
-   `/posts/{post}` (PUT) - Update post
-   `/posts/{post}` (DELETE) - Hapus post

### 5. Fitur-Fitur Laravel Breeze

Laravel Breeze menyediakan:

-   **Login** - Halaman login user
-   **Register** - Halaman registrasi user baru
-   **Password Reset** - Fitur reset password via email
-   **Email Verification** - Verifikasi email (aktif pada route dashboard)
-   **Profile Management** - Halaman untuk update profil user
-   **Password Confirmation** - Konfirmasi password untuk aksi sensitif

### 6. Struktur File Autentikasi

Setelah instalasi Breeze, file-file berikut akan ditambahkan:

-   `app/Http/Controllers/Auth/*` - Controller untuk autentikasi
-   `resources/views/auth/*` - View untuk halaman login, register, dll
-   `resources/views/layouts/*` - Layout aplikasi
-   `resources/views/profile/*` - Halaman profil user
-   `routes/auth.php` - Route khusus untuk autentikasi

### 7. Middleware yang Digunakan

#### Middleware `auth`

Melindungi route agar hanya bisa diakses oleh user yang sudah login:

```php
Route::middleware('auth')->group(function () {
    // Route yang dilindungi
});
```

#### Middleware `verified`

Memastikan user sudah melakukan verifikasi email:

```php
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth', 'verified'])->name('dashboard');
```

### 8. Cara Kerja Aplikasi

1. **Akses Pertama Kali**

    - User mengakses `http://localhost:8000`
    - Langsung ditampilkan halaman login (`auth.login`)

2. **Registrasi**

    - User yang belum punya akun dapat registrasi melalui link di halaman login
    - Setelah registrasi, user akan login otomatis

3. **Login**

    - User memasukkan email dan password
    - Setelah berhasil login, diarahkan ke dashboard

4. **Dashboard**

    - Hanya bisa diakses oleh user yang sudah login dan terverifikasi
    - Menampilkan informasi user dan navigasi ke fitur lain

5. **Manajemen Posts**
    - User dapat membuat, mengedit, dan menghapus posts
    - Semua operasi posts memerlukan autentikasi

## Hasil Running

Setelah instalasi dan modifikasi route:

1. ✅ Akses `http://localhost:8000` menampilkan halaman login secara langsung
2. ✅ User dapat melakukan registrasi akun baru
3. ✅ Setelah login, user diarahkan ke dashboard
4. ✅ User dapat mengakses halaman profil untuk update informasi
5. ✅ User dapat melakukan CRUD operations pada posts
6. ✅ Dashboard dilindungi dengan middleware `auth` dan `verified`

## Kesimpulan

Laravel Breeze menyediakan scaffold autentikasi yang minimal dan sederhana, cocok untuk memulai proyek baru dengan fitur autentikasi yang lengkap. Dengan modifikasi route menggunakan `return view('auth.login')`, user langsung melihat halaman login saat mengakses aplikasi tanpa perlu melakukan redirect tambahan. Proyek ini juga telah dilengkapi dengan fitur manajemen posts yang dilindungi dengan autentikasi.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
