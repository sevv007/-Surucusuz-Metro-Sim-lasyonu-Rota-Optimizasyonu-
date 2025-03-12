# -Sürücüsüz Metro Sim lasyonu (Rota Optimizasyonu )

Bu proje, bir metro ağı üzerinde iki istasyon arasındaki en hızlı ve en az aktarma içeren rotaları bulmayı amaçlayan bir simülasyon geliştirmektedir.


## Kullanılan Teknolojiler ve Kütüphaneler

Projede kullanılan temel Python kütüphaneleri:

* **heapq:** Öncelik kuyruğu yapısı için kullanılmıştır. En hızlı rota hesaplamasında A* algoritmasının verimli çalışmasını sağlar.

* **collections.deque:** BFS (Breadth-First Search) algoritmasında kuyruğa hızlı erişim ve işlem yapmak için çift uçlu kuyruk (deque) kullanılmıştır.

* **defaultdict:** Metro ağı verisini yönetirken varsayılan listeler oluşturmak için kullanılmıştır.

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

* Kuyruk (Heap) ile çalışan A* algoritması kullanılmıştır.
* Her adımda en düşük toplam süreye sahip rota seçilir.
* Güncellenmiş maliyet tahmini ile en kısa sürede hedefe ulaşan yol hesaplanır.
* Algoritma, her istasyon için önceki en kısa süreden daha hızlı bir yol bulursa rotayı günceller.
* A* algoritmasının zaman karmaşıklığı O(b log(m))’dir.

**Neden A*?**
* Çünkü en az sürede gidilen rotayı bulmak için en uygundur.
* Önceliklendirme ile gereksiz düğümleri işlememeyi sağlar.

