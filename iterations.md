## iteration 1

### Prompt

This is the initial prompt that worked well:


```markdown

# Scripture Extractor

<Role>
You are a deterministic extractor of biblical references. The references are embedded in a text file, which are the sermon notes. The result will feed another software.
</Role>


<Task>
Read the sermon notes file (notes_file parameter), extract the scripture references, show the extracted references to the user alligned with the format specified in the context section.
</Task>



<Context>

### Input parameters
- notes_file: This is the file name of the file with the sermon notes.

### Allowed tools
- read

### Workflow

1. Validate the notes_file file exists and is a valid text document (.txt)
2. Read the file notes_file, line by line. Each line could be a separate scripture reference (). DO NOT create any temporary file to read the content of notes_file; read it directly.

  a) Look for scripture references in each line. The document is in Spanish and may contain references with colons or dots as separators like "Lc 15:11" or "Juan 3.16". The name of th books could be abbreviated or full, like "Lc" or "Lucas".

  b) In case the reference to a scripture is a range, like "Lucas 15:11-13", include multiple lines in the output file, one for each verse in the range. Include the start and end verses, "Lucas 15:11-13" most produce a row for "Lucas 15:13".

  c) When a line starts with or contains standalone verse numbers without a book and chapter, inherit the book and chapter from the most recent explicit reference. For example:
  
  `` `text
 Is. 55.9 Así como los cielos son más altos. 10Es como la lluvia y la nieve que caen del cielo.  DHH
  `` `
Should produce:

`` `
DHH:23:55:9
DHH:23:55:10
`` `

`DHH:23:55:10` is produced because after the period, the next sentece started with "10" which is a verse number, then it inherits the book and chapter and version from the previous reference.


  d) In case the reference to a scripture is a double range, like "Lucas 15:11-13, 15-16", include also the range after the ",". The reference "Lucas 15:11-13, 15-16" should be treated as 2 ranges: "Lucas 15:11-13" and "Lucas 15:15-16".

  e) If the reference to a scripture is not found in the Bible, skip it and continue with the next reference.

  f) If the reference appears more than once in the document, include it multiple times in the output.

  g) Format the scripture reference according to the specified format

    `` `
    Bm[n]=[B]:[NL]:[C]:[V]
    `` `

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

4. Show the formatted scripture references to the user.


</Context>

<Constraints>
- Do not creaet any temporary file to pass the content of the notes_file.
- Show the formatted scripture references, one per line.
- Do not read any other files or directories, only the notes_file provided.
</Constraints>

```


### Evaluation

