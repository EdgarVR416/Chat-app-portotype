# Simple Chatroom

Chatroom grup publik realtime menggunakan Next.js, Tailwind CSS, dan Supabase.

## Fitur
- Chatroom realtime tanpa login
- Username otomatis generate (Guest123)
- Pesan muncul realtime tanpa refresh
- UI responsive dengan Tailwind CSS

## Setup Database Supabase

1. Buat project baru di [Supabase](https://supabase.com)
2. Buat tabel `messages` dengan struktur berikut:

```sql
CREATE TABLE messages (
  id BIGSERIAL PRIMARY KEY,
  username TEXT NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enable Row Level Security (opsional)
ALTER TABLE messages ENABLE ROW LEVEL SECURITY;

-- Policy untuk allow semua operasi (karena public chatroom)
CREATE POLICY "Allow all operations" ON messages
FOR ALL USING (true) WITH CHECK (true);

-- Enable realtime
ALTER PUBLICATION supabase_realtime ADD TABLE messages;
```

3. Dapatkan URL dan Anon Key dari Settings > API

## Cara Menjalankan

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Setup environment variables:**
   - Copy file `.env.local.example` menjadi `.env.local`
   - Isi dengan kredensial Supabase Anda:
   ```
   NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
   ```

3. **Jalankan development server:**
   ```bash
   npm run dev
   ```

4. **Buka browser:** http://localhost:3000

## Akses dari HP/Device Lain

Untuk mengakses dari HP atau device lain dalam jaringan yang sama:

1. **Jalankan dengan host flag:**
   ```bash
   npm run dev -- --host
   ```

2. **Cari IP lokal laptop Anda:**
   - Windows: `ipconfig` (cari IPv4 Address)
   - Mac/Linux: `ifconfig` atau `ip addr`

3. **Akses dari HP:**
   - Buka browser di HP
   - Masuk ke: `http://192.168.x.x:3000` (ganti dengan IP laptop Anda)
   - Contoh: `http://192.168.1.100:3000`

## Struktur Project

```
simple-chatroom/
├── lib/
│   └── supabaseClient.js    # Konfigurasi Supabase
├── pages/
│   ├── _app.js              # App wrapper
│   └── index.js             # Halaman utama chatroom
├── styles/
│   └── globals.css          # Global CSS + Tailwind
├── .env.local.example       # Template environment variables
├── package.json
├── tailwind.config.js
├── postcss.config.js
└── next.config.js
```

## Troubleshooting

- **Error koneksi Supabase:** Pastikan URL dan Anon Key benar di `.env.local`
- **Pesan tidak muncul realtime:** Pastikan tabel `messages` sudah enable realtime
- **Tidak bisa akses dari HP:** Pastikan laptop dan HP dalam jaringan WiFi yang sama