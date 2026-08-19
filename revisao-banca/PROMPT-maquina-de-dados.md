# Prompt para a máquina que tem os dados (AirSim + TU Berlin)

> Cole o bloco abaixo (do `---` até o fim) numa sessão do Claude Code aberta **no diretório do repositório da dissertação** na outra máquina.
> São duas tarefas independentes (Capítulo 6 e Capítulo 7). Se quiser rodar só uma, apague a outra seção.

---

# Contexto

Sou Eric E. Y. de Lima. Defendi meu mestrado no ITA em 28/07/2026 e estou finalizando a versão
corrigida da dissertação a partir dos apontamentos da banca (Profs. Tasinaffo, Alberto Ferreira de
Souza, Marcelo Gomes da Silva Bruno, e os orientadores Marcos Maximo e Stiven Schwanz Dias).

A maior parte das correções **já foi aplicada** numa sessão anterior, feita noutra máquina. O
repositório é `github.com/ericyoshida/dissertation` (branch `main`). A dissertação é em inglês,
classe `ita.cls`, arquivo principal `tese.tex`, compila com `latexmk -pdf tese.tex`.

**Passo 0, obrigatório:** rode `git pull` antes de qualquer coisa e confirme que o texto já contém as
correções (por exemplo, deve existir a Seção `\label{sec:compute}` no `Cap7/cap7.tex` e a tabela
`\label{tab:mono_accuracy}` no `Cap6/cap6.tex`). Se não existirem, **pare** e me avise: significa que
as correções ainda não foram enviadas da outra máquina.

O estado atual compila com 0 erros, 0 referências indefinidas e 150 páginas. Existe uma planilha de
acompanhamento para a banca em `revisao-banca/correcoes-banca.csv` e `.xlsx` — ao terminar cada item
abaixo, **atualize a linha correspondente** dessa planilha (colunas: Membro, Comentário, Ação,
Adicionado?, Comentários adicionais).

Faltaram exatamente três coisas, todas por falta de dados na outra máquina. É isso que preciso que
você faça agora.

---

# TAREFA A — Capítulo 6 (TU Berlin, tracker monocular)

## A.0 O que a banca pediu e que ficou pendente

1. **Prof. Alberto:** excluir da métrica de erro os quadros em que a câmera não detecta o alvo
   (durante os giros de 360°), porque inflam o erro.
2. **Prof. Alberto:** o termo "horizontal error" foi renomeado para "ground-plane error" no texto,
   mas **os rótulos dos eixos dentro dos PDFs das figuras ainda dizem "horizontal error"**. É a única
   inconsistência texto↔figura visível na versão atual.
3. **Prof. Bruno:** discussão mais honesta dos resultados (já feita no texto; falta a parte numérica).

## A.1 Onde estão os dados (caminhos verificados)

| Papel | Caminho |
|---|---|
| Código do tracker que produziu o resultado bom | `~/tub/ubaswarm_ws/src/tracker/multi-target-tracking.py` (classe `EKFTracker`; o `np.savez` fica por volta da linha 948; carregue com `importlib`, os hífens impedem `import` normal; **não precisa de ROS**) |
| Poses da plataforma (odometria LiDAR-inercial KISS-ICP, ENU) | `~/tub/ubaswarm_ws/src/tracker/data/Sportcomplex_hallcd_71125/flying_1/rosbag_corrected/rosbag_corrected_0.db3`, tópico `/kiss/aspnpvt`, 2041 mensagens. Ler com `sqlite3` + `struct`; offsets CDR: `stamp @4 <II`, `position @60 <ddd`, `quat @108 <dddd` |
| Vídeo e quadros | `.../flying_1/recording/stream.mp4` (1280×960 @ 20 fps, 3038 quadros) e `.../flying_1/recording/frames/frame_%08d.jpg` |
| Timestamps dos quadros | `.../flying_1/recording/metadata.jsonl`, campo `server_capture_utc_ns`; latência de câmera usada: `CAMERA_LATENCY = 0.1e9` ns |
| Pontos topografados do tatame (o perímetro tracejado) | fixos em `multi-target-tracking.py`, por volta das linhas 798–818: `tatami_points = [[8.918,-1.396,-0.489], [8.629,10.628,-0.477], [19.900,-1.127,-0.650], [19.585,10.709,-0.566]]` |
| Pesos do detector | `~/tub/tracker/model_- 28 october 2025 11_48.pt` |
| Cache de detecções YOLO (3038 quadros, 660 com caixa) | `~/tub/tracker/cache/detections_20027b8419a5bc2fea66e483141dee36.pkl` — para casar com o `.npz`, aplique o espelhamento `u → 1279 - u` e depois `cv2.undistortPoints` |

