# Maya Shelf Sync (2020-2026)

Bu, Windows üzerinde çalışan interaktif bir batch (toplu) script’tir.  
Maya’nın yüklü sürümünden diğer sürümlere **shelf** ve **ikon** klasörlerini senkronize etmek için kullanılır (2020–2026 arası).
Eğer Farklı scriptleriniz çalışmaz ise Documents\maya\2024- Orjinal dosyanız ise içindeki Mmaya.env dosyasını 
diğer sürümlerin içine kopyalayınız ve 2024 üstünde calıştığınız tüm script ve pluginler otomatik calışacak çalışmayanları ise Windows/Settings/Plug-in Manager içinde açabilirsiniz

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
