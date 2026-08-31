# Entrega da versão final — Secretaria da Pós-Graduação do ITA

**Prazo: 31/08/2026** (data limite para entrega do arquivo final após defesa do 1º semestre,
CPG nº 01 de 15/02/2013). Defesa em 28/07/2026. Fonte: http://www.ita.br/posgrad/tese

**Estado: pronto para enviar.** 25 de 25 verificações automáticas passaram.

---

## O que enviar, e para quem

### 1. E-mail para a Secretaria — Elenice (elenice@ita.br)

Texto pronto em `EMAIL_2_secretaria.md`. Anexar:

| # | Item | Arquivo |
|---|---|---|
| 1 | Dissertação completa em PDF | `../../tese.pdf` |
| 2 | Folha de rosto (editável) | `EDITAVEL_1_Folha_de_rosto.docx` |
| 3 | Folha da banca (editável) | `EDITAVEL_2_Folha_da_banca.docx` |
| 4 | Resumo (editável) | `EDITAVEL_3_Resumo.docx` |
| 5 | Abstract (editável) | `EDITAVEL_4_Abstract.docx` |
| 6 | Termo de autorização (assinado) | `COM_DADOS_PESSOAIS/Termo_de_Autorizacao.pdf` |
| 7 | CAPES 1 — Identificação | `COM_DADOS_PESSOAIS/Capes1_Identificacao.pdf` |
| 8 | CAPES 2 — Banca Examinadora | `COM_DADOS_PESSOAIS/Capes2_Banca.pdf` |
| 9 | CAPES 3 — Área de Conhecimento | `COM_DADOS_PESSOAIS/Capes3_Area.pdf` |

O Termo sai em `.pdf`, para assinatura, e também em `.docx`, caso a Secretaria prefira editável.

⚠️ **Mande os arquivos de `COM_DADOS_PESSOAIS/`, não os `PREENCHIDO_*`.** Os `PREENCHIDO_*` são as
cópias sem dado pessoal, versionadas no Git; os de envio ficam fora do Git.

Depois do envio, a Secretaria confirma se está tudo no padrão e manda o **link do Stratus** para o
upload final.

### 2. A única pendência antes de mandar

⬜ **Assinatura digital do Prof. Maximo no `tese.pdf`.** O manual do ITA exige que o arquivo final
contenha a assinatura digital do orientador. É o último passo.

---

## O que já está resolvido

**Folha de registro** — a Biblioteca respondeu em 31/08 com o registro **DCTA/ITA/DM-077/2026**,
data **31 de agosto de 2026** e as palavras-chave de indexação. Também reescreveu o campo 10 em
português. Tudo foi transposto para o `tese.tex`, então **a folha impressa na última página do PDF é
a emitida pela Biblioteca** — conferido campo a campo, os dezesseis batem. Não é preciso colar
página nenhuma.

Uma diferença deliberada: a Biblioteca escreveu "Máximo" com acento no campo 10. A dissertação usa
"Maximo" sem acento em todo o documento, a pedido da banca, e assim foi mantido.

**CPF dos membros da banca** — a Secretaria informou que **não é necessário**. Os campos do CAPES 2
ficaram em branco, sem marcador. Os do orientador e do coorientador, que já haviam sido enviados,
estão preenchidos.

**Campos "(*) Ver Tabela"** — todos resolvidos, com fonte:

- **Área de Conhecimento (CAPES 3)**: `1.03.03.00-6` Metodologia e Técnicas da Computação,
  `3.04.02.05-0` Sistemas Eletrônicos de Medida e de Controle, `3.12.00.00-1` Engenharia
  Aeroespacial — Tabela de Áreas do Conhecimento do CNPq (Plataforma Lattes).
- **Linha de Pesquisa (CAPES 1)**: "Sistemas Autônomos e Ciência de Dados", confirmada num registro
  real do PG/EEC na Plataforma Sucupira, que também confirma a Área de Concentração como
  INFORMÁTICA.
- **Projeto de Pesquisa (CAPES 1)**: "FlyMov", indicado pelo autor.
- **Vínculo Atual / Expectativa de Atuação (CAPES 2)**: "CLT" e "Empresa", com "Mesma Área da
  Titulação: sim" — domínios do Manual do Coleta de Dados da CAPES, aba Atividade Futura.

**Financiamento (CAPES 1)** — Embraer, natureza Bolsa, 29 meses (03/2024 a 07/2026, contando as duas
pontas). O FlyMov é um Centro de Pesquisa em Engenharia ITA–Embraer–FAPESP; a FAPESP financia o
centro, mas quem custeou esta bolsa foi a Embraer.

**Folha de rosto** — o bloco do Pró-Reitor foi removido a pedido do orientador: o template novo do
ITA não o traz mais.

---

## Como regerar, se algo mudar

Três scripts, todos lendo do `tese.tex` para que nada saia de sincronia:

```
python3 revisao-banca/secretaria/gerar_editaveis.py            # os quatro .docx editáveis
python3 revisao-banca/secretaria/gerar_folha_registro.py       # a folha de registro .docx
python3 revisao-banca/secretaria/gerar_formularios_pessoais.py # Termo + CAPES 1, 2 e 3
```

Os dados pessoais ficam em `.dados_pessoais.json` e a saída em `COM_DADOS_PESSOAIS/`; os dois estão
no `.gitignore`, de modo que CPF, RG e telefone não entram no repositório.

---

## Dados usados (conferidos na dissertação)

- Autor: Eric Ezequiel Yoshida de Lima
- Programa: Engenharia Eletrônica e Computação (PG/EEC) — Área de Informática
- Título: Object Detection and Tracking Using an Unmanned Aerial Vehicle
- Defesa: 28/07/2026 · Publicação: 2026 · Nível: Mestrado · Matrícula: 01/2024
- Orientador: Prof. Dr. Marcos Ricardo Omena de Albuquerque Maximo (ITA)
- Coorientador: Dr. Stiven Schwanz Dias (Embraer S.A.)
- Banca: Tasinaffo (Presidente), Maximo (Orientador), Dias (Coorientador), Bruno (Interno),
  Alberto Ferreira de Souza (Externo); suplentes Marcondes e Thiago Oliveira Santos
- Páginas: 155 · Volumes: 1 · Idioma: Inglês
- Biblioteca Depositária: Biblioteca Central do ITA
- Endereço: Rua Francisco Ricci, 181, Apto. 194D, Vila Ema — São José dos Campos/SP — CEP 12.243-261
- E-mail: eric_lima20@yahoo.com