**Intrínsecos e geometria:** `fx=1592.257037`, `fy=1513.221924`, `cx=640.451350`, `cy=434.127426`;
plano do chão em `z = -0.56` m (ENU); câmera 0,21 m abaixo do corpo, inclinada 10° para baixo.

## A.2 Armadilhas já identificadas — leia antes de rodar

- **Existem dois `tracking_results.npz` e eles NÃO são iguais.** `detections` e `timestamps` são
  idênticos; `states` diferem.
  - `~/tub/tracker/tracking_results.npz` → **degenerado**, a trilha fica congelada num blob de 0,75 m
    em torno de (4,0, −3,1). **Não use.** A causa provável é o `cv2.flip(frame, 1)` horizontal que o
    pipeline de produção aplica antes do YOLO e o pipeline legado não aplicava.
  - `~/tub/ubaswarm_ws/tracking_results.npz` → trilha plausível, x ∈ [7,44 , 22,62], y ∈ [−2,50 , 16,72].
- **Atenção:** essa segunda versão corresponde à figura **antiga**, a que foi projetada nos slides da
  defesa, e **não** à figura corrigida que está hoje na dissertação. Em 06/06/2026 o commit `09505f0`
  ("Cap6: correct monocular tracker results after geometry/EKF audit") corrigiu o resultado —
  limitando o horizonte de propagação em malha aberta e reinicializando a trilha por interseção
  raio-plano após uma lacuna de detecção — e os estados corrigidos **não foram persistidos em disco**.
  Só sobrou o PDF da figura. Portanto, para regerar fielmente a figura atual você precisa
  **reproduzir a auditoria**, não apenas replotar o `.npz`.
- **O arquivo de ground truth manual (135 quadros anotados) não foi encontrado em lugar nenhum.**
  Nem `cap6_stats.json`, nem os scripts que geraram as figuras (foram código descartável de sessão).
  **Procurar esse arquivo é a prioridade número um** — ele destrava tudo. Ele deve conter, por quadro:
  índice/timestamp do quadro, o pixel do pé anotado na imagem, e a posição (x, y) no piso obtida do
  mapa LiDAR. Procure por `.json`, `.csv`, `.npy`, `.npz` ou notebooks com ~135 linhas de coordenadas.
- `detections` no `.npz` **não tem sentinela de "sem detecção"**: quadros sem detecção simplesmente
  não aparecem no array (627 de 3038 quadros têm detecção). Logo, "tem detecção" ≠ "está vendo o alvo".

## A.3 O que fazer

**A.3.1 Localizar a ground truth.** Faça uma busca exaustiva pelo arquivo de anotação manual descrito
acima. Se encontrar, use-o. Se não encontrar, me avise antes de prosseguir e proponha re-anotar ~135
dos 3038 JPEGs contra os cantos topografados — mas **não invente timestamps** e não publique números
derivados de reconstrução aproximada.

**A.3.2 Máscara de exclusão (o pedido do Prof. Alberto).** Implemente o teste físico abaixo, que não
depende de anotação nenhuma e é defensável perante a banca (muito mais do que o atual heurístico de
"velocidade > 4 m/s"):

```
raio = R_world2opt.T @ [(u-cx)/fx, (v-cy)/fy, 1]
C    = -R_world2opt.T @ t
lam  = (-0.56 - C[2]) / raio[2]
rejeitar se: lam <= 0, ou |raio[2]| < 1e-6, ou o ponto cai > 2 m fora do tatame
             (tatame: x ∈ [8.629, 19.900], y ∈ [-1.396, 10.709])
rejeitar também os quadros em que nenhum canto do tatame projeta dentro de 1280×960
```

