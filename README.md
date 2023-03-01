# Design Patterns (Tasarım Kalıpları, Tasarım Desenleri)

![Design Patterns](./assets/design-patterns.png)

Yazılım mühendisliğinde bir tasarım kalıbı, yazılım tasarımında yaygın olarak ortaya çıkan bir soruna genel olarak tekrarlanabilir bir çözümdür. Tasarım deseni, doğrudan koda dönüştürülebilen bitmiş bir tasarım değildir. Birçok farklı durumda kullanılabilecek bir sorunun nasıl çözüleceğine ilişkin bir açıklama veya şablondur.

## Tasarım Kalıplarının Kullanımları

Tasarım kalıpları, test edilmiş, kanıtlanmış geliştirme paradigmaları sağlayarak geliştirme sürecini hızlandırabilir. Etkili yazılım tasarımı, uygulamada daha sonra görünür hale gelmeyebilecek konuların dikkate alınmasını gerektirir. Tasarım kalıplarının yeniden kullanılması, büyük sorunlara neden olabilecek ince sorunları önlemeye yardımcı olur ve kalıplara aşina olan kodlayıcılar ve mimarlar için kod okunabilirliğini artırır.

Çoğu zaman, insanlar yalnızca belirli yazılım tasarım tekniklerinin belirli sorunlara nasıl uygulanacağını anlar. Bu tekniklerin daha geniş bir problem yelpazesine uygulanması zordur. Tasarım kalıpları, belirli bir soruna bağlı özellikler gerektirmeyen bir biçimde belgelenen genel çözümler sağlar.

Ayrıca kalıplar, geliştiricilerin yazılım etkileşimleri için iyi bilinen, iyi anlaşılan adlar kullanarak iletişim kurmasına olanak tanır. Ortak tasarım desenleri zaman içinde geliştirilebilir, bu da onları geçici tasarımlardan daha sağlam hale getirir.

Tasarım kalıpları 3 ayrı kategoride toplanmaktadır. Bunlar;

