> Bu makale [patterns.dev](https://www.patterns.dev/) adresinde yayınlanan [_HOC Pattern_](https://www.patterns.dev/posts/hoc-pattern/) makalesinin Türkçe çevirisidir.

# Higher Order Components (HOC) Pattern

> Yeniden kullanılabilir _mantığı_, uygulamanızın her yerinde props olarak component'lere aktarın.

Uygulamamız içerisinde, aynı mantığı farklı _component_'lerde sık sık kullanmak isteriz. Bu mantık, bir _component_'e belirli bir stil vermek, yetkilendirme sağlamak veya global bir _state_ eklemek olabilir.

Aynı mantığı birden fazla _component_'te kullanabilmenin bir yolu, **Higher Order Component** modelini kullanmaktır. Bu model, _component_ mantığını uygulamamızın her yerinde yeniden kullanabilmemizi sağlar.

_Higher Order Component_ (HOC) başka bir _component_'i parametre olarak alan _component_'e denir. HOC, parametre olarak aldığı _component_'e uygulamak istediğimiz mantığı içerir. Bu mantık _component_'e uygulandıktan sonra; HOC, _component_'i eklenmiş mantıkla birlikte döndürür.

Diyelim ki, uygulamamızda birden fazla _component_'e aynı stili uygulamak istiyoruz. Her seferinde _local_ stiller oluşturmak yerine, kendisine aktarılan _component_'lere belirli bir stil uygulayan bir HOC yaratabiliriz.

```js
function withStyles(Component) {
  return props => {
    const style = { padding: '0.2rem', margin: '1rem' }
    return <Component style={style} {...props} />
  }
}

const Button = () = <button>Click me!</button>
const Text = () => <p>Hello World!</p>

const StyledButton = withStyles(Button)
const StyledText = withStyles(Text)
```

Yukarıda, `Button` ve `Text` _component_'lerinin özelleştirilmiş versiyonları olan `StyledButton` ve `StyledText` _component_'lerini oluşturduk. Artık iki _component_ de `withStyles` HOC'si aracılığıyla eklenmiş stilleri içeriyor.

Şimdi de, daha önce [Container/Presentational Pattern](/design-patterns/container-presentational-pattern/README.md) içerisinde kullanılmış olan `DogImages` örneğini inceleyelim. Bu uygulama, _API_ üzerinden gelen köpek fotoğraflarını listeliyor.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/hoc-pattern-1-tzp7i?from-embed) üzerinden erişebilirsiniz.

Şimdi kullancı deneyimini biraz geliştirelim. _API_ üzerinden veri gelirken, kullanıcıya bir `Loading...` ekranı göstermek istiyoruz. Bu veriyi `DogImages` _component_'ine direkt olarak eklemek yerine, bu mantığı bizim için ekleyecek bir HOC kullanabiliriz.

`withLoader` adında bir HOC yaratalım. Bir HOC bir _component_ almalı ve o _component_'i döndürmelidir. Bu durumda; `withLoader`, veri yüklenene kadar `Loading...` yazısını gösterecek bir element almalıdır.

Kullanmak istediğimiz `withLoader` _higher order component_'inin yalın bir halini yaratalım.

```js
function withLoader(Element) {
  return (props) => <Element />;
}
```

Ancak, yalnızca alınan elementi döndürmek istemiyoruz. Bunun yerine, alınacak elementin istenen verinin hala yüklenip yüklenmediğini söyleyen kontrolü içermesini istiyoruz.

`withLoader` _higher order component_'ini yeniden kullanılabilir yapabilmek için, Dog _API URL_'ini _hardcoded_ şekilde yerleştirmeyeceğiz. Bunun yerine, *API URL'*ini `withLoader` _component_'ine argüman olarak geçireceğiz ki bu component, herhangi bir _API_ adresinden veri çekerken `Loading...` göstergesine ihtiyaç duyan başka bir _component_ tarafından kullanılabilsin.

```js
function withLoader(Element, url) {
  return (props) => {};
}
```

