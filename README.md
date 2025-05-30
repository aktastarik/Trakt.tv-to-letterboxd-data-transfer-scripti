# Trakt.tv'den Letterboxd'ye Veri Aktarım Script'i

Bu script, Trakt.tv hesabınızdaki izleme geçmişinizi (filmler), puanlamalarınızı ve özel listelerinizi Letterboxd'ye aktarabilmeniz için Letterboxd uyumlu CSV dosyaları oluşturmanıza yardımcı olur.

**Neden Böyle Bir Şeye İhtiyaç Var?**
Benim gibi hem Trakt.tv'nin detaylı dizi/film takibini ve topluluk özelliklerini seven, hem de Letterboxd'nin sinema odaklı dünyasına, minimalist arayüzüne ve harika günlüğüne adım atmak isteyenler için bu geçiş süreci biraz meşakkatli olabiliyor. Bu script, bu süreci olabildiğince otomatikleştirmeyi ve kolaylaştırmayı amaçlıyor. Özellikle benim gibi teknolojiye meraklı, eski usul çözümleri seven (RSS gibilerini düşünün) ama aynı zamanda pratik dijital araçlarla hayatını kolaylaştırmak isteyenler için birebir!

## ✨ Temel Özellikler

*   **İzlenen Filmleri Aktarma:** Trakt.tv'deki tüm izlenmiş film verilerinizi (izleme tarihi dahil) çeker.
*   **Puanlamaları Aktarma:** Filmlere verdiğiniz puanları Trakt.tv'nin 1-10'luk sisteminden Letterboxd'nin 0.5-5'lik yıldız sistemine dönüştürür.
*   **İzleme Listesi (Watchlist) Aktarma:** Trakt.tv'deki izleme listenizi Letterboxd formatına uygun hale getirir.
*   **Özel Listeleri Aktarma:** Oluşturduğunuz özel film listelerini çeker.
*   **PIN Tabanlı Güvenli Kimlik Doğrulama:** Trakt.tv API'si ile OAuth2 üzerinden, şifrenizi paylaşmadan güvenli bir şekilde bağlantı kurar.
*   **Letterboxd Uyumlu CSV Çıktısı:** Oluşturulan CSV dosyaları doğrudan Letterboxd'ye yüklenebilir.

## ⚠️ Önemli Notlar ve Uyarılar