Como referência do que esperar: numa análise preliminar, 111 de 627 quadros (17,7%) foram inválidos —
44 raios nunca atingem o piso e 65 caem a mais de 5 m do tatame (um deles em x = −173 m). As
detecções inválidas ficam na parte de cima da imagem (linha mediana v ≈ 184, contra 354 nas válidas),
isto é, acima do horizonte. Os intervalos inválidos ficaram em t ≈ 30,8–31,2 s, 32,1–47,5 s e
64,4–89,7 s.

**A.3.3 Regerar as três figuras**, mantendo os mesmos nomes de arquivo (as referências LaTeX já
apontam para eles) e **trocando os rótulos dos eixos**:

| Arquivo | Label LaTeX | Figura na versão atual | Rótulo a corrigir |
|---|---|---|---|
| `Cap6/tracking_traj.pdf` | `fig:mono_traj` | 6.7 | — (sem rótulo de erro) |
| `Cap6/tracking_errors.pdf` | `fig:mono_errors` | 6.8 | `horizontal error [m]` → **`Ground-plane error [m]`** |
| `Cap6/error_vs_range.pdf` | `fig:error_range` | 6.6 | `horizontal localization error [m]` → **`Ground-plane localization error [m]`** |

Na `tracking_traj.pdf`, a legenda do texto já descreve explicitamente: verde = ground truth manual;
trilha contínua amarelo→vermelho = estimativa do EKF **colorida pelo tempo de voo, com barra de cor à
direita** (isso foi um pedido específico do Prof. Alberto, que achou a legenda confusa); cinza =
falsos positivos do detector excluídos da trilha; retângulo tracejado = perímetro topografado.
Mantenha essa semântica.

Na `tracking_errors.pdf`, painel (a) = erro no tempo com a taxa de guinada sobreposta no eixo direito;
painel (b) = CDF empírica. **Agora plote as duas curvas de erro**: com o alvo em campo de visão e com
todos os quadros, para que a figura reflita a Tabela 6.1 do texto.

**A.3.4 Recalcular as estatísticas e sincronizar o texto.** Os números que estão hoje na dissertação
são estes — se os seus recálculos derem diferente, **atualize todos eles de forma consistente**:

| Grandeza | Valor atual no texto | Onde aparece |
|---|---|---|
| Lift geométrico quadro a quadro (sem filtro) | mediana 1,05 m · RMSE 1,36 m · pior caso 3,1 m | `Cap6` §6.3.2 e Tabela `tab:mono_accuracy` |
| Tracker, alvo em campo de visão | mediana 1,28 m · RMSE 2,07 m | idem, e é o número de destaque |
| Tracker, todos os quadros | mediana 1,75 m · média 3,67 m · RMSE 6,27 m · p90 7,75 m | idem |
| Amostras casadas / quadros anotados / quadros ativos | 128 / 135 / 626 | §6.3.1 e §6.3.2 |
| Erro versus alcance | ≈0,7 m em 10–12 m → 1,5–1,9 m além de 18 m; correlação 0,53 | §6.3.2 e legenda da `fig:error_range` |
| NIS | pose desligada 3,4×10⁴; consistente em σ_pose ≈ 10⁻³ | §6.3.3 |

Se algum número mudar, atualize também: a Tabela `tab:mono_accuracy`, as legendas das figuras, o
parágrafo de Discussão no fim do `Cap6/cap6.tex`, e o item correspondente no `Cap8/cap8.tex`
(o resumo de contribuições cita mediana 1,28 m e 1,75 m).

