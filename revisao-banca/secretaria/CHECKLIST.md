# Entrega da versão final — Secretaria da Pós-Graduação do ITA

**Prazo: hoje, 31/08/2026** (data limite para entrega do arquivo final após defesa do 1º semestre,
conforme decisão do CPG nº 01 de 15/02/2013). A defesa foi em 28/07/2026.

Fonte: http://www.ita.br/posgrad/tese

---

## CAMINHO CRÍTICO — faça isto primeiro

A Folha de Registro **não é preenchida por você sozinho**. O fluxo é:

1. Enviar `PREENCHIDO_Folha_de_Registro.docx` para **doc.pt@ita.br**
2. A Biblioteca devolve com os campos **2 (data)**, **3 (registro nº)** e **9 (palavras-chave de
   indexação)** preenchidos
3. Essa folha devolvida vira a **última página** do PDF, sem paginação e fora do sumário

⚠️ **Faltam o campo 2 e o campo 3, ambos da Biblioteca.** O manual é explícito: a data (campo 2) é
a *data de registro*, não a da defesa, e tanto ela quanto o número (campo 3) são "preenchimento de
responsabilidade do processo de registro". O campo 2 agora está em branco na dissertação e na folha;
o campo 3 está como `DCTA/ITA/DM - XXX/2026`. Quando a Biblioteca responder, escreva os dois valores
em `tese.tex` (`\FRDitadata` e `\FRDitadocnro`), recompile e rode
`python3 revisao-banca/secretaria/gerar_folha_registro.py` — o script relê o `tese.tex` e regenera o
`.docx`, garantindo que os dois continuem idênticos.

Sobre o campo 9: o manual também o atribui à Biblioteca, mas a dissertação já o traz preenchido com
as palavras-chave de indexação. Deixei como está, igual nos dois documentos; se a Biblioteca pedir
para esvaziar, basta limpar `\FRDitapalavrasresult` e rodar o script de novo.

✅ **Idioma do resumo resolvido.** O manual pede o campo 11 no idioma do trabalho, que é o inglês.
A dissertação foi ajustada para usar o abstract condensado (`PreTextuais/abstract_frd.tex`) na sua
própria folha de registro, e o `.docx` é transcrição literal dessa página. Conferi os treze campos
um a um: estão idênticos.

---

## O que enviar para a Secretaria (Elenice — elenice@ita.br)

| # | Item | Arquivo | Estado |
|---|---|---|---|
| 1 | Dissertação completa em PDF, com a Folha de Registro como última página e **assinatura digital do orientador** | `tese.pdf` | ⚠️ falta a folha de registro oficial e a assinatura |
| 2 | Folha de rosto (editável) | `EDITAVEL_1_Folha_de_rosto.docx` | pronto |
| 3 | Folha da banca (editável) | `EDITAVEL_2_Folha_da_banca.docx` | pronto |
| 4 | Resumo (editável) | `EDITAVEL_3_Resumo.docx` | pronto |
| 5 | Abstract (editável) | `EDITAVEL_4_Abstract.docx` | pronto |
| 6 | Termo de autorização | `PREENCHIDO_Termo_de_Autorizacao.docx` | falta dado pessoal |
| 7 | CAPES 1 — Identificação | `PREENCHIDO_Capes1_Identificacao.pdf` | falta dado pessoal |
| 8 | CAPES 2 — Banca Examinadora | `PREENCHIDO_Capes2_Banca.pdf` | falta CPF dos membros |
| 9 | CAPES 3 — Área de Conhecimento | `PREENCHIDO_Capes3_Area.pdf` | faltam os códigos |

Depois do envio, a Secretaria confirma se os arquivos estão no padrão e manda o **link do Stratus**
para o upload final.

---

## O que só você pode preencher

Tudo o que falta está marcado **em vermelho** dentro dos arquivos. Lista completa:

**Seus dados** — já preenchidos: CPF, RG, endereço completo, CEP, nacionalidade.

Ainda faltam: **órgão emissor do RG**, **estado civil**, **profissão**, **telefone** e a
**matrícula** (mês/ano de ingresso). Coloque-os em `.dados_pessoais.json` e rode
`python3 revisao-banca/secretaria/gerar_formularios_pessoais.py`.

⚠️ Esses formulários saem em `COM_DADOS_PESSOAIS/`, que está no `.gitignore` junto com o JSON —
CPF e RG não entram no repositório. Os arquivos `PREENCHIDO_*` que estão versionados seguem sem
dado pessoal; use os de `COM_DADOS_PESSOAIS/` para enviar.