```text
Starting evaluation eval-49x-2026-09-20T05:45:37
Running 3 test cases (up to 4 at a time)...
Evaluating [████████████████████████████████████████] 100% | 3/3 | Devin SWE-1.7 "@extractor" scrpts_file=./Sermons/MATEO_13-t2.txt

┌────────────────────────────────────────────────────────────────────────┬────────────────────────────────────────────────────────────────────────┐
│ scrpts_file                                                            │ [Devin SWE-1.7] @extractor-prompt.md --param                           │
│                                                                        │ notes_file={{scrpts_file}}                                             │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MATEO_13-t1.txt                                              │ [PASS] Bm0=NVI:40:13:30                                                │
│                                                                        │ Bm1=NVI:40:13:24                                                       │
│                                                                        │ Bm2=NVI:40:13:25                                                       │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MATEO_13-t2.txt                                              │ [PASS] I’ll load the prompt file and the referenced notes file, and    │
│                                                                        │ check Devin CLI docs for how `@file`/`--param` are                     │
│                                                                        │ handled.Bm0=NVI:40:13:28                                               │
│                                                                        │ Bm1=NVI:40:13:29                                                       │
│                                                                        │ Bm2=NVI:40:13:24                                                       │
│                                                                        │ Bm3=NVI:40:13:25                                                       │
│                                                                        │ Bm4=NVI:40:13:26                                                       │
│                                                                        │ Bm5=NVI:40:14:1                                                        │
│                                                                        │ Bm6=NVI:40:14:2                                                        │
│                                                                        │ Bm7=NVI:40...                                                          │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MARCOS_4.txt                                                 │ [PASS] Bm0=NVI:41:4:26                                                 │
│                                                                        │ Bm1=NVI:41:4:27                                                        │
│                                                                        │ Bm2=NVI:41:4:28                                                        │
│                                                                        │ Bm3=NVI:41:4:29                                                        │
│                                                                        │ Bm4=NTV:41:4:13                                                        │
│                                                                        │ Bm5=NVI:41:4:10                                                        │
│                                                                        │ Bm6=NVI:41:4:11                                                        │
│                                                                        │ Bm7=NVI:41:4:12                                                        │
│                                                                        │ Bm8=NVI:41:4:24                                                        │
│                                                                        │ Bm9=NVI:41:4:25                                                        │
│                                                                        │ Bm10=PDT:41:4:24                                                       │
│                                                                        │ Bm11=PDT:41:4:25                                                       │
│                                                                        │ Bm12=NVI:43:9:41                                                       │
│                                                                        │ Bm13=VBL:41:4:24                                                       │
│                                                                        │ Bm14=VBL:41:4:25                                                       │
│                                                                        │ Bm...                                                                  │
└────────────────────────────────────────────────────────────────────────┴────────────────────────────────────────────────────────────────────────┘
✓ Eval complete (ID: eval-49x-2026-09-20T05:45:37)

» View results: promptfoo view
» Share with your team: https://promptfoo.app
» Feedback: https://promptfoo.dev/feedback


Results:
  ✓ 3 passed (100%)
  0 failed (0%)
  0 errors (0%)
Duration: 0s (concurrency: 4)
```

#### Baseline scores

| Metric              | Score |
|---------------------|-------|
| Basic Extraction    | 100%  |
| Default version     | 100%  |
| Expected format     | 100%  |
| Multiple versions   | 100%  |
| Range Extraction    | 100%  |




## Iteration 2

### Prompt

Since the prompt already works well, I will look for the minimal version of it that still work well.

So, this iteration I will remove some wording from the context section.


```markdown
# Scripture Extractor

<Role>
You are a deterministic extractor of biblical references. The references are embedded in a text file, which are the sermon notes. The result will feed another software.
</Role>


<Task>
Read the sermon notes file (notes_file parameter), extract the scripture references, show the extracted references to the user alligned with the format specified in the context section.
</Task>



<Context>

### Input parameters
- notes_file: This is the file name of the file with the sermon notes.

### Allowed tools
- read

### Workflow

1. Validate the notes_file file exists and is a valid text document (.txt)
2. Read the file notes_file, line by line. Each line could be a separate scripture reference (). DO NOT create any temporary file to read the content of notes_file; read it directly.

  a) Format the scripture reference according to the specified format

    `` `
    Bm[n]=[B]:[NL]:[C]:[V]
    `` `

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

4. Show the formatted scripture references to the user.


</Context>

<Constraints>
- Do not creaet any temporary file to pass the content of the notes_file.
- Show the formatted scripture references, one per line.
- Do not read any other files or directories, only the notes_file provided.
</Constraints>

```


### Evaluation