_Higher order component_'ler, veri hala yükleniyorken, `Loading...` yazısını göstermemizi sağlayan mantığı eklemek istediğimiz, yukarıdaki örnekte `props => {}` şeklinde bir _functional component_ olan, bir element döndürür. Veri yüklendiğinde ise, _component_ alınan verileri _props_ olarak _higher order component_'e geçirmelidir.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/hoc-pattern-2-rslq4?from-embed) üzerinden erişebilirsiniz.

Mükemmel! Herhangi bir _component_ ve herhangi bir _URL_'i alabilen bir HOC yarattık.

`useEffect` _hook_'unda, `withLoader` _higher order component_'i veriyi _URL_'ini parametre olarak verdiğimiz _API_ adresinden çeker. HOC, veri henüz döndürülmemişken `Loading...` yazısını içeren bir element döndürür.
Veri alındığında, bu veriyi `data` _state_'inde tutarız. Artık `data` _state_'i `null` olmadığından, HOC'ye verilmiş olan _component_'i _render_ edebiliriz. Peki, `DogImages` listesinde `Loading...` göstergesinin görünmesini sağlayacak davranışı uygulamamıza nasıl ekleriz?

Artık `DogImages.js` dosyasında, `DogImages` _component_'ini olduğu gibi `export` etmek istemiyoruz. Bunun yerine, _props_ yoluyla `DogImages` _component_'ini almış olan `withLoading` _higher order component_'ini `export` etmek istiyoruz.

```js
export default withLoading(DogImages);
```

`withLoader` _higher order component_'i, veriyi hangi adresten çekeceğini bilmek için aynı zamanda _URL_ de bekliyor.

```js
export default withLoader(
  DogImages,
  "https://dog.ceo/api/breed/labrador/images/random/6"
);
```

`withLoader` _component_'i döndüreceği elementi, bu durumda `DogImages` _component_'ini, bir `data` _prop_'u ile birlikte döndürdüğünden bu `data` prop'una `DogImages` _component_'i içinden erişebiliyoruz.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/hoc-pattern-2-rslq4) üzerinden erişebilirsiniz.

Mükemmel! Artık veri yüklenirken `Loading...` yazısını görebiliyoruz.

_Higher Order Component_ modeli, kullanacağımız mantığı tek yerde tutarken aynı mantığı birden fazla _component_'e aktarabilmemize olanak sağlıyor. `withLoader` _component_'i kendisine verilen _component_ veya _URL_ ile ilgilenmez. Kısaca, geçerli bir _component_ ve geçerli _API_ adresi olduğu sürece _API_'dan gelen veriyi _component_'e aktarır.

---

### Composing

Birden fazla _higher order component_ de oluşturabiliriz. Diyelim ki, kullanıcı `DogImages` listesinin üzerine geldiğinde `Hovering!` yazısını göstermek istiyoruz.

Bunun için, kendisine verdiğimiz elemente bir `hovering` _prop_'u sağlayacak bir HOC yaratmamız gerekiyor. Bu _prop_'u kullanarak, kullanıcının `DogImages` listesinin üzerine gelip gelmediğine bağlı olarak koşullu olarak yazıyı render edebiliriz.

Şimdi de `withLoader` _higher order component_'ini `withHover` _higher order component_'ine aktarabiliriz.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/hoc-pattern-3-whhh0) üzerinden erişebilirsiniz.

Artık, `DogImages` elementi `withHover` ve `withLoader` tarafından sağlanan tüm _prop_'ları içeriyor. Şimdi `hovering` _prop_'unun `true` ya da `false` olması koşuluna bağlı olarak `Hovering!` yazısını render edebiliriz.

> "[_recompose_](https://github.com/acdlite/recompose), HOC oluşturma konusunda iyi bilinen bir kütüphanedir. Ancak React Hook'ları çoğunlukla HOC'lerin yerini aldığından _recompose_ kütüphanesi artık geliştirilmiyor. Bu sebeple de makale de yer verilmeyecek."

---

## Hooks

