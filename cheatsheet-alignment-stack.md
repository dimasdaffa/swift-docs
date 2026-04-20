### SwiftUI Stack Alignment Cheatsheet

Dalam SwiftUI, *alignment* menentukan bagaimana *views* disejajarkan di dalam wadah (*Stack*) mereka. Ingat prinsip dasarnya: efek ini **hanya terlihat jika ukuran isi di dalam Stack tersebut berbeda-beda**.

| Jenis Stack | Parameter `alignment` | Deskripsi / Efek Visual |
| :--- | :--- | :--- |
| **VStack** <br>*(Menyusun Atas ke Bawah)* | `.leading` | Meratakan semua isi ke ujung **kiri**. |
| | `.center` | Meratakan semua isi di **tengah** *(Ini adalah nilai Bawaan/Default)*. |
| | `.trailing` | Meratakan semua isi ke ujung **kanan**. |
| **HStack** <br>*(Menyusun Kiri ke Kanan)*| `.top` | Meratakan semua isi menempel ke **atas**. |
| | `.center` | Meratakan semua isi di **tengah** secara vertikal *(Bawaan/Default)*. |
| | `.bottom` | Meratakan semua isi menempel ke **bawah**. |
| | `.firstTextBaseline` | Menyejajarkan komponen berdasarkan garis dasar (*baseline*) teks **pertama** di dalamnya (Sangat berguna untuk menyamakan teks dengan ukuran font berbeda). |
| | `.lastTextBaseline` | Menyejajarkan komponen berdasarkan garis dasar teks **terakhir** di dalamnya. |
| **ZStack** <br>*(Menumpuk Depan Belakang)*| `.topLeading` | Menempel di **Pojok Kiri Atas**. |
| | `.top` | Menempel di **Tengah Atas**. |
| | `.topTrailing` | Menempel di **Pojok Kanan Atas**. |
| | `.leading` | Menempel di **Kiri Tengah**. |
| | `.center` | Berada tepat di **Tengah-tengah** *(Bawaan/Default)*. |
| | `.trailing` | Menempel di **Kanan Tengah**. |
| | `.bottomLeading` | Menempel di **Pojok Kiri Bawah**. |
| | `.bottom` | Menempel di **Tengah Bawah**. |
| | `.bottomTrailing` | Menempel di **Pojok Kanan Bawah**. |

-----

### Best Practices

1.  **Tidak Perlu Ditulis Jika Center:** Karena `.center` adalah nilai *default* untuk semua *Stack*, Anda tidak perlu menulis `VStack(alignment: .center) { ... }`. Cukup tulis `VStack { ... }` saja untuk menghemat kode.
2.  **Kombinasi dengan Spacer:** Terkadang *alignment* tidak cukup jika Anda ingin mendorong sebuah elemen hingga ke pojok layar sesungguhnya. Dalam kasus tersebut, kombinasikan penggunaan *Stack* dengan `Spacer()`.