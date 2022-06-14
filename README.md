![logo_ironhack_blau 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# LAB | Carro Ironhack

![carret de compra](https://user-images.githubusercontent.com/76580/167435963-34b5ddf0-e318-446a-b59f-2edeed3eb030.gif)

## Introducció

El comerç electrònic ha demostrat ser un gran canvi de joc en l'economia del segle XXI. Com a un dels canals de vendes més grans, després de la venda física, [s'espera](https://www.statista.com/statistics/379046/worldwide-retail-e-commerce-sales/) que el comerç electrònic sigui responsable de 6,3 bilions de dòlars en vendes a tot el món l'any 2024.

El comerç electrònic és un negoci altament competitiu i crear una experiència d'usuari positiva és crucial per retenir els clients i millorar les conversions. No és estrany que les empreses facin una inversió important per optimitzar el flux de compres a les seves plataformes de comerç electrònic.

Un dels components més significatius d'aquesta experiència és **el carretó de la compra** .

En aquest laboratori, construirem l' **IronCart** , un carretó de la compra per a la botiga de marxandatge no oficial Ironhack. Els visitants haurien de poder afegir i eliminar productes del carretó de la compra i modificar el nombre (quantitat) d'articles que volen comprar. A més, els usuaris haurien de poder veure el subtotal i el preu total dels articles que han afegit.

## Requisits

- Bifurca aquest repo.
- Clona'l a la teva màquina.

## Submissió

- En finalitzar, executeu les ordres següents:

```bash
$ git add .
$ git commit -m "Solved lab"
$ git push origin master
```

- Creeu una sol·licitud d'extracció perquè els vostres TA puguin comprovar el vostre treball.

## Instruccions

Fareu la major part del vostre treball al fitxer `js/index.js` . Hem afegit el marcatge inicial a `index.html` i un estil bàsic. Durant el desenvolupament, assegureu-vos d'utilitzar els mateixos noms de classe que els que ja s'utilitzen (i disponibles al fitxer CSS) per fer que el nostre carretó de la compra sigui agradable i net.

Som-hi!

<br/>

<hr/>

### Nota sobre les proves

Aquest LAB està equipat amb proves unitàries per proporcionar comentaris automatitzats sobre el progrés del vostre laboratori.

**Després de completar les iteracions bàsiques** , aneu a la secció **"Prova el teu codi"** a la part inferior. Allà se us demanarà que instal·leu les dependències de prova i que executeu les proves per comprovar quantes proves està passant el vostre codi. Un cop hàgiu executat les proves, corregiu el codi per superar les proves fallides.

<hr/>

<br/>

### Iteració 1: `updateSubtotal`

Obriu l' `index.html` al vostre navegador. Com podeu veure, només hi ha una fila a la taula que representa un producte. En aquesta primera iteració, us centrareu només en aquest producte i, més endavant, us ajudarem a pensar en maneres d'actualitzar-vos per tenir diversos productes.

Fem una ullada al codi HTML proporcionat. Tenim l' **etiqueta de la taula amb l'identificador `#cart`** , tal com es mostra a continuació:

```html
<table id="cart">
  <thead>
    <tr>
      <th>Product Name</th>
      <th>Unit Price</th>
      <th>Quantity</th>
      <th>Subtotal</th>
      <th>Action</th>
    </tr>
  </thead>
  <tbody>
    <tr class="product">
      <!-- ... -->
    </tr>
  </tbody>
  <!-- ... -->
</table>
```

![](https://i.imgur.com/zCWQYg2.png)

L'únic producte que tenim actualment al nostre `#cart` es col·loca a la `tr` amb el `product` de classe ( **que va dins de `tbody`** ):

```html
<tr class="product">
  <td class="name">
    <span>Ironhack Rubber Duck</span>
  </td>
  <td class="price">$<span>25.00</span></td>
  <td class="quantity">
    <input type="number" value="0" min="0" placeholder="Quantity" />
  </td>
  <td class="subtotal">$<span>0</span></td>
  <td class="action">
    <button class="btn btn-remove">Remove</button>
  </td>
</tr>
```

El producte té un **preu** i una **quantitat** (on la quantitat representa quants articles d'un producte específic ha afegit un usuari al carretó). Al codi proporcionat, veiem que també hi ha un **preu subtotal** . El preu subtotal serà el resultat de la _multiplicació_ d'aquests valors.

El vostre objectiu és calcular el preu subtotal, però anem a abordar-lo gradualment. Desglossem-ho en un parell de passos:

- **Pas 0** : en aquest pas, el nostre objectiu és ajudar-vos a entendre el codi proporcionat al `js/index.js` . Gràcies al codi proporcionat, el botó `Calculate Prices` ja té alguna funcionalitat. Utilitzant la manipulació DOM, vam obtenir l'element amb l' `id="calculate"` i hi vam afegir un `click` d'escolta d'esdeveniments. Quan es fa clic, aquest botó activarà la funció `calculateAll()` . El fragment de codi següent fa exactament el que hem explicat:

```javascript
// js/index.js

window.addEventListener('load', () => {
  const calculatePricesBtn = document.getElementById('calculate');
  calculatePricesBtn.addEventListener('click', calculateAll);
});
```

No us confongueu amb el mètode [.addEventListener()](https://www.w3schools.com/jsref/met_document_addeventlistener.asp) , fa exactament el mateix que [onclick()](https://www.w3schools.com/tags/ev_onclick.asp) , amb algunes diferències sobre les quals podeu trobar més informació [aquí](https://stackoverflow.com/questions/6348494/addeventlistener-vs-onclick) . En aquest laboratori, podeu utilitzar el mètode que preferiu.

D'acord, passem a la funció `calculateAll()` . En aquesta funció, hem utilitzat `querySelector` per obtenir el primer (i actualment l'únic) element DOM amb la classe `product` . Aquest element (que hem desat a la variable anomenada `singleProduct` ) es passa com a argument a la funció `updateSubtotal()` . Com podeu trobar als comentaris, el fragment de codi proporcionat només s'utilitza amb finalitats de prova dins de la iteració 1.

```js
function calculateAll() {
  // code in the following two lines is added just for testing purposes.
  // it runs when only iteration 1 is completed. at later point, it can be removed.
  const singleProduct = document.querySelector('.product');
  updateSubtotal(singleProduct);
  // end of test

  // ITERATION 2
  //...
  // ITERATION 3
  //...
}
```

I finalment, vam iniciar la `updateSubtotal(product)` . De moment, l'únic que fa aquesta funció és avisar `Calculate Prices clicked!` quan es fa clic al botó _Calcula preus_ .

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_e50868b669d962f3ddb26802e5a16638.gif)

Comencem:

- **Pas 1** : dins `updateSubtotal()` , creeu dues variables (suggerim anomenar-les `price` i `quantity` ) i utilitzeu les vostres habilitats de manipulació DOM que acabeu d'aconseguir per OBTENIR elements DOM que contenen preu i quantitat. Per donar-vos un avantatge, podeu utilitzar el codi següent per obtenir l'element DOM que tingui el `price` :

```js
// js/index.js
function updateSubtotal(product) {
  const price = product.querySelector('.price span');
  // ... your code goes here
}
```

- **Pas 2** : ara, quan tingueu els elements DOM esmentats anteriorment, el vostre següent pas hauria de ser extreure'n els valors específics. _Pista_ : potser l' `innerHTML` sona? En cas que tingueu curiositat per trobar altres maneres d'aconseguir el mateix resultat, podeu començar per comprovar [`textContent`](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) i [`innerText`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/innerText) a Google. A més, podeu extreure el valor d'una entrada accedint a [la propietat de `value` de l'element d'entrada](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#htmlattrdefvalue) . Tanmateix, no us distreu aquí, que aquesta sigui la vostra _lectura addicional_ quan finalitzeu el laboratori.

- **Pas 3** : utilitzeu els valors que heu extret dels elements DOM esmentats anteriorment per calcular el preu subtotal. Podeu crear una variable nova i el seu valor serà igual al producte d'aquests valors. Ex. Si un usuari va triar 3 ànecs de goma Ironhack, hauria de veure que el subtotal és de 75 dòlars ( 25 \\\* 3 = 75).

- **Pas 4** : ara, quan us convertiu en ninja de manipulació DOM, feu servir les vostres habilitats una vegada més per obtenir l'element DOM que hauria de contenir el subtotal. _Pista_ : és l'element amb el `subtotal` de classe.

- **Pas 5** : al pas 3, heu calculat el preu subtotal i, al pas 4, heu obtingut l'element DOM que hauria de mostrar aquest preu. En aquest pas, el vostre objectiu és establir el preu subtotal a l'element DOM corresponent. A més, assegureu-vos que aquesta funció retorni el valor del subtotal perquè pugueu utilitzar-lo més tard quan sigui necessari.

En aquesta iteració, heu acabat de crear la funció `updateSubtotal` que **calcula el subtotal** d'aquest producte específic, **actualitza el valor del subtotal** d'aquest mateix producte al DOM i retorna el **valor del subtotal** .

Com a argument únic, la funció hauria de prendre un **node DOM** que correspongui a un sol element `tr` amb una classe de `product` . Al codi boilerplate inclòs, l'hem anomenat `product` .

```js
function updateSubtotal(product) {
  // ...
}
```

:bulb : _Suggeriment_ : Assegureu-vos que la vostra funció `calculateAll()` tingui el codi de prova que hem esmentat anteriorment. Si el codi està al seu lloc després d'haver acabat correctament la funció `updateSubtotal()` , hauríeu de poder veure les actualitzacions corresponents al camp `Subtotal` de la taula.

Comproveu [aquí](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload\_30b87c596b79954f63b3482d2f320fe4.gif) la sortida esperada.

<hr/>

### Iteració 2: `calculateAll()`

El nostre codi actual funciona perfectament per a un producte, però esperem tenir més d'un producte al nostre carretó. Com a tal, utilitzarem `calculateAll` per activar l'actualització dels subtotals de cada producte.

Completa la funció anomenada `calculateAll()` . El seu propòsit és cridar la funció `updateSubtotal` amb cada node DOM `tr.product` a la `table#cart` .

Per començar, elimina o comenta el codi existent dins de `calculateAll()` (el codi que hem utilitzat per a la prova). A més, afegim un producte nou al nostre fitxer `index.html` . Dupliqueu el `tr` amb la classe `product` , canvieu el nom del producte dins i canvieu el preu del producte.

![](https://i.imgur.com/Pv4NmR8.png)

:bulb : _Suggeriment_ : comenceu per obtenir els nodes DOM per a cada fila de producte. Actualment, tenim dos productes; per tant, tenim dues files amb el `product` de classe . Potser utilitzar [getElementsByClassName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName) podria servir bé aquí.

```js
function calculateAll() {
  // ...
}
```

La sortida final hauria de semblar a la següent:

![](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload_0efb56fc0e5717469417e806fa7cde12.gif)

<hr/>

### Iteració 3: total

La nostra funcionalitat de càlcul encara està incompleta. El subtotal de cada producte s'està actualitzant, però el valor total no es modifica.

Al final de la funció `calculateAll()` , reutilitzeu el valor total que acabeu de calcular a la iteració anterior i actualitzeu l'element DOM corresponent. Calcula el preu total dels productes del teu carretó sumant tots els subtotals retornats per `updateSubtotal()` quan es va cridar amb cada producte.

Finalment, mostreu aquest valor al vostre DOM.

![](https://i.imgur.com/SCtdzMd.png)

<hr/>

## Iteracions de bonificació

### Iteració 4: eliminació d'un producte

Els usuaris haurien de poder eliminar els productes dels seus carretons. Amb aquesta finalitat, cada fila de producte de la nostra taula té un botó "Elimina" al final.

Però intentem resoldre el nostre problema una mica a la vegada. Dins de la funció existent que esteu passant a `window.addEventListener()` , comenceu consultant el document per tots els botons "Elimina", feu un bucle a través d'ells i afegiu un escolta d'esdeveniments de `click` a cadascun, passant una funció anomenada `removeProduct` com a argument de devolució de trucada. . Si necessiteu una pista sobre com fer-ho, només heu de fer una ullada a com ho hem fet amb l'addició d'un escolta d'esdeveniments a `calculatePricesBtn` .

Ja hem declarat `removeProduct(event)` i hem afegit algun codi d'inici. Un cop hàgiu acabat de consultar els botons d'eliminació i d'afegir-hi l'escolta d'esdeveniments de `click` , obriu la consola i feu clic a qualsevol botó d' `Remove` .

Com podem veure, `removeProduct(event)` espera l'esdeveniment com a argument únic, i això provocarà l'eliminació del producte corresponent del carretó.

:bulb : Consell: per accedir a l'element en què s'ha activat un esdeveniment, podeu fer referència `event.currentTarget` . Per eliminar un node del DOM, heu d'accedir al seu node principal i trucar-hi a [`removeChild`](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild) . Podeu accedir al pare d'un node DOM des de la seva propietat `parentNode` .

Assegureu-vos que el preu s'actualitzi en conseqüència quan elimineu productes del carretó de la compra.

Feu clic [aquí](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload\_17b1e9e4d2606239163dddbc5b2a3d9f.gif) per veure el resultat esperat.

<hr/>

### Iteració 5: Creació de nous productes

Per acabar-ho, permetrem que l'usuari afegeixi un producte personalitzat al seu carretó.

Descomenteu l'element `tfoot` i els seus fills del fitxer `index.html` :

```html
<table>
  <tbody>
    <!-- ... -->
  </tbody>
  <!-- <tfoot>
    <tr class="create-product">
      <td>
        <input type="text" placeholder="Product Name" />
      </td>
      <td>
        <input type="number" min="0" value="0" placeholder="Product Price" />
      </td>
      <td></td>
      <td></td>
      <td>
        <button id="create" class="btn">Create Product</button>
      </td>
    </tr>
  </tfoot> -->
</table>
```

![](https://i.imgur.com/J8aserm.png)

Les dues entrades dins de `tfoot` representen el nom del nou producte i el preu unitari, respectivament. El botó "Crea un producte" hauria d'afegir un producte nou al carretó quan s'activa.

Afegiu un gestor d'esdeveniments de `click` a "Crea un producte" que prendrà una funció anomenada `createProduct` com a devolució de trucada.

A `createProduct` , hauríeu d'orientar els nodes DOM d'entrada de nom i preu unitari, extreure'n els valors, afegir una nova fila a la taula amb el nom del producte i el preu unitari, així com l'entrada de quantitat i el botó "Elimina", i assegurar-vos que tots els la funcionalitat funciona com s'esperava.

Recordeu que el nou producte ha de tenir un aspecte poc distingit i comportar-se com qualsevol dels productes inclosos anteriorment al carretó. Com a tal, s'hauria de poder calcular el seu subtotal quan es fa clic al botó "Calcula-ho tot" i eliminar el producte.

Quan finalitzi la creació del producte, si us plau, esborreu els camps d'entrada del formulari de creació.

Feu clic [aquí](https://s3-eu-west-1.amazonaws.com/ih-materials/uploads/upload\_00abbd15326ec24d93147196024458f6.gif) per veure el resultat esperat.

<br/>

## Prova el teu codi

Tornarem a treballar amb proves automatitzades!

Si us plau, obriu el vostre terminal, canvieu els directoris a l'arrel del laboratori i executeu `npm install` per instal·lar l'executor de proves. Ara, podeu executar l'ordre `npm run test:watch` per executar proves automatitzades en _mode_ de visualització . Obriu el fitxer `lab-solution.html` resultant amb l'extensió VSCode "Servidor en directe" per veure sempre els resultats de la prova.

```bash
$ cd lab-dom-ironhack-cart-cat
$ npm install
$ npm run test:watch
```

En cas que vulgueu consultar les proves per obtenir més detalls, es troben al fitxer `tests/ironhack-cart.spec.js` .

<br/>

**Feliç codificació! 💙**