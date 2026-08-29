# ES-DE Mini Classic — port para RetroBat

## Créditos

- **Tema original ("ESDEmini"):** [Weestuarty-es-de/mini-es-de](https://github.com/Weestuarty-es-de/mini-es-de),
  criado por **Stuart Learmonth** para o ES-DE (EmulationStation Desktop Edition).
  - Suporte e paciência: Leon Styhre
  - Logotipos: Dan Patrick
  - Mini banners: DerSchlachter (LaunchBox)
  - Pixel art adicional (Sinclair Next): Simon Butler
- **Adaptação/port para o RetroBat:** [XPgiu](https://github.com/XPgiu) — reescrita
  completa do tema para o formato do EmulationStation (fork Batocera) usado pelo
  RetroBat, mantendo 100% da arte original. Detalhes técnicos da adaptação
  abaixo.
- **Licença:** Creative Commons Attribution-NonCommercial-ShareAlike (CC-BY-NC-SA)
  — ver [LICENSE](LICENSE). Uso não comercial, mesma licença nas redistribuições,
  crédito aos autores originais mantido. Ver também [CREDITS.md](CREDITS.md) e o
  [README original do ES-DE](README-ESDE-ORIGINAL.md).

Este repositório era um tema do **ES-DE (EmulationStation Desktop Edition)**. O
RetroBat usa outro EmulationStation (o fork Batocera, aqui na versão
`8.2.0-stable-win64`), e os dois motores de tema são **incompatíveis**: nomes de
elementos, sistema de views, variantes, seletores e bindings são todos
diferentes. Não existe conversão automática — o tema foi **reescrito** no
formato do RetroBat reaproveitando 100% da arte original.

## O que mudou

| Antes (ES-DE) | Agora (RetroBat) |
|---|---|
| `theme.xml` (formato ES-DE) | preservado em `theme-esde.xml`, sem uso |
| `capabilities.xml` | não existe no RetroBat — substituído por `<subset>` |
| `<variant>` (12) | subset **Console (moldura)** (6) × view `detailed`/`grid` |
| `<colorScheme>` (12) | subset **Esquema de cores** (12) |
| `<aspectRatio>` | RetroBat resolve sozinho (`verticalScreen`, `tinyScreen`) |
| `<grid>` / `<carousel>` do gamelist | `imagegrid`+`gridtile` / `textlist` |
| `<badges>` | imagens `md_favorite`, `md_kidgame`, `md_manual`, … |
| `<gamelistinfo>` | `<text name="gamelistInfo">` |
| `<systemstatus>` / `<clock>` | view `screen`: `clock`, `controllerActivity` |
| `${systemName}` (de `system/metadata/`) | `${system.fullName}` (nativo do ES) |
| `<systemdata>gamecountGames</systemdata>` | `{binding:total}` |
| `<gameselector>` aleatório | `{random:thumbnail}` / `{random:image}` |
| `navigationsounds.xml` | `<feature supported="navigationsounds">` |

## Estrutura nova

```
theme.xml                     ponto de entrada do RetroBat
_retrobat/
  lang/       en, pt_BR, es, fr  (rótulos)
  colorsets/  12 esquemas de cores (portados de colors.xml)
  consoles/   6 molduras: snes, usnesa, nes, mega, master, psx
  views/      screen, menu, common, system, basic, detailed, grid, gamecarousel
  systems/    95 arquivos de mapeamento de nome de sistema -> nome de arte
theme-esde.xml                theme.xml original do ES-DE (referência)
core/, system/                arte original, intocada
```

### Mapeamento de sistemas

Os nomes de sistema do RetroBat nem sempre batem com os nomes dos arquivos de
arte do ES-DE (`gamecube` vs `gc`, `3ds` vs `n3ds`, `c20` vs `vic20`,
`jaguar` vs `atarijaguar`…). Cada elemento de arte declara dois caminhos:

```xml
<path>${themePath}/core/banner/${system.theme}.png</path>   <!-- nome direto -->
<path>${themePath}/core/banner/${artName}.png</path>        <!-- nome mapeado -->
```

O ES usa o **último** `<path>` que existir no disco. `${artName}` fica vazio por
padrão e só é definido em `_retrobat/systems/<sistema>.xml`. Para corrigir ou
adicionar um sistema, basta criar/editar um desses arquivos:

```xml
<theme>
  <formatVersion>7</formatVersion>
  <variables><artName>snes</artName></variables>
</theme>
```

Sistemas sem arte correspondente caem no nome em texto (`logoText`) — não
quebram nada.

## Como usar

1. Copie a pasta inteira para
   `RetroBat\emulationstation\.emulationstation\themes\ES-DE-Mini-Classic\`
2. No RetroBat: **Menu → Configurações da interface → Tema** → `ES-DE-Mini-Classic`
3. Em **Configuração do tema** ajuste:
   - **Esquema de cores** — SNES, USNESA, NES, Famicom, Sega, Master System,
     Hystoria, Hystoria ALT, Limited Edition, Its a Mario, Mrs LEphant, Gold
   - **Console (moldura)** — barras superior/inferior do console
   - **Destaque do sistema** — `Vídeo aleatório` (padrão: roda um vídeo de um
     jogo qualquer daquele sistema) ou `Logo de jogo aleatório`
   - **Marca d'água do sistema** / **Ilustração do console** — liga/desliga
4. Lista vs grade: **Configurações da interface → Estilo de lista de jogos**
   (`detailed` = lista do tema original, `grid` = grade).

## Pastas legadas do ES-DE

`auto-allgames/`, `auto-favorites/`, `auto-lastplayed/`, `custom-collections/`,
`mario/` e `zelda/` foram movidas para `_esde-legacy/`. O ES do RetroBat trata
`<tema>/<sistema>/theme.xml` como sobreposição por sistema e tentava carregar
esses arquivos no formato do ES-DE, gerando
`<formatVersion> tag missing!` no `es_log.txt` a cada troca de coleção.
Fora do caminho de varredura, o erro some.

`controllerActivity`, `batteryIndicator` e `networkIcon` estão ocultos em
`_retrobat/views/screen.xml`: sem `<imagePath>` o ES desenha retângulos
piscando no canto inferior esquerdo, por cima da barra de ajuda — e o tema
original não tem esses indicadores.

## SVGs: classes CSS não funcionam

O renderizador de SVG do EmulationStation lê apenas `style=` **inline** —
blocos `<style>` e seletores de classe são ignorados, e todo shape com
`class="stN"` cai no preenchimento padrão (preto).

```
nes.svg   <path style="fill: rgb(255,255,255)" …   ✅
cps2.svg  <path class="st1" …                      ❌ vira preto
```

157 artes de `core/systems/`, `system/logos/system-logo-white/` e
`system-logo-color/` estavam nesse formato. Foram convertidas: cada
`class="stN"` virou o `style=` correspondente, com o `style=` próprio do
elemento por último (mantendo a precedência do CSS). Originais em
`_esde-legacy/svg-originais/`.

Nos `cps1/2/3.svg` a classe `.st2` (o corpo do logo) não declarava `fill`
nenhum — por isso o CAPCOM saía preto. Recebeu `fill:#FFFFFF`. O texto
"PLAY SYSTEM" é um recorte (`fill-rule:evenodd`), então continua legível:
aparece o fundo através dele. Em `sgb.svg`, duas classes só de contorno
receberam `fill:none`.

**Ao adicionar arte nova:** use `style=` inline ou atributos `fill=`/`stroke=`.
Classe CSS não renderiza.

## Vídeo e letreiro aleatórios não sincronizam

Cada token `{random...}` é resolvido de forma independente em
`SystemView::getViewElements()`. Não existe "jogo sorteado" compartilhado
entre elementos, nem propriedade para sincronizar, nem manipulação de string
no XML do tema para derivar um caminho de mídia a partir de outro. Dois
elementos `{random}` = dois jogos diferentes, sempre. (O próprio
es-theme-carbon usa `{random:thumbnail}` 8× em `animatedcovers.xml`
justamente para obter 8 jogos distintos.)

O tema mostra letreiro + vídeo lado a lado assumindo isso. Para ver um jogo
só por vez, use **Destaque do sistema → Logo de jogo aleatório**, que deixa
um único elemento na tela.

Tokens aceitos: `{random}`, `{random:image}`, `{random:thumbnail}`,
`{random:marquee}`, `{random:fanart}`, `{random:titleshot}`.

## Limites conhecidos

- O ES-DE tinha 3 tamanhos de fonte (`medium/large/x-large`) por variante; o
  RetroBat não expõe isso. As medidas ficaram no equivalente ao `medium`.
- `system/metadata/` (descrições traduzidas de cada console) e
  `system/coversize/` não são lidos: o ES do RetroBat já fornece
  `${system.fullName}`, `${system.manufacturer}` e `${system.releaseYear}`.
  Os arquivos foram mantidos, mas ficam inertes.
- O ES-DE mostrava "Emulator" no painel direito; aqui a linha virou
  "Partidas" (`md_playcount`), que é o metadado equivalente disponível.
- As medidas do carrossel (`logoSize`, `maxLogoCount` em
  `_retrobat/views/system.xml`) foram calibradas para 16:9. Em telas muito
  largas ou verticais pode valer ajustar esses dois valores.

Licença original mantida: Creative Commons CC-BY-NC-SA — Stuart Learmonth.
