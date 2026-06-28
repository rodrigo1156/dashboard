<!--
  =============================================================================
  PROJETO: Dashboard Orçamentário GEMAK
  ARQUIVO: README.html (Manual de Arquitetura, Regras de Negócio e Componentização)
  =============================================================================
  
  DIRETRIZ DE OURO PARA INTELIGÊNCIAS ARTIFICIAIS (LEIA ANTES DE EDITAR):
  Este dashboard é 100% modularizado no Google Apps Script. Qualquer alteração deve
  ser feita exclusivamente no arquivo/componente responsável. Nunca junte os arquivos
  novamente em um monólito. Respeite os seletores IDs e as dependências globais.
-->

<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <style>
    :root {
      --primary: #2563eb;
      --bg-dark: #0f172a;
      --card-dark: #1e293b;
      --border-dark: #334155;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --accent-green: #10b981;
      --accent-amber: #f59e0b;
    }
    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      line-height: 1.6;
      background-color: var(--bg-dark);
      color: var(--text-main);
      margin: 0;
      padding: 24px;
    }
    .container {
      max-width: 1000px;
      margin: 0 auto;
    }
    .header-box {
      border-left: 6px solid var(--primary);
      padding: 10px 20px;
      background: var(--card-dark);
      border-radius: 0 12px 12px 0;
      margin-bottom: 30px;
    }
    h1, h2, h3 {
      color: #ffffff;
      font-weight: 800;
    }
    h1 { margin: 0 0 10px 0; font-size: 28px; }
    h2 { border-bottom: 2px solid var(--border-dark); padding-bottom: 8px; margin-top: 40px; font-size: 20px; }
    .badge {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 6px;
      font-size: 11px;
      font-weight: bold;
      text-transform: uppercase;
      margin-right: 6px;
    }
    .badge-gs { background: #34a853; color: white; }
    .badge-html { background: #ea4335; color: white; }
    .badge-js { background: #fbbc05; color: black; }
    
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 16px;
      margin-top: 20px;
    }
    .card {
      background: var(--card-dark);
      border: 1px solid var(--border-dark);
      border-radius: 12px;
      padding: 16px;
      transition: transform 0.2s;
    }
    .card:hover {
      transform: translateY(-2px);
      border-color: var(--primary);
    }
    .card-title {
      font-weight: bold;
      font-size: 15px;
      display: flex;
      align-items: center;
      margin-bottom: 8px;
    }
    .rule-box {
      background: rgba(16, 185, 129, 0.1);
      border: 1px solid var(--accent-green);
      border-radius: 12px;
      padding: 20px;
      margin-top: 20px;
    }
    .rule-item {
      margin-bottom: 14px;
    }
    .rule-title {
      font-weight: bold;
      color: #ffffff;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .warn-box {
      background: rgba(245, 158, 11, 0.1);
      border: 1px solid var(--accent-amber);
      border-radius: 12px;
      padding: 20px;
      margin-top: 20px;
    }
    code {
      background: #000000;
      color: #38bdf8;
      padding: 2px 6px;
      border-radius: 4px;
      font-family: monospace;
      font-size: 13px;
    }
    pre {
      background: #000000;
      padding: 16px;
      border-radius: 8px;
      overflow-x: auto;
      border: 1px solid var(--border-dark);
    }
    pre code { color: #a5f3fc; padding: 0; }
  </style>
</head>
<body>

<div class="container">
  <div class="header-box">
    <h1>Manual de Engenharia & Arquitetura</h1>
    <p style="color: var(--text-muted); margin: 0; font-size: 14px;">
      GEMAK Dashboard • Sistema Modularizado em Google Apps Script (GAS)
    </p>
  </div>

  <h2>💡 Filosofia do Projeto</h2>
  <p>
    Para contornar as limitações físicas do ecossistema do Google Apps Script (como travamento de IDE, lentidão no salvamento e limites severos de contexto para Inteligências Artificiais), este projeto foi totalmente decomposto em <strong>componentes e módulos independentes</strong>.
  </p>
  <p>
    A união dos arquivos ocorre em tempo de execução no servidor do Google por meio da função de injeção dinâmica <code>include(filename)</code> no backend.
  </p>

  <h2>📂 Estrutura e Mapa de Arquivos (Árvore de Componentes)</h2>
  <p>O projeto está dividido estritamente em 10 arquivos no painel do Apps Script:</p>

  <div class="grid">
    <!-- BACKEND -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-gs">.gs</span> Codigo.gs
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Backend (Servidor)</strong>: Roteia o ano ativo (2025/2026), gerencia credenciais de planilhas externas, faz a leitura ultrarrápida otimizada por <code>getLastRow()</code> e provê a função <code>include()</code> para a injeção do HTML.
      </p>
    </div>

    <!-- INDEX -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-html">HTML</span> Index.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Orquestrador Principal</strong>: O "esqueleto" do frontend. Define a ordem do layout e injeta os demais componentes HTML, CSS e JS nos lugares correspondentes. Não deve conter regras de negócio diretamente.
      </p>
    </div>

    <!-- HEAD -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-html">HTML</span> Mod_Head.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Estilos e Assets</strong>: Centraliza o carregamento do Tailwind CSS e Chart.js via CDN. Contém todas as configurações globais de estilo de scrollbar personalizada e a animação de entrada de 2 segundos.
      </p>
    </div>

    <!-- LOADER -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-html">HTML</span> Comp_Loader.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Interface Loader</strong>: Elemento absoluto de bloqueio inicial. Exibe a animação de sincronização e o painel de fallback inteligente com opção de carregamento de contingência local (backup) se a API demorar mais de 15s.
      </p>
    </div>

    <!-- HEADER -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-html">HTML</span> Comp_Header.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Barra do Topo</strong>: Contém a identificação visual, filtros de Divisão (Todos/Publicidade/Eventos), Paletas de cores (Padrão/Quente/Neon), o seletor de Ano Ativo, filtro de Período (Mês/ANO) e o banner reativo de Foco Ativo.
      </p>
    </div>

    <!-- KPIS -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-html">HTML</span> Comp_Kpis.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Cards Indicadores</strong>: Define a estrutura HTML/Tailwind dos 4 cards de resumos superiores na ordem correta de negócios: Mês de Pico, Média Mensal, YoY Acumulado e Gasto YTD do Recorte.
      </p>
    </div>

    <!-- GRAFICOS -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-html">HTML</span> Comp_Graficos.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Matriz de Gráficos</strong>: Grid principal que abriga os contêineres e eixos dos gráficos de Distribuição de Parceiros, Linha de Evolução Acumulada, Market Share de fatias, Toasts e a lista do Ranking Top 5 de notas fiscais.
      </p>
    </div>

    <!-- SCRIPT CONFIG -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-js">JS</span> Script_Config.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Configurações Globais</strong>: Define o estado dinâmico do painel (como as variáveis de foco), paletas de cores hexadecimal, base de dados locais de contingência (Backup) e o dicionário com as metas orçamentárias YTD e YoY de 2025/2026.
      </p>
    </div>

    <!-- SCRIPT CONEXAO -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-js">JS</span> Script_Conexao.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Comunicação e Polling</strong>: Realiza a chamada assíncrona bidirecional (<code>google.script.run</code>), faz o tratamento de tempo limite de resposta e orquestra o disparo da carga inicial dos dados.
      </p>
    </div>

    <!-- SCRIPT PAINEL UI -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-js">JS</span> Script_PainelUI.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Lógica da Interface</strong>: Controla o cruzamento de filtros, formata as moedas locais brasileiras, injeta as tags HTML ricas de alto contraste nos subtextos dos cards de KPIs e controla a tabela interativa do Top 5.
      </p>
    </div>

    <!-- SCRIPT GRAFICOS -->
    <div class="card">
      <div class="card-title">
        <span class="badge badge-js">JS</span> Script_Graficos.html
      </div>
      <p style="color: var(--text-muted); font-size: 13px; margin: 0;">
        <strong>Controlador Chart.js</strong>: Gerencia o ciclo de vida completo dos gráficos. Cria, destrói e atualiza os gráficos de barras, linhas e rosca. Implementa o respiro de margem superior (grace) e o truncamento automático do eixo X.
      </p>
    </div>
  </div>

  <h2>⚠️ Regras de Negócio Críticas (Invioláveis)</h2>
  <div class="rule-box">
    <div class="rule-item">
      <div class="rule-title">📅 1. Limite Cronológico de 2026 (Truncamento do Eixo X)</div>
      <p style="color: var(--text-muted); margin: 6px 0 0 0; font-size: 13px;">
        O ano corrente de 2026 está em andamento e tem como limite consolidado o mês de <strong>Junho (Index 5)</strong>. Quando "Ano Completo" estiver selecionado para 2026, os eixos X dos gráficos de Distribuição e Evolução não podem esticar até Dezembro com valores zerados. Eles devem ser truncados dinamicamente em Junho.
      </p>
    </div>
    <div class="rule-item">
      <div class="rule-title">📊 2. Média Mensal Sempre Ativa</div>
      <p style="color: var(--text-muted); margin: 6px 0 0 0; font-size: 13px;">
        O card de média mensal nunca deve ser ocultado ou desativado. Em base consolidada de 2026, ele calcula a média ponderada de Janeiro a Junho (6 meses). Se um mês individual for selecionado (ex: Março), ele calcula a média de Janeiro a Março (3 meses).
      </p>
    </div>
    <div class="rule-item">
      <div class="rule-title">🔄 3. Alinhamento de Cards de Destaque</div>
      <p style="color: var(--text-muted); margin: 6px 0 0 0; font-size: 13px;">
        No topo da tela, o card de <strong>Mês de Pico</strong> deve ser posicionado na primeira coluna para visibilidade imediata de sazonalidade. O card de <strong>Gasto Acumulado YTD</strong> deve ficar na quarta coluna, mostrando o progresso total e sua variação proporcional YoY em percentual e valor nominal.
      </p>
    </div>
    <div class="rule-item">
      <div class="rule-title">📈 4. Prevenção de Cortes de Escala no Chart.js</div>
      <p style="color: var(--text-muted); margin: 6px 0 0 0; font-size: 13px;">
        No gráfico de linhas acumuladas (Evolução), o eixo Y deve obrigatoriamente possuir a propriedade de margem superior <code>grace: '15%'</code>. Isso garante um teto livre para os nós superiores das linhas e evita que os rótulos de valores fiquem comprimidos ou cortados.
      </p>
    </div>
    <div class="rule-item">
      <div class="rule-title">🎨 5. Subtextos Ricos em HTML</div>
      <p style="color: var(--text-muted); margin: 6px 0 0 0; font-size: 13px;">
        Os textos explicativos de rodapé dos cards de KPI não podem ser cinzas ou ilegíveis. Eles devem utilizar tons de alto contraste e tags <code>&lt;strong&gt;</code> coloridas semânticas para focar números importantes: <span style="color: var(--accent-green); font-weight: bold;">verde-esmeralda</span> para variações positivas/dentro da meta, <span style="color: #60a5fa; font-weight: bold;">azul-brilhante</span> para valores nominais em reais, e <span style="color: #f87171; font-weight: bold;">vermelho-coral</span> para desvios excedentes.
      </p>
    </div>
  </div>

  <h2>💡 Guia de Manutenção Rápida para Engenheiros & IAs</h2>
  <div class="warn-box">
    <p style="margin: 0 0 10px 0; font-weight: bold; color: #ffffff;">Como fazer edições seguras no projeto:</p>
    <ul style="margin: 0; padding-left: 20px; font-size: 13px; color: var(--text-muted);">
      <li style="margin-bottom: 6px;"><strong>Adicionar um parceiro ou mudar cores</strong>: Faça isso no mapeamento de paletas de cores dentro do arquivo <code>Script_Config.html</code>.</li>
      <li style="margin-bottom: 6px;"><strong>Mudar o layout de um card</strong>: Ajuste a estrutura Tailwind diretamente no arquivo <code>Comp_Kpis.html</code>.</li>
      <li style="margin-bottom: 6px;"><strong>Alterar regras de cálculo ou formatação de moeda</strong>: Altere as funções correspondentes em <code>Script_PainelUI.html</code>.</li>
      <li style="margin-bottom: 8px;"><strong>Importante</strong>: Toda alteração realizada no Apps Script requer a geração de uma <strong>Nova Implantação (Deploy)</strong> pública no painel para ser refletida no link de visualização dos usuários.</li>
    </ul>
  </div>
</div>

</body>
</html>
