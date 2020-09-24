---
title: "Fonksiyonel Programlamanın Önemi"
date: 2017-04-05
draft: true
---

Benim gibi neolotik çağdan kalmaysanız ilk bilgisayarınızın kapasitesi muhtemelen MB ile temsil edilen depolama birimleri ve Mhz cinsinden işlem ünitelerinden meydana geliyordur. Örneğin benimki, 32 MB RAMli, 2 GB HDDli, Intel Pentium 200MMX işlemciliydi. Windows 95 işletim sistemi vardı. "Çok çekirdek" o dönem bakkaldan alınan bir şeydi. Bu şartlardaki gereksinimleriniz de haliyle elinizdeki donanımı sonuna kadar kullanabilecek, ona hükmedecek yazılım mimarileri kullanmak olacaktır. Lisp gibi matematiksel soyutlama üzerine geliştirilmiş fonksiyonel programlama paradigması da haliyle kimsenin ilgisini çekmiyordu. Donanıma daha yakın olan C gibi imperatif diller endüstri standardı olarak kabul görüyordu. Onlar da zamanla yerlerini nesne yönelimli (object oriented) dillere bıraktılar. Fakat donanımın, yazılım kavramlarının yavaş gelişimi karşısında ışık hızında gelişmeye devam etmesi (bkz. Moore yasası) oyunun kurallarını değiştirdi. Her ne kadar yazılım teknolojileri gelişmeye devam etse de, üzerine oturdukları temel kavramlar hiç değişmedi. Çok çekirdekli sistemler, bulut mimarileri, devasa veriler vs. artık yazılımcıları farklı bir perspektiften bakmaya zorluyor.

![Erlang quicksort örneği](/img/erlang-quicksort.png "Erlang quicksort örneği")

Günümüzde kullanılan modern bilgisayarların en az 4 çekirdeği var. İşletim sistemi iş yükünü çekirdeklere eşit şekilde dağıtmaya çalışıyor. Fakat uygulamalardan hepsi birden fazla çekirdekte çalışacak şekilde geliştirilmemiş. Bu yüzden yeterli performansı sağlayamıyorlar. Daha iki çekirdeği doğru dürüst kullanamazken gelecek nesil sistemlerle nasıl başa çıkabiliriz ki?

Bir uygulamanın birden fazla çekirdeği kullanabilmesi için, threadler yardımıyla işlemleri bölerek birden fazla çekirdeğe dağıtmak gerekiyor. Çoğu zaman veri kaybını önlemek için threadlerin senkronize edilmesi zorunlu hale gelir. Bu senkronizasyonlar karmaşık yazılım mimarileri oluşturulmasına neden olur. Tabi ki karmaşıklık oranı artınca hata oranı da artar. Daha da kötüsü çoklu threadler hata ayıklamayı da bir hayli zorlaştırabilir.

Java, C++, C#, Python, PHP, JavaScript gibi kullandığımız imperatif diller thread senkronizasyonu haricinde başka sorunlara da sebebiyet verirler. Bunlardan en barizi, hatta fonksiyonel açıdan bakacak olursak bütün kötülüklerin anası diyebileceğimiz, mutability yani değiştirilebilir veri yapısıdır. İmperatif diller hafızada yer alan değerlerin değişmesi üzerine kuruludur. Uygulamanın doğrudan hafızaya erişip değişiklik yapabilme hakkı, uygulamanın sahip olduğu durumun değişmesine sebep olmaktadır. Hatalı bir sonlanma sonucunda bu değerlere bağlı olan threadler de sorun yaşayabilecektir.

![Rust quicksort örneği](/img/rust-quicksort.png "Rust quicksort örneği")

