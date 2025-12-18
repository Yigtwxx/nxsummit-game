# 🎮 Nx Summit Oyun & Kayıt (Check-In) Uygulaması

Bu, Nx Summit için hazırlanan resmi etkinlik web uygulamasıdır; kayıt işlemlerini daha sorunsuz hale getirmek ve güne bir eğlence ve oyunlaştırma katmanı eklemek için oluşturulmuştur.

## 🎯 Bunu Kendi Etkinliğiniz İçin Kullanmak İster misiniz?

Bu uygulama Nx Summit için geliştirildi ancak herhangi bir etkinliğe kolayca uyarlanabilir! İster bir konferans, ister buluşma (meetup), atölye çalışması veya kurumsal bir toplantı düzenliyor olun; bu depoyu (repository) fork'layabilir ve ihtiyaçlarınıza göre özelleştirebilirsiniz.

### 📖 [Tam Özelleştirme Kılavuzu](https://www.google.com/search?q=./CUSTOMIZATION.md)

Kılavuz, bilmeniz gereken her şeyi kapsamaktadır:

* 🏷️ **Markalama**: Etkinlik adını, renkleri ve logoları güncelleyin
* 📅 **Program**: Nx Summit ajandasını kendi etkinlik zaman çizelgenizle değiştirin
* 🎫 **Etkinlik Detayları**: Tarihleri, mekanı ve katılımcı bilgilerini özelleştirin
* ⚙️ **Konfigürasyon**: Kendi Supabase veritabanınızı ve ortamınızı kurun
* 🚀 **Dağıtım (Deployment)**: Özelleştirilmiş uygulamanızı etkinliğiniz için canlıya alın

**Değiştirmeniz Gerekecek Şeylere Hızlı Bir Bakış:**

* Uygulama genelindeki etkinlik adı ve markalama
* `src/pages/InfoPage.tsx` dosyasındaki tam etkinlik programı
* `src/pages/TicketPage.tsx` dosyasındaki tarih, saat ve mekan bilgileri
* Veritabanındaki katılımcı listeniz
* Supabase kurulumunuz için ortam değişkenleri (environment variables)

### 🌟 Şunlar İçin Mükemmeldir:

* Teknoloji konferansları ve buluşmaları
* Kurumsal etkinlikler ve ekip toplantıları
* Atölye çalışmaları ve eğitim seansları
* Networking etkinlikleri
* Etkileşimleri oyunlaştırmak istediğiniz her türlü topluluk etkinliği!

