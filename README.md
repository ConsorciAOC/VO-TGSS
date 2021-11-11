# VO-TGSS
## 1- Introducció
Aquest document detalla la missatgeria associada al servei de la Tesorería General de la Seguridad
Social (TGSS en endavant).
Per poder realitzar la integració cal conèixer prèviament la següent documentació:
* Document del Servei Via Oberta.
* Document de Missatgeria Genèrica de la PCI del Consorci AOC. 

## 2- Transmissions de dades disponibles
Les dades disponibles a través del servei són les que es presenten a continuació:
Emissor: TGSS (Tesorería General de la Seguridad Social)

Producte: SCSP_TGSS
| Modalitat | Descripció |
|---|---|
| Q2827003ATGSS001B | Situació de deute. |
| Q2827003ATGSS006 | Alta laboral a data concreta. |
| Q2827003ATGSS008 | Acreditació de jornades agràries. |

## 3- Missatgeria dels serveis
A continuació es detalla la missatgeria corresponent al bloc de dades específiques de les modalitats de consum del producte SCSP_TGSS. 
> La TGSS requereix que s’informin les dades del funcionari que realitza la consulta. Així, cal informar l’element *Funcionario* del bloc de dades genèriques: */Peticion/Funcionario* i *//SolicitudTransmision/DatosGenericos/Solicitante/Funcionario.* 

### 3.1 Situació de deute (Q2827003ATGSS001B)
#### 3.1.1 Petició
##### 3.1.1.1 Dades genèriques
| Element | Descripció |
|---|---|
| //DatosGenericos/Titular/TipoDocumentacion | Tipus de documentació (DNI, NIF, NIE, passaport i CIF). |
| //DatosGenericos/Titular/Documentacion | Documentació. |

##### 3.1.1.2 Dades específiques
Aquesta modalitat no requereix informar dades específiques.

#### 3.1.2 Resposta – dades específiques
De l’schema associat a la resposta especifica, el servei informa les dades que es detallen a continuació.

