TESTE_SELECAO# Noc-Tracking-Gamified

Widget de meta mensal para o Leads Noc. Fica numa janela pequena sempre por cima das outras, mesmo quando você clica em outro programa, e mostra os leads feitos, quanto falta, o ritmo por dia útil e por semana, e se você está à frente ou atrás do ritmo.

## Instalação no Windows

1. Instale o Node.js (versão LTS) em https://nodejs.org.
2. Descompacte esta pasta onde quiser.
3. Dê dois cliques em `Iniciar.bat`. Na primeira vez, ele instala o que precisa.

Para remover o programa, use `Desinstalar.bat`. Para apenas gerar o instalador (`dist\Leads-Noc-Setup.exe`) sem instalar, use `Gerar-instalador.bat`.

### Atualizar para uma versão nova

Descompacte o zip novo sobre a pasta antiga e dê dois cliques em `Atualizar.bat`. Ele gera o instalador, fecha o programa, instala por cima e abre de novo, tudo sozinho. Metas, token e fotos continuam guardados.

Cada edição tem seu próprio `Atualizar.bat` e `Desinstalar.bat`.

## Primeiro uso

1. Digite a meta de leads do mês e salve.
2. Lance leads manualmente com `-1`, `+1` e `+5`. O botão desfazer remove o último.
3. Clique na engrenagem para ligar o Agendor.

## Ligando o Agendor

1. No Agendor, gere seu token de API em Configurações → Integrações → API.
2. Nos ajustes do widget, marque "Puxar leads do Agendor", cole o token e escolha o que conta como lead: negócios, pessoas ou empresas criados no mês.
3. Clique em "Testar conexão" e depois em "Salvar".

O widget atualiza no intervalo escolhido (de 15 min a 2 h) e também pelo botão de atualizar. Os ajustes manuais somam com o que vem do Agendor. O token fica guardado no seu computador, criptografado quando o Windows permite.

## Metas

Quando o mês vira, os rótulos e as metas trocam sozinhos. Em dezembro, por exemplo, aparecem "Leads de dezembro" e a meta de janeiro. As metas vêm da tabela do plano em `lib/plan.js` (leads, matrículas, metas por unidade e por vendedor, de outubro a março).

Para mudar um número, edite `lib/plan.js` e gere o app de novo, ou digite a meta do mês em Ajustes (vale só para o total geral).

## Perfis

Ao abrir pela primeira vez, aparece a tela "Quem está acessando?", no estilo Netflix. O avatar no canto do cartão volta para essa tela, e o app lembra o último perfil.

- **Filipe (Administrador):** visão completa, com Marketing e Vendas, unidades e vendedores.
- **Filipe (Vendedor), Feliep, João, Nycolas e Lehm:** cada um vê a sua meta de matrículas como principal, a unidade como apoio e a sua rotina do dia (leads para abordar, ligações, follow-ups e visitas). No tamanho grande aparece a comparação com os outros vendedores da unidade. Não aparecem leads de marketing nem da outra unidade.
- **Gabs (SDR):** visitas marcadas contra a meta, realizadas, pendentes, atrasadas e remarcadas. Defina as metas de visitas em Ajustes.
- **Moisa (Marketing):** visão de Marketing, sem a parte de Vendas.

A bolinha do vendedor mostra matrículas feitas / matrículas que deveria ter hoje, a meta dele no mês e a posição na unidade. Fica verde quando está no ritmo e vermelha quando não está. A liderança conta o total de matrículas, e o Filipe fica fora dessa disputa.

Visitas e rotina vêm das tarefas e negócios do Agendor. Como o CRM ainda não tem as visitas 100% definidas, o app lê as tarefas do tipo Visita e conta como remarcadas as que têm "remarc" ou "reagend" no texto. Em Ajustes, o diagnóstico lista os tipos de tarefa e as etapas encontradas, para ajustarmos o `taskKind` em `lib/plan.js`.

## Visão e tamanhos

### Marketing e Vendas

