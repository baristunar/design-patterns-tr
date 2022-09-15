# Module Pattern

_Bu yazı_ [Addy Osmani](https://addyosmani.com/) _ve_ [Lydia Hallie](https://www.lydiahallie.io/) _tarafından yazılmış olan_ [Learning Patterns](https://www.patterns.dev/) _kitabının Module Pattern adlı bölümününün Türkçe çevirisini içermektedir._

![cover](./assets/module-pattern.png)

## Kodunuzu daha küçük ve yeniden kullanılabilir parçalara bölün

Uygulamanız ve kod yapınız büyüdükçe, sürdürülebilir ve bölümlere ayrılmış bir yapı oluşturmak giderek daha önemli hale gelir. Module modeli, kodunuzu daha küçük ve yeniden kullanılabilir parçalara ayırmanızı sağlar.

Modüller; kodunuzu küçük ve yeniden kullanılabilir parçalara ayırmak dışında, dosyanızdaki belirli değerleri özel tutmanıza olanak tanır. Bir modül içerisinde tanımlanmış olan değerler, varsayılan olarak sadece bu modül scope'unda ulaşılabilirdir. Eğer bir değeri özellikle dışa aktarmazsak, o değer tanımlandığı modül haricinde ulaşılamaz olacaktır. Bu durum kod yapınızın diğer bölümlerinde tanımlanmış olan değerler ile isim çakışmaları gibi durumları azaltır çünkü bu değerler global scope'da mevcut değildir.

---

## ES2015 Modülleri

ES2015 ile birlikte yerleşik JavaScript modülleri tanıtıldı. Modül, normal bir script dosyasına kıyasla davranışları farklı olan JavaScript kodlarını içeren bir dosyadır.

Matematik fonksiyonları içeren `math.js` adlı örnek modülü inceleyelim.

```js
function add(x, y) {
  return x + y;
}
function multiply(x) {
  return x * 2;
}
function subtract(x, y) {
  return x - y;
}
function square(x) {
  return x * x;
}
```

Toplama, çıkarma, çarpma ve kare alma gibi işlemleri gerçekleştiren fonksiyonları içeren, `math.js` adında bir dosyamız var.

Bununla beraber, bu fonksiyonları yalnızca tanımlanmış oldukları `math.js` dosyası içerisinde kullanmak istemiyoruz. Bunun yerine `index.js` dosyasında referans alarak kullanmak istiyoruz.

Bunu gerçekleştirebilmemiz için `math.js` içerisinde tanımlanmış olan fonksiyonları farklı dosyalarda kullanabilmek adına dışa aktarmamız gerekiyor. Bunu yapmak için de `export` komutunu kullanıyoruz.

Fonksiyonları dışa aktarmanın bir yolu, aktarmak istediğimiz fonksiyonların tanımlandığı satırın başına `export` komutunu eklemektir. `math.js` dosyasında tanımlanmış olan bütün fonksiyonlara `index.js` dosyasında ulaşmak istediğimiz için, dört fonksiyona da `export` komutunu ekliyoruz.

```js
export function add(x, y) {
  return x + y;
}

export function multiply(x) {
  return x * 2;
}

export function subtract(x, y) {
  return x - y;
}

export function square(x) {
  return x * x;
}
```

`export` komutunu ekleyerek, `add`, `multiply`, `subtract` ve `square` fonksiyonlarını dışa aktardık. Ancak, bu fonksiyonları bir modülden dışa aktarmak onları geri kalan dosyalar içerisinde kullanmak için yeterli bir aksiyon değil. Dışa aktardığımız değerleri diğer dosyalara erişilebilir kılmak için, kullanmak istediğimiz her dosyanın içerisinde içe aktarmalıyız.

`import` komutunu kullanarak fonksiyonları `index.js` dosyası içerisinde çağırıyoruz. Bunun için `import` komutu sonrasında çağırmak istediğimiz fonksiyon isimlerini ve modülün bulunduğu dizini sırasıyla yazmamız gerekiyor.

```js
import { add, multiply, subtract, square } from "./math.js";
```

Dört fonksiyonu da `index.js` dosyasında kullamak üzere içe aktardık. Şimdi onları kullanmayı deneyelim.

```js
import { add, multiply, subtract, square } from "./math";

console.log(add(2, 3));
console.log(multiply(2));
console.log(subtract(2, 3));
console.log(square(2));
```

Modülleri kullanmanın yararlarından biri, yalnızca `export` komutunu kullandığımız değerlere erişmemizdir. Bunun haricinde kalan fonksiyonlar yalnızca kendi modül dosyası içerisinde kullanılabilir.

Şimdi, sadece `math.js` dosyası içerisinde referans edilebilen, `privateValue` adında bir değişken oluşturalım.

```js
const privateValue = "This is a value private to the module!";

export function add(x, y) {
  return x + y;
}

export function multiply(x) {
  return x * 2;
}

export function subtract(x, y) {
  return x - y;
}

export function square(x) {
  return x * x;
}
```

`privateValue` değişken başına `export` komutu ekleyerek dışa aktarmadığımız için, bu değere `math.js` modülü haricinde ulaşmamız mümkün değil.

Değeri modüle özel tutarak, global scope'daki değişkenlerin birbirleri ile çakışma riskini azaltmış olduk. Modülleri kullandığınızda, başka geliştiriciler tarafından oluşturulmuş değerler ile oluşabilecek olan çakışma ve isim karışıklığı gibi riskleri azaltmış olursunuz.

---

Bazen, içe aktarılan değerlerlerin isimleri, aktarıldıkları yerel dosyalar içerisindeki isimlerle çakışabilir.

```js
import { add, multiply, subtract, square } from "./math.js";

function add(...args) {
  return args.reduce((acc, cur) => cur + acc);
} /* Error: add has  already been declared */

function multiply(...args) {
  return args.reduce((acc, cur) => cur * acc);
}
/* Error: multiply has already been declared */
```

`index.js` dosyası içerisinde `add` ve `multiply` adında fonksiyonlarımız var. Eğer modülden içe aktaracağımız fonksiyonları da aynı isimle çağırırsak isim çakışmasına sebep oluruz. Bu yüzden, `as` komutunu kullanarak, içe aktaracağımız fonksiyonları yeniden isimlendirmeliyiz.

İçe aktardığımız fonksiyonları `addValues` ve `multiplyValues` isimleriyle güncelleyelim.

```js
import {
  add as addValues,
  multiply as multiplyValues,
  subtract,
  square,
} from "./math.js";

function add(...args) {
  return args.reduce((acc, cur) => cur + acc);
}

function multiply(...args) {
  return args.reduce((acc, cur) => cur * acc);
}

/* From math.js module */
addValues(7, 8);
multiplyValues(8, 9);
subtract(10, 3);
square(3);

/* From index.js file */
add(8, 9, 2, 10);
multiply(8, 9, 2, 10);
```

Bu yöntem haricinde, varsayılan olarak dışa aktarma yöntemini da kullanabiliriz. Bir modül içerisinden sadece tek bir değeri varsayılan olarak dışa aktarabiliriz.

`add` fonksiyonunu varsayılan olarak dışa aktaralım. Bunun yapabilmek için, `export` komutunu `export default` ile değiştirmeliyiz.

```js
export default function add(x, y) {
  return x + y;
}

export function multiply(x) {
  return x * 2;
}

export function subtract(x, y) {
  return x - y;
}

export function square(x) {
  return x * x;
}
```

İsimlendirme değişikliği ve varsayılan dışa aktarımlar arasındaki fark, değerlerin dışa ve içe aktarılma metodlarını efektif bir şekilde değiştirir.

Önceki örnekte, dışa aktarım yaparken süslü parantez kullanıyorduk.  
`import { module } from 'module'`. Varsayılan dışa aktarım ile, süslü parantez kullanmaya gerek kalmadan içe aktarım yapabiliriz. `import module from 'module'`.

```js
import * as math from "./math.js";

math.add(7, 8);
math.multiply(8, 9);
math.subtract(10, 3);
math.square(3);
```

Süslü parantez kullanılmadan içe aktarılmış bir değer, varsayılan bir dışa aktarma varsa, her zaman varsayılan dışa aktarmaya ait olan değerdir.

JavaScript bu değerin varsayılan olarak dışa aktarıldığını bildiği için, bu değere dışa aktarırken kullandığımız isim haricinde farklı bir isim verebiliriz. (Daha önceki yaptığımız bir örnekte, `add` fonksiyonunu `addValue` olarak çağırdığımız gibi.)

```js
import add, { multiply, subtract, square } from "./math.js";

add(7, 8);
multiply(8, 9);
subtract(10, 3);
square(3);
```

Fonksiyonu `add` ismiyle dışa aktarmamıza rağmen, varsayılan olarak dışa aktardığımız için JavaScript bunu anlar ve fonksiyon adını dilediğimiz gibi isimlendirebiliriz.

Ayrıca, bir modülden içe aktaracağımız tüm değerlere `* (asteriksk)` karakterini kullanarak istediğimiz adı verebiliriz. Verdiğimiz bu ad ile içe aktardığımız değeri herhangi bir fark gözetmeksizin kullanabiliriz.

Bütün bir modülü `math` adıyla içe aktaralım.

```js
import * as math from "./math.js";
```

İçe aktardığımız değerleri `math` objesinin property'leri olarak kullanabiliriz.

```js
import * as math from "./math.js";

math.add(7, 8);
math.multiply(8, 9);
math.subtract(10, 3);
math.square(3);
```

---

## React

React ile uygulama geliştirirken, genellikle çok miktarda bileşenlerle uğraşmanız gerekir. Tüm bu bileşenleri tek bir dosya içerisinde oluşturmak yerine, bileşenleri kendi dosyalarındaki ayırabilir ve her bir bileşen için bir modül oluşturabiliriz.

Basit bir to-do list uygulaması geliştirelim. Bir adet `input`, bir `buton`, `liste` ve liste elemanlarına sahibiz.

```js
import React from "react";
import { render } from "react-dom";

import { TodoList } from "./components/TodoList";
import "./styles.css";

render(
  <div className='App'>
    <TodoList />
  </div>,
  document.getElementById("root")
);
```

```js
import React from "react";
import Button from "@material-ui/core/Button";

const style = {
  root: {
    borderRadius: 3,
    border: 0,
    color: "white",
    margin: "0 20px",
  },
  primary: {
    background: "linear-gradient(45deg, #FE6B8B 30%, #FF8E53 90%)",
  },
  secondary: {
    background: "linear-gradient(45deg, #2196f3 30%, #21cbf3 90%)",
  },
};
```

```js
import React from "react";
import Input from "@material-ui/core/Input";

const style = {
  root: { padding: "5px", backgroundColor: "#434343", color: "#fff" },
};

export default function CustomInput(props, { variant = "standard" }) {
  return (
    <Input
      style={style.root}
      {...props}
      variant={variant}
      placeholder='Type...'
    />
  );
}
```

Bileşenleri ayrı ayrı dosyalara ayırdık:

- `TodoList` bileşeni `TodoList.js`,
- `CustomButton` bileşeni `Button.js`,
- `CustomInput` bileşeni `Input.js`.

Bu uygulamada, [MaterialUI](https://mui.com/) tarafından sağlanan varsayılan input ve butonu kullanmak istemiyoruz. Bunun yerine, bileşenleri kendimiz stillendirmek istiyoruz.

Her seferinde varsayılan input ve buton bileşenlerini MaterialUI'dan içe aktarıp stilize etmek yerine, kendimiz custom bir bileşen oluşturup istediğimiz gibi stilize edebilir ve ihtiyacımız olan herhangi bir yerde bu bileşeni kullanabiliriz.

---

## Dinamik içe aktarım (Dynamic Imports)

Modülleri bir dosyanın tepesinde içe aktardığımızda, modül içerisindeki bütün değerleri yüklemiş oluruz. Bu bazı performans kayıplarına sebebiyet verebilir. Bunun önüne geçmek için sadece bazı koşullarda aktarım yapabileceğimiz dinamik içe aktarımları kullanabiliriz.

```js
import("module").then((module) => {
  module.default();
  module.namedExport();
});

// Or with async/await
(async () => {
  const module = await import("module");
  module.default();
  module.namedExport();
})();
```

`math.js` modülünü, kullanıcı butona tıkladığında yüklenecek şekilde düzenleyelim.

```js
const button = document.getElementById("btn");

button.addEventListener("click", () => {
  import("./math.js").then((module) => {
    console.log("Add: ", module.add(1, 2));
    console.log("Multiply: ", module.multiply(3, 2));

    const button = document.getElementById("btn");
    button.innerHTML = "Check the console";
  });
});
```

Dinamik içe aktarımları kullanarak, sayfadaki yüklenme sürelerini azaltabiliriz. Kullanıcının ihtiyaç duyduğu kodu, kullanıcının ihtiyacı olduğunda yüklememiz, ayrıştırmamız ve derlememiz gerekir.

Modül modeli ile, kodumuzu genel kapsamdan ulaşılamayacak şekilde [`encapsulate`](https://press.rebus.community/programmingfundamentals/chapter/encapsulation/#:~:text=Encapsulation%20is%20one%20of%20the,parties'%20direct%20access%20to%20them.) edebiliriz. Bu sayede isim çakışmaları ve global scope kirliliğini önleyebilir ve birden fazla bağımlılık ile çalışmanın riskini azaltabiliriz. [Babel](https://babeljs.io/) gibi bir derleyici kullanarak ES2015 modüllerini JavaScript runtime ile kullanabiliriz.