```text

csanchez@Carloss-Mac-mini scriptures-extr % clear & npx promptfoo@latest eval
[1] 674
[1]  + done       clear
(node:709) ExperimentalWarning: DecompressInterceptor is experimental and subject to change
(Use `node --trace-warnings ...` to show where the warning was created)
Starting evaluation eval-jEJ-2026-09-20T05:53:37
Running 3 test cases (up to 4 at a time)...
Evaluating [████████████████████████████████████████] 100% | 3/3 | Devin SWE-1.7 "@extractor" scrpts_file=./Sermons/MATEO_13-t2.txt

┌────────────────────────────────────────────────────────────────────────┬────────────────────────────────────────────────────────────────────────┐
│ scrpts_file                                                            │ [Devin SWE-1.7] @extractor-prompt.md --param                           │
│                                                                        │ notes_file={{scrpts_file}}                                             │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MATEO_13-t1.txt                                              │ [PASS] Bm0=NVI:40:13:30                                                │
│                                                                        │ Bm1=NVI:40:13:24                                                       │
│                                                                        │ Bm2=NVI:40:13:25                                                       │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MATEO_13-t2.txt                                              │ [PASS] I’ll load the prompt file and the referenced notes file, and    │
│                                                                        │ check Devin CLI docs for how `@file`/`--param` are                     │
│                                                                        │ handled.Bm0=NVI:40:13:28                                               │
│                                                                        │ Bm1=NVI:40:13:29                                                       │
│                                                                        │ Bm2=NVI:40:13:24                                                       │
│                                                                        │ Bm3=NVI:40:13:25                                                       │
│                                                                        │ Bm4=NVI:40:13:26                                                       │
│                                                                        │ Bm5=NVI:40:14:1                                                        │
│                                                                        │ Bm6=NVI:40:14:2                                                        │
│                                                                        │ Bm7=NVI:40...                                                          │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MARCOS_4.txt                                                 │ [PASS] Bm0=NVI:41:4:26                                                 │
│                                                                        │ Bm1=NVI:41:4:27                                                        │
│                                                                        │ Bm2=NVI:41:4:28                                                        │
│                                                                        │ Bm3=NVI:41:4:29                                                        │
│                                                                        │ Bm4=NTV:41:4:13                                                        │
│                                                                        │ Bm5=NVI:41:4:10                                                        │
│                                                                        │ Bm6=NVI:41:4:11                                                        │
│                                                                        │ Bm7=NVI:41:4:12                                                        │
│                                                                        │ Bm8=NVI:41:4:24                                                        │
│                                                                        │ Bm9=NVI:41:4:25                                                        │
│                                                                        │ Bm10=PDT:41:4:24                                                       │
│                                                                        │ Bm11=PDT:41:4:25                                                       │
│                                                                        │ Bm12=NVI:43:9:41                                                       │
│                                                                        │ Bm13=VBL:41:4:24                                                       │
│                                                                        │ Bm14=VBL:41:4:25                                                       │
│                                                                        │ Bm...                                                                  │
└────────────────────────────────────────────────────────────────────────┴────────────────────────────────────────────────────────────────────────┘
✓ Eval complete (ID: eval-jEJ-2026-09-20T05:53:37)

» View results: promptfoo view
» Share with your team: https://promptfoo.app
» Feedback: https://promptfoo.dev/feedback


Results:
  ✓ 3 passed (100%)
  0 failed (0%)
  0 errors (0%)
Duration: 0s (concurrency: 4)

```



#### Baseline scores

| Metric              | Score | Baseline Rate |
|---------------------|-------|---------------|
| Basic Extraction    | 100%  | 100%          |
| Default version     | 100%  | 100%          |
| Expected format     | 100%  | 100%          |
| Multiple versions   | 100%  | 100%          |
| Range Extraction    | 100%  | 100%          |


#### Conclusion of this iteration

- Because scores remain at 100% across all metrics, the prompt is both deterministic and performing as expected.


## Iteration 3

### Prompt

So, in this iteration I will remove the text from the contraints section and evaluate the prompt again.

```markdown

# Scripture Extractor

<Role>
You are a deterministic extractor of biblical references. The references are embedded in a text file, which are the sermon notes. The result will feed another software.
</Role>


<Task>
Read the sermon notes file (notes_file parameter), extract the scripture references, show the extracted references to the user alligned with the format specified in the context section.
</Task>



<Context>

### Input parameters
- notes_file: This is the file name of the file with the sermon notes.

### Allowed tools
- read

### Workflow

1. Validate the notes_file file exists and is a valid text document (.txt)
2. Read the file notes_file, line by line. Each line could be a separate scripture reference (). DO NOT create any temporary file to read the content of notes_file; read it directly.

  a) Format the scripture reference according to the specified format

    `` `
    Bm[n]=[B]:[NL]:[C]:[V]
    `` `

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

4. Show the formatted scripture references to the user.


</Context>

<Constraints>
</Constraints>


```


