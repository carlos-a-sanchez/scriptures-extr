---
name: scripture-reference-extractor
description: Read the sermon notes sent by the pastor and extract the scripture references to create a file to be consumed by the software used by the broadcastng team to show the scritpures
allowed-tools:
  - read
  - write
---

## Input parameters
- **`notes_file`**(required): This is the file with the sermon notes sent by the pastor

## Workflow

1. Validate that the file exists and is a valid text document (.txt)
2. Extract the scripture references from the document. The document is in Spanish and may contain references like "Lucas 15:11" or "Juan 3:16"

  a) In case the reference to a scripture is a range, like "Lucas 15:11-13", include multiple lines in the output file, one for each verse in the range.

  b) If the reference to a scripture is not found in the Bible, skip it and continue with the next reference.

  c) If the reference appears more than once in the document, include it multiple times in the output file.


3. Format the scripture references according to the specified format

  ```
  Bm[n]=[B]:[NL]:[C]:[V]
  ```

  where B is the acronym for the Spanish Bible translation:

  - RVR60,Reina-Valera 1960
  - NVI,Nueva Versión Internacional
  - LBLA,La Biblia de las Américas
  - NTV,Nueva Traducción Viviente
  - DHH,Dios Habla Hoy
  - TLA,Traducción en Lenguaje Actual
  - BJ,Biblia de Jerusalén

  When the reference in the notes does not specify a Bible translation, use NVI as the default.

  n is a sequential number for each scripture reference found in the document, starting at 0.

  NL is the book's position in the Bible; use the following list to find the book by its acronym and obtain its position within the Bible:

  Position,Acronym,Book Name
  1,Gn,Génesis
  2,Ex,Éxodo
  3,Lv,Levítico
  4,Nm,Números
  5,Dt,Deuteronomio
  6,Jos,Josué
  7,Jue,Jueces
  8,Rt,Rut
  9,1 S,1 Samuel
  10,2 S,2 Samuel
  11,1 R,1 Reyes
  12,2 R,2 Reyes
  13,1 Cr,1 Crónicas
  14,2 Cr,2 Crónicas
  15,Esd,Esdras
  16,Neh,Nehemías
  17,Est,Ester
  18,Job,Job
  19,Sal,Salmos
  20,Pr,Proverbios
  21,Ec,Eclesiastés
  22,Cnt,Cantares
  23,Is,Isaías
  24,Jr,Jeremías
  25,Lam,Lamentaciones
  26,Ez,Ezequiel
  27,Dn,Daniel
  28,Os,Oseas
  29,Jl,Joel
  30,Am,Amós
  31,Abd,Abdías
  32,Jon,Jonás
  33,Miq,Miqueas
  34,Nah,Nahúm
  35,Hab,Habacuc
  36,Sof,Sofonías
  37,Hag,Hageo
  38,Zac,Zacarías
  39,Mal,Malaquías
  40,Mt,Mateo
  41,Mc,Marcos
  42,Lc,Lucas
  43,Jn,Juan
  44,Hch,Hechos de los Apóstoles
  45,Ro,Romanos
  46,1 Co,1 Corintios
  47,2 Co,2 Corintios
  48,Ga,Gálatas
  49,Ef,Efesios
  50,Fil,Filipenses
  51,Col,Colosenses
  52,1 Ts,1 Tesalonicenses
  53,2 Ts,2 Tesalonicenses
  54,1 Ti,1 Timoteo
  55,2 Ti,2 Timoteo
  56,Tit,Tito
  57,Flm,Filemón
  58,He,Hebreos
  59,Stg,Santiago
  60,1 P,1 Pedro
  61,2 P,2 Pedro
  62,1 Jn,1 Juan
  63,2 Jn,2 Juan
  64,3 Jn,3 Juan
  65,Jud,Judas
  66,Ap,Apocalipsis

  C is the chapter, and V is the verse.

  Example

  LC 15:11, NTV becomes

  Bm0=NTV:42:15:11

4. Write the formatted scripture references to a file named `[notes_file_name_without_extension]-scriptures.txt` in the same directory as the input file.


