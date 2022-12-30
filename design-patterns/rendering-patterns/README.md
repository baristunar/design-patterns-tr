# JavaScript Rendering Patterns

> Bu yazı [Rendering Patterns](https://javascriptpatterns.vercel.app/patterns/rendering-patterns/introduction) adlı makale serisinin Türkçe çevirisidir. Anlam bozuklu oluşmaması ve kavramları evrensel bir şekilde algılayıp, öğrenebilmemiz adına yazıda kullanılan bir çok terimi orijinal dilindeki haliyle kullanmayı tercih ettim. Keyifli okumalar.

Web teknolojilerinde içeriği render etmek için bir çok farklı yöntem kullanılabilir. Bir uygulamadaki performansın anahtar etkeni, ekranda gösterilen içeriğin uygulamanın neresinde `fetch` ve `render` edildiğidir.

Mevcut framework ve kütüphaneleri kullanarak Client-Side Rendering, Static Rendering, Incremental Static Regeneration, Progressive Rendering ve Server-Side Rendering gibi farklı render modellerini uygulayabilirsiniz. Bu modeller arasındaki kullanım gerekliliklerini ve farkları anlamak, uygulamanızın performansını derinlemesine etkileyebilir ve iyi bir kullanıcı/geliştirici deneyimi ile sonuçlanabilir.

# Web Vitals

Web sayfalarımızın performansının ne kadar iyi olduğunu anlayabilmek için [Web Vitals](https://web.dev/vitals/) adı verilen ölçüm yöntemlerini kullanabiliriz. Bu yöntemlerin alt kümesi olan “the Core Web Vitals”, genellikle bir web sayfasının performansını ölçmek için kullanılır ve sayfanın [SEO](https://developers.google.com/search/docs/fundamentals/do-i-need-seo?hl=tr)’sunu etkileyebilir.

- [**Time To First Byte**](https://web.dev/ttfb/) (**TTFB**): İstemcinin, sayfa içeriğindeki ilk byte’a eriştiği ana kadar geçen süre.
- [**First Contentful Paint**](https://web.dev/first-contentful-paint/) (**FCP**): Kullanıcının sayfayı açması ve tarayıcının sayfa içeriğindeki ilk parçayı render etmesi arasında geçen süre.
- [**Largest Contentful Paint**](https://web.dev/lcp/) (**LCP**): Sayfadaki ana içeriğin yüklenmesi ve render olması arasında geçen süre.
- [**Time To Interactive**](https://web.dev/interactive/) (**TTI**): Sayfanın yüklenmeye başladığı andan, kullanıcı ile tamamen etkileşime geçebildiği ana kadar geçen süre.
- [**Cumulative Layout Shift**](https://web.dev/cls/) (**CLS**): Tasarımdaki beklenmedik kaymaları önlemek için sayfa öğelerinin görsel kararlılığı ölçer.
- [**First Input Delay**](https://web.dev/fid/) (**FID**): Kullanıcının sayfa ile etkileşime girdiği andan, event handler’ların çalışmaya hazır olduğu ana kadar geçen süre.

Farklı render tekniklerini öğrenirken bu terimlere aşina olmak yararlı olacaktır:

- [**Compiling**](https://tr.wikipedia.org/wiki/Derleyici) : JavaScript’in yerel makine koduna dönüştürülmesi.
- **Execution time**: Daha önce [getirilmiş (fetched)](https://www.computerhope.com/jargon/f/fetch.htm#:~:text=Fetch%20is%20the%20retrieval%20of,or%20displayed%20on%20a%20screen.) , [ayrıştırılmış (parsed)](https://stackoverflow.com/a/2933452/15124819) ve [derlenmiş (compiled)](https://www.educative.io/answers/what-is-a-compiled-language) verileri yürütmek için gereken süre.
- [**Hydration**](<https://en.wikipedia.org/wiki/Hydration_(web_development)>): Sunucu tarafında render edilmiş HTML içeriklerine işleyiciler (handlers) ekleyerek bileşenleri etkileşimli hale getirir.
- **Idle**: Herhangi bir işlem yapmadığı zaman tarayıcının durumu.
- **Loading time**: Verinin sunucudan alınması süresince geçen zaman.
- **Main thread**: Tarayıcının tüm JavaScript’i çalıştırdığı, [layout, reflow](https://www.holisticseo.digital/technical-seo/layout-trashing/) ve [garbage collection](https://tr.javascript.info/garbage-collection) gibi işlemleri yaptığı ana thread.
- **Parsing**: HTML kaynağını DOM düğümlerine dönüştürüp, [AST](https://www.digitalocean.com/community/tutorials/js-traversing-ast) oluşturur.
- **Processing**: Önceden getirilmiş verinin parse, compile ve execute edilmesi.
- **Processing time**: Önceden getirilmiş verinin parse ve compile olması süresince geçen zaman.

# **01-Client-Side Rendering**

Uygulamanızın kullanıcı arayüzünü istemci tarafında render edin.

## Tanımlama

Bir kullanıcı, istemci-taraflı bir uygulama render etmek istediği zaman, sunucu ilk olarak HTML dosyası ile yanıt verir.

İstemci bu HTML dosyasına ulaştıktan sonra, `HTML parser` içeriği parse eder ve dosya içerisindeki `script` etiketine ulaştığında JavaScript paketini indirmeye başlar.

JavaScript paketi indiğinde sayfanın içeriğini oluşturmaya başlar. İçerik, dinamik bir şekilde [DOM ağacına](https://javascript.info/dom-nodes) eklenir ve sonucunda sayfa içeriği kullanıcının ekranında render olur.

## Uygulama

Temel bir istemci-taraflı uygulama en az iki dosyadan oluşur.

İlk olarak, sayfa içeriğini dinamik olarak oluşturacak olan JavaScript dosyasını içeren bir HTML dosyasına sahip olmalıyız.

```html
<html>
  <body>
    <div id="root"></div>
    <script src="/bundle.js"></script>
  </body>
</html>
```

Ayrıca, DOM ağacını güncelleme ve dinamik olarak verileri oluşturma yöntemlerini içeren bir JavaScript dosyasına ihtiyacımız var. Bu dosya HTML parse işlemi sonrasında (veya sırasında) fetch edilir.

```js
const root = document.getElementById("root");

// DOM manipülasyonu
root.appendChild(...)
```

## Avantaj / Dezavantajlar

## Performans

- **TTFB**: HTML dosyası ilk başta büyük bileşenler içermediği için, The Time To First Byte hızlı gerçekleşebilir.
- **FCP**: JavaScript paketi indikten sonra parse ve execute edilir ve sonrasında The First Contentful Paint çalışabilir.
- **TTI**: JavaScript paketi indikten sonra parse ve execute edilir. Bunun sonucunda event handler’lar bileşenlere bağlanır ve The Time To Interactive çalışabilir.
- **LCP**: Eğer sayfa içeriğinde büyük boyutlu görsel veya video içeren bileşenler yoksa, The Largest Contentful Paint ve First Contentful Paint aynı anda çalışabilir.

## Artılar

**Etkileşim**: Oluşan içerik anında etkileşimlidir. İndirilen JavaScript paketi, event handler’ları doğrudan DOM node’larına ekleyebilir. Diğer render modellerinin aksine, kullanıcılar asla görünür fakat etkileşimli olmayan bir sayfa ile bırakılmaz.

**Tek taraflı sunucu**: Web uygulamasının tamamı ilk istek üzerine yüklenir. Kullanıcı bağlantılara tıklayarak gezinirken, bu sayfaları oluşturmak için sunucu tarafında herhangi yeni bir istek oluşturulmaz.

## Eksiler

**Paket boyutu**: Modern uygulamalar genellikle birden çok sayfa içerir ve bu yüzden JavaScript paketinin boyutu kolaylıkla büyüyebilir. Paket ne kadar büyük olursa, ilk içerik görünür ve etkileşimli olmadan önce JavaScript’i indirmek ve execute etmek o kadar uzun sürer.

**SEO**: Büyük boyutlardaki paketler ve fazla API istekleri içeriğin dizine eklenmesi için yeterince hızlı bir render süresi oluşturamamasına sebebiyet verebilir. Uygulanan çözümler istemci-taraflı bir web uygulamasını SEO dostu hale getirmek ve bir tarayıcının JavaScript’i anlama yeteneğinin sınırları etrafında çalışmak için gereklidir.

# 02-Static Rendering

Önceden oluşturulmuş HTML içeriğini, web sitesinin yapılandırıldığı anda (build time) edinin.

## Tanımlama

HTML içeriğini, statik render tekniği ile sayfa yapılanmadan önce oluşturulur.

Bir kullanıcı statik olarak oluşturulan bir uygulamaya istek attığında, sunucu HTML dosyası ile yanıt verir.

İstemci bu HTML dosyasına eriştikten sonra, HTML parser içeriği ayrıştırır ve ekrana etkileşimli olmayan bir içerik render eder. Eğer bir `script` etiketine rastlanırsa, istemci bu paketi de indirmek için ek bir istek gönderir.

İstemci JavaScript dosyasını indirir ve çalıştırır. Sayfaya etkileşim kazandıracak olan olay dinleyicileri HTML öğelerine eklenir.

## Uygulama

Statik olarak oluşturulan bir uygulamada en az bir HTML dosyasına ihtiyaç vardır ve bu dosya ekrana yazdırılacak olan bütün sayfa içeriğini kapsamalıdır.

```js
const listings = \[({ id: 1, address: "..." }, { id: 2, address: "..." }, \)];

export default function Home() {
  return <Listings listings={listings} />;
}
```

Eğer sayfa içeriğindeki bileşenler etkileşimliyse, olay dinleyicileri oluşturulmuş HTML öğelerine bağlamak amacıyla ek bir JavaScript dosyası gerekebilir.

> Örneğin kodlarına [SlackBlitz](https://stackblitz.com/edit/nextjs-t6w9gw?embed=1&hideExplorer=0&file=components/Listings.js) üzerinden erişebilirsiniz.

## Avantaj / Dezavantajlar

## Performans

- **TTFB**: HTML dosyası ilk başta büyük bileşenler içermediği için, The Time To First Byte hızlı gerçekleşebilir.
- **FCP**: JavaScript paketi indikten sonra parse ve execute edilir ve sonrasında The First Contentful Paint çalışabilir.
- **TTI**: JavaScript paketi indikten sonra parse ve execute edilir. Bunun sonucunda event handler’lar bileşenlere bağlanır ve The Time To Interactive çalışabilir.
- **LCP**: Eğer sayfa içeriğinde büyük boyutlu görsel veya video içeren bileşenler yoksa, The Largest Contentful Paint ve First Contentful Paint aynı anda çalışabilir.

## Artılar

**Ön belleğe alınabilirlik**: Önceden oluşturulmuş HTML dosyaları ön belleğe alınabilir ve global CDN tarafından sunulabilir. Kullanıcıların sayfaya attığı istek orijinal sunucuya gitmeyeceği için daha hızlı bir yanıt alabilirler.

**SEO**: HTML içeriği, ek bir efor gerekmeden arama robotları tarafından oluşturulabilir.

**Bulunabilirlik**: Statik dosyalar her zaman ulaşılabilirdir. Arka yüz veya veri kaynağınız (örn. veritabanı) çökse bile, önceden oluşturulan sayfalar hala erişilebilir olacaktır.

**Backend yükü**: Statik oluşturma ile, arka yüz veya API bütün istekleri almak zorunda kalmayabilir. Sayfayı oluşturan kodların her istek üzerinde çalışması gerekmez.

## Eksiler

**Dinamik veri**: Statik olarak oluşturulan bir sayfanın, örneğin harici bir veri kaynağından gelen dinamik bir içeriğe ihtiyacı varsa fetch işlemini istemci-taraflı yapması gerekir. Bu durum uzun sürebilecek bir LCP ve yüksek maliyetli sunucu masraflarına yol açabilir.

# Dinamik Veri

Statik oluşturulan sayfalara dinamik veri eklemenin birçok yolu vardır ve aradaki fark verinin ne zaman fetch edileceğidir.

## Dinamik veriyi sayfanın oluştuğunda fetch’lemek

## Tanımlama

Build zamanında sunucu tarafından verileri getirebilir ve getirilen verilere göre HTML dosyaları üretebiliriz. Framework ve kütüphaneler genellikle statik sayfalara dinamik verileri kolayca eklemek için yöntemler sunarlar.

## Uygulama

Next.js kullanıyorsanız, `getStaticProps` metodu ile bu işlemi gerçekleştirebilirsiniz.

```js
import { Listings, ListingsSkeleton } from "../components";

export default function Home(props) {
  return <Listings listings={props.listings} />;
}

export async function getStaticProps() {
  const res = await fetch("https://my.cms.com/listings");
  const listings = await res.json();
  return { props: { listings } };
}
```

Bu yöntem, sayfa oluşurken sunucu tarafında çalışır ve fetch edilen veri ile HTML içeriğini oluşturur.

> Örneğin kodlarına [SlackBlitz](https://stackblitz.com/edit/nextjs-qja71c?embed=1&hideExplorer=0&file=pages/index.js) üzerinden erişebilirsiniz.

## Avantaj / Dezavantajlar

## Performans

- **TTFB**: HTML dosyasi ilk başta büyük bileşenler içermediği için, The Time To First Byte hızlı gerçekleşebilir.
- **FCP**: JavaScript paketi indikten sonra parse ve execute edilir ve sonrasında The First Contentful Paint çalışabilir.
- **TTI**: JavaScript paketi indikten sonra parse ve execute edilir. Bunun sonucunda event handler’lar bileşenlere bağlanır ve The Time To Interactive çalışabilir.
- **LCP**: Eğer sayfa içeriğinde büyük boyutlu görsel veya video içeren bileşenler yoksa, The Largest Contentful Paint ve First Contentful Paint aynı anda çalışabilir.

## Artılar

**Statik yararlar**: HTML dosyalarını statik olarak oluşturduğumuzdan dolayı, statik oluşturulan sayfaların bize sunduğu verileri önbellekte tutabilme, güzel bir SEO desteği, bulunabilirlik ve backend’e daha az yüklenme gibi bazı özelliklerden faydalanabiliriz.

**Dinamik veri**: `getStaticProps` metodu, dinamik verileri kullanmamıza ve verileri build zamanında yenilememize olanak sağlar.

## Eksiler

**Verileri yenilemek için redeployment gereklidir**: Veriler yalnızca sayfa oluştuğunda üretildiğinden, sayfanın içeriğini güncellemek için web sayfasını yeniden deploy etmek gerekir.

**Uzun build süreleri**: Eğer uygulamanız önceden render olması gereken bir çok sayfa içeriyorsa, bu yöntem uzun süren build süreleri ile sonuçlanabilir.

## Dinamik verileri istemci tarafında fetch’lemek

## Tanımlama

Statik olarak oluşturulan bir sayfaya, dinamik veri eklemenin bir yolu da istemci-taraflı `fetch` yaklaşımını kullanmaktır. ([client-side data fetching](https://nextjs.org/docs/basic-features/data-fetching/client-side)) Bu yöntem genellikle her istekte güncellenmesi gereken verilere sahip sayfalar için harika bir modeldir.

## Uygulama

Verileri istemci-taraflı olarak getirmek için [SWR](https://swr.vercel.app/)’ı kullanabilir ve veriler getirilirken bir iskelet bileşeni gösterebiliriz.

```js
import useSWR from "swr";
import { Listings, ListingsSkeleton } from "../components";

export default function Home() {
  const { data, loading } = useSWR("/api/listings", (...args) =>
    fetch(...args).then((res) => res.json())
  );

  if (loading) {
    return <ListingsSkeleton />;
  }

  return <Listings listings={data.listings} />;
}
```

> Örneğin kodlarına [SlackBlitz](https://stackblitz.com/edit/nextjs-h2sds6?embed=1&hideExplorer=0&file=pages/_app.js) üzerinden erişebilirsiniz.

## Avantaj / Dezavantajlar

## Performans

- **TTFB**: HTML dosyası ilk başta büyük bileşenler içermediği için, The Time To First Byte hızlı gerçekleşebilir.
- **FCP**: HTML parse ve render olduktan sonra The First Contentful Paint gerçekleşebilir.
- **LCP**: Eğer sayfa içeriğinde büyük boyutlu görsel veya video içeren bileşenler yoksa, The Largest Contentful Paint ve First Contentful Paint aynı anda çalışabilir.
- **TTI**: HTML render olurken JavaScript paketi indirilir, parse ve execute edilir Bunun sonucunda event handler’lar bileşenlere bağlanır ve The Time To Interactive gerçekleşebilir.

## Artılar

**Statik yararlar**: HTML dosyalarını statik olarak oluşturduğumuzdan dolayı, statik oluşturulan sayfaların bize sunduğu verileri önbellekte tutabilme, güzel bir SEO desteği, bulunabilirlik ve backend’e daha az yüklenme gibi bazı özelliklerden faydalanabiliriz.

## Eksiler

**Sunucu masrafları**: Veriler her sayfa yüklendiğinde sunucu tarafından alınacağı için yüksek sunucu maliyetlerine neden olabilir.

**Tasarımda bozulma**: İskelet bileşenler render olan bileşenlerin boyutlarına uygun olmazsa tasarımda kaymalar gerçekleşebilir.

# **03-Incremental Static Regeneration**

Belirli sayfaları önceden oluşturup, diğer sayfaları talep üzerine oluşturun.

## Tanımlama

Statik render işlemi, performans açısından bir çok fayda sağlar fakat pre-render edilecek çok fazla sayfa varsa build zamanı uzun sürebilir. Ayrıca, sayfa içeriğini web sitesini tekrardan deploy ederek güncelleyebiliriz ve bu da iyi bir kullanıcı deneyimi sunmaz.

Incremental Static Generation, yalnızca bir sayfanın alt kümesini, örneğin kullanıcı tarafından talep edilmesi muhtemel olan sayfaları pre-render eder. Geri kalan sayfalar talep üzerine render edilir. Kullanıcı henüz pre-render edilmemiş bir sayfaya ulaşmak istediğinde, sayfa server-rendered olarak oluşturulur ve sonrasında CDN tarafından önbelleğe alınır.

Bir sayfanın alt kümesini pre-render etmenin yanı sıra, [`stale-while-revalidate`](https://web.dev/stale-while-revalidate/) yaklaşımını kullanarak, önbelleğe alınmış sayfaların otomatik olarak geçersiz kılabiliriz. Kullanıcı `stale` - yani önbelleğe alınması gereken sayıdan daha uzun süredir önbellekte olan - durumundaki bir sayfayı istediğinde arka planda otomatik olarak bir yeniden oluşturma işlemi tetiklenir. Bu gerçekleşirken, kullanıcı stale durumundaki sayfayı görür fakat sonradan gelen istekle güncellenmiş içeriği görebilir.

## Uygulama

Incremental Static Regeneration yöntemini uygulayabilmek için Next.js’in bize sunduğu `getStaticProps` ve `getStaticPaths` metotlarını birlikte kullanabiliriz.

```js
import { Listings, ListingsSkeleton } from "../components";

export default function Listing(props) {
  return <ListingLayout listings={props.listing} />
}

export async function getStaticProps(props) {
  const res = await fetch(`https://my.cms.com/listings/${props.params.id}`);
  const { listing } = await res.json();

  return { props: { listing } }
}

export function getStaticPaths() {
  const res = await fetch(`https://my.cms.com/listings?limit=20`);
  const { listings } = await res.json();

  return {
    params: listings.map(listing => ({ id: listing.id })),
    fallback: false
  }
}
```

## Avantaj / Dezavantajlar

- **TTFB**: HTML dosyası ilk başta büyük bileşenler içermediği için, The Time To First Byte hızlı gerçekleşebilir.
- **FCP**: HTML parse ve render olduktan sonra The First Contentful Paint gerçekleşebilir.
- **LCP**: The Largest Contentful Paint, büyük boyutta görsel veya videolar içeren bileşenler olmadığı sürece First Contentful Paint ile aynı anda gerçekleşebilir.
- **TTI**: HTML render olduktan sonra The Time To Interactive gerçekleşebilir ve JavaScript paketi indikten sonra parse ve execute edilir. Bunun sonucunda event handler’lar bileşenlere bağlanır.

## Artılar

**Statik yararlar**: HTML dosyalarını statik olarak oluşturduğumuzdan dolayı, statik oluşturulan sayfaların bize sunduğu verileri önbellekte tutabilme, güzel bir SEO desteği, bulunabilirlik ve backend’e daha az yüklenme gibi bazı özelliklerden faydalanabiliriz.

## Eksiler

**Sunucu masrafları**: Verilerimiz her saniye güncellenmeyebilir, bu da gereksiz bir önbellek geçersizleştirme ve sayfa yeniden üretme sonucuna neden olabilir.

# 04-Server-Side Rendering

Her istekte HTML oluşturun.

## Tanımlama

Sunucu-taraflı render yöntemi ile, HTML dosyalarını istek bazlı olarak sunucu tarafında (ya da [serverless functions / sunucusuz fonksiyonlar](https://vercel.com/docs/concepts/functions/serverless-functions)) oluşturabiliriz.

Bir kullanıcı, sunucu-tarafında render olan bir uygulamaya istek attığında, sunucu HTML dosyalarını oluşturur ve istemciye gönderir.

Tarayıcı, başlangıçta etkileşimsiz halde olan HTML öğelerini içeren bu içeriği oluşturur. Olay dinleyicileri bileşenlere bağlamak adına, istemci ek bir istek atarak JavaScript paketini indirir. Sonrasında`[hydration](https://en.wikipedia.org/wiki/Hydration_(web_development)` işlemi gerçekleşir.

## Uygulama

Sunucu taraflı bir uygulamayı oluştururken, sunucudaki React bileşenlerinin HTML içeriğini oluşturabilmesi ve istemci tarafındaki etkileşimsiz HTML dosyalarını hydrate edebilmesi için bir metoda ihtiyaçları vardır. HTML’i sunucuda render etmenin bir yöntemi `renderToString` metodunu kullanmaktır. Bu fonksiyon, React öğesine karşılık gelen bir HTML dizisini geri döndürür. Bu şekilde HTML, istemci tarafında daha hızlı bir sayfa yüklenme süresi ile render olabilir ve `hyderateRoot` metodu istemci tarafında çalışarak `[hydration](https://en.wikipedia.org/wiki/Hydration_(web_development))` işlemini gerçekleştirebilir.

[Next.js](https://nextjs.org/), [Remix](https://remix.run/) ve [Astro](https://astro.build/) gibi React framework’leri, sunucu-taraflı render yöntemini uygulamayı kolaylaştıran işlevlere sahiptir.

Next.js kullanırken, `getServerSideProps` fonksiyonu sayesinde bir sayfayı sunucu taraflı olarak oluşturabiliriz.

```js
import { Listings, ListingsSkeleton } from "../components";

export default function Home(props) {
  return <Listings listings={props.listings} />;
}

export async function getServerSideProps({ req, res }) {
  const res = await fetch("https://my.cms.com/listings");
  const listings = await res.json();

  return {
    props: { listings },
  };
}
```

Bu metot her istekte çalışır ve sayfada bulunan prop’ları dinamik bir şekilde oluşturur.

> Örneğin kodlarına [SlackBlitz](https://stackblitz.com/edit/nextjs-jqm32p?embed=1&hideExplorer=0&file=pages/index.js) üzerinden erişebilirsiniz.

## Avantaj / Dezavantajlar

- **TTFB**: Sayfalar talep üzerine oluşturulacağı için, Time To First Byte yavaş olabilir.
- **FCP**: HTML parse edilip oluşturulduktan sonra First Contentful Paint gerçekleşebilir.
- **LCP**: Largest Contentful Paint, büyük görsel ve videolar içeren büyük bileşenler olmadığı sürece First Contentful Paint ile aynı anda gerçekleşebilir.
- **TTI**: HTML render olduktan sonra The Time To Interactive gerçekleşebilir ve JavaScript paketi indikten sonra parse ve execute edilir. Bunun sonucunda event handler’lar bileşenlere bağlanır.

## Artılar

**Kişiselleştirilmiş sayfalar**: Sunucu taraflı render yöntemi, istek tabanlı verileri yönetmek için — kullanıcı cookie’leri gibi- kullanışlı bir yöntemdir.

**Render blocking**: Sunucu taraflı render yöntemi, kimlik doğrulama tabanlı sayfaların oluşturulmasını engelleyebilir.

## Eksiler

**İlk yükleme**: Sayfalar kullanıcı istekte bulunduğu müddetçe oluşturulacağı için, kullanıcının ekranda oluşturulacak içeriği görmesi biraz zaman alabilir. SSR’ı optimize etmek için:

1.  Veri tabanı sorgularını optimize edin. Eğer sunucunuz ve veri tabanınız arasındaki mesafe uzaksa, bu veriyi saptamak ve almak biraz zaman alabilir. Veritabanınızı veya sunucunuzu birbirine yaklaştırmayı düşünün.
2.  İsteklerinizin header kısmına `Cache-Control` ekleyin.

**Ulaşılabilirlik**: Eğer sunucunuz veya bölgenizde çıkan bir sorun sebebiyle bağlantınız koparsa, web sayfanız da kullanılamaz hale gelir.

# 05-Streaming Server-Side Rendering

Her istekte HTML oluşturun ve parça parça uygulamaya gönderin.

## Tanımlama

Streaming Server-Side render yöntemi, bileşenleri oluştukları anda istemci tarafına göndermemizi sağlar.

Normal Server-Side render yönteminde kullanıcı, istemciye gönderilmeden önce HTML dosyasının tamamının sunucuda oluşmasını beklemek durumundadır. `hydration` işlemi başlamadan, bütün paket indirilmeli ve yürütülmelidir.

Fakat, Streaming Server-Side render yöntemi bileşenleri hazır oldukları anda uygulama akışına dahil eder.

## Uygulama

> Makalenin orijinalinde bu yöntem ile ilgili olarak herhangi bir örnek kullanım paylaşılmamış. [Bu bağlantı](https://www.patterns.dev/posts/ssr/) üzerinden konsept ile ilgili daha detaylı bilgi sahibi olabilirsiniz.

## Avantaj / Dezavantajlar

- **TTFB**: Sayfalar talep üzerine oluşturulacağı için, Time To First Byte yavaş olabilir.
- **FCP**: HTML parse edilip oluşturulduktan sonra First Contentful Paint gerçekleşebilir.
- **LCP**: Largest Contentful Paint, büyük görsel ve videolar içeren büyük bileşenler olmadığı sürece First Contentful Paint ile aynı anda gerçekleşebilir.
- **TTI**: HTML render olduktan sonra The Time To Interactive gerçekleşebilir ve JavaScript paketi indikten sonra parse ve execute edilir. Bunun sonucunda event handler’lar bileşenlere bağlanır.

## Artılar

**Sayfa boyutuna bakmaksızın performans sağlar**: Streaming SSR yöntemi ile, ilk byte sunucuda oluşturma başladıktan hemen sonra istemciye iletileceği için hızlı TTFB sonuçları alabiliriz. Bu, sayfa boyutuna bakmaksızın iyi bir TTFB ile sonuçlanır.

**Network Backpressure**: Eğer ağ tıkanırsa ve daha fazla veri akışı yapmaya müsait değilse, renderer bir sinyal alır ve ağ temizlenene kadar akışı durdurur. Böylece, sunucu daha az hafıza kullanır ve I/O koşulları daha duyarlı olur. Bu, Node.js sunucunuzun aynı anda birden fazla istek oluşturmasını sağlar ve daha ağır isteklerin daha uzun süre daha hafif istekleri engellemesini önler. Sonuç olarak, uygulama zor koşullarda bile duyarlı bir şekilde çalışmaya devam eder.

## Eksiler

**Destek**: Bütün runtime’lar HTTP streaming özelliğini desteklemeyebilir ama bu özellik SSR için gereklidir.

# Kaynaklar

- [JavaScript Patterns](https://javascriptpatterns.vercel.app/patterns)
- [JavaScript & React Patterns](https://frontendmasters.com/courses/tour-js-patterns/)