- [Design Patterns (Tasarım Kalıpları, Tasarım Desenleri)](#design-patterns-tasarım-kalıpları-tasarım-desenleri)
  - [Tasarım Kalıplarının Kullanımları](#tasarım-kalıplarının-kullanımları)
  - [Creational Design Patterns (Yaratıcı Tasarım Kalıpları)](#creational-design-patterns-yaratıcı-tasarım-kalıpları)
  - [Structural Design Patterns (Yapısal Tasarım Kalıpları)](#structural-design-patterns-yapısal-tasarım-kalıpları)
  - [Behavioral Design Patterns (Davranışsal Tasarım Kalıpları)](#behavioral-design-patterns-davranışsal-tasarım-kalıpları)
    - [İçindekiler](#i̇çindekiler)
    - [Nasıl Katkıda bulunabilirim?](#nasıl-katkıda-bulunabilirim)
    - [Katkıda bulunurken dikkat edilmesi gerekenler](#katkıda-bulunurken-dikkat-edilmesi-gerekenler)
    - [Katkıda Bulunanlar ❤️🚀](#katkıda-bulunanlar-️)

---

## Creational Design Patterns (Yaratıcı Tasarım Kalıpları)

Bu tasarım kalıpları tamamen sınıf somutlaştırma ile ilgilidir. Bu model ayrıca sınıf yaratma kalıpları ve nesne yaratma kalıplarına ayrılabilir. Sınıf oluşturma kalıpları, örnekleme sürecinde kalıtımı etkin bir şekilde kullanırken, nesne oluşturma kalıpları işi yapmak için yetkilendirmeyi etkili bir şekilde kullanır.

Aşağıdaki tasarım kalıpları Yaratıcı tasarım kalıplarına örnektir;

- [Abstract Factory](./design-patterns/abstract-factory/README.md)
- [Builder](./design-patterns/builder-pattern/README.md)
- Factory Method
- [Prototype](./design-patterns/prototype-pattern/README.md)
- [Singleton](./design-patterns/singleton-pattern/README.md)

---

## Structural Design Patterns (Yapısal Tasarım Kalıpları)

Bu tasarım kalıpları tamamen Sınıf (`Class`) ve Nesne (`Object`) kompozisyonu ile ilgilidir. Yapısal sınıf oluşturma kalıpları, arabirimleri oluşturmak için kalıtımı kullanır. Yapısal nesne kalıpları, yeni işlevsellik elde etmek için nesneleri oluşturmanın yollarını tanımlar.

Aşağıdaki tasarım kalıpları Yapısal tasarım kalıplarına örnektir;

- [Adapter](./design-patterns/adapter-pattern/README.md)
- [Bridge](./design-patterns/bridge-pattern/README.md)
- [Composite](./design-patterns/composite-pattern/README.md)
- [Facade](./design-patterns/facade-pattern/README.md)
- [Decorator](./design-patterns/decorator-pattern/README.md)
- [Flyweight](./design-patterns/flyweight-pattern/README.md)
- Private Class Data
- [Proxy](./design-patterns/proxy-pattern/README.md)
- [Render Props Pattern](./design-patterns/render-props-pattern/README.md)

---

## Behavioral Design Patterns (Davranışsal Tasarım Kalıpları)

Bu tasarım kalıplarının tamamı `Class`'ın nesne iletişimi ile ilgilidir. Davranış kalıpları, nesneler arasındaki iletişimle en özel olarak ilgilenen kalıplardır.

- Chain of responsibility
- Command
- Interpreter
- Iterator
- Mediator
- Memento
- Null Object
- [Observer Pattern](./design-patterns/observer-pattern/README.md)
- State
- Strategy
- [Template method](./design-patterns/template-method/README.md)
- Visitor

---

### İçindekiler

- [Singleton Pattern](./design-patterns/singleton-pattern/README.md)
- [Proxy Pattern](./design-patterns/proxy-pattern/README.md)
- [Provider Pattern](./design-patterns/provider-pattern/README.md)
- [Container/ Presentational Pattern](./design-patterns/container-presentational-pattern/README.md)
- [Module Pattern](./design-patterns/module-pattern/README.md)
- [Render Props Pattern](./design-patterns/render-props-pattern/README.md)
- [Builder Pattern](./design-patterns/builder-pattern/README.md)
- [Prototype Pattern](./design-patterns/prototype-pattern/README.md)
- [Facade Pattern](./design-patterns/facade-pattern/README.md)
- [Decorator Pattern](./design-patterns/decorator-pattern/README.md)
- [Adapter Pattern](./design-patterns/adapter-pattern/README.md)
- [Factory Pattern](./design-patterns/factory-pattern/README.md)
- [HOC Pattern](./design-patterns/hoc-pattern/README.md)
- [Composite Pattern](./design-patterns/composite-pattern/README.md)
- [Abstract Factory](./design-patterns/abstract-factory/README.md)
- [Observer Pattern](./design-patterns/observer-pattern/README.md)
- [Bridge Pattern](./design-patterns/bridge-pattern/README.md)
- [Mixin Pattern](./design-patterns/mixin-pattern/README.md)
- [Hooks Pattern](./design-patterns/hooks-pattern/README.md)
- [Rendering Patterns](./design-patterns/rendering-patterns/README.md)
- [Flyweight Pattern](./design-patterns/flyweight-pattern/README.md)

### Nasıl Katkıda bulunabilirim?

1. Projeyi forklayın.
2. [Issues](https://github.com/baristunar/patterns-dev-tr/issues) bölümünden boştaki bir taska işi aldığınızı yorum olarak belirtip bir branch açın ve çevirinize başlayabilirsiniz.
3. Yorumda belirtmeniz önemlidir çünkü bir başkası aynı taskı alıp çevirisini yapabilir. Bu durumda kimse mağdur olsun istemeyiz.
4. Issuelardaki konular dışında farklı bir pattern çevirisi yapacaksanız kendiniz de issue açıp çevirinize başlayabilirsiniz.
5. Çeviriniz bittikten sonra pull request açabilirsiniz.
6. Çeviri dışında destek olmak isterseniz farklı programlama dillerinde halihazırda çevirisi bulunan patternlere kod örnekleri paylaşabilirsiniz.

---

### Katkıda bulunurken dikkat edilmesi gerekenler

- Readme dosyasına kod bloklarını **image** olarak yüklemeyiniz. Örnek kullanım `console.log("Hello World")`
- Her bir patternin **"Design Patterns"** klasöründe patternin adıyla klasörü açılmalıdır.
- Görsel dosyaları yine patternin kendi klasörü altında **assets** klasöründe depolayınız.
- Son olarak ana readme dosyasında çevirisini yaptığınız patternin klasörüne ilgili pattern [İçindekiler](#i̇çindekiler) bölümünde ve patternin ilgili kategorisinde linklenmelidir. Bkz. [Creational Design Patterns](#creational-design-patterns-yaratıcı-tasarım-kalıpları), [Structural Design Patterns](#structural-design-patterns-yapısal-tasarım-kalıpları), [Behavioral Design Patterns](#behavioral-design-patterns-davranışsal-tasarım-kalıpları).
- Bir çeviri eklerken kaynak belirtmeyi unutmayınız.

---

### Katkıda Bulunanlar ❤️🚀

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center"><a href="https://github.com/baristunar"><img src="https://avatars.githubusercontent.com/u/58105650?v=4?s=100" width="100px;" alt="Barış Tunar"/><br /><sub><b>Barış Tunar</b></sub></a><br /><a href="#translation-baristunar" title="Translation">🌍</a></td>
      <td align="center"><a href="https://www.linkedin.com/in/serdar-gül-ba5352126/"><img src="https://avatars.githubusercontent.com/u/11051271?v=4?s=100" width="100px;" alt="Serdar GÜL"/><br /><sub><b>Serdar GÜL</b></sub></a><br /><a href="#translation-coderserdar" title="Translation">🌍</a></td>
      <td align="center"><a href="https://www.linkedin.com/in/aycanogut/"><img src="https://avatars.githubusercontent.com/u/74212439?v=4?s=100" width="100px;" alt="Aycan Öğüt"/><br /><sub><b>Aycan Öğüt</b></sub></a><br /><a href="#translation-aycanogut" title="Translation">🌍</a></td>
      <td align="center"><a href="cengiz.rocks"><img src="https://avatars.githubusercontent.com/u/13690848?v=4?s=100" width="100px;" alt="Cengiz C. Mataracı"/><br /><sub><b>Cengiz C. Mataracı</b></sub></a><br /><a href="#translation-cengizcmataraci" title="Translation">🌍</a></td>
      <td align="center"><a href="https://www.linkedin.com/in/dogancanulgu/"><img src="https://avatars.githubusercontent.com/u/78671556?v=4?s=100" width="100px;" alt="Doğancan Ülgü"/><br /><sub><b>Doğancan Ülgü</b></sub></a><br /><a href="#translation-dogancanulgu" title="Translation">🌍</a></td>
      <td align="center"><a href="https://f-yesilyurt.medium.com/"><img src="https://avatars.githubusercontent.com/u/52417156?v=4?s=100" width="100px;" alt="Furkan Yesilyurt"/><br /><sub><b>Furkan Yesilyurt</b></sub></a><br /><a href="#translation-furkanyesilyurt" title="Translation">🌍</a></td>
      <td align="center"><a href="https://github.com/ismailgunayy"><img src="https://avatars.githubusercontent.com/u/29682307?v=4?s=100" width="100px;" alt="İsmail Günay"/><br /><sub><b>İsmail Günay</b></sub></a><br /><a href="#translation-ismailgunayy" title="Translation">🌍</a></td>
    </tr>
    <tr>
      <td align="center"><a href="https://github.com/olcaykaplan"><img src="https://avatars.githubusercontent.com/u/4987587?v=4?s=100" width="100px;" alt="Olcay Kaplan"/><br /><sub><b>Olcay Kaplan</b></sub></a><br /><a href="#translation-olcaykaplan" title="Translation">🌍</a></td>
      <td align="center"><a href="http://hatice-surumlu.medium.com/"><img src="https://avatars.githubusercontent.com/u/71832100?v=4?s=100" width="100px;" alt="Haticesurumlu"/><br /><sub><b>Haticesurumlu</b></sub></a><br /><a href="#translation-Haticesurumlu" title="Translation">🌍</a></td>
      <td align="center"><a href="https://github.com/DenizCansever"><img src="https://avatars.githubusercontent.com/u/67337875?v=4?s=100" width="100px;" alt="Deniz Cansever"/><br /><sub><b>Deniz Cansever</b></sub></a><br /><a href="#translation-DenizCansever" title="Translation">🌍</a></td>
      <td align="center"><a href="https://github.com/kadorek"><img src="https://avatars.githubusercontent.com/u/15313515?v=4?s=100" width="100px;" alt="kadorek"/><br /><sub><b>kadorek</b></sub></a><br /><a href="#translation-kadorek" title="Translation">🌍</a></td>
      <td align="center"><a href="https://burakbalci.vercel.app"><img src="https://avatars.githubusercontent.com/u/96511762?v=4?s=100" width="100px;" alt="Burak"/><br /><sub><b>Burak</b></sub></a><br /><a href="#translation-burak-balci" title="Translation">🌍</a></td>
    </tr>
  </tbody>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->