**De terceiros**
- CPF dos cinco membros da banca (Tasinaffo, Maximo, Dias, Bruno, Alberto), que são os mesmos do
  orientador e do coorientador nos formulários CAPES 1 e 2. Peça a cada um com o texto de
  `EMAIL_3_pedido_cpf.md`, e pergunte à Secretaria se ela já os tem do processo de nomeação da
  banca — é o caminho mais rápido (`EMAIL_4_elenice_cpf.md`). O Prof. Maximo levantou que o
  formulário 2 talvez nem seja mais exigido nas Engenharias IV, e que antes esses dados eram
  coletados na própria defesa; confirme isso antes de sair pedindo a todos.
  **Não é motivo para segurar a entrega:** mande hoje com esses campos em branco (o
  `EMAIL_2_secretaria.md` já avisa) e complete depois.

  ⚠️ Os CPFs que você receber **não devem ser commitados**. Digite-os direto no PDF, ou me peça
  para preencher e o arquivo fica fora do Git.

**Campos "(*) Ver Tabela"** — quatro dos cinco resolvidos, com fonte:

- ✅ **Área de Conhecimento (CAPES 3)** — Tabela de Áreas do Conhecimento do CNPq (Lattes):
  `1.03.03.00-6` Metodologia e Técnicas da Computação, `3.04.02.05-0` Sistemas Eletrônicos de
  Medida e de Controle, `3.12.00.00-1` Engenharia Aeroespacial.
- ✅ **Linha de Pesquisa (CAPES 1)** — "Sistemas Autônomos e Ciência de Dados". Confirmada num
  registro real do PG/EEC na Plataforma Sucupira, que também confirma a Área de Concentração
  como INFORMÁTICA.
- ✅ **Vínculo Atual (CAPES 2)** — "CLT". O Manual do Coleta de Dados da CAPES (aba Atividade
  Futura) dá as cinco opções: CLT, Servidor Público, Aposentado, Colaborador, Bolsa de fixação.
  Troque se você for servidor público ou colaborador.
- ✅ **Expectativa de Atuação (CAPES 2)** — "Empresa". As opções são: Ensino e Pesquisa, Pesquisa,
  Empresa, Profissional autônomo, Outras. Marquei também "Mesma Área da Titulação: sim".
  Se a ideia for seguir em pesquisa ou docência, troque para "Pesquisa" ou "Ensino e Pesquisa".
- ✅ **Projeto de Pesquisa (CAPES 1)** — "FlyMov", indicado pelo autor como o projeto de pesquisa
  do trabalho. Grafia conforme a dissertação (FlyMov Engineering Research Center), não "FlyMove".
  Observação: o Portal Individual da CAPES do autor não mostra projeto vinculado a ele, e a defesa
  ainda não está registrada no Sucupira — o programa faz esse vínculo na próxima Coleta e pode
  ajustar o nome para o do projeto que mantém cadastrado.

Fontes: Tabela de Áreas do Conhecimento do CNPq (Plataforma Lattes); Plataforma Sucupira,
programa 33011010005P0; Manual do Coleta de Dados da CAPES, aba Atividade Futura.

Nota do manual: os campos de Atividade Futura **não são obrigatórios** — no registro do PG/EEC que
consultei eles estão em branco.

**Financiamento** (CAPES 1) — ✅ preenchido: **Embraer**, natureza **B (Bolsa)**, **29 meses**.

A bolsa foi paga pela Embraer, de 03/2024 a 07/2026 — 29 parcelas contando março de 2024 e julho de
2026 (28 meses de intervalo). O trabalho foi realizado dentro do FlyMov, Centro de Pesquisa em
Engenharia mantido em parceria ITA–Embraer–FAPESP, como declaram os Agradecimentos e a Seção 1.1.1;
a FAPESP financia o centro, mas quem custeou esta bolsa foi a Embraer, e é ela que consta como
financiadora.

## Dados que já usei (conferidos na dissertação)

- Autor: Eric Ezequiel Yoshida de Lima
- Programa: Engenharia Eletrônica e Computação (PG/EEC) — Área de Informática
- Título: Object Detection and Tracking Using an Unmanned Aerial Vehicle
- Defesa: 28/07/2026 · Publicação: 2026 · Nível: Mestrado
- Orientador: Prof. Dr. Marcos Ricardo Omena de Albuquerque Maximo (ITA)
- Coorientador: Dr. Stiven Schwanz Dias (Embraer S.A.)
- Banca: Tasinaffo (Presidente), Maximo (Orientador), Dias (Coorientador), Bruno (Interno),
  Alberto Ferreira de Souza (Externo); suplentes Marcondes e Thiago Oliveira Santos
- Páginas: 155 · Volumes: 1 · Idioma: Inglês
- Biblioteca Depositária: Biblioteca Central do ITA
- Endereço: Rua Francisco Ricci, 181, Apto. 194D, Vila Ema — São José dos Campos/SP — CEP 12.243-261
- E-mail: eric_lima20@yahoo.com
