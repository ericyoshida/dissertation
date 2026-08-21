# E-mail para a banca — versão final da dissertação

**Para:** tasinaffo@ita.br; bruno@ita.br; maximo.marcos@gmail.com; stiven.dias@embraer.com.br
**Cc:** (Prof. Alberto Ferreira de Souza — inserir o endereço)
**Anexos:** `tese.pdf`, `correcoes-banca.xlsx`

**Assunto:** Versão final da dissertação — Eric E. Y. de Lima (defesa em 28/07/2026)

---

Prezados Prof. Tasinaffo, Prof. Bruno, Prof. Alberto, Prof. Maximo e Dr. Stiven,

Espero que estejam bem. Encaminho a versão final da minha dissertação, revisada a partir dos
apontamentos feitos na defesa do dia 28 de julho e na carta que o Prof. Tasinaffo gentilmente enviou
em seguida.

Vão dois anexos. O primeiro é a dissertação revisada. O segundo é a planilha de acompanhamento que o
Prof. Maximo pediu, com uma linha por comentário da banca, o que foi feito em cada caso e onde a
alteração está no texto. São 75 linhas ao todo. Sugiro que ela seja o ponto de partida da leitura,
porque permite localizar rapidamente o tratamento dado a cada ponto sem precisar percorrer o
documento inteiro.

Abaixo resumo as mudanças principais, agrupadas por quem as levantou.

**Prof. Tasinaffo.** O Resumo e o Abstract foram inteiramente reescritos em linguagem acessível:
partem do cenário concreto de táxis aéreos e drones sobre as cidades, explicam o que é
*detect-and-avoid* e a divisão entre as metades *sense* e *avoid*, e só então descrevem o pipeline e
os resultados. O nome do Prof. Maximo está sem acento. A figura do pipeline, que antes só aparecia na
página 49, agora abre a nova Seção 1.4 "Proposta de Solução", criada a partir da divisão da antiga
Seção 1.3 como o senhor sugeriu. Todas as siglas foram padronizadas com as expansões em maiúsculas na
primeira ocorrência, e a Lista de Abreviaturas passou de 22 para 48 entradas. Inseri um parágrafo
introdutório em cada uma das doze seções que iam direto para uma subseção, e um parágrafo de
encerramento anunciando o capítulo seguinte nos Capítulos 2, 3, 5, 6 e 7. O Capítulo 2 ganhou uma
seção final que mapeia cada equação ao capítulo em que ela é usada. O anúncio do Capítulo 6 saiu da
penúltima seção do Capítulo 5 e passou a ser o último parágrafo do capítulo. A referência clássica do
Adam foi incluída. E as linhas que ultrapassavam a margem direita caíram de catorze para uma, que
está na tabela da Folha de Registro do Documento e pertence ao próprio template do ITA.

**Prof. Alberto.** A questão da Figura 6.6 foi investigada a fundo e o resultado merece registro: a
figura que está na dissertação já era a versão corrigida. Em 6 de junho eu havia feito uma auditoria
da geometria e do filtro que constatou que a projeção quadro a quadro sempre esteve correta e que o
erro maior vinha do modelo de velocidade constante sendo propagado em malha aberta durante as longas
lacunas de detecção; a correção foi limitar esse horizonte e reinicializar a trilha após uma lacuna.
Os slides que apresentei usavam a figura anterior a essa auditoria, e é por isso que os dois não
coincidiam. Peço desculpas pela confusão que isso gerou na sessão.

Sobre excluir os quadros em que a câmera não está olhando para o alvo, o senhor tinha razão e a
mudança foi maior do que uma simples exclusão. Substituí os heurísticos ajustados que eu usava por um
teste geométrico de visibilidade: o raio retroprojetado da detecção precisa encontrar o plano do chão
à frente da câmera, o ponto resultante precisa cair a menos de dois metros do tatame topografado, e
pelo menos um canto do tatame precisa se projetar dentro da imagem. Ele usa apenas os intrínsecos, a
pose registrada e a geometria da sala, de modo que é o critério que o sistema poderia aplicar em
tempo de execução. Ele rejeita 106 dos 626 quadros com detecção. Os resultados passaram a ser
reportados nos dois regimes, com uma tabela nova: com o alvo em campo de visão, mediana de 1,43 m e
RMSE de 2,46 m; com todos os quadros, 1,75 m e 6,27 m. O termo "horizontal error" virou "ground-plane
error" no texto e nos eixos das figuras, e acrescentei um parágrafo explicando por que a medida é
feita no plano do chão e não no plano da aeronave.

A fotografia da plataforma da TU Berlin entrou no texto como Figura 6.5, e aproveitei para corrigir a
descrição da montagem da câmera, que estava como "acima do centro do corpo" e é, na verdade, em um
casulo suspenso 21 cm abaixo, inclinado 10° para baixo. O hardware está documentado em seção própria,
com a caracterização honesta do que roda e do que não roda em tempo real. Criei uma seção dedicada
deixando explícito que a nuvem de pontos do Capítulo 7 vem de uma câmera de profundidade idealizada e
não de um LiDAR, enumerando as diferenças que isso esconde e declarando que as acurácias reportadas
são um limite superior. A origem das nuvens de pontos em cada estudo está agora na introdução. A
necessidade de predizer a trajetória futura, e não apenas rastrear, foi incorporada ao Capítulo 1 e
virou item de limitação e de trabalho futuro. A afirmação sobre profundidade foi suavizada: passou a
ser "não observável diretamente", com a ressalva de que estimadores monoculares aprendidos recuperam
profundidade métrica com acurácia útil. E as fontes e ferramentas da revisão sistemática estão
nomeadas explicitamente, incluindo o registro honesto de que as buscas nas bases por assinatura
foram preparadas mas não executadas.

