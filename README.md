# Hantavirus – Analiza ponavljajućih sekvenci

Seminarski rad iz oblasti analize proteinskih sekvenci hantavirusa.

## Cilj projekta

Cilj projekta je analiza ponavljajućih sekvenci u aminokiselinskim sekvencama hantavirusa i ispitivanje mogućnosti određivanja vrste hantavirusa na osnovu karakteristika dobijenih iz ponavljanja.

Analiza je sprovedena za dva proteina:

- `glycoprotein precursor (GPC)`
- `nucleocapsid protein (N)`

Na osnovu detektovanih ponavljanja formiran je skup podataka koji je zatim korišćen za klasterovanje proteinskih sekvenci.

## Obrada podataka

Proteinske sekvence su preuzete iz NCBI Virus baze i prethodno obrađene i filtrirane prema definisanim kriterijumima (klasifikacija proteina po opisu, filter pune dužine sekvence, uklanjanje sekvenci sa dvosmislenim karakterima).

Formirana su dva skupa podataka za svaki protein:

- skup koji sadrži sve dostupne sekvence (`all`);
- skup koji sadrži samo kompletne sekvence (`complete`).

Za identifikaciju ponavljajućih sekvenci korišćen je alat **RepeatsPlus/StatRepeats** (tačnije, program `StatRepeatsNoDB`), razvijen u okviru Bioinformatics Research Group na Matematičkom fakultetu u Beogradu.

Iz detektovanih ponavljanja formirane su numeričke karakteristike koje opisuju ponavljanja različitih dužina. U analizi su korišćene karakteristike za ponavljanja dužine:

- 3 aminokiseline
- 4 aminokiseline
- 5 aminokiselina
- 6 aminokiselina
- 8 aminokiselina

Pored osnovnih karakteristika korišćene su i njihove normalizovane vrednosti (podeljene dužinom sekvence), kako bi se smanjio uticaj različite dužine proteinskih sekvenci.

## Analiza i klasterovanje

Numeričke karakteristike su standardizovane pre primene metoda klasterovanja. Karakteristike koje su gotovo uvek nula (manje od 10% sekvenci ima nenultu vrednost) su uklonjene pre standardizacije, kako ne bi veštački dominirale rezultatom.

Za redukciju dimenzionalnosti i vizuelizaciju podataka korišćena je Principal Component Analysis (PCA).

Za klasterovanje su isprobane sledeće metode:

- K-Means
- Agglomerative Clustering
- Gaussian Mixture
- DBSCAN
- Spectral Clustering

Rezultati su evaluirani korišćenjem sledećih metrika:

- Silhouette Score
- Adjusted Rand Index (ARI)
- Normalized Mutual Information (NMI)

Analiza je sprovedena na sledećim skupovima:

- `GPC / all`
- `GPC / complete`
- `N / all`
- `N / complete`

## Vizuelizacija

Rezultati klasterovanja prikazani su u dvodimenzionalnom prostoru prve dve glavne komponente PCA analize.

Za svaki analizirani skup prikazana je stvarna pripadnost sekvence vrsti, kao i rezultati pojedinačnih algoritama klasterovanja.

Grafici se nalaze u direktorijumu `figures/`.

## Struktura projekta

```text
Hantavirus/
├── data/
│   ├── raw/
│   │   ├── dobrava_all.fasta
│   │   ├── dobrava_complete.fasta
│   │   ├── hantaan_all.fasta
│   │   ├── ...
│   │   └── sinnombre_complete.fasta
│   └── filtered/
│       ├── dobravaense_all_GPC.fasta
│       ├── dobravaense_all_N.fasta
│       ├── ...
│       └── sinnombreense_complete_N.fasta
├── figures/
├── repeat_output/
├── repeats_tool/
│   ├── Help/
│   ├── Programs/
│   │   ├── DrawRepeats/
│   │   ├── MotifSearch/
│   │   ├── Repeats/
│   │   ├── RepeatsPlus/
│   │   └── StatRepeats/
│   │       └── Linux/
│   │           └── StatRepeatsNoDB
│   ├── licence.txt
│   └── Readme.txt
├── .flake8
├── .gitignore
├── .pre-commit-config.yaml
├── Hantavirus_zapisnik.pdf
├── hantavirus.ipynb
├── poetry.lock
├── pyproject.toml
└── README.md
```

