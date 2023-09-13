# Quest99

Tautan menuju aplikasi adaptable Quest99 bisa diakses melalui [tautan ini](ristek.link/Quest99).

## **Membuat proyek Django baru**

1. Buat folder baru dengan nama `Quest99` dan buka CMD(Command Promt)

2. Buat virtual environment dengan perintah `python -m venv env` untuk mengisolasi proyek Python kita dan aktifkan virtual environment dengan perintah `env\Scripts\activate.bat`

3. Buat file `requirements.txt` yang berisi apa saja yang dibutuhkan dalam membuat program dan nantinya akan digunakan sebagai catatan untuk installer. Lalu install pada CMD yang sudah dalam mode virtual environment dengan command `pip install -r requirements.txt`.

4. Buat proyek Django dengan menjalankan perintah `django-admin startproject (nama project) .` Atau dengn `python -m django startproject (nama project)` (Kemarin saya gagal menggunakan `django-admin` dan diajarkan menggunakan ini).  Nama project disesuaikan dengan keinginan, dan ini akan membuat folder baru dengan nama tersebut.

5. Buka file `settings.py` yang ada di dalam folder proyek, cari variabel `ALLOWED_HOSTS` dan ubah nilainya menjadi `["*"]` untuk mengizinkan akses dari semua host.

6. Kembali ke CMD atau terminal dan jalankan server dengan perintah `python manage.py runserver` di dalam folder proyek (pastikan ada file `manage.py` di dalam folder tempat membuka CMD).

7. Proyek Django baru dapat dibuka di browser dengan mengakses http://localhost:8000. Animasi roket menandakan proyek Django sudah berhasil.

8. Tekan `Ctrl+C` di CMD atau terminal untuk mematikan server. Gunakan command `deactivate` untuk mematikan virtual environment, atau dapat juga dengan menutup CMD.

9. Buat file `.gitignore` dan `add`,`commit`,dan `push` proyek ke github untuk menyimpan  yang telah dibuat sejauh ini.

## **Cara membuat aplikasi dengan nama `main` pada proyek**

1. Masih pada CMD, Jalankan perintah `python manage.py startapp main` untuk membuat folder baru bernama `main`.

2. Dendaftarkan aplikasi `main` ke proyek dengan membuka file `settings.py` dalam folder proyek dan tambahkan `'main'` pada variabel `INSTALLED_APPS`.

## **Cara membuat model pada aplikasi `main`**

1. Buka file `models.py` dan isi file tersebut dengan nama `Item` dan atribut-atribut dan tipe data yang ingin digunakan. Dalam program ini, ada 3 atribut wajib (name, amount, description) dan 3 atribut tambahan (category,char_name, dan role).

```py
class Item(models.Model):
    char_name = models.CharField(max_length=255, default='Bonaparte')
    role = models.CharField(max_length=255, default='Stealth Assasin')
    name = models.CharField(max_length=255)
    description = models.TextField()
    category = models.CharField(max_length=255)
    amount = models.IntegerField()
```

2. Jalankan perintah `python manage.py makemigrations` untuk mempersiapkan migrasi skema model ke dalam database Django lokal.

3. Jalankan perintah `python manage.py migrate` untuk menerapkan skema model yang telah dibuat ke dalam database Django lokal.

> [!IMPORTANT]
> Setiap kali ada perubahan pada model (menambahkan / mengurangi / mengganti atribut), wajib untuk melakukan migrasi untuk merefleksikan perubahan itu

## **Cara membuat sebuah fungsi pada views.py untuk dikembalikan ke dalam sebuah template HTML**

1. Buat folder baru bernama `templates` di dalam folder aplikasi `main` dan buat file `main.html` di dalamnya

2. Buka file `views.py` dalam folder `main` dan tambahkan `from django.shortcuts import render` untuk mengimpor fungsi render dari modul django.shortcuts untuk me-render tampilan HTML dengan menggunakan data yang diberikan.

```py
from django.shortcuts import render
```

3. Buat fungsi `show_main` dengan 1 parameter (anggap namanya `request`) dan di dalam fungsinya, buat sebuah dictionary yang berisi data yang akan dikirimkan ke tampilan yang kemudian di return dengan fungsi `render` dengan 3 argumennya, yaitu `request` (objek permintaan HTTP yang dikirim oleh pengguna), nama file html yang digunakan untuk me-_render_ tampilan, dan `context` (dictionary yang berisi data untuk digunakan dalam penampilan dinamis). Berikut adalah contohnya.

```py
def show_main(request):
    context = {
        'name': 'Smoke Bomb',
        'description': 'Use it to easily dissapear from enemy sight ',
        'category': 'Throwable',
        'amount': 5,
    }

    return render(request, "main.html", context)
```

4. Buka file `main.html` tadi dan ubah kode yang sebelumnya dibuat secara statis menjadi kode Django yang sesuai untuk menampilkan data. Gunakan sintaksis Django yang menggunakan tanda kurung ganda ganda `({{ }})` untuk memasukkan data dari dictionary data yang dikirimkan oleh fungsi `show_main`