**A.3.5 Verificar uma suspeita (importante).** Uma análise por engenharia reversa do PDF publicado
sugeriu que **a curva de erro da figura de erro pode não ser consistente com a figura de trajetória**:
cerca de 32 de 124 amostras de erro pareciam geometricamente impossíveis, com picos de ~22 m em
t ≈ 32,45 s e 32,95 s, onde a distância máxima possível entre a estimativa publicada e **qualquer** dos
135 pontos de ground truth seria ~12,45 m. A hipótese é que a ground truth tenha sido transformada
rigidamente para o referencial de odometria no gráfico espacial, mas o erro tenha sido calculado
contra a ground truth **não transformada** (no referencial do mapa LiDAR), ou com uma transformação
diferente/antiga.

**Isso não foi provado** — foi inferido do PDF, não dos dados. Com os dados reais em mãos, verifique
diretamente: para cada amostra casada, calcule a distância entre o estado do EKF e o ponto de ground
truth e confirme que ela bate com a curva plotada. Se houver de fato uma inconsistência, corrija o
cálculo, regere as figuras e atualize os números. Se não houver, registre que foi verificado.

**A.3.6 Bônus:** se sobrar tempo, regere também as figuras dos slides em
`../DEFESA - SLIDES/figs/` (`tracking_traj.pdf`, `tracking_errors.pdf`, `nis_consistency.pdf`), que
estão desatualizadas — são bit-idênticas às do commit `eb5cb0a`, anteriores à auditoria de 06/06.

---

# TAREFA B — Capítulo 7 (AirSim, detecção 3D)

## B.0 O que a banca pediu e que ficou pendente

**Prof. Bruno:** o Capítulo 7 apresenta apenas médias globais sobre toda a campanha. Ele quer uma
**análise estatística da evolução do erro ao longo do tempo**, como já existe nos Capítulos 5 e 6.
Ele também pediu uma **referência global de comparação** (benchmark externo) antes de submeter esses
resultados para publicação.

Na máquina onde a revisão foi feita **não existe nada do Capítulo 7**: nem `~/airsim/`, nem o clone de
`github.com/ericyoshida/airsim-uav-3d-detection`, nem os dumps das 60 sequências, nem
`make_dissertation_figs.py`. Por isso este item ficou marcado como **NÃO** na planilha da banca.

## B.1 O que localizar nesta máquina

- O diretório `~/airsim/` (ou equivalente) com o pipeline de detecção 3D.
- O script `make_dissertation_figs.py` (a sessão anterior editou a função `fig_tracker` dele).
- Os **dumps de detecção por sequência**. O próprio Capítulo 7 afirma, na Seção de protocolo:
  *"Inference runs offline: one heavy pass per sequence dumps detections, from which all metrics are
  recomputed cheaply while sweeping the detection threshold."* — então esses dumps existem e são
  exatamente o insumo necessário.
- A campanha de avaliação: 60 sequências (20 por ambiente: `AirSimNH`, `City`, `Coastline`),
  75–110 quadros cada, a ≈5 Hz, com observador em movimento.

## B.2 O que produzir

**B.2.1 Figura nova de erro ao longo do tempo.** O casamento entre ground truth e track já é feito por
distância 3D sob um gate de poucos metros, e já existe a estratificação por alcance que gera o
`localization_vs_range.pdf`. Reaproveite **a mesma lista de pares casados**, mas indexada por
**quadro** em vez de faixa de alcance.

Como as sequências têm comprimentos diferentes (75–110 quadros), agregue por **tempo normalizado de
sequência** (0 a 1) e plote, sobre as 60 sequências:

- painel (a): **mediana do erro 3D do centroide** com **banda interquartil** e **envelope do
  percentil 90** ao longo do tempo normalizado;
- painel (b): CDF empírica do erro por quadro, para comparação com o formato já usado no Capítulo 6.

Isso responde literalmente ao pedido ("análise estatística" e não apenas médias globais) e agrega
corretamente apesar dos comprimentos desiguais.

Salve como `Cap7/localization_over_time.pdf`, e insira no `Cap7/cap7.tex` uma nova subseção dentro da
Seção de Resultados, **logo após** a subseção "Three-Dimensional Detection and Tracking", com
`\label{sec:results-temporal}` e a figura com `\label{fig:temporal_error}`. Escreva o texto
interpretando o que a curva mostra: se o erro é estacionário ao longo da sequência, se há transitório
de inicialização das trilhas nos primeiros quadros, e como isso se relaciona com o fato, já reportado,
de o erro ser praticamente **plano com o alcance**.

