# Animação de Cara Reativa ao Som

## Descrição
Página web que mostra um SVG de uma cara que se transforma suavemente entre dois estados (cara 1 ↔ cara 2) conforme o volume captado pelo microfone. Quanto mais alto o som, mais a cara, sobrancelhas e língua transitam para a versão 2.

## SVG Existente
O ficheiro `sound-03.svg` contém 6 grupos:
- `cara_1` / `cara_2` — duas versões da cara
- `sobrancelha1` / `sobrancelhas_2` — duas versões das sobrancelhas
- `lingua_1` / `lingua_2` — duas versões da língua

## Abordagem Escolhida
HTML + JavaScript puro com Web Audio API e smoothing exponencial. Sem dependências externas.

## Arquitetura

```
Microfone → getUserMedia → AudioContext → AnalyserNode → RMS → Smoothing → Opacity mapper → DOM (SVG)
```

### Fluxo de Dados
1. `getUserMedia` captura o stream do microfone
2. `AudioContext` + `AnalyserNode` (FFT size 256) analisa o espectro em tempo real
3. A cada `requestAnimationFrame`: lê `getByteFrequencyData()`, calcula RMS (0–255)
4. Exponential Moving Average: `smooth = smooth * 0.75 + rawRMS * 0.25` (SMOOTHING=0.25, ajustado para maior sensibilidade)
5. Normaliza para t (0–1) com threshold mínimo THRESHOLD=4 para ignorar ruído de fundo
6. Opacidade: `cara_1 = 1 - t`, `cara_2 = t` (o mesmo para sobrancelhas e língua)

### Mapeamento Opacidade
- `t = 0.0` (silêncio): cara_1 visível (opacidade 1), cara_2 invisível (0)
- `t = 1.0` (volume máximo): cara_1 invisível (0), cara_2 visível (1)
- Valores entre 0–1 produzem cross-fade gradual

### Tratamento de Erros
- Microfone negado: mensagem visível, mantém estado 1 (sem som)
- Limpeza automática de recursos (AudioContext, stream) via beforeunload

### Layout
- Página centrada, fundo escuro
- SVG escala ao ecrã mantendo `viewBox`
- Transições suaves via opacity animada

## Ficheiros
- `index.html` — ficheiro único com SVG inline e JS
- `sound-03.svg` — SVG original (fonte)
