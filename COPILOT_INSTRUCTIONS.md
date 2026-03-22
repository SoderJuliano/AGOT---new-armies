# Instruções internas para criar submod AGOT (CK3 1.18.3)

## 1) Estrutura mínima do submod
- `descriptor.mod` dentro da pasta do mod.
- `common/men_at_arms_types/` para unidades novas.
- `localization/english/*.yml` em UTF-8 BOM.
- `gfx/interface/illustrations/men_at_arms_big/` e `men_at_arms_small/` para imagens das tropas.
- `events/`, `common/modifiers/` e `common/on_action/` para mecânicas especiais.

## 2) Men-at-Arms
- Cada unidade precisa de chave única (`icon` e nome de tipo de tropa).
- Para colocar unidades na aba de mercenários, usar `can_recruit = { always = no }` e criar companhias mercenárias que spawnam esses MAA via `create_maa_regiment` com `check_can_recruit = no`.
- Custo de recrutamento em ouro no intervalo solicitado (100–300): `buy_cost = { gold = X }`.
- Ajustar `ai_quality` para IA usar as tropas com frequência adequada.

## 3) Imagens dos cards
- Nome de imagem deve bater com `icon` da tropa.
- Gerar duas versões:
  - `men_at_arms_big`: 680x400
  - `men_at_arms_small`: 160x160
- No AGOT existem arquivos `.dds` com conteúdo PNG; essa abordagem funciona na prática para os cards.

## 4) Localização
- Arquivo começa com `l_english:`.
- Chaves necessárias por tropa:
  - `<maa_key>`
  - `<maa_key>_flavor`
- Modifiers/eventos também precisam de nome e `_desc`.

## 5) Mecânicas especiais
- Criar modifiers em `common/modifiers/`.
- Criar evento oculto mensal em `events/`.
- Encadear no `common/on_action/` usando padrão:
  - `monthly_playable_pulse` -> `on_actions = { ... }`
  - `monthly_ai_ruler_pulse` -> `on_actions = { ... }`
  - on_action intermediário com `events = { ... }`

## 6) Compatibilidade AGOT
- `descriptor.mod` com dependência `"A Game of Thrones"`.
- `supported_version="1.18.3.*"`.
- Evitar sobrescrever arquivos AGOT; criar arquivos com prefixo `zz_` para carregar por último sem `replace_path`.
- Para mercenários no AGOT atual, usar títulos `d_laamp_merc_*` com:
  - `landless = yes` em `common/landed_titles`
  - `government = landless_adventurer_government` + `succession_laws = { landless_adventurer_succession_law }` em `history/titles`
  - adicionar `camp_purpose_mercenaries` no holder
  - `create_landless_adventurer_title_history_effect = yes`

## 7) Checklist final antes de entregar
- Validar que todas as tropas aparecem com nome e flavor.
- Validar imagens small/big presentes para cada `icon`.
- Garantir textos em UTF-8 (YML em UTF-8 BOM).
- Confirmar que não existe README duplicado desnecessário.