## Reprodukcija postupka

Kompletan postupak može se ponoviti korišćenjem podataka, alata, izlaznih fajlova i Jupyter notebook-a koji se nalaze u repozitorijumu.

Za reprodukciju postupka potrebno je prvo pripremiti Python okruženje, a zatim pokrenuti Jupyter notebook.

### 1. Instalacija Python okruženja

Projekat koristi Python i Poetry za upravljanje zavisnostima.

Potrebno je imati instaliran Python i Poetry.

Instalacija svih Python zavisnosti vrši se komandom:

```bash
poetry install
```

Nakon instalacije može se proveriti da li je okruženje pravilno podešeno pomoću:

```bash
poetry run python --version
```

### 2. Pokretanje Jupyter notebook-a

Kompletan Python postupak obrade podataka i klasterovanja nalazi se u fajlu:

`hantavirus.ipynb`

Notebook se može pokrenuti komandom:

```bash
poetry run jupyter notebook
```

Nakon pokretanja Jupyter-a potrebno je otvoriti `hantavirus.ipynb` i izvršavati ćelije redom.

Notebook sadrži korake obrade podataka, konstrukcije karakteristika, pripreme podataka za klasterovanje, primene algoritama, evaluacije rezultata i generisanja grafika.

### 3. Podaci

Ulazne proteinske sekvence nalaze se u direktorijumu:

`data/raw/`

Filtrirane sekvence korišćene u daljoj analizi nalaze se u:

`data/filtered/`

Podaci su organizovani prema vrsti hantavirusa, proteinu i tipu skupa (`all` ili `complete`).

### 4. RepeatsPlus/StatRepeats

Za detekciju ponavljajućih sekvenci korišćen je alat **RepeatsPlus/StatRepeats**.

Alat i prateći fajlovi nalaze se u direktorijumu:

`repeats_tool/`

Glavni programi nalaze se u:

`repeats_tool/Programs/`

Korišćeni izvršni program (Linux, bez interfejsa ka relacionoj bazi):

`repeats_tool/Programs/StatRepeats/Linux/StatRepeatsNoDB`

Dokumentacija za korišćenje alata nalazi se u:

`repeats_tool/Readme.txt`

Za reprodukciju koraka koji koriste RepeatsPlus/StatRepeats potrebno je koristiti alat iz direktorijuma `repeats_tool/` i odgovarajuće ulazne sekvence iz direktorijuma `data/filtered/`.

Rezultati dobijeni korišćenjem alata čuvaju se u direktorijumu:

`repeat_output/`

### 5. Reprodukcija klasterovanja

Nakon pripreme karakteristika iz ponavljanja, ostatak analize može se ponoviti pokretanjem ćelija u fajlu `hantavirus.ipynb`.

Notebook izvršava:

- pripremu podataka;
- standardizaciju karakteristika;
- PCA redukciju dimenzionalnosti;
- K-Means klasterovanje;
- Agglomerative Clustering;
- Gaussian Mixture;
- DBSCAN;
- Spectral Clustering;
- izračunavanje Silhouette Score, ARI i NMI metrika;
- generisanje grafika rezultata.

### 6. Rezultati

Generisani grafici nalaze se u direktorijumu:

`figures/`

U njemu se nalaze grafici za analizirane kombinacije proteina i skupova podataka.

Pored grafika, u repozitorijumu se nalazi i seminarski zapisnik:

[Hantavirus_zapisnik.pdf](Hantavirus_zapisnik.pdf)

koji sadrži detaljan opis postupka, obradu podataka, rezultate i zaključke.
