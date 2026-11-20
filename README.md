# GirlTHing AI Lab, Blok 2

Materijali Bloka 2 (LLM, RAG, chatbot) i finalni projekat.

## Finalni projekat: Siguran put

Chatbot koji ženi objašnjava šta se dešava u njenom tijelu i šta joj u
zdravstvenom sistemu FBiH pripada, oboje isključivo iz zvaničnih
dokumenata i uz citat. Ne daje dijagnozu ni dozu.

Baza znanja su zakoni i podzakonski akti FBiH na jednoj strani, i
zvanične zdravstvene smjernice (WHO, EFSA, FIGO) na drugoj. Poenta je
što se te dvije strane spajaju u jedan odgovor: *kada se nešto radi* i
*ko to plaća*.

### Granice

- odgovara samo iz dokumenata, uz naziv dokumenta i član
- kad odgovora nema u bazi, to i kaže
- nikad dijagnoza, nikad doza, nikad kalorije ni ciljna težina
- na opisan simptom vraća proceduru i pravo, ne tumačenje

## Struktura

| Folder | Šta je |
|---|---|
| `data/` | baza znanja finalnog projekta, 10 dokumenata |
| `data_primjer/` | dokumenti Studentskog asistenta sa radionice, čuvam ih za vježbu |
| `rag.py` | RAG modul sa Radionice 6 |

Nove dokumente dodajem u `data/`, a `rag.load_or_build_index` sam
primijeti promjenu i ponovo napravi indeks.

## Baza znanja

| Dokument | Šta pokriva |
|---|---|
| `zakon-zdravstvena-zastita.pdf` | prava pacijenata, izabrani doktor, upućivanje |
| `zakon-zdravstveno-osiguranje.pdf` | obim prava, lično učešće |
| `osnovni-paket.pdf` | šta ulazi u osnovni paket, zaštita žena |
| `maksimalno-ucesce.pdf` | ko je oslobođen participacije |
| `efsa-referentne-vrijednosti.pdf` | referentne vrijednosti nutrijenata po periodu života |
| `figo-krvarenje.pdf` | definicije normalnog i abnormalnog krvarenja |
| `pcos-smjernica.pdf` | kriteriji i definicija neredovnog ciklusa |
| `who-skrining-grlic.pdf` | od koje godine i u kojem intervalu se radi skrining |
| `who-fizicka-aktivnost.pdf` | preporuke za aktivnost, uključujući trudnoću |
| `who-zeljezo.pdf` | suplementacija u reproduktivnom periodu |

Svaki dokument je provjeren da se tekst izvlači i da nije skeniran.
Ukupno 576 stranica, oko 1,5 miliona znakova.

Pravni tekstovi su neoficijelni prečišćeni tekstovi sa javnih sajtova i
mogu imati sitne greške u prijepisu.

## Pokretanje

```bash
pip install openai numpy pypdf python-dotenv
```

API ključ ide u `.env`, koji je u `.gitignore` i nikad se ne commita.

```python
import rag
index = rag.load_or_build_index(client, "data", "index.npz")
odgovor, izvori = rag.answer(client, index, "Od koje godine se radi papa test?")
```

Prvi `load_or_build_index` traje par minuta jer računa embeddinge za oko
1900 chunkova. Nakon toga se indeks čita iz `index.npz`.

## Napomena o AI alatima

Temu, obim i granice projekta sam odredila sama. AI je pomogao u
pronalaženju i provjeri dokumenata, u pisanju sistemskog prompta i u
strukturiranju plana. Svaki dokument u bazi sam otvorila i provjerila.
