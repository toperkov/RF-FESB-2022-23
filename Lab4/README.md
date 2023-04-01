# Lab 4 - Metapodaci datoteka

U sklopu današnje vježbe analizirat će se metapodaci koji su prisutni u datotekama, a koje mogu dosta otkriti informacija o samoj datoteci te pomoći pri forenzičnoj analizi. Primjerice, ukoliko su fotografije slikane mobitelom, mogu sadržavati informacije o GPS koordinatama na kojoj je kreirana, model mobitela koji je bio korišten, rezolucija, ekspozicija itd.

## Zadatak

U direktoriju [Download](Download) se nalaze dvije datoteke, jedna je tipa PDF, druga JPG datoteka. PDF dokument je enkriptiran, te je vaš zadatak dekriptirati PDF dokument, te izvući sve metapodatke iz njega, kao i metapodatke iz JPG datoteke.

> HINT: Lozinka kojom je PDF dokument enkriptiran odgovara sadržaju u ruci osobe koja se nalazi na Google Street View na GPS koordinati koja je sadržana u JPG datoteci.

Za realizaciju vježbe koristite python programsko okruženje te ekstendirajte kod koji je naveden u nastavku. Možete pristupiti Google maps alatu iz pythona korištenjema biblioteke `webbrowser`, odnosno pozivom `webbrowser.open_new_tab(url)`, gdje je `url` link na Google map koordinatu `http://www.google.com/maps/place/`**lat,long**. Pri tome su varijable `lat` i `long` rezultat konverzije GPS koordinate koji se može dobiti pozivom funkcije `convertGPScoordinate`.

```python
import os, sys, optparse
from exif import Image
import webbrowser
from pypdf import PdfReader, PdfWriter

def convertGPScoordinate(coordinate, coordinate_ref):
    decimal_degrees = coordinate[0] + \
                      coordinate[1] / 60 + \
                      coordinate[2] / 3600
    
    if coordinate_ref == "S" or coordinate_ref == "W":
        decimal_degrees = -decimal_degrees
    
    return decimal_degrees

def figMetaData(file_path):
    img_doc = Image(open(file_path, "rb"))

    if not img_doc.has_exif:
        sys.exit(f"Image does not contain EXIF data.")
    else:
        print(f"Image contains EXIF (version {img_doc.exif_version}) data.")
        
    print(f"{dir(img_doc)}\n")


def pdfMetaData(file_path):
    pdf_doc = PdfReader(open(path, "rb"))
    if pdf_doc.is_encrypted:
        pdf_doc.decrypt("PASSWORD_GOES_HERE")

    pdfWriter = PdfWriter()
    for pageNum in pdf_doc.pages:
        pdfWriter.add_page(pageNum)
    with open('decrypted_output.pdf', 'wb') as f:
        pdfWriter.write(f)


if __name__ == "__main__":
    parser = optparse.OptionParser("Usage: python <script_name> -f <file>")
    parser.add_option("-f", dest="file", type="string", help="please provide full path to the document")

    (options, args) = parser.parse_args()

    path = options.file
    if not path:
        print("please provide full path to the document")
        sys.exit(parser.usage)

    if any(path.endswith(ext) for ext in (".jpg", ".bmp", ".jpeg",)):
        figMetaData(path)
    elif path.endswith(".pdf"):
        pdfMetaData(path)
    else:
        print("File extension not supported/recognized... Make sure the file has the correct extension...")
```

Python skriptu pozivate `python <script_name> -f <file>`, gdje je `<script_name>` ime `.py` skripte, a `<file>` ime PDF ili JPG datoteke
