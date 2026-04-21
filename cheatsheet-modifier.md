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
| **Navigation & Toolbar**| `.navigationTitle()` | `("Beranda")` |
| | `.navigationBarTitleDisplayMode()` | `(.inline)`, `(.large)` |
| | `.toolbar()` | `{ ToolbarItem(placement: .topBarTrailing) { Button("Edit") { } } }` |
| | `.toolbarBackground()` | `(.visible, for: .navigationBar)` |
| | `.navigationDestination()` | `(for: Item.self) { item in DetailView(item: item) }` |
| **Text** | `.font()` | **Dynamic Type:** `(.largeTitle)`, `(.title)`, `(.title2)`, `(.title3)`, `(.headline)`, `(.body)`, `(.callout)`, `(.subheadline)`, `(.footnote)`, `(.caption)`, `(.caption2)`; **Absolute:** `(.system(size: 24))`; **System design:** `(.system(size: 16, weight: .bold, design: .rounded))`, `(.system(.title, design: .monospaced))`; **Custom:** `(.custom("Poppins-Bold", size: 18))` |
| | `.bold()` | Tanpa parameter (Membuat teks tebal) |
| | `.italic()` | Tanpa parameter (Membuat teks miring) |
| | `.lineLimit()` | `(2)` (Membatasi jumlah baris) |
| | `.multilineTextAlignment()` | `(.center)`, `(.leading)`, `(.trailing)` |
| **Image** | `.resizable()` | **Wajib** sebelum modifier ukuran lain |
| | `.aspectRatio()` | `(contentMode: .fit)`, `(contentMode: .fill)`, `(16/9, contentMode: .fit)` |
| | `.scaledToFit()` | Menjaga rasio agar muat di dalam frame |
| | `.scaledToFill()` | Menjaga rasio agar memenuhi seluruh frame |
| | `.renderingMode()` | `(.template)` (Agar warna ikon bisa diubah via foreground) |
| **Styling** | `.foregroundColor()` | `(.blue)`, `(.themePrimary)` |
| | `.tint()` | `(.orange)`, `(.themePrimary)` (Warna aksen untuk `Button`, `Toggle`, `Link`, dll.) |
| | `.background()` | `(Color.red)`, `(Circle())`, `(Material.thin)` |
| | `.glassEffect()` | `()` (Efek kaca/translucent pada platform yang mendukung) |
| | `.fill()` | `(Color.blue)`, `(LinearGradient(...))` (Mengisi warna/gradient pada `Shape`) |
| | `.overlay()` | `(RoundedRectangle(cornerRadius: 12).stroke(.white, lineWidth: 1))` |
| | `.stroke()` | `(Color.white, lineWidth: 2)` (Untuk garis tepi `Shape`) |
| | `.strokeBorder()` | `(Color.white, lineWidth: 2)` (Garis tepi di dalam batas shape) |
| | `.cornerRadius()` | `(12)` (Melengkungkan sudut) |
| | `.shadow()` | `(color: .black, radius: 5, x: 0, y: 2)` |
| | `.opacity()` | `(0.5)` (0.0 transparan - 1.0 solid) |
| | `.clipShape()` | `(Capsule())`,`(Rectangle())`,`(Ellipse())`, `(Circle())`, `(RoundedRectangle(cornerRadius: 10))`, `(UnevenRoundedRectangle(bottomLeadingRadius: 24,topLeadingRadius: 24,topTrailingRadius: 24, bottomTrailingRadius: 24))` |
| **Interaksi** | `.onTapGesture` | `{ // aksi di sini }` |
| | `.buttonStyle()` | `(.plain)`, `(.bordered)`, `(.borderedProminent)` |
| | `.disabled()` | `(true)` (Mematikan fungsi interaksi) |
| **Lifecycle & Async** | `.onAppear()` | `{ loadData() }` |
| | `.onDisappear()` | `{ stopTimer() }` |
| | `.task()` | `{ await fetchData() }` |
| | `.refreshable()` | `{ await reload() }` |
| **Presentasi** | `.sheet()` | `(isPresented: $showSheet) { SheetView() }` |
| | `.fullScreenCover()` | `(isPresented: $showFull) { FullView() }` |
| | `.popover()` | `(isPresented: $showPopover) { PopoverView() }` |
| | `.alert()` | `("Gagal", isPresented: $showAlert) { Button("OK", role: .cancel) { } }` |
| | `.confirmationDialog()` | `("Pilih Aksi", isPresented: $showDialog) { Button("Hapus", role: .destructive) { } }` |
| **Form & Input** | `.textFieldStyle()` | `(.roundedBorder)`, `(.plain)` |
| | `.keyboardType()` | `(.emailAddress)`, `(.numberPad)` |
| | `.submitLabel()` | `(.done)`, `(.next)`, `(.search)` |
| | `.focused()` | `($isFocused)` |
| | `.textInputAutocapitalization()` | `(.never)`, `(.sentences)` |
| | `.autocorrectionDisabled()` | `(true)` |
| **Layout Lanjutan** | `.position()` | `(x: 100, y: 80)` |
| | `.layoutPriority()` | `(1)` |
| | `.fixedSize()` | `()`, `(horizontal: true, vertical: false)` |
| | `.ignoresSafeArea()` | `()`, `(.container, edges: .bottom)` |
| | `.safeAreaInset()` | `(edge: .bottom) { BottomBarView() }` |
| **List & Scroll** | `.listStyle()` | `(.plain)`, `(.insetGrouped)` |
| | `.scrollIndicators()` | `(.hidden)`, `(.visible)` |
| | `.scrollContentBackground()` | `(.hidden)` |
| | `.scrollTargetBehavior()` | `(.paging)` |
| **Animasi & Transformasi** | `.animation(_:value:)` | `(.easeInOut, value: isExpanded)` |
| | `.transition()` | `(.move(edge: .bottom))`, `(.opacity)` |
| | `.matchedGeometryEffect()` | `(id: "card", in: namespace)` |
| | `.rotationEffect()` | `(.degrees(45), anchor: .center)`, `(.radians(.pi), anchor: .topLeading)` |
| | `.scaleEffect()` | `(1.5)`, `(x: 2, y: 1, anchor: .bottom)` |
| **Aksesibilitas** | `.accessibilityLabel()` | `("Tombol Simpan")` |
| | `.accessibilityHint()` | `("Menyimpan perubahan")` |
| | `.accessibilityHidden()` | `(true)` |
| | `.dynamicTypeSize()` | `(.small ... .xxxLarge)` |

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