Bazı durumlarda, HOC modeli yerine [React Hook](https://tr.reactjs.org/docs/hooks-intro.html)'larını kullanabiliriz.

`withHover` _higher order component_'inin yerine `useHover` hook'unu kullanalım. Bir HOC oluşturmak yerine, parametre olarak verdiğimiz elemente `mouseOver` ve `mouseLeave` _event listener_'larını ekleyen bir _hook_ _export_ ediyoruz. Artık, elementi HOC ile kullandığımız gibi kullanamayız. Onun yerine, `mouseOver` ve `mouseLeave` _event_'lerini kontrol etmesi gereken _hook_'tan bir `ref` döndüreceğiz.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/hoc-pattern-4-npo50) üzerinden erişebilirsiniz.

`useEffect` _hook_'u _component_'e bir _event listener_ ekler ve kullanıcının o anda elementi _hover_ edip etmemesine bağlı olarak `hovering` _state_'inin değerini `true` ya da `false` olarak değiştirir. Hem `ref` hem de `hovering` değerleri _hook_'tan döndürülmelidir: `ref` değeri `mouseOver` ve `mouseLeave` _event_'lerini alacak olan _component_'e bir referans eklemek için, `hovering` ise koşullu olarak `Hovering!` yazısını render etmek için döndürülür.

`DogImages` _component_'ini `withHover` _higher order component_'ine vermek yerine, artık `useHover` _hook_'unu direkt olarak `DogImages` _component_'inin içinde kullanabiliriz.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/hoc-pattern-4-npo50) üzerinden erişebilirsiniz.

---

Genel olarak konuşursak, _React Hooks_ HOC modelinin yerini alamaz.

_"Çoğu durumda, React Hook'ları yeterli olacaktır ve katmanlı yapıyı azaltmanıza yardımcı olabilir."_ -[React Docs](https://reactjs.org/docs/hooks-faq.html#do-hooks-replace-render-props-and-higher-order-components)

React'ın dokümantasyonuna göre, _hook_'ları kullanmak _component_ ağacının derinliğini azaltabilir. HOC modelini kullanırsak da, çok katmanlı bir _component_ ağacıyla karşı karşıya kalmak kolaydır.

```html
<withAuth>
  <withLayout>
    <withLogging>
      <Component />
    </withLogging>
  </withLayout>
</withAuth>
```

_Component_'e direkt olarak bir _hook_ ekleyerek _component_'leri iç içe kullanmak zorunda kalmayız.

_Higher Order Component_'leri kullanmak, aynı mantığı tek bir yerde tutup kontrol ederken birden fazla _component_ içinde kullanmayı mümkün kılar. _Hook_'lar ise _component_'e özel bir davranış ekleyebilmemizi sağlar, fakat HOC modeliyle karşılaştırıldığında eğer birden fazla _component_ bu davranışa bağlıysa hatayla karşılaşma ihtimalini de artırmış olur.

#### HOC'ler için uygun durumlar:

- Değiştirilmemiş, aynı davranışın uygulama boyunca birdan fazla _component_ tarafından kullanılması gerekiyorsa,
- _Component_ eklenmiş mantık olmadan, tek başına da çalışabiliyorsa,

#### Hook'lar için uygun durumlar:

- Davranışın, onu kullanan her _component_ tarafından özelleştirilmesi gerekiyorsa,
- Davranış uygulamanın her yerinde değil de, bir veya birkaç _component_ tarafından kullanılıyorsa,
- Davranış _component_'e çok sayıda _prop_ ekliyorsa,

---

## Kullanım Senaryosu

HOC modelini kullanan bazı kütüphaneler _hook_'ların çıkışından sonra _hook_ desteği eklediler. Bunun güzel bir örneği [_Apollo Client_](https://www.apollographql.com/docs/react).

> "Bu örneği anlamak için Apollo Client ile ilgili bir tecrübeye ihtiyaç yoktur."

_Apollo Client_'ı kullanmanın bir yolu `graphql()` _higher order component_'ini kullanmaktır.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/apollo-hoc-hooks-n3td8) üzerinden erişebilirsiniz.

