# Kaggle link: https://www.kaggle.com/code/nisanuraydodu/notebook88bad783d8
# Wine Rewievs Analiz

Bu analizde, şarap incelemeleri veri kümesi üzerinde çeşitli adımlar gerçekleştirilmiştir. İlk olarak, veriye genel bir bakış sağlanarak sütunlar ve veri tipleri incelenmiştir. Ardından, eksik veriler tespit edilerek, bu eksikliklerin silinmesi ya da uygun doldurma yöntemleriyle giderilmesi için işlemler yapılmıştır. Kategorik değişkenler (ülke, şarap türü, bölge vb.) detaylı bir şekilde analiz edilip görselleştirilmiştir, böylece şarapların dağılımı ve bölgesel farklılıklar ortaya konmuştur. Ayrıca, kategorik veriler kullanılarak gruplama işlemleri yapılmış ve ülkeler ile şarap türleri arasındaki ortalama puan ve fiyat farkları hesaplanmıştır. Aykırı değer analizi ise, fiyatlar ve puanlar gibi sayısal değişkenlerde boxplot ve z-skoru gibi yöntemlerle gerçekleştirilmiş, veri setindeki olağan dışı gözlemler belirlenmiştir. Bu adımlar, veri kümesinin temizlenmesi ve düzenlenmesi sürecini tamamlayarak, daha ileri analizler ve modelleme için uygun bir temel oluşturmuştur.



# 1. Veri Kümesine İlk Bakış

Veri kümesi, 150.930 satır ve 11 sütundan oluşmaktadır. Şarapların özellikleri, farklı kategoriler altında toplanmış bilgiler sunmaktadır. Veriye genel bir bakış şu şekildedir:

Unnamed: 0: Satır numarası (index).

country: Şarabın üretildiği ülke.

description: Şarap hakkında açıklamalar ve tadım notları.

designation: Şarap markası veya özel etiket adı.

points: Şarap puanı (genellikle 0 ile 100 arasında).

price: Şarap fiyatı.

province: Şarap üretiminin yapıldığı eyalet veya bölge.

region_1: Daha geniş coğrafi bölge (örneğin, şarap bölgesi).

region_2: Şarap üretiminin daha spesifik alt bölgesi.

variety: Şarap türü (örneğin, Cabernet Sauvignon, Merlot vb.).

winery: Şarap üreticisi.

# Veri Tipleri

Sayısal veriler: points, price

Kategorik veriler: country, description, designation, province, region_1, region_2, variety, winery

# 2. Kullanılan Kütüphaneler

   Pandas
Kullanım Amacı: Veri işleme ve analiz için.
Pandas, veri kümesini yüklemek, işlemek, analiz etmek ve temizlemek için yaygın olarak kullanılan bir kütüphanedir. DataFrame yapısı sayesinde veri üzerinde satır ve sütun bazında kolayca işlemler yapmamızı sağlar. Bu projede, veriyi okuma, sütunlar üzerinde işlem yapma (eksik veri kontrolü, gruplama, filtreleme) ve analiz sonuçlarını toplama için kullanılmıştır.

Matplotlib
Kullanım Amacı: Veri görselleştirme için.
Matplotlib, özellikle grafikler ve görselleştirmeler oluşturmak için kullanılan temel bir Python kütüphanesidir. Bu projede, verilerin görselleştirilmesi için kullanılmıştır. Örneğin, şarap fiyatlarının ve puanlarının dağılımını görselleştirmek için histogramlar ve boxplot’lar oluşturulmuştur.

Seaborn
Kullanım Amacı: Daha estetik ve gelişmiş görselleştirmeler için.
Seaborn, Matplotlib üzerine inşa edilen ve veri görselleştirme sürecini kolaylaştıran bir kütüphanedir. Estetik olarak daha şık grafikler oluşturulmasına olanak tanır. Bu projede, kategorik veriler için bar plot'lar, sayısal veriler için scatter plot’lar ve boxplot’lar kullanılmıştır. Ayrıca, Seaborn ile oluşturulan grafikler daha kolay anlaşılır ve yorumlanabilir olmuştur.

Missingno
Kullanım Amacı: Eksik veri analizi ve görselleştirmesi için.
Missingno, eksik verileri analiz etmek ve görselleştirmek için özel olarak tasarlanmış bir kütüphanedir. Veri kümesindeki eksik değerlerin hangi sütunlarda yoğunlaştığını ve bu eksik değerlerin genel veriye nasıl etki ettiğini görselleştirmemizi sağlar. Bu projede, eksik verilerin görselleştirilmesi ve yönetilmesi için kullanılmıştır. Özellikle veri kümesindeki eksikliklerin belirlenmesi ve eksik değerlerin doldurulması veya silinmesi işlemleri öncesinde kullanılmıştır.

# Problem: Bir Şarap Mağazasının Stok Yönetimi ve Müşteri Tercihlerini Anlamak
Hikaye:

Bir şarap mağazası sahibi olan Selim Bey, mağazasının performansını artırmayı hedefliyor. Yıllardır çeşitli şaraplar satıyor, ancak hangi şarapların daha çok satıldığını, müşterilerinin hangi tür şarapları tercih ettiğini ve fiyatlandırma stratejisinin ne kadar etkili olduğunu tam olarak bilmiyor. Ayrıca, şaraplarını daha verimli bir şekilde stoklamak ve müşteri taleplerine göre nasıl bir çeşitlilik sunması gerektiği konusunda kararsız. Selim Bey, aynı zamanda şaraplarını hangi bölgesel pazarlara yönlendireceği, hangi fiyat aralıklarında daha fazla talep göreceği ve hangi şarap türlerinin daha yüksek puan aldığını da öğrenmek istiyor.

Bir gün, bu sorunları çözmek için şarap incelemeleri veri kümesini kullanarak bir analiz yapmaya karar verir. Bu veri kümesi, dünyanın dört bir yanındaki şaraplar hakkında kullanıcıların verdiği puanlar, açıklamalar, fiyatlar ve şarap türleri gibi bilgiler içeriyor.

## Analiz Süreci ve Çözüm:

Selim Bey, şarap mağazasındaki stok yönetimi ve müşteri tercihlerini optimize etmek için analiz yaptı. İlk olarak, şarap türleri ve ülkeler arasındaki popülerlikleri inceledi; Fransa ve İtalya’dan gelen şaraplar ile Cabernet Sauvignon türünün daha yüksek puanlar aldığını keşfetti. Ayrıca, şarap fiyatları ile puanlar arasındaki ilişkiyi inceleyerek, daha pahalı şarapların genellikle daha yüksek puan aldığını ancak bazı düşük fiyatlı şarapların da yüksek puanlar alabildiğini öğrendi. Bölgesel analizde, Kaliforniya'da Zinfandel türü şarapların popüler olduğunu ve Fransa'da Bordeaux şaraplarının fiyatlarının daha yüksek olduğunu fark etti. Aykırı değer analizi, fiyatlandırma stratejisini iyileştirmesine yardımcı oldu. Bu analizler, Selim Bey’in stoklama, fiyatlandırma ve şarap çeşitlendirme kararlarını daha verimli hale getirdi ve mağazanın satışlarını artırmaya yönelik stratejiler geliştirmesini sağladı.



