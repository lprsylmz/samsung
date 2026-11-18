# Samsung ML-2165 macOS (Apple Silicon & Intel) Sürücüsü

> [!WARNING]
> ### Yasal Uyarı ve Sorumluluk Reddi (Disclaimer)
> Bu sürücü paketi, yapılandırma ve ilgili kurulum dosyaları bir yapay zeka asistanı (**Antigravity**) tarafından oluşturulmuş ve derlenmiştir. Bu projenin sahibi/kullanıcısı, yazılımın kullanımından doğabilecek herhangi bir donanımsal veya yazılımsal arıza, veri kaybı veya uyumsuzluktan dolayı **hiçbir hukuki ve cezai sorumluluk kabul etmemektedir**. Bu yazılım "olduğu gibi" (AS IS) sunulmakta olup, tüm kullanım riski ve sorumluluğu tamamen son kullanıcıya aittir.

Bu paket, **Samsung ML-2165** (ve ML-2160 serisi) yazıcıların modern macOS ve Apple Silicon (M1/M2/M3/M4) cihazlarda çalışmasını sağlayan yerel ARM64 destekli CUPS sürücüsünü içerir.

## Sorun Neydi?
Samsung ve HP'nin resmi eski sürücüleri Intel tabanlı 32-bit/64-bit ikili dosyalar (`rastertosec`) kullanıyordu. Modern macOS sürümlerinde (özellikle Apple Silicon üzerinde) bu filtreler çalışmadığı için *"The printer software is not compatible with this device"* veya *"Bad CPU type in executable"* hatası vermekteydi. Ayrıca bu yazıcılar saf SPL/QPDL protokolü kullandığı için standart AirPrint veya Generic PCL sürücüleriyle çalışmaz.

## Çözüm
Açık kaynaklı **SpliX (QPDL/SPL)** sürücüsü Apple Silicon (ARM64) mimarisine uygun şekilde derlenmiş ve macOS CUPS sistemine entegre edilmiştir. Rosetta 2 gerektirmez.

---

## Kurulum Adımları

### 1. Yöntem: PKG Yükleyicisi ile Kurulum (En Kolayı)
1. Bu dizinde oluşturulan `Samsung_ML-2165_macOS_Driver.pkg` dosyasına çift tıklayarak yükleyiciyi açın.
2. Ekrandaki talimatları takip edip yönetici şifrenizi girerek kurulumu tamamlayın.
3. Yazıcınızı USB kablosuyla bağlayın (veya ağdaysa açık olduğundan emin olun).
4. **Sistem Ayarları (System Settings) > Yazıcılar ve Tarayıcılar (Printers & Scanners)** bölümüne gidin.
5. **+ (Yazıcı Ekle)** butonuna tıklayın.
6. Listeden Samsung ML-2165'i seçin.
7. Alt kısımdaki **Kullan (Use)** açılır menüsünden **Yazılım Seç... (Select Software...)** seçeneğine tıklayın.
8. Arama kutusuna `ML-2165` yazın ve **Samsung ML-2165, 2.0.0** profilini seçip **Ekle**'ye basın.

---

### 2. Yöntem: Terminal / Otomatik Kurulum
Yazıcınız bilgisayara bağlı ve açıkken Terminal üzerinden doğrudan kuyruk oluşturmak için:

```bash
cd /Users/lpr/Herd/samsung/splix-macos-arm64
sudo ./install.sh ml2165
```

Bu komut:
1. `rastertoqpdl` filtresini `/Library/Printers/SpliX/Filters/` dizinine yerleştirir.
2. PPD dosyasını sistem kaynaklarına ekler.
3. Bağlı yazıcıyı otomatik algılayıp `ML2165` adıyla CUPS yazdırma kuyruğunu oluşturur.

---

## Kaldırma (Uninstall)
Sürücüyü kaldırmak isterseniz:
```bash
sudo rm -rf /Library/Printers/SpliX
sudo rm -f /Library/Printers/PPDs/Contents/Resources/SpliX-*.ppd
```
