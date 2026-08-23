# Hantavirus – Analiza ponavljajućih sekvenci

Seminarski rad iz oblasti analize proteinskih sekvenci hantavirusa.

## Cilj projekta

Cilj projekta je analiza ponavljajućih sekvenci u aminokiselinskim sekvencama hantavirusa i ispitivanje mogućnosti određivanja vrste hantavirusa na osnovu karakteristika dobijenih iz tandemskih ponavljanja.

Analiza je sprovedena za dva proteina:

- `glycoprotein precursor (GPC)`
- `nucleocapsid protein (N)`

Na osnovu detektovanih ponavljanja formiran je skup podataka koji je zatim korišćen za klasterizaciju proteinskih sekvenci.

## Obrada podataka

Proteinske sekvence su prethodno obrađene i filtrirane prema definisanim kriterijumima. Za identifikaciju tandemskih ponavljanja korišćen je alat Tandem Repeats Finder.

Iz detektovanih ponavljanja formirane su karakteristike koje opisuju ponavljanja različitih dužina. U analizi su korišćene karakteristike za ponavljanja dužine:

- 3 aminokiseline
- 4 aminokiseline
- 5 aminokiselina
- 6 aminokiselina
- 8 aminokiselina

Pored osnovnih karakteristika korišćene su i njihove normalizovane vrednosti, kako bi se smanjio uticaj različite dužine proteinskih sekvenci.

## Analiza i klasterizacija

Numeričke karakteristike su standardizovane pre primene metoda klasterizacije.

Za redukciju dimenzionalnosti i vizuelizaciju podataka korišćen je Principal Component Analysis (PCA).

Za klasterizaciju su isprobane sledeće metode:

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

Rezultati klasterizacije prikazani su u dvodimenzionalnom prostoru prve dve glavne komponente PCA analize.

Za svaki analizirani skup prikazana je stvarna pripadnost sekvence vrsti, kao i rezultati pojedinačnih algoritama klasterizacije.

Grafici se nalaze u direktorijumu `figures/`.

## Struktura projekta

```text
Hantavirus/
├── figures/                # Grafici i vizuelizacije rezultata
├── .flake8                 # Konfiguracija za proveru Python koda
├── .gitignore              # Git ignore pravila
├── .pre-commit-config.yaml # Konfiguracija pre-commit provera
├── Hantavirus_zapisnik.pdf # Zapisnik sa detaljnim opisom analize i rezultata
├── README.md               # Dokumentacija projekta
├── hantavirus.ipynb        # Analiza i eksperimenti
├── poetry.lock             # Zaključane verzije biblioteka
└── pyproject.toml          # Konfiguracija Python projekta
```

## Pokretanje

Projekat koristi Python i Poetry za upravljanje zavisnostima.

Instalacija zavisnosti:

```bash
poetry install
```

Detaljna analiza i eksperimentalni rezultati nalaze se u Jupyter notebook-u `notebook.ipynb`.

Notebook se može pokrenuti pomoću:

```bash
poetry run jupyter notebook
```

## Dokumentacija

Detaljan opis postupka obrade podataka, analize, klasterizacije, vizuelizacije i dobijenih rezultata nalazi se u dokumentu:

[Hantavirus_zapisnik.pdf](Hantavirus_zapisnik.pdf)

## Autor

Vladimir Mandić
