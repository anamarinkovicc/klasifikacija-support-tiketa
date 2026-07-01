# Klasifikacija support ticketa pomoću neuronskih mreža
Ovaj projekat je razvijen u okviru predmeta Duboko učenje i neuronske mreže. Fokus rada je na automatizaciji procesa klasifikacije korisničkih zahteva (tiketa) primenom dubokih neuronskih mreža.

## 1. Opis problema
Ručna klasifikacija velikog broja korisničkih tiketa u IT sektorima je neefikasna. Cilj ovog projekta je implementacija modela koji na osnovu tekstualnog opisa (body) automatski klasifikuje tiket, čime se ubrzava vreme odgovora i optimizuju resursi podrške.

## 2. Podaci
- Izvor: Skup podataka all_tickets.csv preuzet sa Kaggle platforme
- Struktura: Dataset sadrži 48.549 instanci. Glavne kolone su body (tekstualni ulaz) i ticket_type (ciljna varijabla).
- Analiza: Skup podataka je blago neizbalansiran: Klasa 1 sadrži 34.621 unosa, dok Klasa 0 sadrži 13.928 unosa.
- Preprocesiranje: Tekst je očišćen od specijalnih karaktera i pretvoren u mala slova. Izvršena je tokenizacija na 10.000 najčešćih reči i dopunjavanje (padding) na fiksnu dužinu od 100 reči radi uniformnosti ulaza u mrežu.

## 3. Arhitektura modela
Model je definisan kao sekvencijalna neuronska mreža sa sledećim slojevima:
- Embedding sloj: Vektorizacija teksta (input_dim=10000, output_dim=16).
- GlobalAveragePooling1D: Smanjenje dimenzionalnosti i izvlačenje ključnih karakteristika.
- Dense (skriveni) sloj: 16 ili 32 neurona sa ReLU aktivacionom funkcijom.
- Dense (izlazni) sloj: 1 neuron sa Sigmoid aktivacijom za binarnu klasifikaciju. Ukupan broj parametara modela je 160.289.

## 4. Trening
Trening je sproveden kroz 10 epoha sa batch veličinom 32. Korišćen je Adam optimizator i binary_crossentropy funkcija gubitka. Za validaciju modela tokom učenja odvojeno je 20% podataka iz trening seta.

## 5. Analiza osetljivosti i hiperparametrska optimizacija
U okviru optimizacije, fokus je stavljen na arhitekturu sa 32 neurona u skrivenom sloju kako bi se omogućilo mreži da bolje modeluje kompleksne tekstualne obrasce u poređenju sa jednostavnijim baznim modelima. Analizirana je stabilnost učenja i brzina konvergencije modela pri ovoj konfiguraciji.

## 6. Rezultati evaluacije
Na grafiku prikazani su rezultati preciznosti tokom epoha:
- Trening preciznost: Počinje od 92.3% i raste do nivoa od preko 99.5%.
- Validaciona preciznost: Već u prvoj epohi dostiže visokih 98.4% i održava stabilnost iznad 99% kroz većinu treninga. Finalna preciznost na testnom skupu potvrđuje visoku pouzdanost modela u realnim uslovima.

## 7. Diskusija
Analizom grafika primećuje se da model uči veoma brzo, dostižući visoku preciznost već u prvih nekoliko epoha. Međutim, u 8. epohi zabeležen je pad validacione preciznosti na oko 98.8%, nakon čega se kriva ponovo stabilizuje. Ovo ukazuje na povremenu nestabilnost u procesu optimizacije ili blagi "overfitting" u specifičnim epohama, što je važan uvid za dalje fino podešavanje modela, poput uvođenja Dropout slojeva ili ranog zaustavljanja (Early Stopping).

## 8. Zaključak
Implementirani model pokazao je izuzetne performanse u klasifikaciji podrške. Sa preciznošću od preko 99%, model predstavlja pouzdano rešenje za automatizaciju obrade tiketa, čime je u potpunosti ispunjen cilj projektnog zadatka.

## Podela rada
- Ana Marinković: Prikupljanje i preprocesiranje podataka, definisanje arhitekture modela (16 neurona), trening modela (16 neurona) i diskucija rezultata.
- Lana Spasić: Analiza podataka, tokenizacija i priprema za model, definisanje arhitekture modela (32 neurona) i trening modela (32 neurona) (analiza osetljivosti), generisanje grafika evaluacije i pisanje zaključka.

## Licenca
Ovaj projekat je licenciran pod MIT licencom.
