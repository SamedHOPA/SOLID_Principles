# SOLID_Principles

* `(S)ingle Responsibility Principle` - Sınıflarımızın iyi tanımlanmış tek bir sorumluluğu olmalı.

* `(O)pen/Closed Principle` - Sınıflarımız değişikliğe kapalı ancak yeni davranışların eklenmesine açık olmalı.

* `(L)iskov ‘s Substitution Principle` - Kodumuzda herhangi bir değişiklik yapmaya gerek kalmadan türetilmiş sınıfları (sub class) türedikleri ata sınıfın (base class) yerine kullanabilmeliyiz.

* `(I)nterface Segregation Principle` - Genel kullanım amaçlı tek bir kontrat yerine daha özelleşmiş birden çok kontrat oluşturmayı tercih etmeliyiz.

* `(D)ependency Inversion Principle` - Katmanlı mimarilerde üst seviye modüller alt seviyedeki modüllere doğruda bağımlı olmamalıdır.

## (S)ingle Responsibility Principle - (Tek Sorumluluk İlkesi)

* Bu ilke, “ bir sınıfın değişmesi için tek bir neden olması gerektiğini ” ifade eder , bu da her sınıfın tek bir sorumluluğu olması gerektiği anlamına gelir.
```java
public class Vehicle {
    public void details() {}
    public double price() {}
    public void addNewVehicle() {}
}
```
* Burada sınıfın değişmesi için birden çok nedeni vardır, çünkü Vehicle sınıfın üç ayrı sorumluluğu vardır: ayrıntıları yazdırma, yazdırma fiyatı ve Veritabanına yeni bir araç ekleme.

* Tek sorumluluk ilkesi hedefine ulaşmak için, yalnızca tek bir işlevi gerçekleştiren ayrı bir sınıf uygulamalıyız.
```java
public class VehicleDetails {
    public void details() {}
}

public class CalculateVehiclePrice {
    public double price() {}
}

public class AddVehicle {
    public void addNewVehicle() {}
}
```

## (O)pen/Closed Principle - (Açık-Kapalı Prensibi)

* Bu ilke, “ yazılım varlıklarının (sınıflar vb.) genişletilmeye açık, ancak değiştirilmeye kapalı olması gerektiğini ” belirtir . Bu, bir sınıftaki hiçbir şeyi değiştirmeden genişletilebilir olması gerektiği anlamına gelir.
* Bu prensibi bir bildirim hizmeti örneği ile anlayalım.
```java
public class NotificationService{
    public void sendNotification(String medium) {
         if (medium.equals("email")) {}
    }
}
```
* Burada e-posta dışında yeni bir ortam tanıtmak istiyorsanız, diyelim ki bir cep telefonu numarasına bildirim gönderin, o zaman NotificationService sınıfında kaynak kodunu değiştirmeniz gerekiyor.
* Bunu aşmak için kodunuzu, herkesin özelliğinizi genişleterek yeniden kullanabileceği ve herhangi bir özelleştirmeye ihtiyaç duyarsa sınıfı genişletip üzerine özelliklerini ekleyebileceği şekilde tasarlamanız gerekir.
### Aşağıdaki gibi yeni bir arayüz oluşturabilirsiniz:

```java
public interface NotificationService {
    public void sendNotification(String medium);
}
```
### Eposta bildirimi:

```java
public class EmailNotification implements NotificationService {
    public void sendNotification(String medium){
        // write Logic using for sending email
    }
}
```
### Mobil Bildirim:

```java
public class MobileNotification implements NotificationService {
    public void sendNotification(String medium){
        // write Logic using for sending notification via mobile
    }
}
```
## (L)iskov ‘s Substitution Principle - (Liskov İkame Prensibi)

* Bu ilke, " türetilmiş sınıfların, temel sınıflarının yerine geçebilmesi gerektiğini " belirtir . Başka bir deyişle, A sınıfı B sınıfının bir çocuğuysa, programın mevcut davranışını kesintiye uğratmadan B'yi A ile değiştirebilmeliyiz.

### Rectangle temel sınıfından türetilen bir kare sınıf örneğini ele alalım:
```java
public class Rectangle {
    private double height;
    private double width;
    public void setHeight(double h) { height = h; }
    public void setWidht(double w) { width = w; }
}
public class Square extends Rectangle {
    public void setHeight(double h) {
        super.setHeight(h);
        super.setWidth(h);
    }
    public void setWidth(double w) {
        super.setHeight(w);
        super.setWidth(w);
    }
}
```
* Rectangle Sınıfta genişlik ve yükseklik ayarı son derece mantıklı görünüyor . Ancak square sınıfında SetWidth() ve SetHeight() anlamlı değildir çünkü birini ayarlamak diğerini ona uyacak şekilde değiştirir.
* Bu durumda, Rectangle temel sınıfını türetilmiş Square sınıfıyla değiştiremeyeceğiniz için Square, Liskov ikame testinde başarısız olur. Square sınıfının ekstra kısıtlamaları vardır, yani yükseklik ve genişlik aynı olmalıdır. Bu nedenle, Rectangle sınıfının Square sınıfıyla değiştirilmesi beklenmeyen davranışlara neden olabilir.

