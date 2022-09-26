# HF-FCN
## Feldat
A feladat keretében egy adott háziállat fajtát ábrázoló képekről az állat kiszegmentálását kell konvolúciós hálóval megtanulni. Mivel az adott háziállat fajtához nem áll rendelkezésünkre sok pixelszintű annotációt tartalmazó mintapélda, ezért transfer learning-et fogunk alkalmazni. Így tehát, a szegmentációra való betanítást nem egy véletlenszerűen inicializált hálón kezdjük el, hanem egy másik feladaton előtanított hálón. Az előtanítási feladat 10 kategóriába sorolt, különböző állatokat és járműveket ábrázoló képek klasszifikációja lesz. Ehhez a feladathoz jóval több címkézett adat rendelkezésünkre áll.
## Adatbázis
A feladat során a Pascal VOC adatbázis egy csökkentett felbontású részhalmazával kell dolgozni. Az adatbázisban tíz kategóriába (négy fajta jármű és hat fajta állat) sorolt objektumokról készült képek vannak. Az adatbázis két részből áll:

A klasszifikációs rész két pickle fájlt tartalmaz, különböző kategóriájú képek és a hozzájuk tartozó kategóriacímkék találhatók meg benne.

A szegmentációs rész is két pickle fájlból áll. Az egyikben képek, míg a másikban a hozzájuk tartozó pixelszintű szegmentációs címkék találhatók.

## Az előtanítási feladat
Az előtanítási feladatban képek multiclass klasszifikációját tanuljuk meg egy hagyományos konvolúciós háló segítségével. Mivel a célfeladatunk egy adott háziállat fajta kiszegmentálása lesz (hallgatónként változó, hogy melyik), alapvetően háziállatokra szeretnénk koncentrálni az előtanítás során is. Öt kategóriát alakítunk ki az eredetileg tíz kategóriába sorolt képekből: az öt eredeti háziállat kategóriából (macska, szarvasmarha, kutya, ló, birka) azt a négyet választjuk ki, amelyek nincsenek a célfeladatunkban.

A negyedik, szegmentációhoz használt háziállat fajtát az előtanítás során nem használjuk fel. Ezt azért tesszük, mert a képek egy része megtalálható mind a klasszifikációs, mind a szegmentációs adatbázisban, nincsenek összepárosítva, így ahhoz, hogy elkerüljük azt az esetleges szituációt, hogy a szegmentációs feladat során a teszthalmazba az előtanítás közben látott tanítóminták kerüljenek, inkább teljesen kihagyjuk ezt a kategóriát az előtanításból.

Az előtanítás ötödik kategóriája a négyfajta háziállat mellett az összes, háziállatokat nem tartalmazó kép lesz (repülőgép, madár, hajó, busz, autó), egyetlen közös kategóriába sorolva.

## A célfeladat
A célfeladat bináris szegmentáció lesz az előtanításkor kihagyott háziállat kategóriára. A szegmentációs adatbázisból kinyerjük azokat a képeket, melyeken legalább egy pixelnyi terjedelemben megtalálható ez a háziállat fajta, majd az előtanított hálót átalakítással szegmentációra alkalamssá tesszük (Fully-Convolutional Network architektúra). Az így kapott hálót tanítjuk tovább a képeink minden pixelére megbecsülni, hogy az a pixel az adott háziállat fajtát, vagy bármi mást ábrázol. (Azaz, lényegében pixelenkénti bináris klasszifikációt végzünk.)