**Prof. Bruno.** A Seção 2.3.3 foi reescrita com o rigor que o senhor cobrou: SE(3) como grupo de
Lie, o espaço tangente definido em um ponto, a álgebra de Lie como o espaço tangente na identidade,
seus elementos como matrizes 4×4, os mapas *hat* e *vee* como bijeções lineares e o isomorfismo com
R⁶, deixando claro que o vetor de seis componentes é o vetor de coordenadas de um elemento da
álgebra, e não o próprio elemento. A dedução do Jacobiano extrínseco foi expandida em cinco passos
numerados, com as dimensões de cada matriz anotadas e a aproximação exp(ξ^) ≈ I + ξ^ explicitada
junto com a ordem do resto, e a covariância de inovação passou a ser deduzida como soma de três
formas quadráticas, com a hipótese de independência nomeada. O Capítulo 5 declara agora que não se
faz estimação conjunta de pose própria e rastreamento, ganhou uma observação formal sobre o uso do
alcance predito violar as hipóteses do filtro de Kalman, e uma seção nova que responde qual
referencial recomendar e em que situações o local é preferível. Acrescentei um parágrafo explicando
que o crescimento do erro nas Figuras 5.1 e 5.3 não é divergência, e que a evidência disso é o filtro
acompanhar o limite de Cramér-Rao com o mesmo perfil.

O motivo de não termos a covariância da pose da plataforma está explicado: a solução de odometria
LiDAR-inercial embarcada publica a pose mas não publica a covariância associada, e recuperá-la
exigiria instrumentar e reexecutar a pilha de navegação da TU Berlin. As duas consequências de
modelá-la como constante e isotrópica estão declaradas. O Capítulo 7 explica por que o Jacobiano
extrínseco não foi usado ali — no simulador a pose é exata, então o Σ honesto é nulo e o termo se
anula — e indica como estendê-lo.

Sobre a análise temporal, ela foi feita. O Capítulo 7 tem agora a Seção 7.5.4 e a Figura 7.10, com a
mediana, a banda interquartil e o envelope do percentil 90 do erro ao longo do tempo normalizado de
sequência, sobre 5.359 pares casados. O erro é estacionário, e a seção inclui uma ressalva de que o
transiente de inicialização é absorvido pela confirmação após três detecções e por isso não aparece
na métrica. A associação de dados está descrita explicitamente, e classes múltiplas de objetos e
rastreamento cooperativo conjunto entraram nos trabalhos futuros.

**Dr. Stiven.** O Capítulo 7 passou de observação a recomendação explícita: para *detect-and-avoid*,
o detector voxel de pilares aprendidos, ou a fusão tardia, em vez do pipeline frustum isolado. A
pergunta sobre o recall baixo ser compensado pelo rastreador está respondida em um parágrafo próprio:
a distância entre AMOTA alto e MOTA baixo é de fato a assinatura de um rastreador capaz alimentado
por um detector limitado, mas o AMOTA integra ao longo do eixo de recall e o *coasting* cobre lacunas
de poucos quadros, não ausências prolongadas — uma trilha nunca iniciada não pode ser propagada.
Acrescentei também a qualificação de escopo que o senhor cobrou: o que se compara é o front-end de
detecção, e não a função completa de detectar e evitar, e estabelecer qual abordagem é mais
promissora nesse sentido exigiria fechar a malha e usar métricas preditivas e orientadas a risco,
que estão nomeadas nos trabalhos futuros. A questão do referencial ficou explícita: o resultado é
sobre casamento entre o referencial em que o ruído é definido e aquele em que o estado é carregado, e
não uma ordenação geral entre global e local.

**Prof. Maximo.** Além da planilha, incorporei os pontos que o senhor levantou na sessão: a predição
a partir das estimativas de posição e velocidade do rastreador, a origem da profundidade, o fato de a
TU Berlin não estimar o Σ, o motivo de o referencial local ter sido considerado, a associação pelo
SORT no plano 2D, e o ruído anisotrópico definido em coordenada global.

Um ponto de transparência. Ao longo da revisão fiz auditorias numéricas do texto contra os dados
brutos, e elas corrigiram alguns números e algumas afirmações que não se sustentavam. As mudanças
estão todas registradas na planilha. As mais relevantes são: os resultados do Capítulo 6 no regime
com o alvo em campo de visão; a constatação de que os detectores voxel do Capítulo 7, por serem
treinados na própria campanha, têm desempenho consistentemente pior no subconjunto retido, o que está
agora quantificado e declarado; e o registro de que apenas AMOTA e RMSE foram recomputados nesse
subconjunto, de modo que recall, precisão e F1 daquelas linhas permanecem valores *in-sample*.

Fica um item em aberto, e prefiro declará-lo. O Prof. Bruno pediu uma referência global de
comparação, e rodar o pipeline em um benchmark público não foi feito. Está registrado como limitação
no Capítulo 8 e como trabalho futuro, com os dois caminhos concretos indicados. Como o próprio
professor colocou a questão como pré-requisito para a submissão dos resultados a publicação, e não
para a dissertação, optei por declarar a lacuna em vez de deixá-la implícita. Se a banca entender que
ela deve ser fechada antes da entrega, eu a executo.

Por fim, uma nota prática: a numeração de figuras mudou com as inserções, e a última linha da
planilha traz o mapa de correspondência para que seja possível localizar na versão final o que foi
citado na arguição.

Agradeço muito pela leitura cuidadosa e pelos comentários. Vários deles não apenas melhoraram o
texto, mas me fizeram encontrar coisas que eu não teria encontrado sozinho. Fico à disposição para
qualquer ajuste adicional.

Atenciosamente,

Eric Ezequiel Yoshida de Lima
