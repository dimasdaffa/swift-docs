### Penjelasan Singkat tentang Modifiers
Dalam SwiftUI, **Modifiers** adalah metode yang Anda panggil pada sebuah *View* untuk mengubah tampilannya. Kebanyakan *modifier* mengembalikan *view* baru, sehingga Anda bisa menyusunnya secara berantai (disebut *Method Chaining*).

---

### Daftar Referensi Utama (Cheatsheet)

| Kategori | Modifier | Parameter & Contoh Penggunaan |
| :--- | :--- | :--- |
| **Layout** | `.padding()` | `(.all, 16)`, `(.horizontal)`, `(.top, 20)` |
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
| | `.cornerRadius()` | `(12)` (Melengkungkan sudut) |
| | `.shadow()` | `(color: .black, radius: 5, x: 0, y: 2)` |
| | `.opacity()` | `(0.5)` (0.0 transparan - 1.0 solid) |
| | `.clipShape()` | `(Capsule())`,`(Rectangle())`,`(Ellipse())`, `(Circle())`, `(RoundedRectangle(cornerRadius: 10))` |
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

