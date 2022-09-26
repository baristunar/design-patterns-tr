# Composite Pattern

Bu yazı [RipRutorial](https://riptutorial.com/) sitesi üzerinden hazırlanan **Learning Design Patterns** adlı elektronik kitapta bulunan notlar üzerinden derlenmiştir.

## Composite Pattern

- Bir grup nesneye tek bir nesnenin varlığı gibi davranmasını sağlayan bir tasarım örüntüsüdür.
- İstemcilerin birbirinden bağımsız nesnelere aynı nesneymiş gibi davranmasını sağlayan bir örüntüdür.
- Benzer yapılara sahip nesnelerdeki operasyonel faaliyetleri sadeleştirmek ve temiz kod yazımı için faydalı örüntülerden biridir.
- **GOF**'un (*Gang Of Four*) yapısal tasarım örüntülerinden biridir.

En sık kullanıldığı yerler, benzer yapılara sahip **Loglama** ya da **Dosya/Klasör** sistemi işlemleridir diyebiliriz.

Aşağıda bununla ilgili örnek kod blokları tanıtılacaktır.

##### Örnek 1 - Loglama İçin Composite Yapısı

- Aşağıda kurulacak olan yapı **SOLID** prensiplerine de uygundur.
  - Çünkü hem her bir sınıfın bir sorumluluğu olacak şekilde, yeni bir loglama yapısı sınıfı kodlamak mümkündür
  - Hem de Mevcut olanların yapısını değiştirmeden yeni bir loglama mekanizması eklemek mümkündür

Bu sınıflarla ilgili olarak hazırlanmış örnek kodlar da aşağıdaki gibidir;

**ILogger** sınıfı:
```csharp
public interface ILogger
{
  void Log(string message);
}
```

**Composite Logger** sınıfı (*Pattern Uygulaması* burada yapılıyor):
```csharp
public class CompositeLogger : ILogger
{
  private readonly ILogger[] _loggers;
  public CompositeLogger(params ILogger[] loggers)
  {
    _loggers = loggers;
  }
  
  public void Log(string message)
  {
    foreach (var logger in _loggers)
    {
      logger.Log(message);
    }
  }
}
```

**Console Logger** sınıfı:
```csharp
public class ConsoleLogger : ILogger
{
  public void Log(string message)
  {
    //konsola loglama işlemini yap
  }
}
```

**File Logger** sınıfı:
```csharp
public class FileLogger : ILogger
{
  public void Log(string message)
  {
    //dosyaya loglama işlemini yap
  }
}
```

**Örnek Kullanım** kodu:
```csharp
var compositeLogger = new CompositeLogger(new ConsoleLogger(), new FileLogger());
// yukarıdaki satırla, hem konsola hem de dosyaya tek satırda loglama yaoabilmesi sağlanmaktadır
compositeLogger.Log("some message");
```


##### Örnek 2 - Dosya Klasör ile İlgili Composite Yapısı

- Böyle bir yapı ne işimize yarar?
  - Hem dosya, hem de klasör
    - Benzer yapılara sahip sınıflardır
      - İkisinde de
        - Ad
        - Boyut
        - Oluşturma Tarihi
        - Güncelleme Tarihi gibi bilgiler bulunmaktadır
    - Yukarıda belirtilen sebep yüzünden
      - Ayrı sınıflar için ayrı ayrı kod yazmak yerine
        - Generic diye tabir ettiğimiz
          - Bir sınıf içerisinde iki sınıfın işlemlerinin de yapılması
            - Daha hızlı ve daha güvenilir kod geliştirilmesine yardımcı olacaktır

**Component** sınıfı:
```c++
/*
* File ve Folder gibi benzer yapıların kurulacağı
* Interface sınıfıdır
*/
class Component
{
  public: virtual int getSize() const = 0;
};
```

**File** sınıfı:
```c++
/*
* Dosya sistemindeki Dosyayı belirten sınıf
*/
class File : public Component
{
  public: virtual int getSize() const {
    // Dosya boyutunu döndür
  }
};
```

**Folder** sınıfı:
```c++
/*
* Hiyerarşik olaran alt klasör ve dosyaları içeren
* Klasör sınıfıdır
*/
class Folder : public Component
{
  public: void addComponent(Component* aComponent) {
    // bileşen listesine bir eleman eklemek için kullanılır
  }

  void removeComponent(Component* aComponent) {
    // bileşen listesinden bir eleman çıkarmak için kullanılır
  }

  // hiyerarşik olarak altındaki elemanları gezerek klasörün
  // toplam boyutunu bulmasını sağlar
  virtual int getSize() const {
    int size = 0;
    foreach(component : mList) {
      size += component->getSize();
    }
    return size;
  }

  private: list<Component*> mList;
};
```