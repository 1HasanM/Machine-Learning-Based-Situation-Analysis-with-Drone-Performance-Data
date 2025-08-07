Drone Performans Verileriyle Makine Öğrenmesi Tabanlı Durum Analizi Projesi Raporu
1. Proje Amacı
Bu projenin temel amacı, insansız hava araçlarının (İHA) operasyonlarından elde edilen çevresel, hareket ve iletişim verilerini kullanarak, İHA'nın performans durumunun "Düşük" mü yoksa "Orta Düzey" mi olacağını tahmin eden bir makine öğrenmesi modeli geliştirmektir. Projenin en büyük zorluğu, "Orta Düzey Performans" durumlarının, veri setinde çok nadir bulunmasıydı (veri dengesizliği). Bu nedenle, projenin başarısı, nadir görülen bu durumu doğru bir şekilde tespit etme yeteneğine (recall değeri) odaklanmıştır.

2. Uygulanan Yöntemler ve Aşamalar
Proje, verinin keşfedilmesinden, en uygun modelleme stratejisinin bulunmasına kadar üç ana aşamada ilerlemiştir.

2.1. Aşama: Veri Keşfi, Ön Hazırlık ve Özellik Mühendisliği

Ne yaptık? Projeye, drone_data.csv adlı veri setinin yüklenmesiyle başlandı. Keşifsel veri analizi (EDA) ile Movement Pattern ve Weather gibi kategorik özelliklerin dağılımları ve Energy Consumption ile Throughput gibi sayısal özelliklerin Performance Target ile ilişkisi incelendi. Ardından, Latency_Improvement ve Energy_Efficiency_Index gibi üç yeni ve anlamlı özellik oluşturularak modelin öğrenme potansiyeli artırıldı. Son olarak, kategorik veriler One-Hot Encoding ile, sayısal veriler StandardScaler ile dönüştürülerek modellemeye hazır hale getirildi.

Ne öğrendik? Performance Target sınıfının (Low Performance vs. Moderate Performance) aşırı dengesiz olduğu (86'ya 14 oranı) ve Moderate Performance durumunun, daha düşük enerji tüketimi ve daha yüksek veri akışı hızlarıyla ilişkili olduğu tespit edildi.

2.2. Aşama: Temel Modelleme ve Sorun Tespiti

Ne yaptık? Hazırlanan veri seti üzerinde Logistic Regression, Random Forest, Gradient Boosting dahil olmak üzere birçok temel makine öğrenmesi modeli eğitildi.

Sonuçlar: Bu modeller, dengesiz verinin yarattığı klasik problemle karşılaştı. Random Forest ve Gradient Boosting gibi en iyi modeller bile, Moderate Performance sınıfında sadece 0.67 gibi mütevazı bir recall değeri elde etti. En zayıf model olan Support Vector Machine ise bu sınıfı hiç tespit edemedi (recall=0.00). Bu, dengesizlik sorununun modelin performansını doğrudan etkilediğini bir kez daha kanıtladı.

2.3. Aşama: Dengeleme Stratejisinin Uygulanması ve İyileştirme 

Ne yaptık? Projenin en kritik adımı olarak, veri dengesizliğini doğrudan gidermek için SMOTE (Synthetic Minority Over-sampling Technique) yöntemi uygulandı. SMOTE ile eğitim verisindeki Moderate Performance örneklerinin sayısı, baskın olan Low Performance sınıfıyla eşitlendi. Ardından, en iyi performans gösteren modeller bu dengelenmiş veri seti üzerinde yeniden eğitildi.

Sonuçlar: SMOTE stratejisi, model performansında önemli bir sıçrama yarattı:

K-Nearest Neighbors modeli, recall değerini 0.33'ten 1.00'e (tüm Moderate Performance örneklerini doğru tahmin etti) çıkararak en yüksek performansı gösterdi.

Logistic Regression ve Gradient Boosting modelleri, 0.67 olan recall değerlerini korurken, precision değerlerini de 0.67 seviyesine taşıyarak daha dengeli ve güvenilir tahminler yapabildiklerini gösterdi.

Özet: SMOTE yöntemi, modellerin nadir görülen durumu çok daha etkili bir şekilde öğrenmesini sağlayarak projenin ana hedefine ulaşmasını sağladı.

3. Proje Sonuçları ve Değerlendirme
Bu projenin en büyük başarısı, doğru problemin tanımlanması ve buna uygun stratejinin (SMOTE) uygulanması olmuştur. Sonuçta elde ettiğimiz modeller, başlangıçtaki basit modellere göre çok daha güvenilir ve işlevseldir.

Finalde elde edilen en iyi modeller, Moderate Performance durumunu aşağıdaki başarı oranlarıyla tespit edebilmektedir:

K-Nearest Neighbors (SMOTE): %100 Recall (Tüm orta düzey performans durumlarını tespit etti)

Logistic Regression (SMOTE): %67 Recall ve %67 Precision (Yüksek F1-score ile dengeli performans)

Gradient Boosting (SMOTE): %67 Recall ve %67 Precision (Yüksek F1-score ile dengeli performans)