O botão no topo troca a visão. Em Marketing, os leads são o número principal e as matrículas ficam no quadradinho. Em Vendas é o contrário: as matrículas aparecem em destaque, com a meta de matrículas (novembro 63, dezembro 65). As duas têm a linha vermelha do onde deveríamos estar hoje e a marca clara de amanhã.

### Três tamanhos

- **Bolinha:** mostra o número da área escolhida. Arraste para qualquer altura das laterais, clique para abrir o cartão e clique fora para voltar. Há efeito de conquista a cada matrícula e de cadastro a cada lead novo.
- **Cartão pequeno:** visão geral, com meta em destaque, status, quanto falta, leads por dia útil e por semana, e o quadradinho da outra medida. Clique nele para ver a origem.
- **Grande (botão de expandir):** separação por Geral, Santo André e Mauá (cada uma com a sua meta), comparação, gráficos e a meta do próximo mês. Em Vendas, os vendedores aparecem como jogadores, com emoji de iPhone (imagens em `renderer/emoji`), cor, meta e ritmo. O botão Matrículas/Valores troca para as metas de cartão e Pix.

O botão com o círculo, no topo, volta para a bolinha.

### Meses anteriores e comparação

O botão Meses, ao lado do rótulo da meta, abre a lista dos últimos 13 meses. O botão Mês atual volta. O primeiro acesso a um mês antigo busca os dados no Agendor e leva alguns segundos.

O botão Alternar para: Comparação 2025 compara com o mesmo período de 2025, em vez do mês passado (vale para o mês atual). Para voltar, clique em Alternar para: Mês passado.

## Funis, unidades e vendedores

Só contam os negócios dos funis SAPreuni (Santo André) e MAPreuni (Mauá), tanto para leads quanto para matrículas. Negócios de qualquer outro funil são ignorados. O widget identifica o vendedor pelo responsável do negócio.

Se algum nome aparecer diferente do esperado, ajuste o campo `match` em `lib/plan.js`. Em Ajustes, "Testar conexão" mostra os responsáveis e funis que o Agendor devolveu. Ganhos sem vendedor reconhecido aparecem como aviso no painel de vendedores. Os valores somam o valor dos negócios ganhos.

### Meta de visitas da SDR

A tabela de meta e base de visitas (OUT a MAR) já vem como padrão: a primeira tabela é Santo André, a segunda é Mauá e a terceira é o total. A meta da Gabs é a do total. Dá para sobrescrever o mês em Ajustes.

### Quem marcou a visita

Como os vendedores também marcam visitas por ligação, a visão da Gabs separa as visitas marcadas pela SDR, pelos vendedores e as sem origem no CRM. A origem vem do campo de quem criou a tarefa no Agendor. Se o Agendor não devolver esse campo, tudo cai em "Sem origem no CRM". O diagnóstico em Ajustes mostra o que o Agendor trouxe.

## Gamificação (só nesta edição)

Aparece apenas no perfil de cada pessoa (vendedores, Gabs e Moisa), nunca no ADM.

- **Missão do dia:** faixa com o progresso do dia e a sequência de dias seguidos (chama).
- **Níveis na barra da meta:** Bronze (25%), Prata (50%), Ouro (75%) e Diamante (100%).
- **Conquistas:** seis ícones simples na visão expandida: primeiro resultado, no ritmo, semana batida, 5 dias seguidos, liderança e meta batida.
- **Meu ritmo:** no perfil do vendedor, compara o desempenho dele com o ideal de hoje (matrículas, ligações, follow-ups e visitas).
- **Versus:** botão ao lado de Meu ritmo que coloca o vendedor contra outro da unidade, com quem está ganhando, barra de cabo de guerra e placar por categoria.
- **Aviso:** a bolinha sinaliza quando uma conquista nova é desbloqueada.

## Duas edições

Esta pasta é a edição normal (Leads Noc). A edição "Leads Noc Gamificado" é a mesma coisa com a camada de gamificação e vem em outro zip. As duas têm nome, instalador e pasta de dados próprios, então podem ser instaladas ao mesmo tempo. A gamificada copia o token da edição normal na primeira abertura; metas e fotos não são copiadas.

