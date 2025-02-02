

# Dokumentasi Laravel Pribadi
**Disclaimer:** 

> Dokumentasi ini bersifat argumen, bisa jadi ada kesalahan ataupun
> langkah yang kurang efisien. maka dari itu bila terdapat kesalahan, y
> maaf :3

## BAB I  Permulaan
Dalam pembuatan suatu website. kita memerlukan sebuah informasi mengenai data data apa saja yang akan ditampilkan atau diolah. sehingga pentingnya kita untuk mendeskripsikan **database** terlebih dahulu untuk pembuatan website
Berikut ini adalah rangkuman langkah demi langkah mengembangkan laravel website:
1. Menentukan informasi database
2. membuat tabel database dengan **migration**
3. menentukan **models** dari migration
4. melakukan desain pada **views**
5. Mengidentifikasi **Route** website
6. Menyambungkan dengan **Controller**
7. Keamanan **Middleware** Sederhana

## BAB 2 Langkah Pengerjaan
### 1. Menentukan Informasi Website
Sebagai langkah awal. sangat penting jika memiliki informasi tentang website yang akan dikerjakan. sebagai contoh, saya akan memberikan contoh informasi seperti dibawah ini

> **Pengguna Website serta Aksesibilitas:**
> Admin: CRUD Siswa, CRUD Ortu, CRUD Kota
> User: CRUD Siswa , CRUD Ortu

dari informasi yang didapatkan, anda sudah mengetahui usernya kan?

> **Fitur Lainnya:**
> Login
> Register
> Keamanan Middleware

*Middleware adalah sebuah keamaan yang berbasis autensikasi, jika user tersebut tidak terautemtikasi, maka akan menolak permintaan dari user. sebagai contoh ini dibawah:
>Seseorang belum melakukan login, tetapi ingin mengakses **/codex.com/siswa**
>maka permintaan tersebut akan ditolak, karena belum ter-autentikasi. maka sama halnya jika User mengakses **/codex.com/Kota**. karena hanya **Admin** saja yang dapat mengakses halaman tersebut

### 2. Informasi Tabel Database
sebelum membuat database dengan migration, hendaknya kita membuat rancangan terlebih dahulu tentang tabel yang akan dibuatkan. berikut ini adalah contoh informasi sederhana:

### Tabel Siswa
| Nama Tabel | Tipe Data |
|--|--|
|id| int|
| nama_depan | string |
|nama_belakang|string |
|jenis_kelamin|enum|
|agama|string|
|alamat|string|


### Tabel Kota
|Nama Kolom| Tipe Data|
|--|--|
|id|int|
|nama_kota|string|
|provinsi|string|

### Tabel Ortu
|Nama Kolom| Tipe Data|
|--|--|
|id|int|
|nama_ortu| string|
|kota_id|int, FK|

### Tabel Login
|Nama Kolom| Tipe Data|
|--|--|
|Email|string|
|password|string|
|level|string|

*bagi yang belum familiar dengan **enum**. enum ialah sebuah tipe data yang value nya dideklarasikan dari awal, contohnya **enum: laki / perempuan**. yang artinya value dari jenis_kelamin hanya ada 2, yaitu **laki dan perempuan**.  
FK merupakan Foreign Key yang akan didapatkan dari tabel lain
### 3. Membuat Migration
untuk memudahkan membuat tabel secara singkat. buka file **.env**
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=database_abc  # Ubah nama database yang sesuai
DB_USERNAME=root
DB_PASSWORD=
```

silahkan diubah sesuai dengan keadaan kalian masing masing. jika sudah. jangan lupa save.

kemudian buka terminal, shortcut :

> CRTL + `

jika sudah muncul terminal, ketikkan:
````bash
php artisan make:migration create_{nama tabel}_table
````
contoh:
````bash
php artisan make:migration create_siswa_table
````


#### Hasil Migration
berikut ini adalah file yang terbentuk ketika menjalankan perintah make migration
![Migration Folder](https://raw.githubusercontent.com/Codex2142/Dokumentasi-Laravel-Pribadi/refs/heads/main/images/database%20migration.png)
Secara default, terdapat 3 tabel yang dibuatkan oleh laravel sendiri. antara lain
> create_users_table
> create_cache_table
> create_jobs_table

sedangkan kita hanya membuat tabel seperti dibawah ini
> create_siswa_table
> create_kota_table
> create_ortu_table
> create_login_table

berikut adalah contoh isi file migration, dalam kesempatan ini, saya akan menunjukkan **create_siswa_table**

Tampilan Awal:
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('siswa', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('siswa');
    }
};
```

Pada function **up()**. cobalah memodifikasi sedikit dengan cara menambahkan kolom sesuai dengan informasi yang didapatkan sebelumnya. disini akan saya contohkan secara singkat

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        Schema::create('siswa', function (Blueprint $table) {
            $table->id();
            $table->string('nama_depan');
            $table->string('nama_belakang');
            $table->string('jenis_kelamin');
            $table->string('agama');
            $table->text('alamat');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        Schema::dropIfExists('siswa');
    }
};
```

#### Breakdown Kode satu per satu
```php
public function up(): void //Fungsi untuk membuat tabel
```

```php
public function down(): void // Fungsi untuk menghapus tabel jika ada
```
```php
Schema::create('siswa',  function  (Blueprint $table)  {  
	$table->id();  $table->string('nama_depan');  
	$table->string('nama_belakang');  
	$table->string('jenis_kelamin');  
	$table->string('agama');  
	$table->text('alamat');  
	$table->timestamps();  
	// Ini adalah Kolom pada tabel yang akan dibuat
});
```
Lakukan hal yang serupa pada tabel yang lain ya

#### Bagaimana jika ada tabel foreign key?
contoh disini terdapat kasus tabel ortu memiliki relasi dengan tabel kota. maka jadi begini:

