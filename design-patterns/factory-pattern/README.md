# Factory Pattern (Fabrika Deseni)

---

Bu yazı [patterns.dev](https://www.patterns.dev/posts/factory-pattern/)'den çevirilmiş.

---
Fabrika deseni ile yeni nesneler oluşturmak için fabrika fonksiyonlarını kullanabiliriz. Bir fonksiyon, `new` anahtar sözcüğünü kullanmadan yeni bir nesne döndürdüğü zaman fabrika fonksiyonudur!

Diyelim ki uygulamamız için birçok kullanıcıya ihtiyacımız var. `firstName`, `lastName` ve `email` özelliği ile yeni kullanıcılar oluşturabiliriz. Fabrika fonksiyonu, yeni oluşturulan nesneye, `firstName` ve `lastName`'i döndüren bir `fullName` özelliği de ekler.

```js
const createUser = ({ firstName, lastName, email }) => ({
  firstName,
  lastName,
  email,
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
});
```

Mükemmel! Artık `createUser` fonksiyonunu çağırarak birden çok kullanıcıyı kolayca oluşturabiliriz.

```js
const createUser = ({ firstName, lastName, email }) => ({
  firstName,
  lastName,
  email,
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
});

const user1 = createUser({
  firstName: "John",
  lastName: "Doe",
  email: "john@doe.com"
});

const user2 = createUser({
  firstName: "Jane",
  lastName: "Doe",
  email: "jane@doe.com"
});

console.log(user1);
console.log(user2);
```

Bu kısımdaki kodlara [codesandbox](https://codesandbox.io/s/factory-pattern-1-8s5cv?from-embed) üzerinden erişebilirsiniz.

Çıktı:

```js
{firstName: "John", lastName: "Doe", email: "john@doe.com", fullName: ƒ fullName()}
firstName: "John"
lastName: "Doe"
email: "john@doe.com"
fullName: ƒ fullName() {}
<constructor>: "Function"
name: "Function"

{firstName: "Jane", lastName: "Doe", email: "jane@doe.com", fullName: ƒ fullName()}
firstName: "Jane"
lastName: "Doe"
email: "jane@doe.com"
fullName: ƒ fullName() {}
<constructor>: "Function"    
name: "Function"
```

Nispeten karmaşık ve yapılandırılabilir nesneler oluşturuyorsak, fabrika deseni yararlı olabilir. Anahtarların ve değerlerin değerleri belirli bir ortama veya konfigürasyona bağlı olabilir. Fabrika deseniyle, özel anahtarları ve değerleri içeren yeni nesneleri kolayca oluşturabiliriz!

```js
const createObjectFromArray = ([key, value]) => ({
[key]: value
});

createObjectFromArray(["name", "John"]); // { name: "John" }
```

---
## Artıları

Fabrika deseni, aynı özellikleri paylaşan birden çok küçük nesne oluşturmamız gerektiğinde kullanışlıdır. Bir fabrika fonksiyonu, mevcut ortama veya kullanıcıya özel yapılandırmaya bağlı olarak özel bir nesneyi kolayca döndürebilir.

---
## Eksileri

JavaScript'te fabrika deseni, `new` anahtar sözcüğünü kullanmadan bir nesneyi döndüren bir fonksiyondan çok daha fazlası değildir. [ES6 arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#arrow_functions)(ok fonksiyonları) , her seferinde bir nesneyi **dolaylı olarak döndüren küçük** fabrika fonksiyonları oluşturmamıza olanak tanır.

Ancak birçok durumda, her seferinde yeni nesneler yerine yeni örnekler oluşturmak bellek açısından daha verimli olabilir.

```js
class User {
constructor(firstName, lastName, email) {
this.firstName = firstName;
this.lastName = lastName;
this.email = email;
}

fullName() {
return `${this.firstName} ${this.lastName}`;
}
}

const user1 = new User({
firstName: "John",
lastName: "Doe",
email: "john@doe.com"
});

const user2 = new User({
firstName: "Jane",
lastName: "Doe",
email: "jane@doe.com"
});
```

---

## Kaynakça:

* [JavaScript Factory Functions with ES6+](https://medium.com/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1) - Eric Elliott 
