# ES-DE Mini Classic — port para RetroBat

<img width="1918" height="1008" alt="Screenshot_7" src="https://github.com/user-attachments/assets/5ba6721c-e14e-442f-b1b5-9cdf3225058c" />

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

<img width="1915" height="1009" alt="Screenshot_3" src="https://github.com/user-attachments/assets/7c35bf37-2d1b-4981-9744-bff26230c6a5" />
<img width="1024" height="537" alt="screen13" src="https://github.com/user-attachments/assets/84057244-5f57-4586-8b46-df896e6f50e8" />
<img width="1024" height="541" alt="screen12" src="https://github.com/user-attachments/assets/f1cfb996-dc10-4ff0-b736-7a4b22e4c335" />
