# A dékáni hivatal „működő” kapcsolati űrlapja – hibariport

Repó: https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap
Készítette: Balog Edit, Info III

Tesztkörnyezet: Windows, Google Chrome (DevTools, Lighthouse), W3C HTML validátor (validator.w3.org/nu), W3C CSS validátor (jigsaw.w3.org/css-validator), helyi szerver: Live Server.
Kiinduló állapot: [4f7376c – az eredeti, hibás változat](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/4f7376c53c0b08a97734e1ac0582a22adbe66d5f)

## 1. hiba

- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** Az oldal megnyitása után a képernyőolvasó angol kiejtéssel olvassa fel a magyar szöveget. A W3C validátor figyelmeztet: „Consider adding a lang attribute to the html start tag”.
- **Gyökérok (melyik sor, melyik szabály sérül):** `index.html` 2. sor: a `<html>` elemnek nincs `lang` attribútuma, így a segédtechnológia nem tudja, milyen nyelvű az oldal. Sérül: WCAG 3.1.1 (Az oldal nyelve).
- **Javítás (commit link, előtte/utána kódrészlet):** [09cfdad](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/09cfdad38d5e5f5b1a016815be0f5610d7536ef0)
  - Előtte: `<html>`
  - Utána: `<html lang="hu">`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** W3C validátor: a figyelmeztetés eltűnt. Lighthouse Accessibility: a „`<html>` element does not have a [lang] attribute” jelzés eltűnt.
- **Tanulság / érintett alapelv (pl. Szemantika előbb, stílus később; WCAG-kritérium):** Az oldal nyelvét mindig meg kell adni, mert ettől függ a felolvasás kiejtése (WCAG 3.1.1).

## 2. hiba

- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** A képernyőolvasó a logónál a fájlnevet („logo.png”) olvassa fel. A W3C validátor hibát jelez: „An img element must have an alt attribute”.
- **Gyökérok (melyik sor, melyik szabály sérül):** `index.html` 11. sor: a logó `img` elemén nincs `alt` attribútum, ezért nincs szöveges alternatívája. Sérül: WCAG 1.1.1 (Nem szöveges tartalom).
- **Javítás (commit link, előtte/utána kódrészlet):** [fd9c924](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/fd9c924ae0d89f72abb61af95fa867c7fa42cdd7)
  - Előtte: `<img src="logo.png" width="120" height="40">`
  - Utána: `<img src="logo.png" alt="UMFST – Orvosi Kar" width="120" height="40">`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** W3C validátor: a hiba eltűnt. Lighthouse Accessibility: az „Image elements do not have [alt] attributes” jelzés eltűnt.
- **Tanulság / érintett alapelv (pl. Szemantika előbb, stílus később; WCAG-kritérium):** Minden képnek kell `alt` attribútum: a tartalmat hordozó kép leíró szöveget kap, a díszítő kép üreset (`alt=""`). A hiányzó `alt` hiba, az üres tudatos döntés (WCAG 1.1.1).

## 3. hiba

- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** A képernyőolvasó a mezőknél csak „szerkesztés, üres” vagy „szerkesztés, Teljes név” szöveget mond, gépelés után pedig semmi nem azonosítja a mezőt, mert a placeholder eltűnik. A téma (select) és az üzenet (textarea) mezőnek semmilyen neve nincs.
- **Gyökérok (melyik sor, melyik szabály sérül):** `index.html` 17–24. sor: egyik mezőnek sincs `label` eleme, csak `placeholder`. A placeholder nem címke: gépeléskor eltűnik, és nem ad megbízható akadálymentes nevet. Sérül: WCAG 1.3.1 (Információ és kapcsolatok), 3.3.2 (Címkék vagy utasítások), 4.1.2 (Név, szerep, érték).
- **Javítás (commit link, előtte/utána kódrészlet):** [628ec25](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/628ec25042c528a307fc993031b34965fbb02ca7)
  - Előtte: `<input type="text" name="nev" placeholder="Teljes név" required>`
  - Utána: `<label for="nev">Teljes név</label>` + `<input type="text" id="nev" name="nev" placeholder="Teljes név" required>`
  - Ugyanígy kapott címkét és egyező `id`-t az e-mail (`email`), a téma (`tema`) és az üzenet (`uzenet`) mező.
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** A címkére kattintva a kurzor a hozzá tartozó mezőbe ugrik (a `for`–`id` kapcsolat működik). Lighthouse Accessibility: a „Form elements do not have associated labels” jelzés eltűnt. DevTools Accessibility fül: a mezők „Name” értéke a címke szövege.
- **Tanulság / érintett alapelv (pl. Szemantika előbb, stílus később; WCAG-kritérium):** A placeholder nem címke. Minden űrlapmezőhöz kell egy `label`, amely `for`–`id` párral kapcsolódik hozzá.

