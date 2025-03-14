# Sürücüsüz Metro Simülasyonu (Rota Optimizasyonu )

Bu proje, bir metro ağı üzerinde iki istasyon arasındaki en hızlı ve en az aktarma içeren rotaları bulmayı amaçlayan bir simülasyon geliştirmektedir.


## Kullanılan Teknolojiler ve Kütüphaneler

Projede kullanılan temel Python kütüphaneleri:

* **heapq:** Öncelik kuyruğu yapısı için kullanılmıştır. En hızlı rota hesaplamasında A* algoritmasının verimli çalışmasını sağlar.

* **collections.deque:** BFS (Breadth-First Search) algoritmasında kuyruğa hızlı erişim ve işlem yapmak için çift uçlu kuyruk (deque) kullanılmıştır.

* **defaultdict:** Metro ağı verisini yönetirken varsayılan listeler oluşturmak için kullanılmıştır.
* **math:** Öklid mesafesi hesaplamaları için

## Algoritmaların Çalışma Mantığı
Projede iki farklı algoritma kullanılmıştır:

### BFS (Breadth-First Search) - En Az Aktarma İçin

* Genişlik öncelikli arama (BFS) kullanılarak en az aktarma yapan rota hesaplanır.
* BFS, istasyonları katman katman ziyaret eder, önce en kısa aktarmalı yolları keşfeder.
* Aktarma noktaları önceliklidir, çünkü hat değiştirme maliyetini azaltmak esastır.
* BFS algoritmasının  O(b **m) time complexitye sahiptir.
* space complexity  O(b**d) sahiptir.
* ugulaması FIFO(first ın first out) (queue).
* optimal(en az maliyetle sonuca ulaşmak) ve complete(problemin çözüm sonucuna ulaşmak) dir.
(m = maximum depth of the state space b =  branching factor d = depth of the solution)

**Neden BFS?**
* Çünkü en az duraklı veya en az aktarmalı yolu bulmak için idealdir.
* En kısa sürede bulunan ilk geçerli çözüm en iyisidir.

### A* Algoritması - En Hızlı Rota İçin

* A* algoritması, bir başlangıç noktasından hedefe en kısa sürede ulaşmak için kullanılır.
* Kullanılan heuristic fonksiyonu istasyonlar arasındaki düz çizgi mesafeyi hesaplar.
* Algoritma, toplam geçen süreyi ve tahmini maliyeti göz önüne alarak optimal rota bulur.

**Neden A*?**
* BFS sadece istasyon sayısını minimize ederken, A* algoritması aktarma sürelerini de hesaba katarak en hızlı rotayı belirler.
* İstasyonlar arasındaki tahmini mesafeyi kullanarak en kısa sürede hedefe ulaşmayı amaçlar.


### Örnek Kullanım

    metro = MetroAgi()
    
    # İstasyonlar ekleme
    # Kırmızı Hat
    metro.istasyon_ekle("K1", "Kızılay", "Kırmızı Hat")
    metro.istasyon_ekle("K2", "Ulus", "Kırmızı Hat")
    metro.istasyon_ekle("K3", "Demetevler", "Kırmızı Hat")
    metro.istasyon_ekle("K4", "OSB", "Kırmızı Hat")
    
    # Mavi Hat
    metro.istasyon_ekle("M1", "AŞTİ", "Mavi Hat")
    metro.istasyon_ekle("M2", "Kızılay", "Mavi Hat")  # Aktarma noktası
    metro.istasyon_ekle("M3", "Sıhhiye", "Mavi Hat")
    metro.istasyon_ekle("M4", "Gar", "Mavi Hat")
    
    # Turuncu Hat
    metro.istasyon_ekle("T1", "Batıkent", "Turuncu Hat")
    metro.istasyon_ekle("T2", "Demetevler", "Turuncu Hat")  # Aktarma noktası
    metro.istasyon_ekle("T3", "Gar", "Turuncu Hat")  # Aktarma noktası
    metro.istasyon_ekle("T4", "Keçiören", "Turuncu Hat")
    
    # Bağlantılar ekleme
    # Kırmızı Hat bağlantıları
    metro.baglanti_ekle("K1", "K2", 4)  # Kızılay -> Ulus
    metro.baglanti_ekle("K2", "K3", 6)  # Ulus -> Demetevler
    metro.baglanti_ekle("K3", "K4", 8)  # Demetevler -> OSB
    
    # Mavi Hat bağlantıları
    metro.baglanti_ekle("M1", "M2", 5)  # AŞTİ -> Kızılay
    metro.baglanti_ekle("M2", "M3", 3)  # Kızılay -> Sıhhiye
    metro.baglanti_ekle("M3", "M4", 4)  # Sıhhiye -> Gar
    
    # Turuncu Hat bağlantıları
    metro.baglanti_ekle("T1", "T2", 7)  # Batıkent -> Demetevler
    metro.baglanti_ekle("T2", "T3", 9)  # Demetevler -> Gar
    metro.baglanti_ekle("T3", "T4", 5)  # Gar -> Keçiören
    
    # Hat aktarma bağlantıları (aynı istasyon farklı hatlar)
    metro.baglanti_ekle("K1", "M2", 2)  # Kızılay aktarma
    metro.baglanti_ekle("K3", "T2", 3)  # Demetevler aktarma
    metro.baglanti_ekle("M4", "T3", 2)  # Gar aktarma
    
    # Test senaryoları
    print("\n=== Test Senaryoları ===")
    
    # Senaryo 1: AŞTİ'den OSB'ye
    print("\n1. AŞTİ'den OSB'ye:")
    rota = metro.en_az_aktarma_bul("M1", "K4")
    if rota:
        print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))
    
    sonuc = metro.en_hizli_rota_bul("M1", "K4")
    if sonuc:
        rota, sure = sonuc
        print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))
    
    # Senaryo 2: Batıkent'ten Keçiören'e
    print("\n2. Batıkent'ten Keçiören'e:")
    rota = metro.en_az_aktarma_bul("T1", "T4")
    if rota:
        print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))
    
    sonuc = metro.en_hizli_rota_bul("T1", "T4")
    if sonuc:
        rota, sure = sonuc
        print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota))
    
    # Senaryo 3: Keçiören'den AŞTİ'ye
    print("\n3. Keçiören'den AŞTİ'ye:")
    rota = metro.en_az_aktarma_bul("T4", "M1")
    if rota:
        print("En az aktarmalı rota:", " -> ".join(i.ad for i in rota))
    
    sonuc = metro.en_hizli_rota_bul("T4", "M1")
    if sonuc:
        rota, sure = sonuc
        print(f"En hızlı rota ({sure} dakika):", " -> ".join(i.ad for i in rota)) 

### Geliştirme Fikirleri

* Gerçek zamanlı trafik bilgisi ekleme
* Mobil uygulama entegrasyonu
* Harita desteği ve görselleştirme
* API Entegrasyonu 
