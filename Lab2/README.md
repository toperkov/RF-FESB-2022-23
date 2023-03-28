# Lab 2 - Razumijevanje *hash* funkcija, ekstenzija 

U sklopu ove vježbe student će se upoznati sa radom hash funkcija kao i sa hexadecimalnom reprezentacijom/notacijom.

## Vježba 1

Cilj ove vježbe je razumijeti da datoteke imaju jedinstvena zaglavlja na osnovu tipa datoteke. Pokazat ćemo kako datoteke ne trebaju imati ekstenziju za koje se trenutno prikazuju. Veoma bitno je kod računalne forenzike detektirati datateke koje imaju promijenjenu ekstenziju, jer one mogu ukazivati na potencijalno skrivanje informacije.

- Iz direktorija [Download](Download) sačuvajte datoteku Lab2_download_1.zip te je raspakirajte.

- Vaš zadatak je saznati i izlistati sve datoteke koje se nalaze u direktoriju i sačuvati ih u pandas dataframeu. Da biste to napravili pratite upute kako je navedeno:

1. Prvo specificirate putanju direktorija gdje se datoteke nalaze koristeći varijablu `dir_path`. Zatim stvorite praznu listu `file_names` za pohranjivanje naziva datoteka.

2. Zatim prolazite kroz sve datoteke u direktoriju koristeći funkciju `os.listdir()`. Za svaku datoteku koristite funkciju `os.path.isfile()` kako biste provjerili je li obična datoteka (tj. nije direktorij). Ako je to slučaj, dodajete naziv datoteke u listu `file_names`.

3. Konačno, stvorite Pandas okvir podataka (dataframe) koristeći funkciju `pd.DataFrame()` s rječnikom gdje je ključ naziv stupca (`file_name`), a vrijednost je lista `file_names`. Tada možete ispisati okvir podataka pomoću funkcije `print()`.

3. Napomena: Pazite da zamijenite `'path/to/directory'` stvarnom putanjom do vašeg direktorija.

Dolje je naveden primjer koda kako bi trebalo izgledati

```python
import os
import pandas as pd

# specify the directory path where the files are located
dir_path = 'path/to/directory'

# create an empty list to store the file names
file_names = []

# iterate through all files in the directory
for file in os.listdir(dir_path):
    # check if the file is a regular file (i.e., not a directory)
    if os.path.isfile(os.path.join(dir_path, file)):
        # if so, add the file name to the list
        file_names.append(file)

# create a Pandas dataframe with the file names
df = pd.DataFrame({'file_name': file_names})

# print the dataframe
print(df)
```

## Vježba 2

Cilj ove vježbe je sačuvati i ekstenziju datoteke u pandas dataframeu. Da bismo to realizirali pratite sljedeće korake:

1. Prvo stvaramo praznu listu `extensions` za pohranjivanje ekstenzija datoteka zajedno s listom `file_names` (prethodni zadatak).

2. Zatim, za svaku datoteku, razdvajamo naziv datoteke na dva dijela: naziv i ekstenziju, koristeći funkciju `os.path.splitext()`. Ova funkcija vraća tuple koji sadrži naziv datoteke bez ekstenzije (ime) i samu ekstenziju (ekstenzija).

3. Zatim dodajemo ime i ekstenziju njihovim odgovarajućim listama. Konačno, stvaramo Pandas okvir podataka s dvije kolone, `'file_name'` i `'extension'`, koristeći funkciju `pd.DataFrame()` s rječnikom gdje su ključevi nazivi stupaca ('file\_name' i 'extension'), a vrijednosti su liste `'file_names'` i `'extensions'`, redom.

Cilj ove vježbe je pokazati kako dvije datoteke kreirane u na različitim uređajima imaju iste hash otiske ukoliko je njihov sadržaj identičan. Također ćemo pokazati iako je sadržaj datoteke identičan (razlikuje se po kapitalizaciji) hash otisak će u tom slučaju biti isti.

## Vježba 3

