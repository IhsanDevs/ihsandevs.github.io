---
title: "Lumen Laravel Micro Framework"
date: 2023-09-10T14:10:16+07:00
draft: false
url: "/lumen-laravel-micro-framework"
disableShare: false
---

# Lumen Laravel - Micro Framework RESTFull API

## Apa itu Lumen?

Lumen adalah salah satu micro-framework dari Laravel, yang dirancang khusus untuk membangun aplikasi web dan service yang ringan dan cepat. Dengan Lumen, kita dapat membuat RESTful API dalam waktu singkat dan mudah diimplementasikan.

## Kelebihan Lumen

Lumen memiliki beberapa kelebihan, di antaranya:

- Ringan dan cepat: Lumen sangat ringan dan cepat, karena hanya memiliki fitur-fitur dasar dari Laravel. Hal ini membuatnya cocok untuk membangun aplikasi web dan service yang membutuhkan performa tinggi.
- Mudah diimplementasikan: Lumen dapat diimplementasikan dengan mudah, baik pada server lokal maupun server produksi. Kita hanya perlu melakukan beberapa konfigurasi sederhana untuk menjalankan aplikasi yang telah dibuat.
- Mendukung RESTful API: Lumen didesain khusus untuk membangun RESTful API. Hal ini membuatnya sangat cocok untuk membangun aplikasi web dan service yang mengandalkan pertukaran data melalui API.
- Kompatibel dengan Laravel: Lumen memiliki struktur dan sintaks yang mirip dengan Laravel, sehingga mudah dipelajari oleh pengguna Laravel.

## Cara Menggunakan Lumen

Untuk menggunakan Lumen, kita perlu melakukan beberapa langkah dasar, di antaranya:

1. Install Lumen: Kita dapat menginstall Lumen dengan menggunakan Composer. Jalankan perintah berikut pada terminal:

```
composer create-project --prefer-dist laravel/lumen nama-proyek

```

1. Konfigurasi environment: Lumen memiliki file `.env` yang berisi konfigurasi untuk lingkungan aplikasi. Konfigurasikan file `.env` sesuai dengan kebutuhan aplikasi kita.
2. Buat routing: Buat routing untuk mengatur bagaimana aplikasi kita akan menerima dan menangani request dari client.
3. Buat controller: Buat controller untuk menangani request dari client dan mengembalikan response yang sesuai.
4. Implementasikan middleware: Implementasikan middleware untuk melakukan validasi dan autentikasi terhadap request yang masuk.

## Contoh Implementasi Lumen untuk RESTful API - Program CRUD Manajemen Artikel

### Langkah 1: Install Lumen

Pertama, install Lumen menggunakan Composer dengan menjalankan perintah berikut pada terminal:

```bash
composer create-project --prefer-dist laravel/lumen nama-proyek

```

### Langkah 2: Konfigurasi Environment

Lumen memiliki file `.env` yang berisi konfigurasi untuk lingkungan aplikasi. Konfigurasikan file `.env` untuk koneksi database dan konfigurasi lain yang dibutuhkan.

### Langkah 3: Buat Model serta Controller Resource artikel

Karena laravel menggunakan konsep ORM yang mana setiap membuat suatu model, maka penamaan table nya adalah kosakata plural bahasa Inggris dari nama model tersebut. Jadi disarankan menggunakan bahasa Inggris dalam setiap membuat project laravel. Agar lebih rapi dan readable juga ya kannnn…

Oke lanjut.. jadi disini kita akan membuat model dengan nama `Post` . Dan kita akan generate migration, factory, serta resource controllernya. Jalankan perintah ini pada terminal:

```bash
php artisan make:model Post -mfr
```

Laravel akan generate semua file migration, controller dan factory serta pastinya model `Post` .

### Langkah 4: Buat Migration untuk Tabel Artikel

Setelah kita membuat model `Post`, selanjutnya kita akan membuat migration untuk tabel artikel. Jalankan perintah ini pada terminal:

```bash
php artisan migrate

```

Perintah ini akan membuat tabel `posts` pada database yang telah kita konfigurasi sebelumnya.

### Langkah 5: Buat Routing untuk CRUD Artikel

Selanjutnya, kita akan membuat routing untuk CRUD artikel. Buka file `routes/web.php` dan tambahkan kode berikut:

```php
$router->get('/posts', 'PostController@index');
$router->get('/posts/{id}', 'PostController@show');
$router->post('/posts', 'PostController@store');
$router->put('/posts/{id}', 'PostController@update');
$router->delete('/posts/{id}', 'PostController@delete');

```

### Langkah 6: Buat Controller untuk CRUD Artikel

Selanjutnya, kita akan membuat controller untuk CRUD artikel. Buka file `app/Http/Controllers/PostController.php` dan tambahkan kode berikut:

```php
namespace App\\Http\\Controllers;

use App\\Post;
use Illuminate\\Http\\Request;

class PostController extends Controller
{
    public function index()
    {
        $posts = Post::all();

        return response()->json([
            'success' => true,
            'data' => $posts
        ]);
    }

    public function show($id)
    {
        $post = Post::find($id);

        if (!$post) {
            return response()->json([
                'success' => false,
                'message' => 'Post not found'
            ], 400);
        }

        return response()->json([
            'success' => true,
            'data' => $post
        ]);
    }

    public function store(Request $request)
    {
        $post = new Post();
        $post->title = $request->title;
        $post->body = $request->body;
        $post->save();

        return response()->json([
            'success' => true,
            'data' => $post
        ]);
    }

    public function update(Request $request, $id)
    {
        $post = Post::find($id);

        if (!$post) {
            return response()->json([
                'success' => false,
                'message' => 'Post not found'
            ], 400);
        }

        $post->title = $request->title;
        $post->body = $request->body;
        $post->save();

        return response()->json([
            'success' => true,
            'data' => $post
        ]);
    }

    public function delete($id)
    {
        $post = Post::find($id);

        if (!$post) {
            return response()->json([
                'success' => false,
                'message' => 'Post not found'
            ], 400);
        }

        $post->delete();

        return response()->json([
            'success' => true
        ]);
    }
}

```

### Langkah 7: Uji Coba API dengan Postman

Setelah kita selesai membuat routing dan controller untuk CRUD artikel, selanjutnya kita akan menguji coba API yang telah kita buat menggunakan Postman. Pastikan Lumen telah berjalan pada server lokal kita.

1. Buka Postman dan buat request baru.
2. Set URL request ke `http://localhost:8000/posts`.
3. Pilih method `GET`.
4. Klik tombol `Send`.
5. Anda akan melihat response yang berisi data artikel yang telah kita buat sebelumnya.

Yeayy! kita telah berhasil membuat RESTful API menggunakan Lumen!