### Font Weight
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

---

### `.tint()` dan `.glassEffect()`

Gunakan **`.tint()`** untuk mengubah warna aksen kontrol interaktif, dan **`.glassEffect()`** untuk tampilan material kaca pada platform yang mendukung.

**Contoh `.tint()`:**
```swift
VStack {
    Button("Simpan") { }
    Toggle("Aktif", isOn: .constant(true))
}
.tint(.orange)
```

**Contoh `.glassEffect()`:**
```swift
RoundedRectangle(cornerRadius: 24)
    .fill(.white.opacity(0.15))
    .frame(height: 140)
    .glassEffect()
```

Catatan: `.glassEffect()` bersifat version/platform dependent, jadi jika target deployment Anda lebih lama, gunakan pengecekan ketersediaan API (`#available`) atau fallback ke `.background(.ultraThinMaterial)`.

---

### `.navigationTitle` dan `.buttonStyle(.plain)`

`navigationTitle` dipakai di dalam `NavigationStack` untuk judul halaman, sedangkan `buttonStyle(.plain)` membuat tombol tampil tanpa gaya default (tidak seperti tombol biru standar iOS).

### Posisi ToolbarItem & Praktik Terbaik (Semantic Placements)

Saat Anda menggunakan modifier `.toolbar()`, Anda menyusun satu atau banyak `ToolbarItem`. SwiftUI punya dua cara menempatkan tombol: **Penempatan Eksplisit (Kiri/Kanan)** dan **Penempatan Semantik (Makna Aksi)**.

**Sangat disarankan** memakai opsi *Semantik* (misal `.confirmationAction`). SwiftUI akan secara otomatis menyesuaikan tampilan tombol tersebut di semua perangkat lunak Apple (misal: tombol konfirmasi akan otomatis ditebalkan di iOS, dan ditaruh di posisi yang disepakati oleh sistem operasi terkait).

| Placement (Posisi) | Penjelasan / Kegunaan |
| :--- | :--- |
| **`.automatic`** | Nilai default. Biasanya tombol ditaruh di pojok kanan atas Bar Navigasi. |
| **`.primaryAction`** | Aksi utama halaman tersebut (contoh: tombol "+" atau "Edit"). |
| **`.topBarLeading` / `.topBarTrailing`** | (Eksplisit) Memaksa isi ditaruh di pojok kiri / kanan navigasi atas layar. *(Ingat: Dulu ditulis `navigationBarLeading` dan `navigationBarTrailing`, namun ini perlahan-lahan di-*deprecate*)*. |
| **`.bottomBar`** | (Eksplisit) Memaksa isi ditaruh di bar bagian bawah (hanya di iOS/iPadOS). |
| **`.principal`** | Menaruh item persis di bagian **tengah** *NavigationBar* (Seringnya untuk mengganti teks Judul biasa menjadi *custom view*/ikon kecil). |
| **`.keyboard`** | Memasang komponen di atas *keyboard virtual* (berguna untuk tombol "Done" saat mengisi *Textfield*). |
| **`.confirmationAction`** | (Semantik) Tombol konfirmasi ("Save" / "Done") di halaman *Modal/Sheet*. Biasanya font **ditebalkan** otomatis. |
| **`.cancellationAction`** | (Semantik) Tombol batal ("Cancel" / "Dismiss") di halaman *Modal/Sheet*. Biasanya diletakkan di sisi kiri atas. |
| **`.destructiveAction`** | (Semantik) Tombol bahaya ("Delete" / "Remove"). Biasanya ditekankan dengan *tint* warna merah. |