Başka bir sorun ise yan etkilerdir. Fonksiyonel dillerde, özellikle de saf fonksiyonel dillerde bu yapıların değiştirilemez olması nedeniyle size o fonksiyonun istenilen işlemi yapmak dışında bir ek iş yapmayacağını garanti eder. Örneğin globalde bir g değişkeinmiz olduğunu düşünelim. Verilen iki sayıyı toplayan bir fonksiyonumuz olsun. İmperatif dillerde bu iki sayının toplamının sonucu bize verilirken, diğer yandan g değişkeninin içeriğini değiştirmek mümkündür. Buna yan etki deniyor. Fonksiyonel diller, matematiksel modelin üzerine oturduğu için değer alıp değer döndürmek dışında bir işlem yapmaktan sizi alıkoymaktadır. Dolayısıyla yan etki olmaz. Uygulamanın karmaşıklığı azalır ve daha az hata yapmaya yönlendirilirsiniz.

Hafıza paylaşımını da unutmamak gerek. Ee şey.. Bu da aslında temelde bir mutability yani değiştirilebilir veri yapısı ile alakalıdır. Belli bir hafıza bölgesine tahsis edilen değişken içindeki değerde yani o bölgede yapılan değişiklik aynı anda birden fazla threadin buraya erişmeye çalışmasıyla sorun haline gelebilir. Bunu önlemek için locklar, mutexler, dining philosopherlar ve bilimum çözümle takla attırmanız gerekecek. Hangi thread ne şekilde çalışacak? Kime ne verilecek? Kaynaklar nasıl paylaştırılacak? Gibi sorulara çözüm ararken işlerin daha karmaşık hale gelmesi kaçınılmaz oluyor. Haliyle daha fazla bug.

Şimdi bundan 10–20 yıl sonrasını hayal edelim. Muhtemelen elimizde binlerce çekirdek, ihtiyaca göre istenildiği kadar yatay büyüme gerçekleştirebilecek ve birçok farklı cihazın haberleşeceği mimariler olacaktır. İmperatif paradigma ile bütün bunları nasıl verimli kullanabiliriz? Thread senkronizasyonunu tek tek ele alırsak bunu başaramayız. Yazılımcı olarak geleceği nasıl şekillendireceğimiz buna vereceğimiz cevaptadır.

Bu sorunun cevabı fonksiyonel paradigmadır. Paradigma diyorum çünkü fonksiyonel programlamaya geçiş yapmanız için öncelikle problemlere çözüm üretmedeki bakış açınızı değiştirmeniz gerekiyor. Ardışık komutlar içeren imperatif paradigmadan kurtulup, çözümü bütüncül, soyut ve matematiksel bir model olarak ele alan fonksiyonel paradigmaya geçmeniz gerekiyor. Tabi ki bu bir anda olabilecek bir şey değil. Bu bir süreç ve zaman alacaktır.

Fonksiyonel dillerin bazı özellikleri şöyledir:

1. Fonksiyonlar parametre değerleri aynı olduğu sürece hep aynı sonucu döndürürler.
2. Parametreler pass bu value yöntemi ile verilir. Bu parametreler fonksiyon tarafından değiştirilemezler ve her yeni değer için yeni bir değişken oluştururlar.
3. Fonksiyonlar kendi bünyeleri dışında hiç bir veri ya da durum üzerinde değişiklik yapamazlar. Bu yüzden fonksiyonların yan etkileri yoktur.
4. Saf fonksiyonel dillerde bir değişkene bir değer atandığı zaman bu değer program son bulana kadar değişmez. Bu metot bünyesinde tanımlanan parametre değişkenleri için de geçerlidir.
5. Bu immutable (değiştirilemez değer) yapısı nedeniyle fonksiyonel diller doğası gereği döngülere (for, while vs) sahip değillerdir. Bir döngüsel tekrarı gerçekleştirmek için özyineleme (recursion) kullanılır.
6. Doğaları gereği fonksiyonel diller eşzamanlılık (concurrency) sorunlarını daha kolay çözerler.

Fonksiyonel dillere özellikle değinmedim. Zira dilin kendisinden çok kavramın bize sunduğu çözüm daha önemlidir. Eğer geleceğe yatırım yapmak istiyorsanız mutlaka göz atmanız gereken önemli teknolojiler arasında yer alacak bir konudur.
