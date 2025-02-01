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
*bagi yang belum familiar dengan **enum**. enum ialah sebuah tipe data yang value nya dideklarasikan dari awal, contohnya **enum: laki / perempuan**. yang artinya value dari jenis_kelamin hanya ada 2, yaitu **laki dan perempuan**.  
FK merupakan Foreign Key yang akan didapatkan dari tabel lain
### Membuat Migration
untuk memudahkan membuat tabel secara singkat. buka file **.env**

    DB_CONNECTION=mysql
    
    DB_HOST=127.0.0.1
    
    DB_PORT=3306
    
    DB_DATABASE=database_abc
    
    DB_USERNAME=root
    
    DB_PASSWORD=
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


## Switch to another file

All your files and folders are presented as a tree in the file explorer. You can switch from one to another by clicking a file in the tree.

## Rename a file

You can rename the current file by clicking the file name in the navigation bar or by clicking the **Rename** button in the file explorer.

## Delete a file

You can delete the current file by clicking the **Remove** button in the file explorer. The file will be moved into the **Trash** folder and automatically deleted after 7 days of inactivity.

## Export a file

You can export the current file by clicking **Export to disk** in the menu. You can choose to export the file as plain Markdown, as HTML using a Handlebars template or as a PDF.


# Synchronization

Synchronization is one of the biggest features of StackEdit. It enables you to synchronize any file in your workspace with other files stored in your **Google Drive**, your **Dropbox** and your **GitHub** accounts. This allows you to keep writing on other devices, collaborate with people you share the file with, integrate easily into your workflow... The synchronization mechanism takes place every minute in the background, downloading, merging, and uploading file modifications.

There are two types of synchronization and they can complement each other:

- The workspace synchronization will sync all your files, folders and settings automatically. This will allow you to fetch your workspace on any other device.
	> To start syncing your workspace, just sign in with Google in the menu.

- The file synchronization will keep one file of the workspace synced with one or multiple files in **Google Drive**, **Dropbox** or **GitHub**.
	> Before starting to sync files, you must link an account in the **Synchronize** sub-menu.

## Open a file

You can open a file from **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Open from**. Once opened in the workspace, any modification in the file will be automatically synced.

## Save a file

You can save any file of the workspace to **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Save on**. Even if a file in the workspace is already synced, you can save it to another location. StackEdit can sync one file with multiple locations and accounts.

## Synchronize a file

Once your file is linked to a synchronized location, StackEdit will periodically synchronize it by downloading/uploading any modification. A merge will be performed if necessary and conflicts will be resolved.

If you just have modified your file and you want to force syncing, click the **Synchronize now** button in the navigation bar.

> **Note:** The **Synchronize now** button is disabled if you have no file to synchronize.

## Manage file synchronization

Since one file can be synced with multiple locations, you can list and manage synchronized locations by clicking **File synchronization** in the **Synchronize** sub-menu. This allows you to list and remove synchronized locations that are linked to your file.


# Publication

Publishing in StackEdit makes it simple for you to publish online your files. Once you're happy with a file, you can publish it to different hosting platforms like **Blogger**, **Dropbox**, **Gist**, **GitHub**, **Google Drive**, **WordPress** and **Zendesk**. With [Handlebars templates](http://handlebarsjs.com/), you have full control over what you export.

> Before starting to publish, you must link an account in the **Publish** sub-menu.

## Publish a File

You can publish your file by opening the **Publish** sub-menu and by clicking **Publish to**. For some locations, you can choose between the following formats:

- Markdown: publish the Markdown text on a website that can interpret it (**GitHub** for instance),
- HTML: publish the file converted to HTML via a Handlebars template (on a blog for example).

## Update a publication

After publishing, StackEdit keeps your file linked to that publication which makes it easy for you to re-publish it. Once you have modified your file and you want to update your publication, click on the **Publish now** button in the navigation bar.

> **Note:** The **Publish now** button is disabled if your file has not been published yet.

## Manage file publication

Since one file can be published to multiple locations, you can list and manage publish locations by clicking **File publication** in the **Publish** sub-menu. This allows you to list and remove publication locations that are linked to your file.


# Markdown extensions

StackEdit extends the standard Markdown syntax by adding extra **Markdown extensions**, providing you with some nice features.

> **ProTip:** You can disable any **Markdown extension** in the **File properties** dialog.


## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```