**Contoh 1 (Toolbar Semantik pada halaman Modals/Sheets):**
```swift
NavigationStack {
    Form {
        TextField("Nama", text: .constant(""))
    }
    .navigationTitle("Edit Profil")
    .navigationBarTitleDisplayMode(.inline)
    .toolbar {
        ToolbarItem(placement: .cancellationAction) {
            Button("Batal", role: .cancel) { dismiss() }
        }
        ToolbarItem(placement: .confirmationAction) {
            Button("Simpan") { saveProfile() }
        }
    }
}
```

**Contoh 2 (Menaruh Judul/Ikon Khusus di Tengah & Tombol di atas Keyboard):**
```swift
NavigationStack {
    TextField("Ketik sesuatu...", text: .constant(""))
        .padding()
        .toolbar {
            // Mengubah Judul bagian tengah dengan logo Apple
            ToolbarItem(placement: .principal) {
                Image(systemName: "apple.logo")
                    .font(.title2)
            }
            // Tombol "Tutup Keyboard" (hanya muncul saat keyboard aktif)
            ToolbarItem(placement: .keyboard) {
                Button("Tutup Keyboard") { 
                    UIApplication.shared.sendAction(#selector(UIResponder.resignFirstResponder), to: nil, from: nil, for: nil)
                }
            }
        }
}
```

**Contoh Penggunaan Dasar (.buttonStyle):**
```swift
NavigationStack {
    VStack(spacing: 16) {
        Button {
            // aksi
        } label: {
            HStack {
                Image(systemName: "chevron.left")
                Text("Kembali")
            }
        }
        .buttonStyle(.plain)
    }
    .navigationTitle("Detail")
    .navigationBarTitleDisplayMode(.inline)
}
```

---

### Tambahan Contoh Modifier Penting

**Lifecycle + Refreshable:**
```swift
List(items) { item in
    Text(item.title)
}
.task {
    await viewModel.load()
}
.refreshable {
    await viewModel.reload()
}
```

**Sheet + Alert:**
```swift
Button("Buka Form") {
    showSheet = true
}
.sheet(isPresented: $showSheet) {
    FormView()
}
.alert("Gagal", isPresented: $showAlert) {
    Button("OK", role: .cancel) { }
}
```

**Input + Keyboard:**
```swift
TextField("Email", text: $email)
    .textFieldStyle(.roundedBorder)
    .keyboardType(.emailAddress)
    .textInputAutocapitalization(.never)
    .autocorrectionDisabled(true)
    .submitLabel(.done)
```

**Presentasi Lanjutan (`fullScreenCover`, `confirmationDialog`):**
```swift
Button("Hapus Data") {
    showDialog = true
}
.confirmationDialog("Pilih Aksi", isPresented: $showDialog) {
    Button("Lihat Preview") { showFull = true }
    Button("Hapus", role: .destructive) { deleteItem() }
    Button("Batal", role: .cancel) { }
}
.fullScreenCover(isPresented: $showFull) {
    PreviewView()
}
```

**Layout Lanjutan (`safeAreaInset`, `ignoresSafeArea`, `layoutPriority`):**
```swift
VStack {
    Text(longText)
        .lineLimit(1)
        .layoutPriority(1)

    Spacer()
}
.ignoresSafeArea(.container, edges: .bottom)
.safeAreaInset(edge: .bottom) {
    BottomBarView()
}
```

**List & Scroll (`listStyle`, `scrollIndicators`, `scrollContentBackground`):**
```swift
List(items) { item in
    Text(item.title)
}
.listStyle(.insetGrouped)
.scrollIndicators(.hidden)
.scrollContentBackground(.hidden)
```

**Animasi (`animation`, `transition`, `matchedGeometryEffect`):**
```swift
if isExpanded {
    RoundedRectangle(cornerRadius: 20)
        .matchedGeometryEffect(id: "card", in: namespace)
        .transition(.move(edge: .bottom).combined(with: .opacity))
}
.animation(.easeInOut, value: isExpanded)
```

**Transformasi (`rotationEffect`, `anchor`):**
Parameter `anchor` ini menentukan "titik poros" (seperti paku pada jam dinding) yang dipakai sistem untuk memutar view. Ini menggunakan *UnitPoint* persis seperti posisi alignment stack (`.center`, `.topLeading`, `.bottom`, dll).

```swift
RoundedRectangle(cornerRadius: 15)
    .fill(.blue)
    .frame(width: 100, height: 100)
    // Putar sebesar 45 derajat menggunakan ujung Kanan Bawah sebagai poros
    .rotationEffect(.degrees(rotation), anchor: .bottomTrailing)
```

**Aksesibilitas (`accessibilityLabel`, `accessibilityHint`, `dynamicTypeSize`):**
```swift
Image(systemName: "square.and.arrow.down")
    .accessibilityLabel("Simpan")
    .accessibilityHint("Menyimpan data ke perangkat")

Text("Judul Penting")
    .dynamicTypeSize(.small ... .xxxLarge)
```

---
