# DMZ SSJ4 Armors

Addon do **DragonMine Z** (Forge 1.20.1) que troca a textura das armaduras do mod quando o jogador está em:

- **Super Saiyajin 4** — tanto o do GT quanto o do Daima
- **Super Saiyajin Full Power** (a forma de cabelo verde do Broly)

A troca acontece com uma **transição suave** (fade) entre a roupa normal e a da forma, no mesmo ritmo da pelagem do SSJ4 do DMZ. Funciona no corpo, na mão em primeira pessoa e nas capas.

## Como compilar pelo GitHub

1. Suba esta pasta inteira para um repositório seu no GitHub.
2. `gradle.properties` já aponta para o DragonMine Z **v2.2** no CurseForge (`dmz_curse_file_id=9282248`). Para compilar contra outra versão, abra a [página de arquivos do DragonMine Z](https://www.curseforge.com/minecraft/mc-mods/dragonminez/files/all) e troque esse valor pelo **File ID** do arquivo desejado.
3. Faça o commit. A aba **Actions** roda o build sozinha; o `.jar` fica em **Artifacts → dmz-ssj4-armors**.
4. Coloque o `.jar` na pasta `mods` junto com o DragonMine Z.

Se o build falhar, o motivo aparece no log da Action. Uma causa provável é uma mudança no código do DMZ entre a versão 2.2 e a que você usar.

## Configuração

**Opções gerais** — `config/dmz_ssj4_armors-client.toml`:

| Opção | Padrão | O que faz |
|---|---|---|
| `enabled` | `true` | Liga/desliga o addon |
| `fadeTicks` | `10` | Duração da transição (20 ticks = 1 s) |

**Formas** — `config/dmz_ssj4_armors-forms.json` (criado na primeira vez que o jogo abre). As chaves seguem o padrão do lang do DMZ, `race.dragonminez.<raça>.form.<grupo>.<forma>`, com `true` (usa a roupa alternativa) ou `false`:

```json
{
  "race.dragonminez.saiyan.form.oozaru.supersaiyan4": true,
  "race.dragonminez.saiyan.form.oozaru.superfullpowersaiyan4": true,
  "race.dragonminez.saiyan.form.supersaiyan.supersaiyan4": true,
  "race.dragonminez.saiyan.form.legendaryforms.ssjfullpower": true
}
```

- Para usar a roupa em outra forma, adicione a linha com `true`.
- Para desligar uma, troque para `false`.
- Formas que não estão no arquivo seguem a detecção automática do SSJ4 (modelos `ssj4gt` / `ssj4d`).
- O arquivo é relido quando você salva; não precisa reiniciar o jogo.

## Como funciona

- As texturas alternativas ficam em `src/main/resources/assets/dmz_ssj4_armors/textures/armor/`, **com o mesmo nome** das do DMZ (`goku_gi_layer1.png`, `goku_gi_damaged_layer1.png`, ...).
- Um mixin no `ArmorTextureResolver` do DMZ olha qual textura ele escolheu e, se o jogador está numa forma-alvo e existe uma versão alternativa com o mesmo nome, devolve a alternativa. Se não existe, a roupa normal é usada, então nada quebra.
- Na transição, o addon gera texturas intermediárias (12 passos) e usa a do passo atual. A armadura do DMZ não aceita meia transparência, então peças que só existem numa das roupas (a camisa, a munhequeira...) *dissolvem* em pontos espalhados, igual na ida e na volta; o resto mistura as cores.
- Mixins usados: `ArmorTextureResolver`, `DbzArmorItem` / `DbzArmorPatrollerItem` / `DbzArmorCapeItem`, `DMZCustomArmorLayer`, `DMZCapeLayer`, `DMZRenderHand`.

Para trocar ou adicionar uma roupa: coloque o PNG na pasta acima com o nome exato da textura original do DMZ.

## Limitações conhecidas

- A transição começa quando o DMZ muda a forma ativa do jogador, não durante a animação de carga.
- As mãos em primeira pessoa e as capas dependem de métodos internos do DMZ (`renderDbzArmor` e `DMZCapeLayer.render`). Se uma versão futura do DMZ mudar isso, só essas duas partes deixam de trocar.
- Este projeto foi escrito lendo o código do DMZ 2.2 e **ainda não foi testado dentro do jogo**.

## Créditos e licença

- As texturas são do texturepack **"SSJ4 Armors", por Miiguel765Gamer**. Antes de publicar este addon (CurseForge, Modrinth etc.), confirme com ele que a redistribuição é permitida e mantenha o crédito.
- DragonMine Z é de seus autores (licença GPL v3 no repositório oficial). Este addon não inclui código nem texturas do DMZ.
- Defina uma licença para o **código** do addon (por exemplo MIT) e atualize `mod_license` e `mod_authors` em `gradle.properties`.
