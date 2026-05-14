# Beyond Caption Quality: Multi-Objective Evaluation of Compact Vision-Language Models for Real-Time Brazilian Portuguese Audiodescription

This repository packages the paper artifacts and supporting assets for a study on compact vision-language models (VLMs) for Brazilian Portuguese assistive image description. The evaluation goes beyond caption quality alone and compares semantic quality, image-text alignment, speech robustness, and real-time latency under a unified protocol.

Repository assets:
- [dataset/U81-Scenes](dataset/U81-Scenes)
- [prompts](prompts)
- [figures/analysis](figures/analysis)
- [figures/dataset](figures/dataset)

## Study at a glance

| Item | Value |
| --- | --- |
| Task | Brazilian Portuguese assistive image description / audiodescription |
| Dataset | U81-Scenes |
| Images | 16 |
| Reference captions | 1 professional PT-BR caption per image |
| Model configurations | 9 |
| Prompt variants | 3 |
| Repetitions | 5 |
| Total runs | 2160 |
| Inference backend | Ollama |
| TTS | Kokoro-82M (`pf_dora`) |
| ASR | `openai/whisper-small` |

## Evaluated models

- `gemma3:4b`
- `gemma4:e2b`
- `gemma4:e4b`
- `ministral-3:3b`
- `qwen2.5vl:3b`
- `qwen3-vl:2b`
- `qwen3.5:0.8b`
- `qwen3.5:2b`
- `qwen3.5:4b`

## Main results

### Best model by objective

| Objective | Model | Evidence |
| --- | --- | --- |
| Reference overlap | `qwen2.5vl:3b` | Highest CIDEr: `0.518` |
| Semantic similarity | `qwen3.5:4b` | Highest SPICE: `1.159`; highest BERTScore: `0.634` |
| Image-text alignment | `gemma4:e2b` | Highest CLIPScore: `2.222`; highest RefCLIPScore: `1.273` |
| Speech robustness | `ministral-3:3b` | Lowest WER: `0.038`; lowest CER: `0.030` |
| Lowest latency | `ministral-3:3b` / `gemma3:4b` | Mean TTFA about `496 ms` / `510 ms` |

### Prompt-level findings

- `p01` achieved the best METEOR (`4.373`), CLIPScore (`2.076`), RefCLIPScore (`1.202`), and lowest WER (`0.065`).
- `p02` achieved the best CIDEr (`0.411`), SPICE (`0.998`), CAPIVARAScore (`0.458`), BERTScore (`0.628`), and lowest CER (`0.051`).
- `p03` was the least deployment-friendly prompt. It increased WER (`0.126`), CER (`0.103`), and latency, and also showed worse repeat stability.

### Main conclusion

The central finding is that model selection for assistive audiodescription should be treated as a multi-objective deployment problem rather than a single-metric ranking problem. Models that score well on semantic or reference-based metrics can still be poor choices if they are slow or generate text that degrades downstream TTS/ASR performance.

## Dataset

The repository includes the dataset under [dataset/U81-Scenes](dataset/U81-Scenes).

Structure:
- `dataset/U81-Scenes/images`: the 16 input images used in the experiments
- `dataset/U81-Scenes/annotations`: professional Brazilian Portuguese reference descriptions
- `dataset/U81-Scenes/annotations_en`: English translations used for metrics that depend on English linguistic resources

Dataset overview image:
- [figures/dataset/u81_scenes_dataset_grid.png](figures/dataset/u81_scenes_dataset_grid.png)

## Prompts

The exact prompt texts are available in:
- [prompts/p01.txt](prompts/p01.txt)
- [prompts/p02.txt](prompts/p02.txt)
- [prompts/p03.txt](prompts/p03.txt)
- [prompts/README.md](prompts/README.md)

Prompt summaries:
- `p01`: shorter accessibility-oriented prompt aimed at blind or low-vision users
- `p02`: accessibility-oriented prompt that emphasizes scene purpose, spatial relations, relevant objects, actions, and visible text
- `p03`: more constrained professional audiodescription prompt with stylistic and structural guidance

<details>
<summary><strong>Prompt p01</strong></summary>

```text
Você é uma inteligência artificial que provê audiodescrição profissional para deficientes visuais. Descreva o ambiente, objetos e pessoas em Português do Brasil com clareza e objetividade.

Descreva o que há à frente na imagem. Detalhe o ambiente, a disposição dos objetos principais, pessoas e cores. Dê contexto suficiente para que eu entenda onde estou e o que me cerca, mas sem suposições.

Em texto corrido, sem classificação.
```

</details>

<details>
<summary><strong>Prompt p02</strong></summary>

```text
Descreva esta imagem para um usuário cego ou com baixa visão.
Priorize as informações necessárias para entender o propósito da imagem em contexto.
Comece com uma breve visão geral e, em seguida, descreva os objetos, pessoas, ações, relações espaciais e qualquer texto na tela mais relevantes e importantes.

Seja específico, preciso e conciso.

Não tente adivinhar detalhes incertos; diga quando algo não estiver claro.

Evite julgamentos estéticos irrelevantes ou detalhes excessivos que não contribuam para a compreensão.

Em texto corrido, sem classificação.
```

</details>

<details>
<summary><strong>Prompt p03</strong></summary>

```text
Descreva esta imagem em português do Brasil como uma áudio-descrição:

- Seja claro, conciso, correto, específico e vívido.
- Use terceira pessoa e tempo presente.
- Priorize o que é essencial para compreender a cena.
- Descreva apenas o que pode ser observado na imagem.
- Não infira intenção, emoção, pensamento ou contexto não visível.
- Em vez de interpretar, descreva os sinais visuais observáveis.
- Informe o ambiente, os elementos principais, suas posições relativas, ações visíveis, aparência relevante e qualquer texto legível importante.
- Evite adjetivos ornamentais, opinião estética e detalhes irrelevantes.
- Se houver incerteza, sinalize a incerteza brevemente.

Estruture a resposta assim:
1. Visão geral da cena em 1-2 frases.
2. Detalhes essenciais em ordem do geral para o particular.
3. Texto visível na imagem, se houver.
4. Ambiguidades ou limitações, se houver.

Em texto corrido, sem classificação.
```

</details>

## Figures

All exported analysis figures were copied to [figures/analysis](figures/analysis). This folder includes:

- `prompt_metric_scores.png`
- `avg_times_bar.png`
- `ttfa_boxplot.png`
- `tps_boxplot.png`
- `heatmap_model_prompt.png`
- `metric_correlation_heatmap.png`
- `metric_vs_word_count.png`
- `prompt_stability.png`
- `ranking_by_model.png`
- `ranking_by_model_prompt.png`
- `word_count_by_model.png`
- `word_count_by_model_prompt.png`
- `word_count_vs_wer_rtf.png`

Figures used directly in the current paper draft include:
- `prompt_metric_scores.png`
- `avg_times_bar.png`
- `u81_scenes_dataset_grid.png`

## Repository layout

```text
.
├── README.md
├── dataset
│   └── U81-Scenes
│       ├── annotations
│       ├── annotations_en
│       └── images
├── prompts
│   ├── p01.txt
│   ├── p02.txt
│   ├── p03.txt
│   └── README.md
└── figures
    ├── analysis
    └── dataset
```
