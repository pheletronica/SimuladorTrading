# Pasta de cache (data/)

Esta pasta guarda o histórico de candles já baixado, em arquivos `.json`,
um por combinação de corretora + ativo + timeframe.

## Nome do arquivo

```
data/<corretora>_<ativo>_<timeframe>.json
```

Exemplos:
- `data/binance_BTCUSDT_4h.json`
- `data/bybit_ETHUSDT_1d.json`

## Como funciona

1. Quando você abre o `index.html` e clica em **"Carregar dados"**, o site
   primeiro tenta ler o arquivo correspondente aqui na pasta `data/`.
2. Se encontrar, ele usa esse histórico como base e busca na corretora
   **só os candles mais novos** (a partir da última data salva) --
   muito mais rápido que baixar tudo de novo.
3. Se não encontrar (primeira vez, ou um ativo/timeframe novo), ele baixa
   o histórico completo direto da corretora.
4. Depois de carregar, aparece o botão **"Baixar JSON atualizado"** --
   clique nele, o navegador baixa um arquivo `.json` atualizado
   (histórico completo + os candles novos).
5. Pegue esse arquivo baixado e **substitua o arquivo correspondente
   aqui nesta pasta** (mesmo nome). Depois é só commitar e subir pro
   GitHub Pages -- da próxima vez que alguém abrir o site, ele já
   carrega esse cache e só busca o que faltou desde então.

## Importante

- Esse mecanismo de ler o arquivo local **só funciona quando o site está
  hospedado via http(s)** -- ou seja, no GitHub Pages, ou rodando por um
  servidor local (`python3 -m http.server`, por exemplo). Se você abrir
  o `index.html` direto no navegador (`file://...`), a leitura do JSON
  local é bloqueada por segurança do navegador, e ele vai sempre baixar
  o histórico completo da corretora -- o que ainda funciona
  normalmente, só não usa o cache.
- Formato interno de cada arquivo (colunar, pra ficar compacto):
  ```json
  {
    "t": [1640995200000, 1641009600000, ...],
    "o": [46000.0, 46200.5, ...],
    "h": [46500.0, 46700.0, ...],
    "l": [45800.0, 46000.0, ...],
    "c": [46200.5, 46400.0, ...],
    "v": [1234.5, 987.3, ...]
  }
  ```