## ADM

No perfil ADM, a visão geral é usada por você e pelo Filipe. Há quatro abas: Marketing, Vendas, Equipe e SDR. No modo compacto, as abas ficam logo abaixo da foto.

- **Equipe:** alterna entre Matrículas e Valores e mostra um gráfico com uma linha por vendedor (cada um com sua cor e ícone), o ritmo da soma das metas e o quadro de cada vendedor com feito, meta e ideal de hoje. As bolinhas de "Quem aparece" ligam e desligam cada pessoa, inclusive o Filipe, e a escolha fica salva.
- **SDR:** a mesma visão de visitas da Gabs (marcadas, realizadas, quem marcou e por unidade).

## Ajustes

- Metas de leads e de matrículas do mês atual e do próximo (já vêm 650 leads em dezembro e 63 matrículas em novembro).
- Atualização "Ao vivo" (a cada 1 minuto) ou a cada 5, 15, 30 e 60 minutos.
- Calendário de feriados: nacionais, Carnaval e Corpus Christi vêm marcados. Clique num dia para marcar ou desmarcar. Os municipais (Santo André em 8/4 e Mauá em 8/12) podem ser ligados ou desligados; vale conferir.
- Fotos do fundo: adicione fotos da galeria para passarem atrás do cartão. As 8 fotos da Noc continuam como padrão e podem ser desligadas pelo interruptor.
- Bolinha: são treze opções. Sete redondas (Foto clara, Foto escura, Orbe escuro, Branco, Anéis duplos, Traços e Verde Noc) e seis em outros formatos (Cápsula, Cartão, Barra, Líder, Placar e Número). O Placar mostra todos os vendedores ao mesmo tempo, com as matrículas feitas contra as que cada um deveria ter hoje. O tamanho vale para todas e a mudança é imediata.

## Visual

- Cartão branco e limpo, com foto no topo e a meta em destaque sobre a foto. A parte de baixo da foto é desfocada para o número nunca se perder.
- As 8 fotos ficam em `renderer/photos` e passam sozinhas, bem devagar. Para trocar, substitua os arquivos `1.jpg` a `8.jpg` e ajuste o enquadramento em `PHOTOS` no `renderer/renderer.js`.
- Fonte Poppins incluída no app. Se o computador tiver a Cera Pro (fonte oficial da marca) instalada, ela passa a ser usada.
- Vidro só nas barras e nos botões.
- Ao expandir, a janela cresce para os lados e para baixo, em vez de esticar a lista.
- O ícone do app está em `assets/` (`icon.png` e `icon.ico`). Para usar o logo oficial da Noc, troque esses dois arquivos pelo logo e rode `Gerar-instalador.bat`.

## Perfis e estilos (ajustes recentes)

- Perfis na ordem Moisa, Filipe, João, Lehm, Feliep, Nycolas, Gabs e ADM. O ADM é a visão geral, usada por você e pelo Filipe. O Filipe vendedor aparece como Líder de Vendas.
- Nos estilos Líder e Placar, Moisa e Gabs veem só os próprios números. O Placar da Moisa e do ADM mostra os 4 vendedores. A largura do Placar acompanha quantas pessoas aparecem.
- Visitas da Gabs: contamos tarefas do tipo Visita nos funis SAPreuni e MAPreuni juntos. Se o Agendor não tiver tarefas de visita, passamos a contar os negócios dos dois funis que entraram em etapa de visita no mês, e a tela avisa isso.

## Mexendo no código

- `lib/calc.js`: cálculo de ritmo, dias úteis e semana (testes em `npm test`).
- `lib/agendor.js`: leitura do Agendor (leads, origem, ganhos, unidade, vendedor e valores). Testes em `node test/agendor.test.js`.
- `lib/plan.js`: metas por mês, unidade e vendedor, e os jogadores (emoji e cor).
- `renderer/`: visual da janela.
- `main.js`: janela, sincronização e armazenamento.
