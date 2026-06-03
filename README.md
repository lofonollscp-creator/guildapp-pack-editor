# GuildApp Pack Editor

Editor web per crear, editar, validar, exportar i publicar paquets de contingut de GuildApp.

Web publicada:

- [https://lofonollscp-creator.github.io/guildapp-pack-editor/](https://lofonollscp-creator.github.io/guildapp-pack-editor/)

## Què és

Aquest editor serveix per:

- crear paquets nous d'`Escape Room` o `Visita Guiada`;
- editar paquets existents;
- importar i exportar el JSON complet del paquet;
- preparar paquets perquè surtin al selector `Selecciona Aventura`;
- publicar paquets remots a `guildapp-content`;
- definir textos, etapes, narracions, blocs, validacions IA i metadades;
- generar o traduir contingut amb IA;
- preparar QRs i material auxiliar.

## Diferència entre mode web i mode localhost

La web publicada funciona bé per a:

- edició completa del paquet;
- càrrega del catàleg remot;
- importació i exportació;
- publicació a GitHub amb token;
- validació en navegador;
- configuració d'OpenRouter i perfils Ollama;
- ús de GuildAI.

Limitacions del mode web:

- no existeix el proxy local `/__ollama_proxy`;
- no existeix l'endpoint `/__validate_pack`;
- no llegeix `local_ai_config.json` local;
- les proves d'Ollama Cloud poden dependre de CORS si el navegador bloqueja la crida directa.

Per al mode complet local:

```bash
python3 editor/guildapp_editor_server.py
```

## Estructura general de la interfície

La pantalla té 4 zones principals:

1. Barra superior.
2. Sidebar esquerra amb el contingut del paquet.
3. Panell central d'edició.
4. Columna dreta amb la previsualització JSON.

## Barra superior

### Bloc de marca

- `GuildApp Pack Editor`
- versió visible del fitxer

### Bloc IA

- selector de proveïdor:
  - `OpenRouter`
  - `Ollama`
- selector de model d'OpenRouter;
- indicador d'estat IA;
- botó `⚙ Configuració IA`;
- botó de canvi de tema dia/nit.

### Accions principals

- `↻ Repo`: obre el catàleg remot de paquets.
- `✓ Validar`: valida el paquet en navegador.
- `🛡 Auditar`:
  - en `localhost`, fa validació web + auditoria addicional del servidor local;
  - en la web pública, es converteix en validació web pura.
- `↑ Publicar`: prepara la publicació del paquet a GitHub.
- `💬 GuildAI`: obre el xat d'ajuda per dissenyar o modificar el pack amb IA.
- `🎲 Encontres`: editor d'encontres aleatoris d'autoría.
- `📚 Coneixement`: base de coneixement per a La Padrina.
- `🖨 QRs`: genera i imprimeix els codis QR de les etapes.
- `📂 Importar`: importa un paquet enganxant JSON.
- `⬇ Exportar`: exporta el paquet complet.

## Sidebar esquerra

La sidebar mostra `Contingut del paquet`.

Botons:

- `＋ Etapa`: crea una nova etapa.
- `📖 Narrador`: crea una nova narració.

La llista central de la sidebar mostra:

- etapes;
- narracions;
- sub-etapes associades;
- ordre actual del contingut.

## Panell central: Metadades del paquet

Aquest bloc sempre és visible i defineix la capçalera funcional del paquet.

### Tipus d'experiència

- `🏰 Escape Room`
- `🗺 Visita Guiada`

El tipus activa o amaga parts de l'editor.

### Camps bàsics

- `ID del paquet`
- `Versió`
- `Títol` en `CA / ES / EN / FR`
- `Descripció` en `CA / ES / EN / FR`
- `Durada estimada (min)`
- `Bundled a l'app`
  - `Sí — inclòs a l'APK`
  - `No — fitxer extern`

### Publicació i selector de l'app

Serveix per controlar com apareix el paquet a la pantalla `Selecciona Aventura`.

Camps:

- `Autor`
- `Disponible al selector`
  - `Sí — es pot instal·lar`
  - `No — mostrar com pròximament`
- `URL base de descàrrega`
- `Etiqueta d'estat` en `CA / ES / EN / FR`

Ús habitual:

- si el paquet és descarregable, `Disponible al selector = Sí`;
- si encara no està llest, `Disponible al selector = No` i s'omple l'etiqueta `Pròximament`.

### Puntuació

Només per `Escape Room`.

Camps:

- `Puntuació inicial`
- `Penalització per saltar etapa`
- `Penalització per pista avançada`
- `Penalització per resposta incorrecta`

### Blocs del paquet

Només per `Escape Room`.

Els blocs agrupen etapes per al hub d'activitats.

Funcions:

- `＋ Afegir bloc`
- editar bloc existent
- ordenar blocs

Cada bloc té:

- `ID`
- `Ordre`
- `Icona`
- `Títol` multilingüe
- `Descripció` multilingüe
- `Pista per trobar el QR del següent bloc`

## Panell d'etapa

S'obre quan selecciones una etapa.

Botons del capçal:

- `✨ Generar`: genera contingut amb IA.
- `🌐 Traduir`: tradueix el contingut de l'etapa.

### Identificació

- `ID`
- `Codi QR`
- `Ordre`
- `Tipus de resposta`
  - `Resposta oberta`
  - `Text exacte`
  - `Número`
  - `Paraula clau`
  - `Foto`
  - `Visita guiada`
- `Temps màx (s)`
- `Punts base`
- `GPS (lat, lon)`
- `Pertany al bloc` en mode `Escape Room`

### Contingut multilingüe

Per idiomes `CA / ES / EN / FR`:

- `Títol del punt`
- `On anar / instruccions`
- `Descripció del lloc`
- `Nota educativa` en els camps on toca

### Respostes correctes

Només en etapes que no són purament de visita.

Permet:

- afegir múltiples respostes;
- definir alternatives vàlides.

### Pistes

- llista ordenada de pistes;
- una mateixa etapa pot tenir diverses pistes escalades.

### Verificació IA La Padrina / El Meco

Defineix com s'ha de validar la prova quan la resposta és oberta o necessita criteri semàntic.

Camps:

- criteri principal de validació;
- `Keywords de validació`;
- `Longitud mínima`;
- `Requereix número`.

### Regles IA anti-spoiler

Serveixen per limitar què pot dir la IA a l'usuari.

Botons:

- `⚙ Generar guardrails`
- `🧪 Simular guardrails`

Camps:

- `Temes permesos`
- `Nivell màxim de pista`
- `Mode estricte`
- `No revelar coordenades`
- `Respostes/fragments prohibits`
- `Keywords internes de solució`
- `Alias de resposta`
- `Frases spoiler`
- `Ubicacions sensibles`

### Funcions avançades de la app

Camps:

- `Pista de camp anti-bloqueig`
- `Prompt de foto`
- `Rals atorgats`
- `Item atorgat`
- `Rol requerit`
- `Items requerits`
- `Frase AR / aparició`

### Imatges de contingut

Permet afegir imatges visuals a l'etapa.

Funcions:

- pujar una o diverses imatges;
- mantenir-les associades a l'etapa.

### Sub-etapes d'aquest punt

Les sub-etapes es guarden com a autoría i edició futura.

Botó:

- `＋ Nova sub-etapa`

### Accions d'etapa

- `🗑 Eliminar`
- `📋 Duplicar`
- `↑ Pujar`
- `↓ Baixar`

## Panell de sub-etapa

S'obre quan selecciones una sub-etapa.

Compatibilitat actual:

- es conserva al paquet exportat/publicat;
- la app actual no l'executa com a flux nadiu independent.

Inclou:

- `ID`
- `Ordre dins l'etapa`
- `Punts`
- `Tipus de resposta`
- `Temps màx (s)`
- `Títol / context / pista` en 4 idiomes
- `Respostes correctes`
- `Pistes`

Accions:

- `🗑 Eliminar`
- `↑`
- `↓`
- `← Tornar a l'etapa`

## Panell de narració

S'obre quan selecciones una narració.

Compatibilitat actual:

- `before_stage` i `after_stage` sí que encaixen amb la lògica de la app;
- `standalone` es conserva sobretot com a material d'autoría.

Botons del capçal:

- `✨ Generar`
- `🌐 Traduir`

Camps:

- `ID`
- `Moment d'aparició`
  - `Autònom (QR o GPS)`
  - `Abans d'una etapa`
  - `Després d'una etapa`
  - `En entrar a la zona`
- `Etapa de referència`
- `Fitxer àudio`
- `Text` en `CA / ES / EN / FR`

Accions:

- `🗑 Eliminar`
- `↑ Pujar`
- `↓ Baixar`

## Columna dreta: Vista prèvia JSON

Mostra en temps real:

- recompte d'etapes;
- recompte de narracions;
- recompte de sub-etapes;
- JSON complet del paquet tal com s'exportarà.

Serveix per:

- entendre l'estructura final;
- detectar errors de dades;
- copiar o inspeccionar el resultat abans de publicar.

## Finestra `Repo`

Mostra els paquets del catàleg remot.

Funcions:

- `↻ Actualitzar catàleg`
- `Esborrar token GitHub`
- `⚙ Configuració IA`
- llistar paquets publicats;
- carregar un paquet remot existent per editar-lo.

Cada targeta acostuma a mostrar:

- títol;
- descripció;
- `id`;
- tipus;
- estat (`Bundle`, `Descarregable`, `Pròximament`).

## Finestra `Validar`

Fa la validació estructural del paquet.

Comprova, entre altres coses:

- camps obligatoris;
- localitzacions mínimes;
- coherència d'etapes i narracions;
- fitxers de coneixement;
- paquets remots i etiquetes d'estat;
- fitxers necessaris per a publicació.

Sortida:

- nombre d'errors;
- nombre d'avisos;
- detall per incidència.

## Finestra `Auditar`

Comportament segons entorn:

- en web pública: fa validació web;
- en `localhost`: afegeix auditoria del servidor local.

És útil per detectar problemes abans d'exportar o publicar.

## Finestra `Exportar`

Opcions:

- `📋 Copiar`
- `⬇ Descarregar .json`
- `⬇ Entrada manifest`

Serveix per obtenir:

- el bundle complet;
- l'entrada de catàleg/manifest.

## Finestra `Importar JSON`

Permet enganxar un paquet complet en text JSON i carregar-lo a l'editor.

Flux:

1. Obres `📂 Importar`.
2. Enganxes el JSON.
3. Prems `Importar`.

## Finestra `Publicar`

Flux de publicació:

1. valida i audita;
2. comprova si el paquet és extern i no bundlejat;
3. prepara la previsualització de fitxers;
4. mostra el resum de publicació;
5. `Confirmar i publicar`.

Requisits:

- token GitHub amb permís `Contents: Read and write`;
- paquet remot ben format;
- si surt al selector, metadades coherents.

## Finestra `Configuració IA`

Té dues pestanyes:

### `OpenRouter`

Funcions:

- afegir una o més claus;
- marcar clau activa;
- tenir claus de respaldo;
- guardar-ho en `localStorage`.

### `Ollama`

Funcions:

- afegir perfils;
- definir:
  - `Etiqueta`
  - `Model`
  - `Endpoint`
  - `Format de l'API`
  - `Clau API`
- provar la connexió activa.

Formats suportats:

- `OpenAI Compatible (/v1/chat/completions)`
- `Ollama Natiu (/api/chat)`

## Finestra `GuildAI`

És el xat d'assistència per construir el paquet amb IA.

Funcions:

- conversa lliure sobre el pack;
- suggeriments de noves etapes, textos o modificacions;
- `📦 Estat` per injectar l'estat actual del paquet al prompt;
- generació d'accions proposades;
- aplicació d'accions:
  - individualment;
  - `Aplicar totes`.

Botons:

- `🔄 Nou`
- `📦 Estat`
- `Envia`
- `Aplicar totes` quan hi ha canvis pendents

Drecera:

- `Cmd+Enter` o `Ctrl+Enter` envia el missatge.

## Finestra `Encontres`

Editor d'encontres aleatoris d'autoría.

Compatibilitat actual:

- es desen al paquet/editor;
- la runtime actual encara pot mantenir la seva implementació interna.

Funcions:

- crear `＋ Nou encontre`;
- editar:
  - `ID`
  - `Rals (+/-)`
  - `Icona`
  - text en `CA / ES / EN / FR`;
- `🌐 Traduir amb IA`;
- `⬇ Exportar Dart`;
- `📂 Importar JSON`;
- eliminar l'encontre seleccionat.

Subfinestres:

- `Exportar encontres`
  - copiar Dart
  - descarregar snippet `.dart`
  - copiar JSON
- `Importar encontres (JSON)`

## Finestra `QRs`

Genera els codis QR físics de les etapes.

Funcions:

- veure totes les targetes QR;
- `🖨 Imprimir`;
- `⬇ Descarregar tots (.png)`.

Cada targeta pot incloure:

- número d'etapa;
- títol;
- QR;
- pista de localització;
- instruccions d'escaneig.

## Finestra `Coneixement`

És la base de coneixement usada per La Padrina.

Té dues parts:

### Fitxers Markdown

Funcions:

- `＋ Nou fitxer`
- renombrar fitxer
- editar contingut per idiomes
- eliminar fitxer

Cada fitxer:

- es guarda per nom lògic;
- pot tenir contingut `CA / ES / EN / FR`.

### Glossari

Funcions:

- definir termes clau;
- donar traduccions per idiomes;
- reforçar el context de la IA.

## Finestra `Editar Bloc`

Permet editar un bloc del paquet.

Camps:

- `ID del bloc`
- `Ordre`
- `Icona`
- `Títol` en 4 idiomes
- `Descripció` en 4 idiomes
- `Pista per trobar el QR del següent bloc`

Accions:

- `💾 Desar`
- `Cancel·lar`

## Funcions IA dins dels panells

Segons el panell seleccionat, apareixen:

- `✨ Generar`
- `🌐 Traduir`

Actualment es poden usar sobre:

- etapes;
- sub-etapes;
- narracions;
- encontres.

## Compatibilitat funcional important

### Natiu o gairebé natiu

- metadades del paquet;
- etapes;
- validacions;
- selector remot;
- QRs;
- publicació remota;
- base de coneixement;
- guardrails IA.

### Conservat per autoría o parcial

- sub-etapes;
- narracions `standalone`;
- encontres aleatoris mentre no es connectin del tot a runtime.

## Fluxos recomanats

### Crear un paquet nou

1. Defineix tipus de paquet.
2. Omple metadades.
3. Crea etapes.
4. Afegeix validacions, pistes i guardrails.
5. Passa `Validar`.
6. Exporta o publica.

### Publicar un paquet descarregable

1. Marca `Bundled a l'app = No`.
2. Omple `Disponible al selector = Sí`.
3. Revisa `URL base de descàrrega`.
4. Valida.
5. Publica a GitHub.

### Publicar un paquet com `Pròximament`

1. `Disponible al selector = No`.
2. Omple `Etiqueta d'estat` en idiomes.
3. Desa o publica el catàleg corresponent.

## Dreceres

- `Cmd+Enter` / `Ctrl+Enter`: enviar missatge a GuildAI.
- `Cmd+E` / `Ctrl+E`: obrir exportació.

## Persistència local

L'editor guarda estat local de:

- configuració IA;
- esborranys;
- part del material d'autoría.

## Requisits per publicar a GitHub

Necessites un token amb:

- `Contents: Read and write`

S'usa per:

- pujar paquets nous;
- actualitzar paquets existents;
- carregar el catàleg remot i preparar publicació.

## Manteniment del lloc web

Aquest repo es publica amb GitHub Pages.

Qualsevol canvi a:

- `index.html`
- `README.md`
- `local_ai_config.example.json`
- `.github/workflows/deploy-pages.yml`

pot tornar-se a desplegar amb el workflow del repo.