![Resposta Dades Específiques](https://github.com/ConsorciAOC/VO-TGSS/blob/main/images/resposta%20dades%20especifiques.png)

| Element | Descripció |
|---|---|
| DatosEspecificos/Retorno/Codigo | Codi d’estat (vegeu apartat 3.1.2.1). |
| DatosEspecificos/Retorno/Resultado | Codi informatiu del resultat si la consulta s’ha realitzat correctament (S: està al corrent de pagament, N: amb deute).  |
| DatosEspecificos/Retorno/Descripcion | Descripció del resultat. |

##### 3.1.2.1 Codis de resultat
* 0: Consulta realitzada correctament.
* 0251: Tipus de documentació incorrecta.
* 0252: Documentació incorrecta.
* 0253: La persona física o jurídica té més d’un identificador principal.
* 0254: La persona física o jurídica no figura inscrita com empresari en el sistema de la Seguridad Social i no té assignat CCC en cap Règim del sistema de la Seguridad Social.
* 0502: Error realizant la consulta.
* 0401: Format de petició incorrecte (no compleix l’schema).

#### 3.1.3 Joc de proves
Aquests són alguns dels titulars amb els quals es pot provar en l’entorn de proves de la TGSS (les dades no són reals per aquests casos).

| Tipus documentació | Documentació | Corrent de pagament |
|---|---|---|
| DNI | 12345678Z | S |
| DNI | 23456789D | N |
| DNI | 34567890V | NO CONSTA |
| NIE | X1234567L | S |
| NIE | Y2345678Z | N |
| NIE | Z3456789D | NO CONSTA |
| CIF | A78518966 | S |
| CIF | 2A44162428 | N |
| CIF | B07696073 | NO CONSTA |
| Pasaporte | RE20110111 | S |
| Pasaporte | AB20110222 | N |
| Pasaporte | CA20110333 | NO CONSTA |

### 3.2 Alta laboral a data concreta (Q2827003ATGSS006)

![3.2 Alta laboral a data concreta](https://github.com/ConsorciAOC/VO-TGSS/blob/main/images/3.2%20Alta%20laboral%20a%20data%20concreta%20(Q2827003ATGSS006).png)

#### 3.2.1 Petició
##### 3.2.1.1 Dades genèriques
| Element | Descripció |
|---|---|
| //DatosGenericos/Titular/TipoDocumentacion | Tipus de documentació de la persona física: DNI, NIF, NIE, Passaport. |
| //DatosGenericos/Titular/Documentacion | Documentació de la persona física. |

El sistema composa l’identificador de persona física (IPF) que es transmet a la TGSS en funció d’aquestes dades segons el format establert: cadena d’onze posicions sent el primer dígit el tipus de documentació (1 per DNI, 6 per NIE i 7 per passaport) seguit de la documentació.

##### 3.2.1.2 Dades específiques 
| Element | Descripció |
|---|---|
| DatosEspecificos/FECHA | Data que determina la data sobre la que es consulta la situació (DDMMAAAA). |

#### 3.2.2 Resposta – dades específiques 
| Element | Descripció |
|---|---|
| DatosEspecificos/IPF | Identificació de la persona física. Numèric d’onze posicions sent el primer dígit el tipus de documentació (1 per DNI, 6 per NIE i 7 per passaport) seguit de la documentació. |
| DatosEspecificos/FECHA | Data que determina la data sobre la que es consulta la situació (DDMMAAAA). |
| DatosEspecificos/Retorno/Codigo | Codi de resultat de la consulta (vegeu apartat 3.2.2.1) |
| DatosEspecificos/Retorno/Descripcion | Literal de la resposta. |

##### 3.2.2.1 Codis de resultat 
* 0001: En situació d’Alta laboral a la data.
* 0002: No figura d’Alta laboral a la data.
* 0003: IPF erroni.
* 0004: IPF inexistent en la base de dades de Seguridad Social.
* 0005: IPF està duplicat en la base de dades de Seguridad Social.
* 0006: El format de la data de petició és incorrecte.
* 0502: Error realizant la consulta.
* 0401: Format de petició incorrecte (no compleix l’schema).

#### 3.2.3 Joc de proves
| Tipus documentació | Documentació | Data |
|---|---|---|
| DNI | 53208491S | alta desde 07101991 a 06041993 |
| DNI | 53208491S | alta desde 15061993 a 30061997 |
| DNI | 53208491S | alta desde 01071997 |
| DNI | 62467526D | alta desde 03072008 |
| DNI | 68014083R | alta desde 01011969 a 31122002 |
| DNI | 77618734J | IPF inexistent |
| DNI | 97124184Z | alta desde 06111974 a 31011975 |
| DNI | 97124184Z | alta desde 01081977 a 09051990 |
| DNI | 97124184Z | alta desde 13051990 a 19062002 |
| DNI | 97124184Z | alta desde 21062002 |
| DNI | 98987630T | IPF duplicat |
| NIE | X9875775N | alta desde 03012008 a 08032011 |

### 3.3 Adreditació de jornades agràries (Q2827003ATGSS008)
#### 3.3.1 Petició
##### 3.3.1.1 Dades genèriques
| Element | Descripció |
|---|---|
| //DatosGenericos/Titular/TipoDocumentacion | Tipus de documentació de la persona física: DNI, NIF, NIE, Passaport. |
| //DatosGenericos/Titular/Documentacion | Documentació de la persona física. |

El sistema composa l’identificador de persona física (IPF) que es transmet a la TGSS en funció d’aquestes dades segons el format establert: cadena d’onze posicions sent el primer dígit el tipus de documentació (1 per DNI, 6 per NIE i 7 per passaport) seguit de la documentació.

##### 3.3.1.2 Dades específiques
| Element | Descripció |
|---|---|
| DatosEspecificos/Fecha_Desde | Camp opcional que conté el mes i any d’inici per un període determinat (AAAAMM). |
| DatosEspecificos/Fecha_Hasta | Camp opcional que conté el mes i any de fi per un període determinat (AAAAMM). |

![3.3.1.2 Dades específiques](https://github.com/ConsorciAOC/VO-TGSS/blob/main/images/3.3.1.2%20Dades%20espec%C3%ADfiques.png)

El servei retorna 100 situacions com a màxim per a un determinat rang de dates. Així, en cas que en el període sol·licitat hi hagi més de 100 situacions caldrà reduïr el rang de sol·licitud.
El servei permet no informar el rang de dates però en cas que de s’informi, requereix els dos llindars (Fecha_Desde i Fecha_Hasta).

#### 3.3.2 Resposta – dades específiques
| Element | Descripció |
|---|---|
| DatosEspecificos/IPF | Identificació de la persona física. Numèric d’onze posicions sent el primer dígit el tipus de documentació (1 per DNI, 6 per NIE i 7 per passaport) seguit de la documentació. |
| DatosEspecificos/Fecha_Desde | Camp opcional que conté el mes i any de fi per un període determinat (AAAAMM). |
| DatosEspecificos/Fecha_Hasta | Camp opcional que conté el mes i any d’inici per un període determinat (AAAAMM). |
| DatosEspecificos/Retorno/Codigo | Codi de resultat de la consulta (vegeu apartat 3.2.2.1.) |
| DatosEspecificos/Retorno/Descripcion | Literal de la resposta. |
| DatosEspecificos/Situaciones/SITUACION | Bloc de dades corresponent a les dades d’una situació. |
| /SITUACION/NORMALIZACION | Indica si està normalitzat el camp nom i cognoms (1: no normalitzat, 2: normalizado)|
| //SITUACION/NOMBRE | Nom. |
| //SITUACION/NAF | Numero d’afiliació sobre el que s’informa la situació. |
| //SITUACION/REGIMEN | Literal del Régimen de Seguridad Social. |
| //SITUACION/CNAE | Activitat económica. |
| //SITUACION/FECHA_REAL_ALTA | Data d’inici de l’activitat laboral (AAAAMMDD). |
| //SITUACION/FECHA_EFECTO_ALTA | Data en que es produeixen els efectes de l’alta (AAAAMMDD). |
| //SITUACION/FECHA_REAL_BAJA |Data de finalizació de l’activitat laboral (AAAAMMDD). |
| //SITUACION/FECHA_EFECTO_BAJA | Data en que es produeixen els efectes de la baixa (AAAAMMDD). |

##### 3.3.2.1 Codis de resultat
* 0000: Operació correcta.
* 0001: No hi ha dades enregistrades per al període sol·licitat.
* 0002: IPF erroni.
* 0003: IPF inexistent en la base de dades de Seguridad Social.
* 0004: IPF està duplicat en la base de dades de Seguridad Social.
* 0005: El format de la data de petició és incorrecte.
* 0006: Existeixen més dades en el rang de dates indicat. Redueixi el rang.
* 0502: Error realizant la consulta.

#### 3.3.3 Joc de proves

| Tipus documentació  | Documentació | Dates | Resultat |
|---|---|---|---|
| DNI | 007177097Q | Des de 1971-01-01 al 1971-01-31 | |
| DNI | 007177097Q | Des de 2010-08-01 | |
| DNI | 080979388E | Des de 2009-05-01 | |
| DNI | 002223762F | Des de 2000-01-01 al 2011-08-01 | IPF Duplicat |

