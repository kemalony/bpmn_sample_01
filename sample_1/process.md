# İnşaat Hakediş Kontrol ve Onay Süreci

Bu paket, SpiffWorkflow üzerinde çalıştırılmak üzere hazırlanmış yürütülebilir BPMN 2.0 hakediş onay sürecini içerir.

## Ana Dosyalar

- `process_model.json`: Süreç kimliği, birincil BPMN dosyası ve takip metadata alanları.
- `hakedis_onay_sureci.bpmn`: Yürütülebilir BPMN akışı ve SpiffWorkflow form bağlantıları.
- `hakedis_giris.json`: Saha hakediş girişi formu.
- `hakedis_giris.ui.json`: Saha hakediş girişi görünüm ayarları.
- `hse_kontrol.json`: İSG / HSE kontrol formu.
- `hse_kontrol.ui.json`: İSG / HSE kontrol görünüm ayarları.
- `pm_onay.json`: Proje müdürü teknik onay formu.
- `pm_onay.ui.json`: Proje müdürü teknik onay görünüm ayarları.
- `finans_kontrol.json`: Finans ön kontrol formu.
- `finans_kontrol.ui.json`: Finans ön kontrol görünüm ayarları.
- `gm_onay.json`: GM / CFO final onay formu.
- `gm_onay.ui.json`: GM / CFO final onay görünüm ayarları.

## Akış

1. Saha mühendisi hakediş no, sözleşme no, proje, taşeron, dönem, iş kapsamı, gerçekleşme oranı, talep tutarı ve destek dosyalarını girer.
2. İSG uzmanı denetim tarihi, risk seviyesi, ihlal durumu, açık aksiyon sayısı, kesinti tutarı ve HSE notlarını kaydeder.
3. Proje müdürü teknik uygunluk, metraj uygunluğu ve revize hakediş tutarı üzerinden onay veya red kararı verir.
4. Finans ekibi fatura, cari hesap mutabakatı, vergi borcu yoktur belgesi, yasal kesintiler, KDV, net ödeme tutarı ve önerilen ödeme tarihini kontrol eder.
5. GM / CFO final onay tutarı, ödeme önceliği ve nihai gerekçe ile süreci onaylar veya reddeder.

## Karar Mantığı

- `pm_onay_durum == False` ise süreç `Reddedildi` olarak kapanır.
- `finans_onay_durum == False` ise süreç `Reddedildi` olarak kapanır.
- `gm_onay_durum == True` ise süreç `Onaylandı` olarak kapanır.
- `gm_onay_durum == False` ise süreç `Reddedildi` olarak kapanır.

## SpiffWorkflow Notları

Tüm kullanıcı görevlerinde `spiffworkflow:properties` altında `formJsonSchemaFilename` ve `formUiSchemaFilename` tanımlıdır. Form ve UI schema dosyaları Spiff Arena GitHub import akışıyla uyumlu olması için BPMN ile aynı dizinde tutulur. BPMN içindeki kullanıcı talimatları, sonraki onay adımlarında önceki aşama verilerini özet olarak gösterir.
