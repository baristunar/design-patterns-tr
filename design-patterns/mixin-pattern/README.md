
# Mixin Patterns
Bu yazı , Addy Osmani ve Lydia Hallie tarafından yazılmış olan Learning Patterns (www.patterns.dev) kitabının Mixing Pattern adlı bölümününün Türkçe çevirisini içermektedir .[patterns.dev](https://www.patterns.dev/posts/mixin-pattern/)'den çevirilmiş. 

Mixin, kalıtım kullanmadan,başka bir nesneye veya sınıfa yeniden kullanılabilir işlevsellik (reusable functionality) eklemek için kullanabileceğimiz bir nesnedir.

Mesela, oluşturacağımız bir uygulamamızda birden çok "köpek"(Dog) oluşturmamız gereksin. Ama oluşturacağımız ana "köpek"(Dog) nesnemizin "isim" özelliği dışında başka bir özelliği olmasın.

```js
class Dog {
  constructor(name) {
    this.name = name;
  }
}
```
Bir "köpek"(Dog), bir isme sahip olmaktan daha fazlasını yapabilmelidir. Bir köpek havlayabilmeli, kuyruğunu sallayabilmeli ve oynayabilmeli!
Bunu doğrudan "Dog" sınıfına eklemek yerine, bizim için havlama (bark ), kuyruğunu sallama(wagTail )ve oyun (play) özelliği sağlayan bir mixin oluşturabiliriz.

```js
const dogFunctionality = {
  bark: () => console.log("Woof!"),
  wagTail: () => console.log("Wagging my tail!"),
  play: () => console.log("Playing!")
};
```

"dogFunctionality" mixinini , Object.assign metodu ile "Dog" prototipine ekleyebiliriz. Bu metod, bize hedef nesneye :Dog.prototype yeni özellikler eklemize izin verir. 
Dog nesnesinden oluşturulan her yeni örnek (instance) , Dog prototipine eklendiği için , dogFunctionality mixinin içeriğindeki  özelliklere erişebilecektir.
```js
class Dog {
  constructor(name) {
    this.name = name;
  }
}

const dogFunctionality = {
  bark: () => console.log("Woof!"),
  wagTail: () => console.log("Wagging my tail!"),
  play: () => console.log("Playing!")
};

Object.assign(Dog.prototype, dogFunctionality);
```
Hadi şimdi gelin, ilk evcil hayvanımız olan pet1'i Daisy isminde oluşturalım. DogFunctionality mixini Dog prototipine eklediğimiz için,Daisy yürüyebilmeli, kuyruğunu sallayabilmeli ve oynayabilmelidir.
```js 
const pet1 = new Dog("Daisy");

pet1.name; // Daisy
pet1.bark(); // Woof!
pet1.play(); // Playing!
```
---

Mükemmeeel! Mixinler , kalıtım kullanmadan sınıflara veya nesnelere özel metodlar eklememizi kolaylaştırır. 

---

Kalıtım olmadan mixinler ile işlevsellik ekleyebilsek de, mixinler kendileri kalıtımı kullanabilir!
Birçok memeli (yunuslar da dahil , ve hatta belki daha da fazlası ) yürüyebilir ve uyuyabilir. Köpekler de bir memelidir dolayısıyla bir köpek hem yürüyebilmeli hem de koşabilmelidir.
Haydi şimdi yürüme(walk) ve uyuma(sleep)özelliklerini ekleyen bir animalFunctionality mixini oluşturalım .

```js 
const animalFunctionality = {
  walk: () => console.log("Walking!"),
  sleep: () => console.log("Sleeping!")
};
 ```
Biz bu özellikleri , Object.assign kullanarak dogFunctionality prototipine ekleyebiliriz. Bu durumda, hedef nesnemiz : dogFunctionality olmaktadır.
```js
const animalFunctionality = {
  walk: () => console.log("Walking!"),
  sleep: () => console.log("Sleeping!")
};

const dogFunctionality = {
  bark: () => console.log("Woof!"),
  wagTail: () => console.log("Wagging my tail!"),
  play: () => console.log("Playing!"),
  walk() {
    super.walk();
  },
  sleep() {
    super.sleep();
  }
};

Object.assign(dogFunctionality, animalFunctionality);
Object.assign(Dog.prototype, dogFunctionality);
 ```
Mükemmel! Dog 'un her yeni örneği artık yürüme(walk) ve uyku(sleep) metodlarına erişebilir.


https://codesandbox.io/embed/zen-franklin-gvusj


Mixin'in gerçek dünyadaki örneği, tarayıcı ortamında Window arayüzünde görülebilir. Window nesnesi , setTimeout ve setInterval, indexedDB, ve isSecureContext gibi özelliklere erişmemizi sağlayan birçok özelliği,  WindowOrWorkerGlobalScope veWindowEventHandlers mixinlerinden alır.
Since it's a mixin, thus is only used to add functionality to objects, you won't be able to create objects of type WindowOrWorkerGlobalScope.
Bu bir mixin olduğu için ve sadece nesnelere işevsellik eklemek için kullanıldığı için WindowOrWorkerGlobalScopetipinde nesneler oluşturamazsınız.


https://codesandbox.io/s/mixin-2-p8zhf?from-embed


React (pre ES6)
Mixin'ler, ES6 sınıflarının tanıtılmadan önce, React bileşenlerine işlevsellik eklemek için sıklıkla kullanılıyordu. React ekibi, bileşenlere gereksiz karmaşıklık ekleyerek bakımını ve yeniden kullanımını zorlaştırdığı için mixinlerin kullanımını önermezler. React ekibi, bunun yerine artık Hooks ile değiştirilebilen, yüksek dereceli bileşenlerin  kullanımını tavsiye eder.
Mixinler, bir nesnenin prototipine işlevsellik tanıtarak, nesnelere kalıtım kullanmadan, işlevsellik eklememize izin verir. Bir nesnenin prototipini değiştirmek, prototip kirliliğine ve işlevlerimizin kökeni ile ilgili belirsizliğe yol açabileceğinden, kötü bir kullanım olarak görülür.

### Kaynaklar

* [Functional Mixins - Eric Elliott ]( https://medium.com/javascript-scene/functional-mixins-composing-software-ffb66d5e731c)
* [Mixins - JavaScript Info ]( https://javascript.info/mixins )