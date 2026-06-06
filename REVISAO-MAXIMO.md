# Revisão do Prof. Máximo — 119 comentários (status)

Fonte: `tese_Eric_revMaximo.pdf` (96 pp). ✅ feito · 🟡 precisa info/decisão sua · 🔵 precisa você apontar o trecho exato · 🟣 referência real (Zotero) · 🖼️ figura nova · 📐 layout (passar no build) · 💬 elogio (sem ação).

## Decisões transversais aplicadas
- ✅ **Inglês americano** em toda a tese (-ize, center, meter, modeling, artifact, maneuver, rigor…). Varredura por palavra.
- ✅ **`safety-critical` removido** em todos os capítulos (recomendação do prof.), frases reescritas neutras.
- ✅ **Título trocado** (#1) em `tese.tex` → *Object Detection and Tracking Using an Unmanned Aerial Vehicle*.

## Por comentário
**Front matter**
- [1] ✅ título trocado. [2] 🟡 garantir Resumo completo na versão da banca.

**Cap.1** — [3]🟣 [4]🟣 (citações UAM/D&A → marcador `TODO-REV`) · [5]✅ [6]📐 [7]✅ [8]💬 [9]✅ [10]✅ [11]✅ [12]✅ [13]✅ [14]✅ [15]✅ [16]✅ [17]✅ [18]✅

**Cap.2** — [19]🔵 ("explains" não existe no fonte) [20]🟣+✅(suavizado) [21]✅ [22]✅ [23]✅ [24]🟣(COCO) [25]✅ [26]✅ [27]🔵(prov. "SECOND", nome próprio) [28]✅

**Cap.3** — [29]💬 [30]✅(já "prior art") [31]✅ [32]🔵 [33]✅ [34]✅(retítulo) [35]🟣 [36]🟣 [37]🟣 [38]✅

**Cap.4** — [39]✅ [40]✅ [41]✅ [42]✅ [43]✅ [44]✅ [45]✅ [46]🟣(Ultralytics) [47]✅

**Cap.5** — [48]✅ [49]✅ [50]✅ [51]✅ [52]✅ [53]🔵 [54]✅ [55]✅ [56]✅ [57]✅ [58]✅(via \citen) [59]🔵 [60]🔵 [61]✅ [62]✅ [63]✅ [64]✅ [65]✅ [66]✅ [67]📐 [68]✅ [69]✅

**Cap.6** — [70]🔵(refs a Ch.2 são legítimas; aponte qual) [71]✅ [72]🟣 [73]✅ [74]✅ [75]🔵 [76]✅ [77]📐 [78]✅ [79]✅ [80]🖼️ [81]✅ [82]✅ [83]🔵 [84]✅ [85]✅ [86]💬 [87]🔵 [88]📐 [89]🟡

**Cap.7** — [90]✅ [91]💬 [92]✅ [93]✅ [94]✅ [95]🔵 [96]💬 [97]✅("timid"→under-confident) [98]✅ [99]🖼️ [100]✅ [101]💬 [102]✅ [103]✅ [104]✅ [105]💬 [106]🟡 [107]✅ [108]✅ [109]🟡 [110]✅(suavizado) [111]✅ [112]✅ [113]🟡 [114]🟡 [115]✅

**Referências** — [116]🟣+✅(mês add) [117]🟣 [118]🟣 [119]🟣 — bug de export do Zotero nas entradas `{Sobrenome} and others` (ye2024uav3d, ma2021uavfusion, cherif2023aerial, dolph2023airborne, fu2025dmtrack). Re-exportar do Zotero na máquina principal.

## Sessão 2 — resolvido com os dados reais (repo ~/airsim, ~/tub) + compilação
- ✅ **Compila** com `tectonic` (~/.local/bin). PDF: 98 páginas, 0 refs indefinidas, BibTeX limpo.
- ✅ **Referências reais adicionadas e citadas** (de `~/airsim/*.md` e padrão): #3/#37 Straubinger 2020 (UAM), #4 RTCA DO-365 (D&A), #20 LeCun 2015 (CNN), #24 Lin 2014 (COCO/mAP), #36 Livox Mid-360 (LiDAR), #46 Ultralytics YOLO11.
- ✅ **#106** hiperparâmetros YOLO inseridos (YOLOv11s ~10M, 80 épocas, imgsz 1280, recipe Ultralytics default SGD+Nesterov+cosine).
- ✅ **#109/#114** tempo computacional inserido honestamente (YOLO ~5 ms/frame em GPU; latência ponta-a-ponta não medida → trabalho futuro; FPS do PointNet++ é O(N²)).
- ✅ **#80, #99** figuras geradas (`Cap6/footpoint_geometry.pdf`, `Cap7/depthband_diagram.pdf`) e inseridas.
- ✅ **#6, #77** overflow de margem corrigidos (Cap.1 e Cap.6). Cap.3 tabela ajustada.

## Sessão 3 — referências reais + 10 destaques localizados
- ✅ **#116–119** — busquei os 5 papers na web (arXiv/Springer/Scholars' Mine) e pus autores/títulos/venues reais no `.bib`. Descoberta: o arXiv 2310.09589 era **Manduhu et al.**, não "Dolph" (ID trocado na metadata provisória). Falta só confirmar o venue do Cherif (deixei TODO).
- ✅ **Os 10 destaques localizados** (extraí o texto sob cada anotação): #19 "collects"→"introduces"; #27 "SECOND" (nome de método, esclarecido); #32 = era o unscented (=#31, feito); #53 "the block"→"a block"; #59 "With **the** innovation"; #60 "…, **we obtain**"; #70 removido "(Chapter 2)"; #75 removida "(Section 6.2.2)"; #83 adicionada frase introdutória aos parágrafos State/Extrinsic Jacobian; #87 corrigida inconsistência 10.3→10.1 m.

## Ainda pendente (só você resolve)
- 🟡 **#89** — `Sigma=0.1*eye(6)` no simulador (`tub/.../mot_simulator.py`); nav GPS ~2,5–10 m de incerteza (`GPS_PROJECTION_README.md`). "Sophie" = **Anne-Sophie Polz**. Confirmar a covariância real da nav.
- 🟡 **#113** — vídeo: existe `airsim/session_rec2/PIPELINE_BEST.mp4` e `bev_gt_vs_tracker.mp4`. Para a tese (papel), o caminho é link/QR ou frames extra — você decide.
- 📐 menores: #67 (indent), #88 (aumentar figura) — 2 overfull ~0,3 cm (Cap.2 RMSE, Cap.5), não-flagrados.
- 🟣 confirmar venue exato do `cherif2023aerial` (TODO no `.bib`).
