### Penjelasan Singkat tentang Modifiers
Dalam SwiftUI, **Modifiers** adalah metode yang Anda panggil pada sebuah *View* untuk mengubah tampilannya. Kebanyakan *modifier* mengembalikan *view* baru, sehingga Anda bisa menyusunnya secara berantai (disebut *Method Chaining*).

---

### Daftar Referensi Utama (Cheatsheet)

| Kategori | Modifier | Parameter & Contoh Penggunaan |
| :--- | :--- | :--- |
| **Layout** | `.padding()` | `(.all, 16)`, `(.horizontal)`, `(.top, 20)`, `(.bottom, 20)` |
| | `.frame()` | `(width: 100, height: 100, alignment: .center)` |
| | `.offset()` | `(x: 10, y: -5)` (Menggeser posisi visual) |
| | `.zIndex()` | `(1.0)` (Mengatur tumpukan tampilan) |
| **Text** | `.font()` | **Dynamic Type:** `(.largeTitle)`, `(.title)`, `(.title2)`, `(.title3)`, `(.headline)`, `(.body)`, `(.callout)`, `(.subheadline)`, `(.footnote)`, `(.caption)`, `(.caption2)`; **Absolute:** `(.system(size: 24))`; **System design:** `(.system(size: 16, weight: .bold, design: .rounded))`, `(.system(.title, design: .monospaced))`; **Custom:** `(.custom("Poppins-Bold", size: 18))` |
| | `.bold()` | Tanpa parameter (Membuat teks tebal) |
| | `.italic()` | Tanpa parameter (Membuat teks miring) |
| | `.lineLimit()` | `(2)` (Membatasi jumlah baris) |
| | `.multilineTextAlignment()` | `(.center)`, `(.leading)`, `(.trailing)` |
| **Image** | `.resizable()` | **Wajib** sebelum modifier ukuran lain |
| | `.scaledToFit()` | Menjaga rasio agar muat di dalam frame |
| | `.scaledToFill()` | Menjaga rasio agar memenuhi seluruh frame |
| | `.renderingMode()` | `(.template)` (Agar warna ikon bisa diubah via foreground) |
| **Styling** | `.foregroundColor()` | `(.blue)`, `(.themePrimary)` |
| | `.background()` | `(Color.red)`, `(Circle())`, `(Material.thin)` |
| | `.fill()` | `(Color.blue)`, `(LinearGradient(...))` (Mengisi warna/gradient pada `Shape`) |
| | `.overlay()` | `(RoundedRectangle(cornerRadius: 12).stroke(.white, lineWidth: 1))` |
| | `.stroke()` | `(Color.white, lineWidth: 2)` (Untuk garis tepi `Shape`) |
| | `.strokeBorder()` | `(Color.white, lineWidth: 2)` (Garis tepi di dalam batas shape) |
| | `.cornerRadius()` | `(12)` (Melengkungkan sudut) |
| | `.shadow()` | `(color: .black, radius: 5, x: 0, y: 2)` |
| | `.opacity()` | `(0.5)` (0.0 transparan - 1.0 solid) |
| | `.clipShape()` | `(Capsule())`,`(Rectangle())`,`(Ellipse())`, `(Circle())`, `(RoundedRectangle(cornerRadius: 10))`, `(UnevenRoundedRectangle(bottomLeadingRadius: 24,topLeadingRadius: 24,topTrailingRadius: 24, bottomTrailingRadius: 24))` |
| **Interaksi** | `.onTapGesture` | `{ // aksi di sini }` |
| | `.disabled()` | `(true)` (Mematikan fungsi interaksi) |

---

### Tips Penting: Urutan Itu Segalanya
Urutan penulisan *modifier* sangat menentukan hasil akhir. 

**Contoh 1 (Padding dulu baru warna):**
```swift
Text("Halo")
    .padding()
    .background(Color.blue) // Warna mengisi hingga area padding
```

**Contoh 2 (Warna dulu baru padding):**
```swift
Text("Halo")
    .background(Color.blue) // Hanya teks yang berwarna biru
    .padding()              // Padding ditambahkan di luar kotak biru
```

---

### Mengendalikan Siapa di Atas (Z-Index)
Saat menggunakan spacing minus di `VStack`, elemen yang ditulis lebih bawah di kode bisa tampak berada di lapisan paling depan. Untuk mengontrol urutan lapisan sesuai desain, gunakan modifier **`.zIndex()`** pada setiap kartu.

- Semakin besar angka `zIndex`, semakin depan posisinya di layar.
- Untuk susunan 3 kartu: beri `.zIndex(3)` pada kartu teratas, `.zIndex(2)` pada kartu tengah, dan `.zIndex(1)` pada kartu terbawah.

**Contoh Penggunaan:**
```swift
VStack(spacing: -24) {
    RoundedRectangle(cornerRadius: 20)
        .fill(Color.white)
        .frame(height: 150)
        .zIndex(3)

    RoundedRectangle(cornerRadius: 20)
        .fill(Color.orange)
        .frame(height: 150)
        .zIndex(2)

    RoundedRectangle(cornerRadius: 20)
        .fill(Color.black)
        .frame(height: 150)
        .zIndex(1)
}
```

---

### Ketebalan Huruf (Font Weight)
Gunakan modifier **`.fontWeight()`** untuk mengatur ketebalan teks. Berikut adalah daftar lengkap parameternya, diurutkan dari yang paling tipis hingga paling tebal:

| Kategori | Modifier | Parameter & Efek Visual |
| :--- | :--- | :--- |
| **Font Weight** | `.fontWeight()` | `(.ultraLight)` : Sangat, sangat tipis. |
| | | `(.thin)` : Sangat tipis. |
| | | `(.light)` : Tipis / Ringan. |
| | | `(.regular)` : **Normal / Bawaan.** |
| | | `(.medium)` : Sedikit lebih tebal dari normal. |
| | | `(.semibold)` : Setengah tebal (Sering dipakai untuk sub-judul). |
| | | `(.bold)` : Tebal (Bisa juga ditulis singkat dengan modifier `.bold()`). |
| | | `(.heavy)` : Sangat tebal. |
| | | `(.black)` : Paling tebal dan pekat. |

**Contoh Penggunaan:**
```swift
Text("Waktu Tersisa")
    .font(.body)
    .fontWeight(.medium) 
```

---

### Rounded di Sisi Tertentu Saja
Jika ingin rounded hanya di sudut tertentu (misalnya hanya bawah kiri/kanan), pakai `UnevenRoundedRectangle`.

**Contoh sebagai bentuk utama (disarankan):**
```swift
UnevenRoundedRectangle(bottomLeadingRadius: 24, bottomTrailingRadius: 24)
    .fill(Color.themeBlack)
    .frame(height: 140)
```

**Contoh sebagai clip pada view biasa:**
```swift
VStack {
    Text("Kartu")
}
.padding()
.background(Color.themeBlack)
.clipShape(UnevenRoundedRectangle(bottomLeadingRadius: 24, bottomTrailingRadius: 24))
```

