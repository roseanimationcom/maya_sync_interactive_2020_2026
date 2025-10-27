# Maya Shelf Sync (2020-2026)

Bu, Windows üzerinde çalışan interaktif bir batch (toplu) script’tir.
Maya’nın yüklü sürümünden diğer sürümlere shelf ve ikon klasörlerini senkronize eder (2020–2026 arası).

Eğer bazı scriptler veya pluginler çalışmazsa:

Kaynak sürümünüzün (ör. 2024) Documents\maya\2024 klasöründeki maya.env dosyasını diğer sürümlere kopyalayın.

Bu sayede 2024’te kullandığınız tüm script ve pluginler diğer sürümlerde de çalışacaktır.

Hâlâ çalışmayan pluginler varsa, Windows → Settings → Plug-in Manager üzerinden etkinleştirebilirsiniz.

## Özellikler
- Kullanıcının bilgisayarında hangi Maya sürümünün yüklü olduğunu sorar  
- Mevcut hedef klasörleri otomatik olarak yedekler  
- Shelf ve ikon klasörleri için sembolik link oluşturur  
- Kurulu Maya sürümlerinin herhangi kombinasyonu ile çalışır  

## Kullanım
1. Batch dosyasını indirin: `maya_sync_interactive_2020_2026.bat`  
2. Sağ tık → **Yönetici olarak çalıştır**  
3. CMD ekranında yüklü Maya sürümünü seçin  
4. Script diğer tüm sürümlere shelf ve ikonları senkronize eder  

## Yapılandırma
- `SYNC_SHELVES=1` → Shelf’leri senkronize et  
- `SYNC_ICONS=1` → İkonları senkronize et (0 yaparsanız ikonlar kopyalanmaz)  