## 4. hiba

- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** Mobilon (Chrome DevTools eszköznézet, Ctrl+Shift+M) az e-mail mezőnél a sima betű-billentyűzet jelenik meg, és az űrlap az „alma” szöveget is elfogadja e-mail címnek.
- **Gyökérok (melyik sor, melyik szabály sérül):** `index.html` 18. sor (eredeti számozás): az e-mail mező `type="text"`, ezért a böngésző nem tudja, hogy e-mail címet vár: nincs beépített formátum-ellenőrzés, e-mailes billentyűzet és automatikus kitöltés. A név mezőn sincs `autocomplete` (WCAG 1.3.5, Beviteli cél azonosítása).
- **Javítás (commit link, előtte/utána kódrészlet):** [19a8c59](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/19a8c5976aa7ff0b6fe35c2d5e736499396203a7)
  - Előtte: `<input type="text" id="email" name="email" placeholder="E-mail cím" required>`
  - Utána: `<input type="email" autocomplete="email" id="email" name="email" placeholder="E-mail cím" required>`
  - A név mező kiegészítése: `autocomplete="name"`.
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** Az „alma” beírása után a Küldésre a böngésző megállít, és jelzi, hogy hiányzik a @. Eszköznézetben az e-mail mezőnél a @ jel elérhető a billentyűzeten. A böngésző felajánlja a mentett nevet és e-mail címet.
- **Tanulság / érintett alapelv (pl. Szemantika előbb, stílus később; WCAG-kritérium):** A megfelelő input-típus ingyen ad validációt és jobb mobilos használhatóságot. Ez azonban csak kliensoldali ellenőrzés, a szerveroldali validációt nem helyettesíti.

## 5. hiba

- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** Tab-bal haladva a Küldés gombra kétszer kell lépni, és Enterre nem egyértelmű, hogy a link vagy a gomb működik. A W3C validátor hibát jelez a form utolsó sorainál.
- **Gyökérok (melyik sor, melyik szabály sérül):** `index.html` 25. sor: a gomb egy link belsejében van (`<a href="#"><button>Küldés</button></a>`). Interaktív elem nem lehet interaktív elemben, ezért ez érvénytelen HTML, és két fókuszpontot hoz létre. A gombnak `type` attribútuma sincs.
- **Javítás (commit link, előtte/utána kódrészlet):** [16c5e5c](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/16c5e5cd4b01cecb44a7939f2a4a0ae542da2900)
  - Előtte: `<a href="#"><button>Küldés</button></a>`
  - Utána: `<button type="submit">Küldés</button>`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** W3C validátor: a „The element button must not appear as a descendant of the a element” hiba eltűnt. Tab-bejárás: az üzenet mező után egyetlen Tab-bal a gombra jutok. Üres mezőkkel Enterre a böngésző a kötelező mezőknél megállít, tehát a gomb a formot küldi.
- **Tanulság / érintett alapelv (pl. Szemantika előbb, stílus később; WCAG-kritérium):** A link navigál, a gomb cselekszik. Interaktív elemet nem szabad egymásba ágyazni (Szemantika előbb, stílus később).

## 6. hiba

- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** A főcím fekete marad, pedig a CSS szerint zöldnek kellene lennie. DevTools Styles panel: a `colour` sor át van húzva, „Unknown property name” figyelmeztetéssel.
- **Gyökérok (melyik sor, melyik szabály sérül):** `style.css` 3. sor: elgépelt tulajdonságnév, `colour` (brit írásmód). A CSS csak a `color` nevet ismeri, az ismeretlen tulajdonságot a böngésző csendben figyelmen kívül hagyja.
- **Javítás (commit link, előtte/utána kódrészlet):** [8fc8157](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/8fc8157694afc36f3babd77ed0cff337314f8281)
  - Előtte: `.cim { colour: #0e7c66; font-size: 2rem; }`
  - Utána: `.cim { color: #0e7c66; font-size: 2rem; }`
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** A cím zöld lett. DevTools Styles panel: a `color` érvényes, nincs figyelmeztetés. W3C CSS validátor: a „Property colour doesn't exist” hiba eltűnt.
- **Tanulság / érintett alapelv (pl. Szemantika előbb, stílus később; WCAG-kritérium):** A CSS az ismeretlen tulajdonságot hibaüzenet nélkül eldobja, ezért ha egy stílus nem érvényesül, először a DevTools Styles paneljét és a CSS validátort kell megnézni.

## 7. hiba

- **Hiba leírása és reprodukálása (lépések, böngésző, segédtechnológia):** Az oldalon Tab-bal navigálva nem látszik, melyik mező vagy gomb van fókuszban, így billentyűzettel nem lehet követni, hol tartok.
- **Gyökérok (melyik sor, melyik szabály sérül):** `style.css` 6. sor: `outline: none` minden fókuszálható elemen (input, select, textarea, button), pótlás nélkül. Sérül: WCAG 2.4.7 (Látható fókusz).
- **Javítás (commit link, előtte/utána kódrészlet):** [7e913b8](https://github.com/baloogedit/umfst-bh01-kapcsolati-urlap/commit/7e913b8738dc545d61b7d76e867643126bff176f)
  - Előtte: `input:focus, select:focus, textarea:focus, button:focus { outline: none; }`
  - Utána: `input:focus-visible, select:focus-visible, textarea:focus-visible, button:focus-visible { outline: 2px solid #0e7c66; outline-offset: 2px; }` (a commitban a fájl formázása is áttekinthetőbb lett: szabályonként több sorba tördelve)
- **Hogyan tesztelted (W3C validátor, Lighthouse, Tab-bejárás, képernyőolvasó):** Tab-bejárás Chrome-ban (a módosítás után az oldalt frissíteni kellett, mert a böngésző a régi CSS-t mutatta): minden mezőn és a gombon jól látható zöld keret jelenik meg. A sorrend: név, e-mail, téma, üzenet, Küldés, mindegyik egy Tab-bal. Egérkattintásra a gombon nem jelenik meg keret, ahogy a `:focus-visible`-től várható.
- **Tanulság / érintett alapelv (pl. Szemantika előbb, stílus később; WCAG-kritérium):** A fókuszkeretet soha nem szabad pótlás nélkül eltávolítani. A `:focus-visible` a billentyűzetes felhasználónak mutat keretet, az egeresnek nem (WCAG 2.4.7).

## Összegzés

- Mit tanultam: Egy oldal attól, hogy „működik” és jól néz ki egérrel, még nem használható mindenkinek. A 7 hibából 5 akadálymentességi volt, és ezek többsége egérrel ki sem derül: Tab-bejárással, W3C validátorral és a Lighthouse Accessibility auditjával lehetett megtalálni őket. Megtanultam, hogy a placeholder nem címke, hogy interaktív elemet nem ágyazunk egymásba, és hogy a CSS az elgépelt tulajdonságot csendben eldobja. Azt is megtanultam, hogy CSS-módosítás után frissíteni kell az oldalt, különben a böngésző a régi fájlt használhatja.
- Melyik alapelv sérült a leggyakrabban: Az akadálymentesség (WCAG), azon belül a „Szemantika előbb, stílus később” elv: hiányzott a nyelv, az alt szöveg, a címkék, a megfelelő input-típus és a valódi gomb (1.1.1, 1.3.1, 3.1.1, 3.3.2, 4.1.2), a stílus pedig elvette a látható fókuszt (2.4.7).
