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

---

### Alignment Lanjutan

#### 1. Alignment Hanya Bekerja di Cross-Axis

- Pada `VStack`, parameter `alignment` mengatur posisi horizontal isi.
- Pada `HStack`, parameter `alignment` mengatur posisi vertikal isi.
- Pada `ZStack`, parameter `alignment` mengatur posisi tumpukan di dua sumbu sekaligus.

**Contoh `VStack` (cross-axis horizontal):**
```swift
VStack(alignment: .leading) {
	Text("Judul")
		.font(.headline)
	Text("Subjudul yang lebih panjang")
}
```

**Contoh `HStack` (cross-axis vertical):**
```swift
HStack(alignment: .top) {
	Image(systemName: "person.circle")
	VStack(alignment: .leading) {
		Text("Nama")
		Text("Detail lain")
	}
}
```

#### 2. Per Item dengan `alignmentGuide`

Gunakan `alignmentGuide` jika hanya satu elemen yang ingin digeser dari alignment bawaan stack.

```swift
VStack(alignment: .leading) {
	Text("Baris 1")
	Text("Baris 2")
		.alignmentGuide(.leading) { d in d[.leading] + 24 }
}
```

#### 3. Custom Alignment

Pakai custom alignment kalau Anda ingin beberapa nested stack mengikuti garis sejajar yang sama.

```swift
extension HorizontalAlignment {
	private enum IconTitleAlignment: AlignmentID {
		static func defaultValue(in d: ViewDimensions) -> CGFloat {
			d[.leading]
		}
	}

	static let iconTitle = HorizontalAlignment(IconTitleAlignment.self)
}

VStack(alignment: .iconTitle) {
	Image(systemName: "star.fill")
		.alignmentGuide(.iconTitle) { d in d[.trailing] }
	Text("Favorit")
		.alignmentGuide(.iconTitle) { d in d[.leading] }
}
```

#### 4. `alignment` di `frame` Berbeda dari `Stack`

`frame(alignment:)` mengatur isi di dalam kotaknya sendiri, bukan alignment antar item di stack.

```swift
Text("Halo")
	.frame(width: 200, height: 80, alignment: .topLeading)
	.background(Color.yellow.opacity(0.2))
```

#### 5. `overlay(alignment:)` dan `background(alignment:)`

Ini sering dipakai untuk badge, ikon pojok, atau label kecil.

```swift
Image(systemName: "bell.fill")
	.font(.title)
	.padding()
	.background(Circle().fill(Color.blue))
	.overlay(alignment: .topTrailing) {
		Circle()
			.fill(Color.red)
			.frame(width: 12, height: 12)
	}
```

```swift
Text("Pro")
	.padding(.horizontal, 10)
	.padding(.vertical, 6)
	.background(alignment: .center) {
		Capsule().fill(Color.green.opacity(0.2))
	}
```

#### 6. `multilineTextAlignment` untuk Isi Teks

Modifier ini mengatur perataan isi teks di dalam `Text`, bukan posisi view terhadap view lain.

```swift
Text("Teks yang panjang dan bisa turun ke beberapa baris")
	.multilineTextAlignment(.center)
	.frame(width: 220)
```

#### 7. Kapan Pakai Apa

- Pakai alignment di stack jika ingin meratakan anak-anak view di dalam container.
- Pakai alignment di frame jika ingin mengatur posisi satu view di dalam kotaknya.
- Pakai overlay/background alignment jika ingin menempelkan badge, icon, atau elemen kecil di sudut.
- Pakai alignmentGuide atau custom alignment jika layout sudah kompleks dan perlu kontrol presisi.

---

### Catatan Penting: Alignment itu Sama Seperti Titik Poros (Anchor)

Sistem penamaan `.topLeading`, `.center`, `.bottomTrailing` dsb, pada SwiftUI menggunakan apa yang disebut **UnitPoint**. Hal yang menarik adalah Anda bisa menggunakan *nilai yang sama persis* dengan *alignment* sebuah view sebagai titik poros (**anchor**) saat memberi rotasi ([`rotationEffect`](cheatsheet-modifier.md)) atau skala.

Bayangkan *anchor* seperti "memaku" kertas. Jika Anda memaku kertas di tengah (`.center`), rotasinya berputar di tempat. Jika dipaku di sudut kiri atas (`.topLeading`), dia akan terayun dari sudut situ.

```swift
Rectangle()
	.fill(Color.purple)
	.frame(width: 100, height: 100)
    // Putar kertas sejauh rotasi, pakunya dipasang di sudut Kanan Bawah
	.rotationEffect(.degrees(rotation), anchor: .bottomTrailing)
```