**B.2.2 Coerência numérica.** Os números que já estão no texto e que **não podem divergir** do que
você recalcular:

- 2D: `mAP50 = 0,994` (sintético) e `0,73` com `0,64` de recall (rótulos da API do simulador).
- Extração de foreground: RMSE 3D de `2,94` m (normalização por máximo) → `2,87` m (p95) →
  `1,20` m (depth band) → `1,05` m (T-Net segmentation sobre a band) → `2,14` m (segmentação **sem** a
  band). Precisão 0,21 → 0,90; AMOTA 0,04 → 0,81.
- Tabela `tab:main` (por ambiente): AirSimNH `0,75 / 0,25 / 0,26 / 0,86 / 0,31 / 1,40`;
  City `0,79 / 0,56 / 0,59 / 0,83 / 0,58 / 1,19`; Coastline `0,88 / 0,50 / 0,46 / 0,99 / 0,52 / 1,06`;
  média `0,81 / 0,44 / 0,44 / 0,90 / 0,47 / 1,20` (AMOTA, MOTA@0.5, IDF1, Precisão, Recall, RMSE).
- Localização versus alcance: `1,25` m (0–30 m), `1,16` m (30–50 m), `1,30` m (50–90 m).
- Tabela `tab:arch`: melhor voxel PointPillars-PFN `AMOTA 0,68 / MOTA 0,68 / P 0,92 / R 0,80 /
  F1 0,83 / RMSE 0,87`; fusão tardia `0,75 / 0,56 / 0,90 / 0,66 / 0,74 / 1,07`; held-out dos voxel
  concorda com a campanha completa dentro de 0,02 AMOTA e 0,02 m de RMSE.

**B.2.3 Atualizar o Capítulo 8.** Assim que a figura temporal existir, remova ou reescreva o trecho
das Limitações que hoje diz que a análise temporal não foi feita, e ajuste o item correspondente nos
Trabalhos Futuros.

**B.2.4 Benchmark externo (decidir, não necessariamente executar).** O Capítulo 8 já registra
explicitamente, como limitação, que não há comparação contra método publicado em benchmark público, e
os Trabalhos Futuros já apontam os dois caminhos: avaliar no **UAV3D** (`\cite{ye2024uav3d}`, o único
benchmark aéreo 3D de larga escala) e reportar as linhas de base do domínio automotivo em **KITTI**
(`\cite{geiger2012kitti}`) e **nuScenes** (`\cite{caesar2020nuscenes}`). Avalie se dá para rodar o
pipeline no UAV3D com o tempo disponível. Se der, faça e reporte. Se não der, **deixe como está** — o
texto já é honesto sobre a lacuna — e apenas me diga o esforço estimado.

---

# Regras para as duas tarefas

- **Não invente números.** Todo valor que entrar no texto tem de vir de um recálculo sobre dados
  reais. Se um dado não existir, diga que não existe e deixe a limitação registrada no texto.
- **Edite sempre por `\label`, nunca por número de figura.** A numeração mudou: a inserção da foto da
  plataforma da TU Berlin (agora Figura 6.5) deslocou as figuras seguintes do Capítulo 6 — a antiga
  Figura 6.6 é agora a 6.7 e a antiga 6.7 é agora a 6.8.
- Ao final, rode `latexmk -pdf tese.tex` e confirme **0 erros e 0 referências indefinidas**. Hoje há
  exatamente 1 linha excedendo a margem, na tabela da Folha de Registro do Documento, que é do próprio
  template do ITA e deve ser ignorada.
- Atualize `revisao-banca/correcoes-banca.csv` e `.xlsx` com o que foi feito, mantendo as colunas
  existentes e o esquema de status (SIM / NÃO / PARCIAL / PENDENTE / N/A).
- Commit e push ao final, para que a outra máquina receba as mudanças.
