# tesseractOCR-PyMuPdf-basics

Mientras checaba el tema de LangChain en la anterior práctica, me dio curiosidad por saber el cómo funcionaban
los OCR's para extracción de datos, puesto que pudiera existir la posibilidad de querer
extraer datos viejos de documentos digitalizados a lo mejor.

Lo cual en el área de datos si sera una tareita exhuberante, lidiar con precisión de los datos numéricos,
ver la viabilidad de exportar esa información, o tener que depurar una base de datos superllena
con ciertos datos que tu ya tienes reflejados en documentos, pero que si tu quisieras volver a ocupar más
adelante y de a fuerza requirieras recuperar esa data pues tendrías que recuperarlos de documentos
que acreditan un flujo, un precio pre aprobado tal vez para analizar ciertos aspectos en especifido,
o validar rendimiento de algunos usuarios en el pasado, o validar el número de transacciones de un cliente
en fechas pasadas y crear un bench mark, o extraer el valor total de mercancias en el pasado y crear un historico
de los últimos 5 años sin necesidad de tener información guardada para liberar espacio... supongo son los
escenarios que yo imaginaría.

Hay 2 herramientas que me llamaron la atención para la aplicación de esto: TesseractOCR, y PyMuPDF, una libreria más
actualizada. Para esta práctica solo quise saber las limitaciones de aplicar OCR directo sobre un documento, lo cual
si, repercute en el tiempo de desarrollo y de acuerdo a las varianzas de las plantillas. También hay soluciones más
modernas y aplicables como aws text extract o el mismo openai, pero si quieres ahorrar costos, un script no cuesta nada...
dependiendo contexto y precisión, así como costos.

Para esta práctica usé un PDF que saqué de por ahi... no puedo mostrarlo todo. pero maso una parte
luce así:

<img width="1426" height="224" alt="image" src="https://github.com/user-attachments/assets/574f7fc1-a874-46b0-885d-193e58847260" />

Si lo metes en OpenAI la imagen, te extrae esto:




Es preciso, pero puedes lograr una extracción sucia de la misma información y tu mismo limpiar la data ^^.
Para esto se uso TesseractOCR que podría configurarse en un docker sin tema y PyMuPDF que es instalado con pip.

El código queda de la siguiente manera:

```python
import sys, pymupdf  # type: ignore # import the bindings
doc = pymupdf.open("asdasd.pdf")  # open document
import io
from PIL import Image
import sys, pymupdf  # type: ignore # import the bindings
doc = pymupdf.open("asdasdasdas.pdf")  # open document
import io
from PIL import Image

import os
os.environ["TESSDATA_PREFIX"] = r"C:\\Program Files\\Tesseract-OCR\\tessdata\\"

class ClientInformation:
    denominacion: str | None = None
    rfc: str | None = None
    direccion: str | None = None
    postalCode: str | None = None
    fiscalResidence: str | None = None
    bultos: str | None = None
    tcPedido: str | None = None
    peso: str | None = None
    vendedor: str | None = None

    def __str__(self):
        return f"OBJECT(denominacion={self.denominacion},rfc={self.rfc},direccion={self.direccion},postalcode={self.postalCode},fiscalResidence={self.fiscalResidence},bultos={self.bultos},tcPedido={self.tcPedido},peso={self.peso},vendedor={self.vendedor})"



for page in doc:  # iterate through the pages
    zoom_x = 3.0  # horizontal zoom
    zoom_y = 3.0  # vertical zoom
    mat = pymupdf.Matrix(zoom_x, zoom_y)
   ## datos_cliente_sec = pymupdf.Rect((22,120,394,220))
    datos_cliente_sec = pymupdf.Rect((22,120,394,130))
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )

  ##  print(doc2.get_text(textpage=tp))

    ## AQUÍ EMPIEZA LA EXTRACCIÓN
    objInstance = ClientInformation()

    datos_cliente_sec = pymupdf.Rect((22,130,394,138))
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )

    objInstance.denominacion = doc2.get_text(textpage=tp)
 ##   print(objInstance.denominacion)

    datos_cliente_sec = pymupdf.Rect((22,140,394,148))
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )

    objInstance.rfc = doc2.get_text(textpage=tp)
##    print(objInstance.rfc)

    datos_cliente_sec = pymupdf.Rect((22,148,394,156))
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )

   ## print(doc2.get_text(textpage=tp))

    objInstance.direccion = doc2.get_text(textpage=tp)
##    print(objInstance.direccion)

    datos_cliente_sec = pymupdf.Rect((22,158,394,166))
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )

   ## print(doc2.get_text(textpage=tp))

    objInstance.postalCode = doc2.get_text(textpage=tp)
##    print(objInstance.postalCode)

    datos_cliente_sec = pymupdf.Rect((22,163,394,176))
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )


    objInstance.fiscalResidence = doc2.get_text(textpage=tp)
##    print(objInstance.fiscalResidence)

    datos_cliente_sec = pymupdf.Rect((22,183,394,196))
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )

    objInstance.bultos = doc2.get_text(textpage=tp)
 ##   print(objInstance.bultos)

    ## extraccion sub campos
    datos_cliente_sec = pymupdf.Rect((96,194,180,204)) ## en algunos casos y por x razon puedde haber conflicto entre letras y numeros, para numeros precisos, encierra todo el bloque
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )


    objInstance.bultos = doc2.get_text(textpage=tp)
 ##   print(objInstance.bultos)

    datos_cliente_sec = pymupdf.Rect((180,194,250,204)) ## en algunos casos y por x razon puedde haber conflicto entre letras y numeros, para numeros precisos, encierra todo el bloque
    pix = page.get_pixmap(clip=datos_cliente_sec,matrix=mat)
    pix.save("datos_cliente.png")

    doc2inst = pymupdf.open("datos_cliente.png")

    doc2 = doc2inst[0]
    tp = doc2.get_textpage_ocr(
        language="spa",
        full=True
    )

  ##  print(doc2.get_text(textpage=tp))

    objInstance.tcPedido = doc2.get_text(textpage=tp) ## en otros casos, puede haber variabilidad por temas de
  ##  print(objInstance.tcPedido)


    print(str(objInstance))

```

En resumidas, vas haciendo capturas por caaada fila que aparezca en tu documento porque el OCR no puede interpretar con presición
grandes bloques de tablas. Y por medio de esos recortes precisos ya extraes textos, Por eso hay que decidir que campos
extraer y sobre todo discernir cuales son los campos más dinamicos que pueden crecer dentro del bloque.

El output en este pequeño ejemplo con mi script sale así:

<img width="683" height="271" alt="image" src="https://github.com/user-attachments/assets/c335f252-3265-4b70-a74b-0b591213916b" />

Con OpenAI sale más limpio e individual pero es lo mismo casi:

<img width="1360" height="1030" alt="image" src="https://github.com/user-attachments/assets/5783a91d-ae06-40a2-a39c-4ce9871c09e2" />

También puede ser aplicable si se requiriera de pdfs digitalizados para crear un RAG e IMPORTANTE, en esta práctica quise extraer por medio de OCR,
esto solo es en PDFS digitalizados, es más fácil usar pymupdf y extraer el texto directamente con la librería, solo si en el pdf te permite
seleccionar el texto ^^

<img width="702" height="487" alt="image" src="https://github.com/user-attachments/assets/f086e13c-8518-46ea-a5fe-f4aa0348781c" />


Muy importante que si tendrás estos escenarios contemplados, uses los metadatos de un documento para guardar información extra
que pueda servirte más adelante, entonces considerarías guardar esto en algún Data warehouse.