`graphql()` _higher order component_'iyle, çekilen veriyi bu _higher order component_'ine geçirilerek kullanılan _component_'lerde erişilebilir kılarız. Hala, `graphql()` _higher order component_'ini direkt olarak kullanabiliyor olsak da bazı dezavantajları var.

Bir _component_ birden fazla _resolver_'e ihtiyaç duyduğunda birden fazla `graphql()` _higher order component_'i [**compose**](#composing) etmek gerekiyor. Birden fazla HOC _compose_ etmek verinin _component_'e nasıl aktarıldığının anlamayı zorlaştırıyor. Ayrıca bazı durumlarda HOC'lerin sırasının da önemli olmasıyla birlikte kodu _refactor_ ederken kolayca hataya sebep olabiliyor.

_Hook_'ların çıkışından sonra; _Apollo_, _Apollo Client_ kütüphanesine _Hooks_ desteği ekledi. `graphql()` _higher order component_'ini kullanmak yerine, geliştiricler kütüphanenin sağladığı _hook_'larla artık veriye direkt olarak erişebiliyorlar.

`graphql()` _higher order component_'inin örneğinde gördüğümüz verinin aynısını kullanan bir başka örneği inceleyelim. Bu sefer, _component_'lere veriyi _Apollo Client_ kütüphanesinin bize sağladığı `useMutation` _hook_'u ile sağlayacağız.

> Örneğin kodlarına [CodeSandbox](https://codesandbox.io/s/apollo-hoc-hooks-n3td8) üzerinden erişebilirsiniz.

`useMutation` _hook_'unu kullanarak, veriyi sağlamak için yazmamız gereken kod miktarını azaltmış olduk.

Kod miktarındaki azalmanın yanı sıra, bir _component_'te birden fazla _resolver_ kullanmak da oldukça kolay. Birden fazla HOC _compose_ etmektense, bir _component_ içinde birden fazla _hook_ kullanabiliriz. Verinin _component_'e nasıl geldiğini anlamak bu şekilde çok daha kolaydır. Ayrıca _component_'leri _refactor_ ederken ya da daha küçük parçalara ayırırken de geliştirici deneyimini artırmış olur.

---

### Artıları

HOC modelini kullanmak, yeniden kullanılabilir olmasını istediğimiz mantığın tek bir yerde kalmasını sağlar. bu da aynı kodu uygulama bayınca farklı yerlerde kullanıp hata yayma olasılığımızı azaltır. Tüm mantığı tek bir yerde tutarak, yazdığımız kodu `DRY` prensibine uygun yazmış ve **seperation of concerns** konseptini de zorunlu olarak uygulamış oluruz.

---

### Eksileri

HOC'nin elemente geçirdiği _prop_'un ismi bir isimlendirme çakışmasına sebep olabilir.

```js
function withStyles(Component) {
  return props => {
    const style = { padding: '0.2rem', margin: '1rem' }
    return <Component style={style} {...props} />
  }
}

const Button = () = <button style={{ color: 'red' }}>Click me!</button>
const StyledButton = withStyles(Button)
```

Bu durumda, `withStyles` _higher order component_'i ona verilen elemana `style` adında bir _prop_ geçiriyor. Ama `Button` _component_'inin zaten `style` adında bir _prop_'u var. _Prop_'ların ismini değiştirerek ya da onları birleştirerek, HOC'nin isim çakışması durumunda düzgün çalışacağından emin olun.

```js
function withStyles(Component) {
  return props => {
    const style = {
      padding: '0.2rem',
      margin: '1rem',
      ...props.style
    }

    return <Component style={style} {...props} />
  }
}

const Button = () = <button style={{ color: 'red' }}>Click me!</button>
const StyledButton = withStyles(Button)
```

Ona verilen _component_'e _prop_'ları aktaran birden fazla **compose** edilmiş HOC kullanırken, hangi HOC'nin hangi _prop_'tan sorumlu olduğunu bilmek zor olabilir. Bu da, uygulamanın _debug_ edilmesini ve ölçeklenmesini engelleyebilir.

---

### Kaynaklar

- [Higher Order Components - React](https://tr.reactjs.org/docs/higher-order-components.html)