## (I)nterface Segregation Principle - (Arayüz Ayrıştırma İlkesi)

* Bu ilke Arayüzler için geçerlidir ve tek sorumluluk ilkesine benzer. “** İstemci kullanmadığı bir arabirimi uygulamaya veya istemciler kullanmadıkları yöntemlere bağımlı olmaya asla zorlanmamalıdır.**“.
### Bunu bir araç arayüzü örneği ile anlayalım:
```java
public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
    public void openDoors();
}
```
* Bike, Şimdi bu Araç arayüzünü kullanarak bir sınıf oluşturduğumuzu varsayalım.

```java
public class Bike implements Vehicle {
    public void drive() {}
    public void stop() {}
    public void refuel() {}

    // Can not be implemented
    public void openDoors() {}
}
```
* Bisikletin kapıları olmadığı için son işlevi uygulayamıyoruz.
* Bunu düzeltmek için, hiçbir sınıfın ihtiyaç duymadığı herhangi bir arabirimi veya yöntemi uygulamak zorunda kalmaması için arabirimleri küçük çoklu arabirimlere ayırmanız önerilir.

```java
public interface Vehicle {
    public void drive();
    public void stop();
    public void refuel();
}
```

```java
public interface Doors{
    public void openDoors();
}
```
### İki sınıf oluşturma - Car ve Bike

```java
public class Bike implements Vehicle {
    public void drive() {}
    public void stop() {}
    public void refuel() {}
}
```

```java
public class Car implements Vehicle, Door {
    public void drive() {}
    public void stop() {}
    public void refuel() {}
    public void openDoors() {}
}
```
## (D)ependency Inversion Principle - (Bağımlılık Tersine Çevirme İlkesi)

* Dependency Inversion Principle (DIP), " varlıkların somut uygulamalara (sınıflara) değil, soyutlamalara (soyut sınıflar ve arayüzler) bağlı olması gerektiğini belirtir . Ayrıca, yüksek seviyeli modül düşük seviyeli modüle değil, her ikisine de bağlı olmalıdır. soyutlamalara dayanmalıdır ".
* Müşterilerin en sevdikleri kitapları belirli bir rafa koymalarını sağlayan bir kitapçı olduğunu varsayalım.
* Bu işlevi uygulamak için bir Book sınıf ve bir Shelf sınıf oluşturuyoruz. Kitap sınıfı, kullanıcıların raflarda depoladıkları her kitabın incelemelerini görmelerine olanak tanır. Raf sınıfı, raflarına bir kitap eklemelerine izin verecektir. Örneğin,
```java
public class Book {
    void seeReviews() {}
}


public class Shelf {
     Book book;
     void addBook(Book book) {}
}
```
* Her şey yolunda görünüyor, ancak high-level Shelf sınıf low-level'e bağlı Book olduğundan, yukarıdaki kod Dependency Inversion Principle'ı ihlal ediyor. Bu, mağaza bizden müşterilerin kendi yorumlarını da raflara eklemelerini sağlamamızı istediğinde açıkça ortaya çıkıyor. Talebi karşılamak için yeni bir UserReview sınıf oluşturuyoruz:

```java
public class UserReview{
     void seeReviews() {}
}
```
* Şimdi, Kullanıcı İncelemelerini de kabul edebilmesi için Shelf sınıfını değiştirmeliyiz. Ancak bu, Açık/Kapalı İlkesini de açıkça bozacaktır.
* Çözüm, alt düzey sınıflar (Kitap ve UserReview) için bir soyutlama katmanı oluşturmaktır. Bunu Ürün arabirimini tanıtarak yapacağız, her iki sınıf da onu uygulayacak. Örneğin, aşağıdaki kod kavramı gösterir.

```java
public interface Product {
    void seeReviews();
}

public class Book implements Product {
    public void seeReviews() {}
}

public class UserReview implements Product {
    public void seeReviews() {}
}
```
* Artık Raf, uygulamaları yerine Ürün arayüzüne başvurabilir (Kitap ve Kullanıcı İncelemesi). Yeniden düzenlenmiş kod ayrıca, müşterilerin raflarına da koyabilecekleri yeni ürün türlerini (örneğin, Dergi) daha sonra tanıtmamıza da olanak tanır.

```java
public class Shelf {
    Product product;
    void addProduct(Product product) {}
    void customizeShelf() {}
}
```