> kota (1) to ortu (many)

ortu hanya memiliki 1 kota, sedangkan kota dapat memiliki banyak ortu
```php
$table->unsignedBigInteger('kota_id'); //Baris ini membuat kolom dengan nama kota_id
$table->foreign('kota_id')->references('id')->on('kota'); // kota_id mengambil value dari kolom (ID) di tabel (KOTA)
```


jika sudah mengisikan kolom pada migration, selanjutnya adalah melakukan migrasi ke MySQL dengan cara mengetikkan perintah
```bash
php artisan migrate
```

Untuk melihat hasilnya, silahkan membuka [localhost / 127.0.0.1 | phpMyAdmin 5.2.1](http://localhost/phpmyadmin/)
![Berhasil Migrasi](https://raw.githubusercontent.com/Codex2142/Dokumentasi-Laravel-Pribadi/refs/heads/main/images/migration%20success.png)

### 4. Membuat Models Database
Mungkin beberapa dari kalian bertanya-tanya mengenai Models. sebenarnya models adalah penanggung jawab dari CRUD dari sebuah database. intinya gitu deh. atau mungkin seperti perumpamaan dibawah ini:

-   **Database** adalah seluruh perpustakaan itu sendiri.
-   **Tabel** dalam database adalah rak buku yang mengelompokkan buku berdasarkan kategori tertentu (misalnya: Rak Fiksi, Rak Sains, Rak Sejarah).
-   **Model** adalah **petugas perpustakaan** yang bertugas mencari, menambahkan, menghapus, atau memperbarui informasi buku dalam rak.

ok karena sudah paham. Langsung saja kita membuatnya
```bash
	php artisan make:model {nama_model}
```
atau seperti contoh dibawah ini
```bash
	php artisan make:model Siswa
```

jika sudah selesai dibuatkan. maka akan muncul tampilan default seperti dibawah ini

```php
<?php
namespace  App\Models;
use Illuminate\Database\Eloquent\Model;
class  Siswa  extends  Model
{

  

}
```

dalam pelatihan awal saat pembelajaran laravel. biasanya user hanya mendeklarasikan **table** dan **fillable** di dalam class Siswa extends Model
```php
protected  $table = 'siswa';
protected  $fillable = [
	'nama_depan',
	'nama_belakang',
	'jenis_kelamin',
	'agama',
	'alamat',
];
```
artinya adalah:
>table adalah nama tabel yang akan diawasi
>fillable adalah nama kolom yang dapat diisikan, **pastikan sesuai dengan yang ada di migration sebelumnya**

#### Bagaimana dengan Foreign Key?
diingatkan sekali lagi:
> kota (1) to ortu (many)

ortu hanya memiliki 1 kota, sedangkan kota dapat memiliki banyak ortu

maka kedua Model tersebut perlu ditambahkan dengan kode sebagai berikut

**Model Ortu:**

```php
public  function  kota(){
	return  $this->belongsTo(Kota::class, 'kota_id', 'id');
}
```
Artinya, setiap **ortu** memiliki **satu kota** berdasarkan `kota_id` yang merujuk ke `id` di tabel **kota**

**Model Kota:**
```php
public  function  ortu(){
	return  $this->hasMany(Ortu::class, 'kota_id', 'id');
}
```
Artinya, satu **kota** dapat memiliki **banyak ortu**, karena `id` di tabel **kota** direferensikan oleh banyak `kota_id` di tabel **ortu**


## Membuat Form Views 
form views adalah tampilan yang digunakan dalam website. disini kalian akan menyajikan tampilan website serta form untuk CRUD dan Login. maka dari itu kalian usahakan untuk mendesain sebagus mungkin. untuk membuat viewsnya, kalian ketikkan perintah:
1. Membuat halaman untuk **Read & Delete**
```bash
php artisan make:view siswa.read
```
2. Membuat halaman Edit
```bash
php artisan make:view siswa.edit
```
3. Membuat halaman Create
```bash
php artisan make:view siswa.create
```

***Penjelasan:** siswa.read memiliki arti bahwa file bernama *read.blade.php* berada di dalam folder *siswa*
anda dalam melihat hasilnya di *resources/views*

![Views](https://raw.githubusercontent.com/Codex2142/Dokumentasi-Laravel-Pribadi/refs/heads/main/images/views.png)

### Unsur penting Views
1. Read Views
	Hal yang harus diperhatikan dalam membuat read views yaitu menampilkan seluruh data yang ada di database. yang artinya kita menampilkan data secara looping, dikarenakan banyaknya data yang akan ditampilkan
	
	maka dari itu kita gunakan @foreach yang diakhiri dengan @endforeach

```html
<!-- Start Loop -->
@foreach ($siswa as $i)
    <tr>
        <td>{{ $loop->iteration }}</td>
        <td>{{ $i->nama_depan }}</td>
        <td>{{ $i->nama_belakang }}</td>
        <td>{{ $i->jenis_kelamin }}</td>
        <td>{{ $i->agama }}</td>
        <td>{{ $i->alamat }}</td>
        <td>{{ $i->created_at->format('d-m-Y H:i') }}</td>
        <td>{{ $i->updated_at->format('d-m-Y H:i') }}</td>
        <td>
            <a href="{{ route('siswa.edit', $i->id) }}" class="btn btn-warning">Edit</a>
        </td>
        <td>
            <form action="{{ route('siswa.delete', $i->id) }}" method="POST" onsubmit="return confirm('Apakah Anda yakin ingin menghapus data ini?')">
                @csrf
                @method('DELETE')
                <button type="submit" class="btn btn-danger">Hapus</button>
            </form>
        </td>
    </tr>
@endforeach
<!-- End Loop -->
```