*   **Sadece Filmler:** Letterboxd **sadece** film odaklı bir platformdur. Bu script, Trakt.tv'deki **dizi** izleme geçmişinizi veya dizi listelerinizi Letterboxd'ye aktaramaz. Dizi verileriniz için ayrı bir CSV dosyası oluşturulabilir (bu script'in mevcut sürümünde bu özellik eklenmemiştir ancak gelecekte eklenebilir).
*   **Geri Alınamaz İşlem:** Letterboxd'ye veri aktarımı genellikle geri alınamaz. Lütfen oluşturulan CSV dosyalarını yüklemeden önce dikkatlice kontrol edin ve gerekirse Letterboxd hesabınızın yedeğini alın (eğer böyle bir seçenek varsa).
*   **API Kullanım Limitleri:** Trakt.tv API'sinin kullanım limitleri olabilir. Çok büyük bir kütüphaneniz varsa, script'in çalışması biraz zaman alabilir veya nadiren de olsa limitlere takılabilir.

## ⚙️ Gereksinimler

*   Python 3.7+
*   `requests` kütüphanesi
*   `PyTrakt` kütüphanesi (önerilir, API etkileşimini kolaylaştırır, ancak sadece `requests` ile de yapılabilir)
*   Bir Trakt.tv hesabı
*   Trakt.tv API Anahtarları (`Client ID` ve `Client Secret`)

## 🚀 Kurulum

1.  **Projeyi İndirin/Klonlayın:**
    ```
    git clone https://github.com/SENIN_KULLANICI_ADIN/trakt-to-letterboxd-migrator.git
    cd trakt-to-letterboxd-migrator
    ```

2.  **(Önerilir) Sanal Ortam Oluşturun ve Aktifleştirin:**
    ```
    python -m venv venv
    # Windows için:
    venv\Scripts\activate
    # macOS/Linux için:
    source venv/bin/activate
    ```

3.  **Gerekli Kütüphaneleri Yükleyin:**
    ```
    pip install -r requirements.txt
    ```

## 🔑 Trakt.tv API Ayarları

Script'in Trakt.tv hesabınıza erişebilmesi için bir API uygulaması oluşturmanız ve `Client ID` ile `Client Secret` almanız gerekmektedir.

1.  Trakt.tv API Uygulamaları sayfasına gidin: [https://trakt.tv/oauth/applications](https://trakt.tv/oauth/applications)
2.  "NEW APPLICATION" butonuna tıklayın.
3.  Aşağıdaki bilgileri girin:
    *   **Name:** Script'iniz için bir isim (örneğin, "Letterboxd Göçmen Kuşum" ya da daha ciddisi "Letterboxd Migrator").
    *   **Redirect URI:** `urn:ietf:wg:oauth:2.0:oob`
        *   Bu çok önemli! Bu URI, PIN tabanlı kimlik doğrulamayı mümkün kılar. Senin de keşfettiğin gibi, bu ayar sayesinde script, tarayıcıyı açıp seni bir sayfaya yönlendirmek yerine, doğrudan Trakt.tv'den bir PIN almanı ve bu PIN'i script'e girmeni sağlar.
    *   **Permissions:** Genellikle `/sync` (izleme geçmişi, puanlar için) ve `/users/settings` (liste erişimi için) yeterli olacaktır. Script'in ihtiyaç duyduğu minimum izinleri vermek en iyisidir. Şimdilik varsayılan izinler (`Default`) genellikle iş görür.
4.  Uygulamayı kaydedin. Size bir **Client ID** ve **Client Secret** verilecektir. Bu bilgileri SAKIN kimseyle paylaşmayın ve güvenli bir yerde saklayın. Script'i çalıştırırken bunlara ihtiyacınız olacak.

## 🛠️ Yapılandırma

Script (`trakt_to_letterboxd_migrator.py`) ilk çalıştırıldığında sizden `Client ID` ve `Client Secret` bilgilerinizi isteyecektir. Bu bilgiler, yerel olarak (örneğin bir `config.ini` dosyasına veya işletim sisteminin güvenli depolama alanına) kaydedilebilir, böylece her seferinde girmeniz gerekmez.
*Alternatif olarak, script'in içine doğrudan yazabilirsiniz ama bu güvenlik açısından önerilmez, özellikle kodu GitHub gibi platformlarda paylaşacaksanız.*

## 🏃‍♀️ Script'i Kullanma

1.  **Script'i Çalıştırın:**
    Terminal veya komut isteminde, script'in bulunduğu dizindeyken aşağıdaki komutu girin:
    ```
    python trakt_to_letterboxd_migrator.py
    ```

2.  **Trakt.tv Yetkilendirmesi (PIN ile):**
    *   Script sizden `Client ID` ve `Client Secret` bilgilerinizi isteyecektir (eğer daha önce kaydedilmemişse).
    *   Ardından, script size Trakt.tv'de uygulamanızı yetkilendirmeniz için bir URL verecek VEYA doğrudan Trakt.tv API uygulama sayfanızdaki (hani o "Authorize" butonu olan) adımları izleyerek 8 haneli PIN kodunu almanızı söyleyecektir.
        *   **Senin Yöntemin:** `https://trakt.tv/oauth/applications` adresindeki uygulamanın yanındaki "AUTHORIZE" butonuna tıkla. Açılan sayfada sana 8 haneli bir PIN kodu verilecektir.
    *   Bu **8 haneli PIN kodunu** script sizden istediğinde terminale girin ve Enter'a basın.

3.  **Veriler Çekiliyor ve CSV Oluşturuluyor:**
    *   PIN kodu doğrulandıktan sonra script, Trakt.tv hesabınızdan verileri çekmeye başlayacaktır. Bu işlem, kütüphanenizin büyüklüğüne bağlı olarak biraz zaman alabilir. Sabırlı olun, belki bir kahve molası? ☕
    *   İşlem tamamlandığında, aşağıdaki gibi CSV dosyaları script'in çalıştığı dizinde oluşturulacaktır:
        *   `letterboxd_watched_films.csv` (İzlenen filmler)
        *   `letterboxd_ratings.csv` (Puanlamalar)
        *   `letterboxd_watchlist.csv` (İzleme listenizdeki filmler)
        *   `letterboxd_list_[liste_adi].csv` (Her özel listeniz için ayrı bir dosya)

4.  **Letterboxd'ye Aktarma:**
    *   Letterboxd hesabınıza giriş yapın.
    *   Ayarlar (Settings) > Import & Export bölümüne gidin.
    *   "Import your data" seçeneğini kullanarak ilgili CSV dosyalarını yükleyin.
        *   İzlenen filmler için: `letterboxd_watched_films.csv`
        *   Puanlamalar için: `letterboxd_ratings.csv` (Genellikle izlenen filmlerle birlikte puanlar da aktarılır, Letterboxd'nin formatını kontrol edin.)
        *   İzleme listesi için: `letterboxd_watchlist.csv` (Letterboxd'de "Import watchlist" gibi özel bir seçenek olabilir.)
        *   Özel listeler için: Letterboxd'de yeni bir liste oluşturun ve "Import items" seçeneğiyle ilgili `letterboxd_list_[liste_adi].csv` dosyasını yükleyin.
    *   Letterboxd'nin eşleştirmelerini kontrol edin.

## 📄 CSV Formatı Hakkında (Letterboxd İçin)

Letterboxd genellikle aşağıdaki sütunları içeren CSV dosyalarını bekler:

*   **İzlenen Filmler İçin:**
    *   `Title` (Film Başlığı)
    *   `Year` (Film Yılı)
    *   `WatchedDate` (İzleme Tarihi - YYYY-MM-DD formatında)
    *   `Rating10` (Trakt puanı, Letterboxd bunu kendi 5'lik sistemine çevirir) veya `Rating` (0.5-5 arası)
    *   `imdbID` (IMDb ID'si - eşleştirme için çok faydalı)
    *   `tmdbID` (TMDb ID'si - eşleştirme için çok faydalı)
*   **Listeler/İzleme Listesi İçin:**
    *   `Title`
    *   `Year`
    *   `imdbID`
    *   `tmdbID`

Script, bu formatlara uygun çıktılar üretmeye çalışacaktır.

## 🤯 Sorun Giderme

*   **404 Hatası (PIN alma sayfasında):** Muhtemelen Trakt.tv API uygulamanızdaki `Redirect URI` yanlış ayarlanmıştır. `urn:ietf:wg:oauth:2.0:oob` olduğundan emin olun.
*   **PIN Kodu Çalışmıyor:** PIN kodunu doğru girdiğinizden emin olun. PIN kodları genellikle kısa süreliğine geçerlidir.
*   **API Anahtarı Hatası:** `Client ID` veya `Client Secret` bilgilerinizi yanlış girmiş olabilirsiniz.
*   **"Rate Limit Exceeded" Hatası:** Trakt.tv API'sini kısa sürede çok fazla sorguladınız. Biraz bekleyip tekrar deneyin.

Eğer bir sorunla karşılaşırsan, GitHub'da bir "Issue" açmaktan çekinme!

## 🤝 Katkıda Bulunma

Bu projeye katkıda bulunmak istersen harika olur! Hata düzeltmeleri, yeni özellikler, dokümantasyon iyileştirmeleri... Her türlü katkıya açığım.
1.  Projeyi fork'la.
2.  Yeni bir branch oluştur (`git checkout -b ozellik/harika-bir-ozellik`).
3.  Değişikliklerini commit'le (`git commit -am 'Harika bir özellik eklendi'`).
4.  Branch'ini push'la (`git push origin ozellik/harika-bir-ozellik`).
5.  Bir Pull Request aç.

## 📜 Lisans

Bu proje MIT Lisansı altında lisanslanmıştır. Detaylar için `LICENSE` dosyasına bakınız.

---
*Biraz da teknoloji ve nostalji tutkunu bir öğrencinin iç sesi: Bu script'i hazırlarken aklıma hep o eski BBS'lere bağlanıp dosya indirdiğimiz günler geldi. Şimdi API'ler, OAuth'lar falan... Teknoloji ne kadar değişse de bir şeyleri alıp başka bir yere "taşımak", verilerimizi kontrol etmek hala aynı heyecanı veriyor. Umarım bu script senin de işini görür ve Letterboxd macerana güzel bir başlangıç yapmanı sağlar!* 🤘