**[👉 Tam Özelleştirme Kılavuzu ile Başlayın](https://www.google.com/search?q=./CUSTOMIZATION.md)**

## 🌟 Özellikler

### 📱 Kayıt (Check-in) Sistemi

* Personel, QR kodlarını tarayarak katılımcıların kaydını hızlıca yapabilir
* Gerçek zamanlı durum güncellemeleri
* Verimli sıra yönetimi

### 🎫 Katılımcı Bilet Sayfası

* Her katılımcı için kişiselleştirilmiş bilet bağlantısı (örneğin, `https://your-event.com/ticket?email=...`)
* Özellikler:
* Benzersiz QR kod
* İsim ve kayıt durumu
* Oyun arayüzü (aktif olduğunda)
* Puan göstergesi



### 🎲 Oyunlaştırma Sistemi

Kayıt yaptıktan sonra katılımcılar şunları yapabilir:

* Diğer katılımcıların QR kodlarını tarayabilir
* Her benzersiz etkileşim için puan kazanabilir
* Nx ekip üyelerinden bonus puanlar toplayabilir
* Mekan etrafındaki gizli QR kodlarını keşfedebilir

### 🎁 Bonus Puan Sistemi

* Mekan genelinde gizli QR kodları
* Personel, şu durumlar için manuel bonus puan verebilir:
* Soru sormak
* Tartışmalara katılmak
* Diğer anlamlı etkileşimler



### 🎯 Yönetici (Admin) Özellikleri

* **Kayıt Tarayıcı**: Hızlı katılımcı işleme
* **Yönetici Paneli**:
* Gerçek zamanlı katılımcı yönetimi
* Puan takibi
* Oyun durumu kontrolü
* Manuel puan ödülleri


* **Çekiliş Sistemi**:
* İki çekiliş modu:
* Ağırlıklı (puana dayalı olasılık)
* Pay tabanlı (her puan için bir giriş hakkı)


* Canlı çekiliş animasyonu
* Kazanan takibi



## 🛠️ Teknoloji Yığını (Tech Stack)

* **Frontend**: TypeScript ile React 18
* **Styling**: Tailwind CSS
* **Icons**: Lucide React
* **Database**: Supabase
* **QR Kod**:
* Tarama: html5-qrcode
* Oluşturma: qrcode.react


* **Serverless**: Supabase Edge Functions

## 📋 Gereksinimler

* Node.js 18.x veya daha yükseği
* npm 9.x veya daha yükseği
* Supabase projesi

## 🔧 Ortam Kurulumu

1. Bir `.env` dosyası oluşturun:

```env
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key

```

2. Supabase ortam değişkenlerini ayarlayın:

* `STAFF_ACCESS_PASSWORD`: Personel kimlik doğrulaması için
* `SUPABASE_URL`: Proje URL'niz
* `SUPABASE_ANON_KEY`: Genel API anahtarı
* `SUPABASE_SERVICE_ROLE_KEY`: Yönetici API anahtarı

## 🚀 Kurulum

1. Depoyu klonlayın
2. Bağımlılıkları yükleyin:

```bash
npm install

```

3. Geliştirme sunucusunu başlatın:

```bash
npm run dev

```

## 📊 Veritabanı Şeması

### Katılımcılar (Attendees)

```sql
CREATE TABLE attendees (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  email text UNIQUE NOT NULL,
  name text NOT NULL,
  checked_in boolean DEFAULT false,
  points integer DEFAULT 0,
  value integer DEFAULT 1,
  role text DEFAULT 'attendee' NOT NULL,
  created_at timestamptz DEFAULT now()
);

```

### Taramalar (Scans)

```sql
CREATE TABLE scans (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  scanner_id uuid REFERENCES attendees(id),
  scanned_id uuid REFERENCES attendees(id),
  timestamp timestamptz DEFAULT now(),
  UNIQUE(scanner_id, scanned_id)
);

```

### Bonus Kodları (Bonus Codes)

```sql
CREATE TABLE bonus_codes (
  code text PRIMARY KEY,
  description text NOT NULL,
  points integer NOT NULL,
  max_claims integer,
  created_at timestamptz DEFAULT now()
);

```

### Bonus Talepleri (Bonus Claims)

```sql
CREATE TABLE bonus_claims (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  attendee_id uuid REFERENCES attendees(id),
  bonus_code text REFERENCES bonus_codes(code),
  claimed_at timestamptz DEFAULT now(),
  UNIQUE(attendee_id, bonus_code)
);

```

### Çekiliş Kazananları (Raffle Winners)

```sql
CREATE TABLE raffle_winners (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  attendee_id uuid REFERENCES attendees(id),
  raffle_type text NOT NULL,
  created_at timestamptz DEFAULT now()
);

```

### Ayarlar (Settings)

```sql
CREATE TABLE settings (
  key text PRIMARY KEY,
  value boolean NOT NULL,
  updated_at timestamptz DEFAULT now()
);

```

## 📁 Proje Yapısı

```
├── src/
│   ├── components/     # Tekrar kullanılabilir React bileşenleri
│   │   ├── admin/      # Yöneticiye özel bileşenler
│   │   └── ...
│   ├── lib/            # Yardımcı fonksiyonlar ve API istemcileri
│   ├── pages/          # Sayfa bileşenleri
│   └── main.tsx        # Uygulama giriş noktası
├── supabase/
│   ├── functions/      # Edge Functions
│   └── migrations/     # Veritabanı migrasyonları
└── public/             # Statik varlıklar (assets)

```

## 🔒 Güvenlik Özellikleri

* Satır Seviyesi Güvenlik (RLS) politikaları
* Şifre korumalı personel kimlik doğrulaması
* Mükerrer tarama önleme
* Rol tabanlı erişim kontrolü
* Ortam değişkeni koruması

## 🎯 Kullanılabilir Komutlar (Scripts)

* `npm run dev`: Geliştirme sunucusunu başlatır
* `npm run build`: Prodüksiyon için derler
* `npm run preview`: Prodüksiyon derlemesini önizler
* `npm run lint`: ESLint'i çalıştırır

## 🎮 Oyun Mekanikleri

### Puan Sistemi

* Her katılımcı 0 puanla başlar
* Puanlar şu yollarla kazanılır:
* Diğer katılımcıları taramak
* Bonus QR kodlarını bulmak
* Personel ödülleri
* Katılım ödülleri



### Çekiliş Sistemi

İki çekiliş modu:

1. **Ağırlıklı Çekiliş**:
* Olasılık = katılımcı_puanı / toplam_puan
* Katılıma göre ağırlıklandırılmış, herkes için adil bir şans


2. **Pay Tabanlı Çekiliş**:
* Her puan = bir giriş hakkı
* Daha fazla puan = kazanmak için daha fazla şans



## 📄 Lisans

MIT Lisansı - detaylar için LICENSE dosyasına bakın
