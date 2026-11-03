# HTML → APK Şablonu (Capacitor + GitHub Actions)

Bu şablon, tek bir HTML dosyasından Android APK üretmeni sağlar.
Kod bilmeden, sadece GitHub üzerinden çalışır.

## Hızlı başlangıç

1. GitHub'da yeni repo oluştur ve bu şablon dosyalarını yükle
   (veya "Use this template" ile kopyala).
2. `www/index.html` dosyasını kendi oyununla değiştir.
3. `capacitor.config.json` içinde şunları kendine göre düzenle:
   - `appId`: benzersiz paket adı, örn. `com.ahmet.yildizkosusu`
     (ters alan adı formatı, boşluk/Türkçe karakter yok)
   - `appName`: uygulamanın telefonda görünen adı
4. `package.json` içindeki `"name"` ve `"version"` alanlarını güncelle.
5. Dosyaları GitHub'a gönder (push).
6. Repo'da **Actions** sekmesine gir → çalışan işi aç → **Artifacts**
   bölümünden `oyun-debug-apk` dosyasını indir. İşte APK!

## İmzalı yayın APK'sı (Play Store için)

Play Store'a yüklemek veya güvenle dağıtmak için imzalı APK gerekir:

1. Bilgisayarında bir kez keystore oluştur:

       keytool -genkeypair -v -keystore release.keystore -alias oyun          -keyalg RSA -keysize 2048 -validity 10000

2. Repo ayarlarında **Settings → Secrets and variables → Actions**
   bölümüne şu 4 secret'ı ekle:
   - `KEYSTORE_BASE64`: keystore'un base64 hali
     (`base64 -w0 release.keystore` ile üret, Windows'ta `certutil -encode` kullan)
   - `KEYSTORE_PASSWORD`
   - `KEY_ALIAS` (yukarıda `oyun` olarak verdik)
   - `KEY_PASSWORD`
3. Git'te sürüm etiketi oluştur ve gönder:

       git tag v1.0.0
       git push origin v1.0.0

4. **Releases** bölümünde imzalı APK otomatik yayınlanır.

## Bilgisayarında yerel test (isteğe bağlı)

    npm install
    npx cap add android      # ilk seferde
    npx cap sync android     # her HTML değişikliğinde
    npx cap open android     # Android Studio'da aç → Run

Gereksinimler: Node 22+, JDK 17+, Android Studio.

## Yeni oyun ekleme (her seferinde)

1. `www/index.html` değiştir
2. `capacitor.config.json`'da `appId` ve `appName` güncelle
3. Pushla → yeni APK hazır. Hepsi bu!

## Capacitor güncelleme

    npm install @capacitor/core@latest @capacitor/android@latest
    npm install -D @capacitor/cli@latest
    npx cap sync