### Evaluation

```text

Starting evaluation eval-529-2026-09-20T05:59:19
Running 3 test cases (up to 4 at a time)...
Evaluating [████████████████████████████████████████] 100% | 3/3 | Devin SWE-1.7 "@extractor" scrpts_file=./Sermons/MATEO_13-t2.txt

┌────────────────────────────────────────────────────────────────────────┬────────────────────────────────────────────────────────────────────────┐
│ scrpts_file                                                            │ [Devin SWE-1.7] @extractor-prompt.md --param                           │
│                                                                        │ notes_file={{scrpts_file}}                                             │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MATEO_13-t1.txt                                              │ [PASS] Bm0=NVI:40:13:30                                                │
│                                                                        │ Bm1=NVI:40:13:24                                                       │
│                                                                        │ Bm2=NVI:40:13:25                                                       │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MATEO_13-t2.txt                                              │ [PASS] I’ll load the prompt file and the referenced notes file, and    │
│                                                                        │ check Devin CLI docs for how `@file`/`--param` are                     │
│                                                                        │ handled.Bm0=NVI:40:13:28                                               │
│                                                                        │ Bm1=NVI:40:13:29                                                       │
│                                                                        │ Bm2=NVI:40:13:24                                                       │
│                                                                        │ Bm3=NVI:40:13:25                                                       │
│                                                                        │ Bm4=NVI:40:13:26                                                       │
│                                                                        │ Bm5=NVI:40:14:1                                                        │
│                                                                        │ Bm6=NVI:40:14:2                                                        │
│                                                                        │ Bm7=NVI:40...                                                          │
├────────────────────────────────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
│ ./Sermons/MARCOS_4.txt                                                 │ [PASS] Bm0=NVI:41:4:26                                                 │
│                                                                        │ Bm1=NVI:41:4:27                                                        │
│                                                                        │ Bm2=NVI:41:4:28                                                        │
│                                                                        │ Bm3=NVI:41:4:29                                                        │
│                                                                        │ Bm4=NTV:41:4:13                                                        │
│                                                                        │ Bm5=NVI:41:4:10                                                        │
│                                                                        │ Bm6=NVI:41:4:11                                                        │
│                                                                        │ Bm7=NVI:41:4:12                                                        │
│                                                                        │ Bm8=NVI:41:4:24                                                        │
│                                                                        │ Bm9=NVI:41:4:25                                                        │
│                                                                        │ Bm10=PDT:41:4:24                                                       │
│                                                                        │ Bm11=PDT:41:4:25                                                       │
│                                                                        │ Bm12=NVI:43:9:41                                                       │
│                                                                        │ Bm13=VBL:41:4:24                                                       │
│                                                                        │ Bm14=VBL:41:4:25                                                       │
│                                                                        │ Bm...                                                                  │
└────────────────────────────────────────────────────────────────────────┴────────────────────────────────────────────────────────────────────────┘
✓ Eval complete (ID: eval-529-2026-09-20T05:59:19)

» View results: promptfoo view
» Share with your team: https://promptfoo.app
» Feedback: https://promptfoo.dev/feedback


Results:
  ✓ 3 passed (100%)
  0 failed (0%)
  0 errors (0%)
Duration: 0s (concurrency: 4)

```
#### Baseline scores

| Metric              | Score | Baseline Rate |
|---------------------|-------|---------------|
| Basic Extraction    | 100%  | 100%          |
| Default version     | 100%  | 100%          |
| Expected format     | 100%  | 100%          |
| Multiple versions   | 100%  | 100%          |
| Range Extraction    | 100%  | 100%          |


#### Conclusion of this iteration

- Because scores remain at 100% across all metrics, the prompt is both deterministic and performing as expected. This shorten version is reliable enough to be used in production.