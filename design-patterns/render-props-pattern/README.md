## Provider Pattern

---

Bu yazı [patterns.dev](https://www.patterns.dev/posts/render-props-pattern/)'den çevirilmiştir.

---

*JSX elementleri propslar aracılığyla componentlere iletin.*


[Higher Order Component](https://www.patterns.dev/posts/hoc-pattern/) bölümünde, birden fazla componentin aynı dataya veya aynı mantık içeriğine ihtiyaç duyuyorsa, yeniden kullanılan component mantığının elverişli olabileceğini gördük.

Componentleri tekrar kullanılabilir hale getirmenin diğer bir yolu ise **render prop** desenini kullanmaktır. Render prop component üzerindeki bir poptur, bu prop JSX element dönen bir fonksiyon değerdir.Componentin kendisi render prop dışında hiçbir şeyi render etmez. Bunun yerine kendi render mantığını kullanmak yerine component sadece render prop u çağırır.

Düşünelimki `Title` adında bir componentimiz bulunuyor. Bu durumda, `Title` componenti kendisine ilettiğimiz değeri renderlamak dışında başka hiçbir şey yapmamalı. Bunun için render prop  kullanabiliriz! `Title` componentin render etmesini istediğimiz değeri `render` prop’una gönderelim.


```js
<Title render={() => <h1>I am a render prop!</h1>} />
```

`Title` component içinde, çağrılan `render` propu döndürerek bu datayı render edebiliriz!

```js
const Title = props => props.render();
```

`Componente`, `render` adında, React öğesi dönen bir fonksiyonu prop olarak iletmeliyiz.

---

Bu kısımdaki kodlara [codesandbox](https://codesandbox.io/embed/renderprops2-y6wst) üzerinden erişebilirsiniz.

---

Mükemmel, sorunsuz çalışıyor! Render props’un güzel bir yanı da prop alan componentin yeniden kullanılabilir olması. Her seferinde `render` propa farklı değerler geçerek bir çok kez kullanabiliriz.

---

Bu kısımdaki kodlara [codesandbox](https://codesandbox.io/embed/young-silence-hou0c) üzerinden erişebilirsiniz.

---

Render Props olarak adlandırılmalarına rağmen, render proplar `render` olarak adlandırılmak zorunda değiller. JSX oluşturan herhangi bir prop, render prop olarak kabul edilir! Önceki örnekteki render propsları yeniden isimlendirelim ve onlara özel isimler verelim!

---

Bu kısımdaki kodlara [codesandbox](https://codesandbox.io/embed/renderprops2-u0sfh) üzerinden erişebilirsiniz.

---

Harika! Az önce gördük ki componenti tekrar kullanabilir hale getirmek için render prop’ları kullanabiliriz, çünkü render prop’a her seferinde farklı bir veri iletebiliyoruz. Peki bunu neden kullanmak isteyelim?

Render props alan bir component genellikle `render` prop çağırmaktan çok daha fazlasını yapar. Bunun yerine, genellikle render props alan componentdeki veriden render prop olarak geçtiğimiz elemente veri iletmek isteriz!

```js
function Component(props) { 
const data = { ... } 
return props.render(data)
}
```
Render prop argüman olarak ilettiğimiz değeri artık alabilir

```js
<Component render={data => <ChildComponent data={data} />}
````

Örneğe bir göz atalım! Kullanıcının santigrat cinsinden sıcaklık girebileceği bir uygulamamız var. Uygulama girilen sıcaklığın Fahrenheit ve Kelvin değerlerini gösteriyor.

---

Bu kısımdaki kodlara [codesandbox](https://codesandbox.io/s/renderprops-4-wk0uy?from-embed) üzerinden erişebilirsiniz.

---

Hmm… Burada bir sorun var. Stateful `Input` component, user input değerini içeriyor ama `Fahrenheit` ve `Kelvin` componentleri user input değerine erişemiyor.

## Lifting state

Users inputun `Fahrenheit` ve `Kelvin` componentleri için kullanabilir olmasının bir yolu, **statetin yükseltilmesidir**.

Bu senaryoda, bir stateful `Input` componentimiz var. Ancak sibling component olan `Fahrenheit` ve `Kelvin`’in de bu dataya erişmeye ihtiyacı var.  Stateful `Input` component’e sahip olmak yerine, `Input`, `Fahrenhiet` ve `Kelvin` componentlerinin bağlı olduğu ilk ortak ana componente (`App` componenti) state’ti yükseltebiliriz.

```js
function Input({ value, handleChange }) {
  return <input value={value} onChange={e => handleChange(e.target.value)} />;
}
 export default function App() {  
const [value, setValue] = useState("");  
return (  
  <div className="App">   
   <h1>☃️ Temperature Converter 🌞</h1>  
    <Input value={value} handleChange={setValue} />  
    <Kelvin value={value} />      <Fahrenheit value={value} /> 
   </div>  
	);
}
```

Bu geçerli bir çözüm olsa da, çok fazla child componenti yönettiğimiz componentlerin olduğu çok daha büyük uygulamalarda **list state** kullanımı zorlayıcı olabilir. Her state değişimi tüm children componentleri ve hatta dataya sahip olmayan children componentlerin bile tekrardan render edilmesine neden olabilir ve bu uygulmanızın performansını kötü yönde etkileyebilir.

## Render Props

Bunun yerine, render propsları kullanabiliriz! `Input` componenti render props alabilecek şekilde değiştirelim.

```js
function Input(props) {
  const [value, setValue] = useState("");

  return (
    <>
      <input
        type="text"
        value={value}
        onChange={e => setValue(e.target.value)}
        placeholder="Temp in °C"
      />
      {props.render(value)}
    </>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>☃️ Temperature Converter 🌞</h1>
      <Input
        render={value => (
          <>
            <Kelvin value={value} />
            <Fahrenheit value={value} />
          </>
        )}
      />
    </div>
  );
}
```

Mükemmel, artık `Kelvin` ve `Fahrenheit` componentleri user input değerine erişebiliyor.

## Children as a function

Sıradan JSX componentlerinin yanı sıra, fonksyionları da React componentlere children olarak iletebiliriz. Bu fonksiyona, aynı zamanda teknik olarak render prop olan  `children` prop ile de erişebiliriz.

`Input` componenti değiştirelim. Açıkca `render` propu iletmek yerine, fonksiyonu `Input` componente child olarak göndereceğiz.

```js
export default function App() {
  return (
    <div className="App">
      <h1>☃️ Temperature Converter 🌞</h1>
      <Input>
        {value => (
          <>
            <Kelvin value={value} />
            <Fahrenheit value={value} />
          </>
        )}
      </Input>
    </div>
  );
}
```
`Input` componentinde  `props.children` prop’uyla  fonksiyona erişebiliriz. User input değerini `props.render` ile çağırmak yerine `props.children` ile çağırcağız.

```js
function Input(props) {
  const [value, setValue] = useState("");

  return (
    <>
      <input
        type="text"
        value={value}
        onChange={e => setValue(e.target.value)}
        placeholder="Temp in °C"
      />
      {props.children(value)}
    </>
  );
}
```

Harika, Bu şekilde `Kelvin` Ve `Fahrenheit` componentleri `render` prop ismini bilmeseler de değere erişebilirler. 

## Hooks

Bazı durumlarda render propsu hooklar ile değiltirebiliriz. [Apollo Client](https://www.apollographql.com/docs/react/) buna iyi bir örnek. 

> *"Bu örneği anlamak için Apollo Client konusunda herhangi bir deneyime sahip olmanıza gerek yok."*


Apollo Client kullanmanın bir yolu `Mutation` ve `Query` componentleridir. Higher Order Component bölümünde ele alınan aynı `Input` örneğine bir bakalım. `Graphql()` higher order componentini kullanmak yerine, render props alan `Mutation` componenti kullanacağız. 

---

Bu kısımdaki kodlara [codesandbox](https://codesandbox.io/s/renderprops-7-jfdxg?from-embed) üzerinden erişebilirsiniz.

---

Mutation componentinden dataya ihtiyaç duyan elementlere data aktarmak için fonksiyonları çocuk olarak iletiriz. Fonksiyon argümanlar sayesinde datanın değerini alır.

```js
<Mutation mutation={...} variables={...}>
  {addMessage => <div className="input-row">...</div>}
</Mutation>
```
Render prop pattern hala  kullanılabilir olup higher order componentle kıyasla daha sık tercih edilmesine rağmen kendi dezavantajları bulunmaktadır.

Dezavantajlarından biri iç içe derin component oluşumu.  Eğer component çoklu `Mutaiton`’a veya `Query`lere ihtiyaç duyarsa, çoklu Mutation veya Query componentlerini iç içe koyabiliriz.

```js
<Mutation mutation={FIRST_MUTATION}>
  {firstMutation => (
    <Mutation mutation={SECOND_MUTATION}>
      {secondMutation => (
        <Mutation mutation={THIRD_MUTATION}>
          {thirdMutation => (
            <Element
              firstMutation={firstMutation}
              secondMutation={secondMutation}
              thirdMutation={thirdMutation}
            />
          )}
        </Mutation>
      )}
    </Mutation>
  )}
</Mutation>
```

Hooklar çıktıktan sonra, Apollo, Apollo Client kütüphanesine Hook desteğini ekledi. Geliştiriciler `Mutation` ve `Query` render proplarını kullanmak yerine artık kütüphanenin sağladığı hooklar aracılığıyla dataya doğrudan erişebilecekler.

`Query` render prop ile önceki örnekte görmüş olduğumuz, aynı datayı kullanan örneğe bakalım. Bu sefer component’e veriyi Apollo Clientin bize sunduğu `useQuery` hook’unu kullanarak sağlayacağız.


---

Bu kısımdaki kodlara [codesandbox](https://codesandbox.io/embed/apollo-hoc-hooks-n3td8) üzerinden erişebilirsiniz.

---

UseQuery hookunu kullanarak componente veri sağlamak için gereken kod miktarını azalttık.

## Artıları
Render props pattern ile birkaç component arası mantık ve data paylaşımı kolaydır. Componentler render propları veya `children` propları kullanarak tekrardan kullanılabilir hale getirilebilir. Higher Order Component modeli, **yeniden kullanılabilirlik** ve **veri paylaşımı** gibi temelde aynı sorunları çözse de, render props modeli, HOC modelini kullanarak karşılaşabileceğimiz bazı sorunları çözmektedir.

HOC pattern’i kullanırken karşılaşabildiğimiz **naming collisions** sorununu render props pattern kullandığımızda artık karşılaşmayız çünkü Props’u otomatik olarak birleştirmeyiz. Parent component tarafından sağlanan değeri doğrudan child componente aktarıyoruz.

Props’ları açıkça ilettiğimiz için HOC’un implicit props sorununu çözüyoruz. Alt elemente iletilmesi gereken propsların tümü render propsun argüman listesinde görünebilir olmalı. Böylece popsun tam olarak nereden geldiğini bilebiliyoruz.

Uygulamamızın mantığını render propslar aracılığıyla render componentlerden ayırabiliriz. Render prop alan stateful component datayı sadece datayı renderlayan stateless component’e iletebilir.

## Eksileri
Render props ile çözmeye çalıştığımız sorun büyük ölçüde React Hooks ile değiştirilidi. Hooklar  componentlere yeniden kullanılabilirlik ve  data paylaşımı ekleme şeklimizi değiştirdiği için bir çok durumda props patternini değiştirebilirler.

Render poplara lifecycle metodları ekleyemediğimiz için sadece aldığı datayı değiştirmesi gerekmeyen componentleri kullanabiliriz.