## **Cara membuat sebuah routing pada urls.py aplikasi `main` untuk memetakan fungsi yang telah dibuat pada views.py**

1. Buat file `urls.py` di dalam folder `main` (jika belum ada) dan import modul path dari `django.urls` dan juga import views yang telah dibuat sebelumnya di `views.py`.

```py
from django.urls import path
from main.views import show_main
```

2. Tambahkan urlpatterns untuk menghubungkan path dengan fungsi yang telah Anda buat di `views.py`, yaitu `show_main`

```py
app_name = 'main'

urlpatterns = [
    path('', show_main, name='show_main'),
]
```

## **Cara melakukan routing pada proyek agar dapat menjalankan aplikasi `main`**

1. Buka file `urls.py` di dalam folder proyek `quest99` dan import modul `include` dari `django.urls` (`from django.urls import path, include`) untuk melakukan konfigurasi routing tampilan `main`

2. Di dalam variabel urlpatterns, tambahkan path yang akan mengarahkan ke aplikasi 'main', bisa menggunakan `include()` untuk menghubungkan ke file `urls.py` di aplikasi 'main'.

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('main/', include('main.urls')),
    path('', include('main.urls')),
]
```

## **Cara _deployment_ aplikasi ke Adaptable**

1. Setelah login, pilih "New App" dan "Connect an Existing Repository".

2. Hubungkan Adaptable.io dengan GitHub, pilih "All Repositories" (jika baru pertama kali menghubungkan).

3. Pilih repositori proyek aplikasi yang telah diunggah ke github dan branch untuk _deployment_.

4. Pilih template deployment "Python App Template" dan pilih PostgreSQL sebagai tipe basis data.

5. Sesuaikan versi Python dengan yang dibutuhkan (cek menggunakan perintah `python --version` pada command prompt).

6. Isi Start Command dengan `python manage.py migrate && gunicorn (nama folder utama).wsgi`.

7. Tentukan nama aplikasi yang juga akan menjadi nama domain situs web.

8. Centang "HTTP Listener on PORT" dan klik "Deploy App" untuk memulai proses deployment aplikasi.

## **Bagan yang berisi request client ke web aplikasi berbasis Django beserta responnya**

![Alt text](gambar/bagan.jpg)
Dalam web aplikasi Django, ketika client mengirimkan permintaan HTTP, Django menggunakan file `urls.py` `views.py` mengatur logika aplikasi, termasuk interaksi dengan models dalam `models.py`. Data yang diperlukan untuk merender tampilan dikumpulkan dalam view, dan hasilnya dirender menggunakan file HTML. File HTML mengandung kode HTML dan tag-template Django untuk memasukkan data dari view. Setelah selesai dirender, tampilan tersebut dikirim sebagai respon ke client, membentuk aliran pengembangan yang terstruktur dalam Django: `urls.py` mengelola routing, `views.py` mengatur logika, `models.py` mengelola data, dan file HTML mengontrol tampilan, menciptakan aplikasi web yang berfungsi dengan baik.

## **Mengapa kita menggunakan virtual environment?**

Agar kebutuhan (dependencies) yang dibutuhkan python dan django hanya berlaku pada virtual enviroment tersebut tanpa mengganggu folder maupun file lain pada perangkat yang kita gunakan.

## **Apakah kita tetap dapat membuat aplikasi web berbasis Django tanpa menggunakan virtual environment?**

Ya, web tetap dapat terbuat.

Namun terdapat beberapa kendala seperti file di perangkat berantakan, antar file ada peluang bertabrakan satu sama lain, hingga ketidakstabilan program karena terganggu dengan adanya program lain.

## **Jelaskan apakah itu MVC, MVT, MVVM dan perbedaan dari ketiganya**

### MVC (Model - View - Controller)

Model = Menyimpan Aplikasi data (logika dan hubungan dengan database) tanpa mengetahui  interface (tampilan) sama sekali.

View = UI (User Interface ) yang bertugas menampilkan data ke layar.

Controller = Menjadi penghubung antara Model dan View dari mulai menerima input, memprosesnya, hingga mengupdate `Model` dan `View`.

### MVT (Model - View - Template)
Model = Mengelola data dan logika tanpa mengetahui tampilan.

View = Menampilkan data ke pengguna.

Template = Menentukkan tampilan dari `View`

### MVVM (Model - View - ViewModel)
Model = Mengelola data dan logika tanpa mengetahui tampilan.

View = Menampilkan data ke pengguna.

ViewModel = Mengelola tampilan dan logika tampilan yang terkait dengan `View`.
### Perbedaan MVC, MVT, dan MVVM

MVC memegang fungsi masing - masing dalam menampilkan program. Controller berperan penting dalam mengontrol aliran `Model` dan `View`. MVC sendiri tidak berbeda jauh dengan MVC hanya saja `Template` memegang kendali dalam menentukkan tampilan. Sedangkan MVVM memisahkan `View` dari `Model` sehingga fokus pada tampilan dan logika serta tampilan khusus diurus `ViewModel` 