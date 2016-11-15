# Atoto Frontend Challenge
## Zadání
Aplikace má za úkol získat z API seznam košíků, vzít první (s indexem `0`) a k němu vypočítat hodnotu alternativních košíků u ostatních prodejců. Rovněž vypíše obsah kosíku a patřičný identický nebo alternativní produkt u aktuálně vybraného prodejce.

- Alternativní košíky jsou seřazeny podle celkové ceny od nejlevnějšího, který je i na začátku vybraný.
- Na levé straně jsou položky hlavního košíku, na pravé straně jsou položky aktuálně vybraného košíku.
- Na začátku se načte množství položek v košíku shodně pro levou i pravou stranu. Dále je již možné množství nezávisle měnit, přičemž dojde k přepočtu částek za jednotlivé položky i celkové ceny košíku.
- Pokud alternativní košík obsahuje shodu (`Product.identical`), označí se zeleným rámečkem, pokud alternativu (`Product.replacements`), označí se oranžovou barvou. Pokud není k dispozici náhrada ani alternativa, označí se červeně a vpravo se nezobrazí žádný produkt.
- Jediným omezením je použití Reactu (doporučuji využít `create-react-app`).

## Zdroje
### Grafický návrh
Ve složce `design` je k dispozici `.sketch` soubor a export v PNG.

### API
Pro získání dat je potřeba použít [_Contentful API_](https://www.contentful.com/) s následujícím nastavením:

```
{
	space: 'gytep4to3b7c',
	accessToken: 'f76dbf64be48ac7d5fb09068d06d98a27406e09d02b5575fe4dfc9f14e6410c9'
}
```

## Použité entity
V závorce je typ, který se používá při dotazování API _Contentful_.

### Asset (`asset`)
Soubor uložený v rámci _Contentful_. Tento typ je použitý pro loga prodejců.

```
type Asset = {
	file: {
		url: String
	},
	id: String
}
```

### Prodejce (`provider`)
Online prodejce potravin – například _iTesco_ nebo _Rohlík_.

```
type Provider = {
	id: String,
	logo: Asset,
	slug: String,
	title: String
}
```

### Produkt (`product`)
Konkrétní produkt – je vždy unikátní pro každého prodejce. `identical` je seznam identický produktů u ostatních prodejců, zatímco `replacements` alternativ.

```
type Product = {
	id: String,
	identical: ?Product[],
	image: String,
	price: Number,
	provider: Provider,
	replacements: ?Product[],
	slug: String,
	title: String
}
```

### Položka v košíku (`cartItem`)
Produkt uložený v košíku v určitém množství.

```
type CartItem = {
	amount: Number,
	product: Product
}
```

### Košík (`cart`)
Košík určitého prodejce. V rámci aplikace prosím o použití prvního nalezeného košíku jakožto hlavního (tedy toho, ke kterému se vytvářejí alternativy).

```
type Cart = {
	items: CartItem[],
	provider: Provider
}
```

> To je vše. Hodně štěstí!
