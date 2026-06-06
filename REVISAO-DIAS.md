# Revisão do Prof. Stiven S. Dias — 226 comentários

Fonte: `tese_rev_ssdias_chap_7_8.pdf` (96 pp; apesar do nome, cobre Cap.1–8).
Tipos: **Caret** = inserção/troca de palavra sugerida · **Text/Highlight** = comentário.
Status: ✅ feito · 🟡 decisão/dado seu · 🖼️ figura nova · ♻️ já resolvido na revisão do Máximo · 💬 elogio.

## PROGRESSO
- ✅ **Cap.1** [2-18]: multi-sensor fusion, intracity/intercity, "explicitly accounted for", "computational resources", objetivo 1 = "nonlinear Bayesian filtering problem solved with EKF", "dominant factor", etc.
- ✅ **Cap.2** [19-52]: Jacobianos ∂f(x)/∂x|_{x=x̂} + "and"; "EKF's recursion"; equação **homogênea SE(3)**; métricas "accuracy/consistency/optimality"; PCRB J⁻¹⪰cov.
- ✅ **Cap.3** [53-62]: PICOC expandido; "The review is driven…"; Parsifal = ferramentas de SLR.
- ✅ **Cap.4** [63-84]: fusão geométrica = "line of sight (azimuth+elevation) + depth"; dashes na saída do detector.
- ✅ **Cap.5** [85-101]: definida posição global do objeto + velocidade da plataforma; **Q 7×7 completo** (=#54 Máximo, era a crítica principal); refs de eq corrigidas.
- ✅ **Figuras novas** (Cap.7): `pointnet_topology.pdf` (#174), `approaches_diagram.pdf` 3 abordagens (#175/#197/#198), `pipeline_frames.pdf` (vídeo→imagem #113/#139).
- ✅ **Cap.6** [100-152]: global-frame EKF explicitado (#117), novo parágrafo (#119), títulos de parágrafo em negrito+ponto (#130), "right-hand side of (6.8)" (#135), comentário **outdoor×indoor** dos datasets (#114-116), e **diagrama do pipeline recolorido** com legenda+fonte maior e o esquema de cores que o Dias pediu (#104-110).
- ✅ **Cap.7** [153-210]: métricas em lista estruturada (#189), termo "sprite" definido (#164), **recomendação explícita de favorecer recall p/ D&A** (#199), topologia/abordagens já com figuras (#174/175/197/198).
- ✅ **Cap.8**: a futura-obra já recomenda a fusão de alto recall (#214); contribuições recapituladas como concluding remarks (#223/#226).

## Figuras corrigidas a partir dos dados reais (sessão 4)
- ✅ **Cap.6** (dados do repo `tub`/tracker, output/e1b + flying_1/tracking_results.npz):
  - `tracking_result_raw.pdf` QUEBRADA em `tracking_traj.pdf` (trajetória ampliada, EKF colorido por tempo c/ colorbar, "Tatami perimeter" rotulado) + `tracking_errors.pdf` (erro no tempo + CDF) — #140/#141/#142/#143/#144.
  - `nis_consistency.pdf` EMPILHADA (a) sweep / (b) NIS no tempo, aumentada — #145.
  - Texto: 90th pct corrigido para **10.3 m** (cap6_stats.json p90=10.33; o "10.1" da figura antiga é que estava errado).
- ✅ **Cap.7** `tracking_stage.pdf` (editei `airsim/make_dissertation_figs.py::fig_tracker`): subcaptions encurtadas e movidas para ABAIXO de cada painel (a)/(b) — #186.
- ✅ **#95** (Cap.5 `ekf_equivalence.pdf`, cor do global EKF) → FEITO na **máquina principal** (ver seção "Regenerar plots" abaixo).

## Ainda em aberto (decisão sua / precisa de fonte)
- ✅ **#225** (Cap.8): contribuições convertidas em `enumerate` e **reordenadas pela sequência do pipeline** (SLR → detecção → tracking → generalidade). Intro ajustada; parágrafo Synthesis é temático, sem conflito.
- ✅ **Regenerar plots** — todos os 4 itens de figura concluídos:
  - **#95** (`ekf_equivalence`): máquina principal (tracker), via `tracking-comparison/fig_ekf_equivalence.py` — global = linha azul-clara grossa, local frame-consistent = pontilhado preto por cima (dados idênticos, só apresentação); legenda do Cap.5 ajustada (dashed→dotted).
  - **#140 / #145 / #186**: máquina de dados — ver seção "Figuras corrigidas a partir dos dados reais" acima.
- ✅ **#61**: busca web (plano A) NÃO achou paper verificável com a frase "show promise para drones" — era afirmação inverificável (provável floreio). Apliquei o plano B: troquei pela afirmação geral ancorada nos frustum reais (qi2018frustum, wang2019fconvnet, paigwar2021fpp). Sem flanco.
- micro-carets de pura preferência (pluralizar "s", trocar vírgula): pulados onde o texto já está correto.

## Temas sistêmicos (muitos comentários de uma vez)
- **MUITOS pedidos de figura ilustrativa** (#52, 58, 59, 121, 123, 161, 162, 174, 175, 198) — "se houver tempo". Diagramas: PointNet++ topologia, single-stage voxel, geometria do frustum/depth-band, abordagens lado-a-lado.
- **Consistência de títulos de parágrafo** (#130, 177) — negrito OU itálico + ":" sempre. (Os `\emph{State} Jacobian.` etc.)
- **Inglês britânico → americano** (#42, 53, 103, 149, 179) — ♻️ já convertido na revisão do Máximo.
- **`enumerate` para listas** (#156 objetivos, #163 motivos, #189 métricas, #225 contribuições).
- **Notação**: `\boldsymbol{}` (#146), boldface (#91), `{\bf X}^{O}` vs `{\bf p}^{O}` consistente (#125,127,129,159,181), definir/referenciar operadores e funções (#170,171,172).
- **Carets triviais** (pluralizar "s", inserir "the/this/with", trocar verbo): a maioria dos 226. Aplico em lote.

## Já resolvidos (coincidem com Máximo / trabalho anterior) ♻️
- #134 "remove the box!" = `\boxed` removido (Máximo #84). 
- #42/53/103/149/179 British→American = varredura feita.
- #130/177 títulos de parágrafo = relacionado ao #83 do Máximo (intro adicionada; falta padronizar formato).
- #139 link do vídeo no YouTube → você pediu p/ **remover o link e usar imagem** (figura `pipeline_frames` adicionada). ✅

## Pendências que dependem de você 🟡
- #51 relação inversa da Fisher ↔ variância (deixar mais claro) — posso, mas confirme se quer mais detalhe.
- #114/115/116 datasets são outdoor mas teste foi indoor (quadra) — cabe comentário; é fato seu, escrevo se aprovar.
- #199/#214 recomendar explicitamente: para detect-and-avoid, **maior recall** (mesmo com ruído) é preferível a menor erro. (Posso adicionar — alinha com a tese.)
- #83/#164 termos "cunhados"/de jogos (sprite?) — confirmar se quer trocar.
- Figuras novas 🖼️ que dependem de fonte/tempo: topologia PointNet++ (#174), voxel (#175), block-diagram das 3 abordagens (#198).

## Lista completa (por página) — para rastreio
p1 [1]💬"não atuo mais como professor". p21 [2-6] carets intro. p22 [7]. p23 [8-13] (EKF é solução etc). p24 [14-16]. p25 [17-18]. p26 [19-24]. p27 [25-35] notação (z, **x**, etc). p28 [36-39 escrever p^W=T·p^B] [40]frase pouco clara. p29 [41-42]. p30 [43-49]. p31 [50-52 Fisher/figuras]. p34 [53-55 PICOC]. p35 [56-57]. p36 [58 figuras]. p37 [59-60]. p38 [61]. p39 [62]. p40 [63-69]. p41 [70-79]. p42 [80-82]. p43 [83-84]. p45 [85-88]. p46 [89-91 Q completa/boldface]. p48 [92 introduzir termos 5.18]. p49 [93 P não definido][94💬]. p50 [95 cor do EKF global]. p51 [96]. p52 [97💬][98-99]. p54 [100-101]. p55 [102-103]. p56 [104-110 cores do diagrama pipeline + legenda + fonte]. p57 [111-120]. p58 [121-123 figuras/quebra eq]. p59 [124-133 notação X^O / títulos parágrafo]. p60 [134 remove box][135-136]. p61 [137-139 link vídeo]. p62 [140-144 quebrar figura 6.3]. p63 [145-147]. p64 [148-150]. p65 [151-152]. p66 [153-156 enumerate objetivos]. p67 [157-159 dimensões estranhas]. p68 [160-164 reescrever/enumerate/sprite]. p69 [165-166]. p70 [167-168 tabela sem uso]. p71 [169-172 definir funções/operadores]. p72 [173-175 figuras PointNet++/voxel]. p73 [176-184 títulos/2D tracking motivação]. p74 [185-187 subcaptions]. p75 [188-189 enumerate métricas]. p76 [190-192]. p77 [193-194]. p78 [195-198 destacar paradigmas/diagrama]. p79 [199 recomendar recall]. p80 [200-202]. p81 [203-206]. p83 [207-210]. p85 [211-212]. p86 [213-214 recomendar recall]. p87 [215-222 incerteza imagem→3D]. p88 [223-226 contribuições repetidas/enumerar/concluding remarks].