Cilj ove vježbe je izračunati hash vrijednosti datoteka i spremiti ih u pandas dataframe.

1. Prvo stvaramo tri prazne liste `md5s`, `sha1s` i `sha256s` za pohranjivanje vrijednosti sažetaka (hash) MD5, SHA1 i SHA256 za svaku datoteku, redom.

2. Zatim, za svaku datoteku, računamo vrijednosti sažetaka koristeći modul `hashlib`. Koristimo funkcije `hashlib.md5()`, `hashlib.sha1()` i `hashlib.sha256()` za izračunavanje vrijednosti sažetaka MD5, SHA1 i SHA256 za datoteku, redom. Tada dodajemo vrijednosti sažetaka njihovim odgovarajućim listama (koristite `.hexdigest()` funkciju za pohranu).

Konačno, stvaramo Pandas okvir podataka s pet stupaca, `'file_name'`, `'extension'`, `'md5'`, `'sha1'` i `'sha256'`, koristeći funkciju `pd.DataFrame()` s rječnikom gdje su ključevi nazivi stupaca, a vrijednosti su liste `file_names`, `extensions`, `md5s`, `sha1s` i `sha256s`, redom.

## Vježba 4

U ovom dijelu vježbe za svaku datoteku pokušavamo saznati njenu stvarnu vrstu.

1. Prvo stvaramo praznu listu `magic_numbers` za pohranjivanje magičnih brojeva (magic numbers) za svaku datoteku.

2. Zatim stvaramo magični objekt koristeći funkciju `magic.Magic()` za detekciju vrsta datoteka.

3. Za svaku datoteku, detektiramo vrstu datoteke pomoću magicnog objekta i dodajemo magični broj u listu `magic_numbers`.

4. Konačno, stvaramo Pandas okvir podataka s šest stupaca, `'file_name'`, `'extension'`, `'md5'`, `'sha1'`, `'sha256'` i `'magic_number'`, koristeći funkciju `pd.DataFrame()` s rječnikom gdje su ključevi nazivi stupaca, a vrijednosti su liste `file_names`, `extensions`, `md5s`, `sha1s`, `sha256s` i `magic_numbers`, redom.

Napomena: Imajte na umu da morate imati instaliranu biblioteku `python-magic` za pravilno funkcioniranje modula `magic`.

## Vježba 5

U ovom dijelu vježbe, ideja je provjeriti odgovara li vrsta datoteke njenoj ekstenziji.

1. Prvo stvaramo praznu listu `extension_matches` za pohranjivanje vrijednosti True ili False koje označavaju sadrži li magicni broj ekstenziju datoteke.

2. Zatim, za svaku datoteku, provjeravamo sadrži li magicni broj ekstenziju datoteke koristeći operator `in` sa `lower()` metodom kako bi usporedili ekstenziju i magicni broj u malim slovima. Dodajemo vrijednost True ili False u listu `extension_matches`.

```python
# check if the magic number contains the file extension
if extension.lower() == '':
    extension_matches.append(False)
elif mimetypes.guess_type('test'+extension.lower())[0] in magic_number.lower():
    extension_matches.append(True)
else:
    extension_matches.append(False)
```

Napomena: potrebno je uvesti biblioteku `import mimetypes`.

### Vježba 6

Pretpostavimo da je tvrtka prijavila problem korporativne špijunaže u kojem smatraju da im je ukraden/kopiran tekstualni dokument u `PDF-u` iznimne važnosti. Budući da tvrtka ne želi otkriti sadržaj dokumenta, forenzični istražitelj dobiva u uvid hash otisak dokumenta:

`c15e32d27635f248c1c8b66bb012850e5b342119`

Također, sa računala osumnjičene osobe ste izuzeli niz dokumenata koji bi mogli ukazivati na potencijalni dokaz. Dokumente u datoteci `Dokaz.zip` možete preuzeti iz direktorija [Download](Download). Raspakirajte dokumente i napravite analizu te navedite o kojem se dokumentu radi.

