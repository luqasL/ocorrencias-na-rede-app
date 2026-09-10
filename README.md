# Ocorrências PA — Registrador Rápido

Ferramenta pessoal para adicionar, fechar, editar e excluir linhas na
planilha **Ocorrências PA** sem precisar digitar célula por célula. É um
site simples (HTML) que conversa com um script publicado na própria
planilha (Google Apps Script).

**Link do site:** https://luqasl.github.io/ocorrencias-na-rede-app/ocorrencias-na-rede-app.html

> ⚠️ Esse link e a senha de acesso não devem ser compartilhados — veja
> [Segurança](#segurança) mais abaixo.

## Índice

- [Funcionalidades](#funcionalidades)
- [Manual de uso](#manual-de-uso)
- [Instalação / configuração](#instalação--configuração)
- [Segurança](#segurança)
- [Limitações conhecidas](#limitações-conhecidas)
- [Histórico de versões](#histórico-de-versões)

## Funcionalidades

- **Novo registro** — formulário único, um incidente por vez.
- **Adicionar eventos massivo** — fila com vários incidentes de uma vez,
  cada um podendo afetar várias portas (Slot/PON).
- **Gerenciar incidentes** — ver e fechar os abertos, buscar e editar os
  fechados, seleção múltipla pra fechar/excluir em lote.
- **OLTs e Eventos** — gerenciar as listas usadas nos formulários
  (adicionar, renomear, excluir).

## Manual de uso

### Novo registro

Preenche Evento, OLT, Slot/PON (seleciona Slot e PON nos dropdowns, ou usa
"Texto livre" pra casos como "--"), Início (obrigatório), Término (deixe em
branco se o incidente ainda está em aberto), Observação, Horário da
informação e Fonte. Nos campos de Evento, OLT e Fonte, a opção **"+
Adicionar..."** no fim da lista deixa cadastrar um valor novo na hora.

### Adicionar eventos massivo

Pensada pra quando um mesmo evento afeta várias portas, ou quando vários
incidentes diferentes acontecem ao mesmo tempo (ex: uma OLT caiu em vários
slots, e outra OLT caiu também, tudo pra registrar de uma vez).

- Preenche os campos do incidente (Evento, OLT, Início etc.) normalmente.
- Em **Portas afetadas**, adiciona uma porta por vez (Slot + PON, ou texto
  livre) — os chips vão se acumulando.
- Quando todas as portas desse incidente estiverem nos chips, clica em
  **"Adicionar item à fila"**. Isso fecha esse item e limpa o formulário
  pro próximo.
- Repete quantas vezes precisar (cada clique em "Adicionar item à fila" =
  um incidente novo na fila, podendo ter OLT/horário diferentes).
- No final, clica em **"Enviar tudo"** — manda a fila inteira numa vez só.
- A fila fica salva automaticamente no navegador enquanto não for enviada
  (sobrevive a um fechar de aba sem querer).
- **Regra importante:** itens com o mesmo Início + mesma OLT viram o mesmo
  `ID_INCIDENTE` na planilha (mesmo estando em itens diferentes da fila).
  Itens com OLT ou horário diferentes sempre viram incidentes separados.

### Gerenciar incidentes

**Abertos** — lista os incidentes sem término. Dá pra filtrar por OLT e
por intervalo de datas. Cada card tem **Fechar** (define o término) e
**Excluir**. Marcando o checkbox de vários, aparecem os botões **Fechar
selecionados** (um único horário de término aplicado a todos) e **Excluir
selecionados**.

**Buscar fechados** — mesmo filtro (OLT + De/Até). Mostra sempre os mais
recentes primeiro; se a busca trouxer mais de 50 resultados, mostra só os
50 mais recentes com um aviso pra refinar o filtro. Cada card tem
**Editar** (abre um formulário completo pra corrigir qualquer campo) e
**Excluir**. Seleção múltipla disponível só para excluir em lote.

### OLTs e Eventos

Gerencia as listas usadas nos dropdowns de OLT e Evento em todo o site.
**Adicionar** cria um valor novo. **Renomear** troca o nome na lista **e**
atualiza os registros já lançados que usavam o nome antigo (mostra quantas
linhas foram atualizadas). **Excluir** só tira da lista de sugestões —
não mexe em registros já lançados.

## Instalação / configuração

O passo a passo completo está no `LEIA-ME.md` original, resumindo:

1. Cole o conteúdo do `Codigo.gs` no Apps Script da planilha
   (Extensões > Apps Script).
2. Defina uma senha em Configurações do projeto > Propriedades do
   script > `ACCESS_TOKEN`.
3. Implante como Web App (Implantar > Nova implantação > App da Web,
   executar como "Eu", acesso "Qualquer pessoa) e copie a URL gerada.
4. No arquivo `.html`, preencha `WEBAPP_URL` e `ACCESS_TOKEN` com os
   valores acima.
5. Publique o `.html` (esse repositório, via GitHub Pages) ou abra
   localmente.

Pra atualizar o `Codigo.gs` depois de uma mudança: cole o código novo no
editor do Apps Script e use **Implantar > Gerenciar implantações > lápis
> Nova versão > Implantar** (mantém a mesma URL).

## Segurança

- O `.html` publicado aqui contém a `WEBAPP_URL` e o `ACCESS_TOKEN` no
  código-fonte, visível pra qualquer um que abrir a página. **Não
  compartilhe o link deste repositório nem o link do site.**
- Se o link vazar, troque o `ACCESS_TOKEN` no Apps Script (invalida o
  antigo imediatamente) e considere renomear o repositório (muda a URL
  do GitHub Pages).
- Login individual por usuário, com permissões diferentes por perfil
  (admin x colaborador), ainda não foi implementado — está planejado
  como próxima etapa.

## Limitações conhecidas

- Excluir uma linha no meio do histórico da planilha pode quebrar a
  fórmula de `ID_INCIDENTE` das linhas seguintes (vira `#REF!`) — é um
  comportamento da própria fórmula da planilha, não do código desta
  ferramenta. Correção é manual, direto na planilha.
- Sem controle de acesso por usuário — todo mundo que tem a senha tem
  acesso total (exceto pelo que a interface deixa fazer).

## Histórico de versões

- **v1** — Novo registro, Gerenciar incidentes (abertos/fechados) básico.
- **v2** — Adicionar eventos massivo (fila), OLTs e Eventos (listas
  gerenciáveis), filtro em Abertos, seleção múltipla, edição de fechados,
  correção de formatação de data/hora ao criar e fechar registros.
