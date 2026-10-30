import os
import webview
import sqlite3

DIRETORIO_ATUAL = os.path.dirname(os.path.abspath(__file__))
CAMINHO_BANCO = os.path.join(DIRETORIO_ATUAL, 'meu_banco.db')

def inicializar_banco():
    conn = sqlite3.connect('meu_banco.db')
    cursor = conn.cursor()
    cursor.execute('''
    CREATE TABLE IF NOT EXISTS estoque (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            nome TEXT,
            tipo TEXT,
            preco INTEGER,
            quantidade REAL
        )
    ''')
    # ADICIONE ESTE BLOCO ABAIXO:
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS vendas (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            data_hora TEXT,
            cliente TEXT,
            item TEXT,
            valor TEXT
        )
    ''')
    cursor.execute('SELECT COUNT(*) FROM estoque')
    if cursor.fetchone()[0] == 0:
           produtos [
            ('Picolé Kids', 'Unidade', 500, 80),
            ('Picolé Tradicional', 'Unidade', 700, 140),
            ('Picolé Recheado', 'Unidade', 1090, 65),
            ('Açaí Premium 500ml', 'Unidade', 2990, 50),
            ('Açaí Tradicional a Granel', 'Peso (Kg)', 4500, 24.0),
            ('Açaí Zero / Especial a Granel', 'Peso (Kg)', 5500, 18.0),
            ('Sorvete de Massa a Granel', 'Peso (Kg)', 3990, 30.0), 
        cursor.executemany('INSERT INTO estoque (nome, tipo, preco, quantidade) VALUES (?, ?, ?, ?)', produtos_iniciais)]

    

    conn.commit()
    conn.close()
        
# ==========================================


# A classe API também usa o banco de dados para buscar e salvar
class Api:
    def obter_estoque(self):
        # Aqui ele abre o banco para ler os dados
        conn = sqlite3.connect('meu_banco.db')
        cursor = conn.cursor()
        cursor.execute('SELECT nome, quantidade FROM estoque')
        dados = cursor.fetchall()
        conn.close()
        return dados

    def realizar_venda(self, nome_produto):
        # Aqui ele abre o banco para alterar (atualizar) os dados
        conn = sqlite3.connect('meu_banco.db')
        cursor = conn.cursor()
        cursor.execute('''
            UPDATE estoque 
            SET quantidade = quantidade - 1 
            WHERE nome = ?
        ''', (nome_produto,))
        conn.commit()
        conn.close()
        return {'status': 'sucesso'}


# Inicializa o banco de dados antes de abrir a janela
inicializar_banco()
api = Api()

# Sistema Frutos de Goiás - Edição Completa (Nome, Quantidade e Exclusão Individual no Estoque)
HTML_CONTENT = """
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Frutos de Goiás - Sistema de Gestão</title>
  <style>
    :root {
      --acai-profundo: #1E0615;
      --roxo-frutos: #4B1537;
      --roxo-claro: #722354;
      --amarelo-destaque: #FFC72C;
      --laranja-tropical: #FF7B00;
      --fundo-creme: #FDF9F5;
      --texto-escuro: #2B2B2B;
      --branco: #FFFFFF;
      --perigo: #D93838;
      --sucesso: #28A745;
    }

    * { box-sizing: border-box; }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: var(--fundo-creme);
      color: var(--texto-escuro);
      margin: 0;
      display: flex;
      height: 100vh;
      overflow: hidden;
    }

    /* Barra Lateral */
    aside {
      width: 320px;
      min-width: 320px;
      background: linear-gradient(180deg, var(--acai-profundo) 0%, var(--roxo-frutos) 100%);
      color: var(--branco);
      display: flex;
      flex-direction: column;
      box-shadow: 6px 0 20px rgba(0, 0, 0, 0.2);
    }

    .logo-area {
      padding: 20px 20px;
      text-align: center;
      border-bottom: 2px solid rgba(255, 199, 44, 0.2);
      background: rgba(0, 0, 0, 0.35);
    }

    .logo-img {
      max-width: 170px;
      height: auto;
      margin-bottom: 12px;
      filter: drop-shadow(0 4px 8px rgba(0,0,0,0.4));
      border-radius: 8px;
    }

    .logo-area p {
      font-size: 0.8rem;
      margin: 4px 0 0;
      color: #F1D4E8;
      text-transform: uppercase;
      letter-spacing: 1.5px;
      font-weight: 600;
    }

    .menu {
      list-style: none;
      padding: 0;
      margin: 0;
      flex: 1;
      overflow-y: auto;
    }

    .menu li {
      padding: 16px 24px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
      font-weight: 600;
      font-size: 0.95rem;
      cursor: pointer;
      transition: all 0.25s ease;
      color: #EFE3EC;
    }

    .menu li:hover {
      background-color: rgba(255, 199, 44, 0.12);
      color: var(--amarelo-destaque);
      padding-left: 30px;
    }

    .menu li.ativo {
      background-color: rgba(255, 199, 44, 0.22);
      border-left: 6px solid var(--amarelo-destaque);
      color: var(--amarelo-destaque);
      font-weight: bold;
    }

    /* Conteúdo Principal */
    main {
      flex: 1;
      padding: 40px;
      overflow-y: auto;
      background: radial-gradient(circle at top right, #FFF6E8 0%, var(--fundo-creme) 65%);
    }

    header h1 {
      color: var(--roxo-frutos);
      margin-top: 0;
      font-size: 1.8rem;
      font-weight: 800;
    }

    header p {
      color: #665563;
      font-size: 1rem;
    }

    .aba-conteudo { display: none; }
    .aba-conteudo.ativa { display: block; animation: fadeIn 0.3s ease-in-out; }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(4px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .card {
      background: var(--branco);
      border-left: 6px solid var(--amarelo-destaque);
      padding: 25px;
      margin-bottom: 22px;
      border-radius: 10px;
      box-shadow: 0 4px 18px rgba(75, 21, 55, 0.07);
    }

    .card h3 {
      margin-top: 0;
      color: var(--roxo-frutos);
      font-size: 1.25rem;
      border-bottom: 2px solid #F8F1F5;
      padding-bottom: 12px;
    }

    label {
      font-weight: 700;
      display: block;
      margin-bottom: 6px;
      color: var(--roxo-frutos);
      font-size: 0.9rem;
    }

    select, input, textarea {
      width: 100%;
      padding: 12px;
      border: 1.5px solid #E2D4DE;
      border-radius: 6px;
      margin-bottom: 18px;
      font-size: 0.95rem;
      background-color: #FAFAFC;
    }

    select:focus, input:focus, textarea:focus {
      outline: none;
      border-color: var(--laranja-tropical);
      background-color: var(--branco);
    }

    .btn-principal {
      background: linear-gradient(135deg, var(--roxo-frutos) 0%, var(--acai-profundo) 100%);
      color: var(--branco);
      border: none;
      padding: 12px 20px;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
      width: 100%;
      font-size: 1rem;
      box-shadow: 0 4px 12px rgba(75, 21, 55, 0.25);
      transition: opacity 0.2s;
    }

    .btn-principal:hover { opacity: 0.93; }
    .btn-perigo { background: linear-gradient(135deg, #E64C4C 100%, var(--perigo) 0%); }

    .total-destaque {
      font-size: 1.8rem;
      font-weight: bold;
      color: var(--roxo-frutos);
      background-color: var(--amarelo-destaque);
      padding: 12px 20px;
      border-radius: 8px;
      display: inline-block;
      margin-top: 8px;
      box-shadow: inset 0 2px 4px rgba(0,0,0,0.05);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 12px;
      border-radius: 6px;
      overflow: hidden;
    }

    th, td {
      border: 1px solid #EFE5EC;
      text-align: left;
      padding: 12px;
      font-size: 0.9rem;
    }

    th {
      background-color: #F4EBEF;
      color: var(--roxo-frutos);
      font-weight: 700;
    }

    #painel-nf-vazio { text-align: center; padding: 30px; color: #666; }
    #painel-nf-lista { display: none; }

    /* Modal de Pré-visualização / PDF */
    #modal-preview {
      display: none;
      position: fixed;
      top: 0; left: 0; width: 100%; height: 100%;
      background-color: rgba(30, 6, 21, 0.75);
      z-index: 1000;
      justify-content: center; align-items: center;
    }

    .preview-box {
      background: #FFFFFF; width: 480px; padding: 30px;
      border-radius: 10px; box-shadow: 0 10px 30px rgba(0,0,0,0.35);
      font-family: 'Courier New', Courier, monospace; color: #111;
    }

    .preview-header {
      text-align: center; border-bottom: 2px dashed #4B1537; padding-bottom: 12px; margin-bottom: 15px;
    }
    .preview-header h4 { margin: 0; color: var(--roxo-frutos); font-size: 1.15rem; }
    .preview-body p { margin: 6px 0; font-size: 0.85rem; }
    .preview-footer { border-top: 2px dashed #4B1537; margin-top: 15px; padding-top: 12px; text-align: center; }
    .btn-fechar-modal { background: #666; color: white; border: none; padding: 8px 15px; border-radius: 4px; cursor: pointer; margin-top: 15px; font-family: 'Segoe UI', sans-serif; }

    @media print {
      body * { visibility: hidden; }
      #modal-preview, #modal-preview * { visibility: visible; }
      #modal-preview {
        position: absolute; left: 0; top: 0; width: 100%; height: 100%;
        background: white !important; display: flex !important; justify-content: center; align-items: flex-start;
      }
      .preview-box { box-shadow: none; border: none; width: 100%; }
      .btn-fechar-modal, .preview-footer button { display: none !important; }
    }
  </style>
</head>
<body>

  <aside>
    <div class="logo-area">
      <img src="https://static.wixstatic.com/media/60bc3b_e1b4ce39962547348f11db4604ffb5a2~mv2.png" alt="Frutos de Goiás" class="logo-img">
      <p>Sabor Natural</p>
    </div>
    <ul class="menu">
      <li id="m-pedidos" class="ativo" onclick="trocarAba('pedidos', this)">🍦 Pedidos</li>
      <li id="m-estoque" onclick="trocarAba('estoque', this)">📦 Estoque e Cadastro</li>
      <li id="m-excluir" onclick="trocarAba('excluir', this)">🗑️ Exclusão de Registros</li>
      <li id="m-relatorios" onclick="trocarAba('relatorios', this)">📊 Relatórios Financeiros</li>
      <li id="m-nf" onclick="trocarAba('nf', this)">🧾 Notas Fiscais Emitidas</li>
    </ul>
  </aside>

  <main>
    <!-- ABA PEDIDOS -->
    <div id="aba-pedidos" class="aba-conteudo ativa">
      <header>
        <h1>Módulo de Pedidos e Vendas</h1>
        <p>Atendimento rápido para produtos por unidade ou por peso (kg / balança).</p>
      </header>

      <div class="card">
        <h3>Nova Venda (Leitor ou Balança)</h3>
        
        <label for="codigoBarras">Código de Barras ou Leitura de Balança:</label>
        <input type="text" id="codigoBarras" placeholder="Passe o leitor ou digite (Ex: 789101 ou peso em kg)" autofocus onkeypress="verificarLeitura(event)">
        <p style="font-size: 0.75rem; color: #666; margin-top: -12px; margin-bottom: 15px;">
          • Unidade (Ex: <strong>789101</strong>) | • Peso em KG (Ex digite: <strong>0.500</strong> para 500g)
        </p>

        <label for="tipoVenda">Modo de Seleção:</label>
        <select id="tipoVenda" onchange="alternarModoPeso()">
          <option value="unidade">Produto por Unidade (Picolés, Potes)</option>
          <option value="peso">Produto por Peso / Granel (R$ por Kg)</option>
        </select>

        <div id="bloco-unidade">
          <label for="produtoUnidade">Produto (Catálogo por Unidade):</label>
          <select id="produtoUnidade" onchange="calcularTotal()">
            <option value="500" data-name="Picolé Kids">Picolé Kids - R$ 5,00 un</option>
            <option value="700" data-code="789101" data-name="Picolé Tradicional">Picolé Tradicional - R$ 7,00 un</option>
            <option value="1090" data-name="Picolé Recheado">Picolé Recheado - R$ 10,90 un</option>
            <option value="2990" data-code="789500" data-name="Açaí Premium 500ml">Açaí Premium 500ml - R$ 29,90 un</option>
          </select>

          <label for="quantidade">Quantidade (Unidades):</label>
          <input type="number" id="quantidade" value="1" min="1" oninput="calcularTotal()">
        </div>

        <div id="bloco-peso" style="display: none;">
          <label for="produtoPeso">Produto a Granel:</label>
          <select id="produtoPeso" onchange="calcularTotal()">
            <option value="4500" data-name="Açaí Tradicional a Granel">Açaí Tradicional a Granel - R$ 45,00 / kg</option>
            <option value="5500" data-name="Açaí Zero / Especial a Granel">Açaí Zero / Especial a Granel - R$ 55,00 / kg</option>
            <option value="3990" data-name="Sorvete de Massa a Granel">Sorvete de Massa a Granel - R$ 39,90 / kg</option>
          </select>

          <label for="pesoKg">Peso Lido na Balança (em KG):</label>
          <input type="number" id="pesoKg" step="0.001" value="0.500" min="0.010" placeholder="Ex: 0.450" oninput="calcularTotal()">
        </div>

        <button class="btn-principal" onclick="calcularTotal()">Atualizar Cálculo</button>
      </div>

      <div class="card">
        <h3>Resumo do Pedido</h3>
        <p>Status: <span style="color: var(--sucesso); font-weight: bold;">Pendente / Em Separação</span></p>
        
        <label for="clienteCpf">CPF/CNPJ do Cliente (Opcional):</label>
        <input type="text" id="clienteCpf" placeholder="000.000.000-00">

        <div>Valor Total:</div>
        <div id="valorTotal" class="total-destaque">R$ 7,00</div>
        <br><br>
        <button class="btn-principal" style="background: linear-gradient(135deg, #28A745, #1E7E34);" onclick="finalizarVenda()">Finalizar Venda e Gerar Nota Fiscal</button>
      </div>
    </div>

    <!-- ABA CONTROLE DE ESTOQUE E CADASTRO -->
    <div id="aba-estoque" class="aba-conteudo">
      <header>
        <h1>Controle de Estoque e Cadastro</h1>
        <p>Cadastre novos produtos, edite nomes e inventário ou acompanhe a baixa em tempo real.</p>
      </header>

      <!-- Formulário para Cadastrar Novo Produto -->
      <div class="card">
        <h3 style="color: var(--laranja-tropical);">➕ Cadastrar Novo Produto</h3>
        
        <label for="novoNomeProduto">Nome do Produto:</label>
        <input type="text" id="novoNomeProduto" placeholder="Ex: Picolé de Morango / Açaí Especial">

        <label for="novoTipoProduto">Tipo de Venda:</label>
        <select id="novoTipoProduto">
          <option value="unidade">Por Unidade (Picolés, Potes, etc.)</option>
          <option value="peso">Por Peso / Granel (R$ por Kg)</option>
        </select>

        <label for="novoPrecoProduto">Preço (R$):</label>
        <input type="number" id="novoPrecoProduto" step="0.01" placeholder="Ex: 8.50 (se unidade) ou 45.00 (se por kg)">

        <label for="novoQtdEstoque">Quantidade Inicial em Estoque:</label>
        <input type="number" id="novoQtdEstoque" step="1" value="100" placeholder="Ex: 150 unidades ou 30 kg">

        <button class="btn-principal" onclick="cadastrarProduto()">Salvar e Adicionar ao Catálogo</button>
      </div>

      <!-- Tabela de Inventário -->
      <div class="card">
        <h3>Inventário Atualizado em Tempo Real</h3>
        <table>
          <thead>
            <tr><th>Item</th><th>Tipo</th><th>Quantidade / Volume Disponível</th><th>Unidade de Medida</th><th>Status</th><th>Ações</th></tr>
          </thead>
          <tbody id="tabelaInventario">
            <tr data-nome="Picolé Kids"><td>Picolé Kids</td><td>Unidade</td><td class="qtd-val">80</td><td>unidades</td><td></td><td><button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button><button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button></td></tr>
            <tr data-nome="Picolé Tradicional"><td>Picolé Tradicional</td><td>Unidade</td><td class="qtd-val">140</td><td>unidades</td><td></td><td><button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button><button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button></td></tr>
            <tr data-nome="Picolé Recheado"><td>Picolé Recheado</td><td>Unidade</td><td class="qtd-val">65</td><td>unidades</td><td></td><td><button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button><button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button></td></tr>
            <tr data-nome="Açaí Premium 500ml"><td>Açaí Premium 500ml</td><td>Unidade</td><td class="qtd-val">50</td><td>unidades</td><td></td><td><button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button><button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button></td></tr>
            <tr data-nome="Açaí Tradicional a Granel"><td>Açaí Tradicional a Granel</td><td>Peso (Kg)</td><td class="qtd-val">24.000</td><td>kg</td><td></td><td><button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button><button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button></td></tr>
            <tr data-nome="Açaí Zero / Especial a Granel"><td>Açaí Zero / Especial a Granel</td><td>Peso (Kg)</td><td class="qtd-val">18.000</td><td>kg</td><td></td><td><button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button><button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button></td></tr>
            <tr data-nome="Sorvete de Massa a Granel"><td>Sorvete de Massa a Granel</td><td>Peso (Kg)</td><td class="qtd-val">30.000</td><td>kg</td><td></td><td><button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button><button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button></td></tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- ABA EXCLUSÃO DE REGISTROS -->
    <div id="aba-excluir" class="aba-conteudo">
      <header><h1>Exclusão de Registros</h1></header>
      <div class="card" style="border-left-color: var(--perigo);">
        <h3 style="color: var(--perigo);">Área de Exclusão</h3>
        <label>Identificação:</label>
        <input type="text" id="idExclusao" placeholder="Ex: Pedido #992">
        <button class="btn-principal btn-perigo" onclick="confirmarExclusao()">Excluir</button>
      </div>
    </div>

    <!-- ABA RELATÓRIOS -->
    <div id="aba-relatorios" class="aba-conteudo">
      <header><h1>Relatórios e Financeiro</h1></header>
      <div class="card">
        <h3>Fechamento de Caixa</h3>
        <p>• <strong>Vendas Registradas:</strong> <span id="contadorVendas">0</span><br>
           • <strong>Faturamento:</strong> <span id="faturamentoTotal">R$ 0,00</span></p>
      </div>
    </div>

    <!-- ABA NOTAS FISCAIS -->
    <div id="aba-nf" class="aba-conteudo">
      <header><h1>Notas Fiscais Emitidas</h1></header>
      <div id="painel-nf-vazio" class="card">
        <h3>Nenhuma Venda Realizada</h3>
        <p>Conclua vendas na aba de Pedidos para gerar e visualizar as notas fiscais correspondentes.</p>
      </div>
      <div id="painel-nf-lista" class="card" style="border-left-color: var(--sucesso);">
        <h3 style="color: var(--sucesso);">Histórico de Notas Fiscais</h3>
        <p>Selecione uma venda abaixo para visualizar ou gerar o PDF da nota fiscal:</p>
        <table>
          <thead>
            <tr>
              <th># NF</th>
              <th>Data/Hora</th>
              <th>Cliente</th>
              <th>Valor</th>
              <th>Ações</th>
            </tr>
          </thead>
          <tbody id="tabelaNotasFiscais">
            <!-- Preenchido via JavaScript -->
          </tbody>
        </table>
      </div>
    </div>
  </main>

  <!-- MODAL DE PRÉ-VISUALIZAÇÃO / PDF -->
  <div id="modal-preview">
    <div class="preview-box">
      <div class="preview-header">
        <h4>FRUTOS DE GOIÁS - CUPOM FISCAL</h4>
        <p style="margin:2px 0; font-size:0.75rem;">CNPJ: 00.123.456/0001-89</p>
      </div>
      <div class="preview-body">
        <p><strong>NF #ID:</strong> <span id="prevIdNF">--</span></p>
        <p><strong>Data/Hora:</strong> <span id="prevDataHora">--</span></p>
        <p><strong>Cliente:</strong> <span id="prevCliente">--</span></p>
        <p style="border-bottom: 1px dotted #888;"></p>
        <p><strong>Item:</strong> <span id="prevItem">--</span></p>
        <p><strong>Total:</strong> <span id="prevValor">R$ 0,00</span></p>
      </div>
      <div class="preview-footer">
        <button class="btn-principal" style="background: linear-gradient(135deg, #FF7B00, #D96A00); margin-bottom: 8px;" onclick="gerarPDFNotaFiscal()">📥 Salvar / Gerar PDF</button>
        <br>
        <button class="btn-fechar-modal" onclick="fecharPreviewNF()">Fechar</button>
      </div>
    </div>
  </div>

  <script>
    var totalVendasAcumulado = 0;
    var faturamentoAcumulado = 0;
    var listaVendas = [];

    window.onload = function() {
      atualizarStatusEstoqueTabela();
    };

    function trocarAba(nomeAba, elementoLi) {
      var abas = document.getElementsByClassName('aba-conteudo');
      for (var i = 0; i < abas.length; i++) abas[i].classList.remove('ativa');
      var itensMenu = document.querySelectorAll('.menu li');
      for (var j = 0; j < itensMenu.length; j++) itensMenu[j].classList.remove('ativo');

      document.getElementById('aba-' + nomeAba).classList.add('ativa');
      if (elementoLi) elementoLi.classList.add('ativo');
    }

    function alternarModoPeso() {
      var tipo = document.getElementById('tipoVenda').value;
      if (tipo === 'peso') {
        document.getElementById('bloco-unidade').style.display = 'none';
        document.getElementById('bloco-peso').style.display = 'block';
      } else {
        document.getElementById('bloco-unidade').style.display = 'block';
        document.getElementById('bloco-peso').style.display = 'none';
      }
      calcularTotal();
    }

    function calcularTotal() {
      var tipo = document.getElementById('tipoVenda').value;
      var totalReais = 0;

      if (tipo === 'unidade') {
        var select = document.getElementById('produtoUnidade');
        var qtd = parseInt(document.getElementById('quantidade').value, 10) || 1;
        if (qtd < 1) qtd = 1;
        totalReais = (parseInt(select.value, 10) * qtd) / 100;
      } else {
        var select = document.getElementById('produtoPeso');
        var peso = parseFloat(document.getElementById('pesoKg').value) || 0;
        if (peso < 0) peso = 0;
        totalReais = (parseInt(select.value, 10) / 100) * peso;
      }

      document.getElementById('valorTotal').innerText = 'R$ ' + totalReais.toFixed(2).replace('.', ',');
      return totalReais;
    }

    function verificarLeitura(event) {
      if (event.key === 'Enter') {
        var codigo = document.getElementById('codigoBarras').value.trim();
        var selectUni = document.getElementById('produtoUnidade');
        var encontrado = false;

        for (var i = 0; i < selectUni.options.length; i++) {
          if (selectUni.options[i].getAttribute('data-code') === codigo) {
            selectUni.selectedIndex = i;
            document.getElementById('tipoVenda').value = 'unidade';
            alternarModoPeso();
            encontrado = true;
            break;
          }
        }

        if (encontrado) {
          calcularTotal();
          alert('Produto por unidade localizado!');
          document.getElementById('codigoBarras').value = '';
        } else {
          var possivelPeso = parseFloat(codigo.replace(',', '.'));
          if (!isNaN(possivelPeso) && possivelPeso > 0) {
            document.getElementById('tipoVenda').value = 'peso';
            alternarModoPeso();
            document.getElementById('pesoKg').value = possivelPeso.toFixed(3);
            calcularTotal();
            alert('Peso em kg aplicado com sucesso!');
            document.getElementById('codigoBarras').value = '';
          } else {
            alert('Código não reconhecido ou peso inválido.');
          }
        }
      }
    }

    function cadastrarProduto() {
      var nome = document.getElementById('novoNomeProduto').value.trim();
      var tipo = document.getElementById('novoTipoProduto').value;
      var preco = parseFloat(document.getElementById('novoPrecoProduto').value);
      var qtd = parseFloat(document.getElementById('novoQtdEstoque').value);

      if (!nome || isNaN(preco) || preco <= 0 || isNaN(qtd)) {
        alert('Por favor, preencha todos os campos corretamente.');
        return;
      }

      var precoEmCentavos = Math.round(preco * 100);
      var precoFormatadoStr = 'R$ ' + preco.toFixed(2).replace('.', ',');

      if (tipo === 'unidade') {
        var selectUni = document.getElementById('produtoUnidade');
        var opt = document.createElement('option');
        opt.value = precoEmCentavos;
        opt.setAttribute('data-name', nome);
        opt.text = nome + ' - ' + precoFormatadoStr + ' un';
        selectUni.add(opt);
      } else {
        var selectPeso = document.getElementById('produtoPeso');
        var opt = document.createElement('option');
        opt.value = precoEmCentavos;
        opt.setAttribute('data-name', nome);
        opt.text = nome + ' - ' + precoFormatadoStr + ' / kg';
        selectPeso.add(opt);
      }

      var tabelaInv = document.getElementById('tabelaInventario');
      var tr = document.createElement('tr');
      tr.setAttribute('data-nome', nome);
      tr.innerHTML = `
        <td>${nome}</td>
        <td>${tipo === 'unidade' ? 'Unidade' : 'Peso (Kg)'}</td>
        <td class="qtd-val">${tipo === 'unidade' ? qtd : qtd.toFixed(3)}</td>
        <td>${tipo === 'unidade' ? 'unidades' : 'kg'}</td>
        <td></td>
        <td>
          <button class="btn-principal" style="padding: 4px 8px; font-size: 0.75rem; background-color: #17a2b8; margin-right: 4px;" onclick="editarItemEstoque(this)">✏️ Editar</button>
          <button class="btn-principal btn-perigo" style="padding: 4px 8px; font-size: 0.75rem;" onclick="excluirItemEstoque(this)">🗑️ Excluir</button>
        </td>
      `;
      tabelaInv.appendChild(tr);
      atualizarStatusEstoqueTabela();

      document.getElementById('novoNomeProduto').value = '';
      document.getElementById('novoPrecoProduto').value = '';
      document.getElementById('novoQtdEstoque').value = '100';

      alert('Produto cadastrado e integrado com sucesso!');
    }

    function editarItemEstoque(botao) {
      var tr = botao.closest('tr');
      var nomeAntigo = tr.getAttribute('data-nome');
      var tdNome = tr.cells[0];
      var tdQtd = tr.querySelector('.qtd-val');
      var qtdAtual = tdQtd.innerText;
      var tipoTexto = tr.cells[1].innerText;

      var novoNome = prompt('Digite o novo nome para o produto:', nomeAntigo);
      if (novoNome === null) return;
      novoNome = novoNome.trim();
      if (!novoNome) {
        alert('O nome do produto não pode ficar vazio.');
        return;
      }

      var novaQtdStr = prompt('Digite a nova quantidade/volume para ' + novoNome + ':', qtdAtual);
      if (novaQtdStr === null) return;

      var novaQtd = parseFloat(novaQtdStr.replace(',', '.'));
      if (isNaN(novaQtd) || novaQtd < 0) {
        alert('Valor de quantidade inválido.');
        return;
      }

      tr.setAttribute('data-nome', novoNome);
      tdNome.innerText = novoNome;
      if (tipoTexto.includes('Unidade')) {
        tdQtd.innerText = Math.round(novaQtd);
      } else {
        tdQtd.innerText = novaQtd.toFixed(3);
      }
      atualizarStatusEstoqueTabela();
      atualizarSelectsVendaNome(nomeAntigo, novoNome);

      alert('Item atualizado com sucesso!');
    }

    function excluirItemEstoque(botao) {
      var tr = botao.closest('tr');
      var nomeProduto = tr.getAttribute('data-nome');

      if (confirm('Tem certeza que deseja excluir o produto "' + nomeProduto + '" do estoque e do catálogo de vendas?')) {
        removerSelectsVendaNome(nomeProduto);
        tr.remove();
        alert('Produto excluído com sucesso!');
      }
    }

    function removerSelectsVendaNome(nomeProduto) {
      var selectUni = document.getElementById('produtoUnidade');
      for (var i = selectUni.options.length - 1; i >= 0; i--) {
        if (selectUni.options[i].getAttribute('data-name') === nomeProduto) {
          selectUni.remove(i);
          break;
        }
      }

      var selectPeso = document.getElementById('produtoPeso');
      for (var j = selectPeso.options.length - 1; j >= 0; j--) {
        if (selectPeso.options[j].getAttribute('data-name') === nomeProduto) {
          selectPeso.remove(j);
          break;
        }
      }
      calcularTotal();
    }

    function atualizarSelectsVendaNome(nomeAntigo, nomeNovo) {
      var selectUni = document.getElementById('produtoUnidade');
      for (var i = 0; i < selectUni.options.length; i++) {
        var opt = selectUni.options[i];
        if (opt.getAttribute('data-name') === nomeAntigo) {
          opt.setAttribute('data-name', nomeNovo);
          var precoTexto = opt.text.split(' - ')[1] || '';
          opt.text = nomeNovo + ' - ' + precoTexto;
          break;
        }
      }

      var selectPeso = document.getElementById('produtoPeso');
      for (var j = 0; j < selectPeso.options.length; j++) {
        var optPeso = selectPeso.options[j];
        if (optPeso.getAttribute('data-name') === nomeAntigo) {
          optPeso.setAttribute('data-name', nomeNovo);
          var precoTextoPeso = optPeso.text.split(' - ')[1] || '';
          optPeso.text = nomeNovo + ' - ' + precoTextoPeso;
          break;
        }
      }
    }

    function darBaixaEstoque(nomeProdutoVendido, quantidadeVendida, tipoModo) {
      var linhas = document.querySelectorAll('#tabelaInventario tr');
      for (var i = 0; i < linhas.length; i++) {
        var tr = linhas[i];
        var nomeItem = tr.getAttribute('data-nome');
        if (nomeItem === nomeProdutoVendido) {
          var tdQtd = tr.querySelector('.qtd-val');
          if (tdQtd) {
            var qtdAtual = parseFloat(tdQtd.innerText.replace(',', '.')) || 0;
            var novaQtd = qtdAtual - quantidadeVendida;
            if (novaQtd < 0) novaQtd = 0;

            if (tipoModo === 'unidade') {
              tdQtd.innerText = Math.round(novaQtd);
            } else {
              tdQtd.innerText = novaQtd.toFixed(3);
            }
          }
          break;
        }
      }
      atualizarStatusEstoqueTabela();
    }

    function atualizarStatusEstoqueTabela() {
      var linhas = document.querySelectorAll('#tabelaInventario tr');
      for (var i = 0; i < linhas.length; i++) {
        var tr = linhas[i];
        var tdQtd = tr.querySelector('.qtd-val');
        if (!tdQtd) continue;

        var valorQtd = parseFloat(tdQtd.innerText.replace(',', '.')) || 0;
        var tipoTexto = tr.cells[1].innerText;
        var tdStatus = tr.cells[4];
        var limiteBaixo = (tipoTexto.includes('Unidade')) ? 10 : 5;

        if (valorQtd <= 0) {
          tdStatus.innerHTML = '<span style="color: var(--perigo); font-weight: bold;">Esgotado</span>';
        } else if (valorQtd <= limiteBaixo) {
          tdStatus.innerHTML = '<span style="color: var(--laranja-tropical); font-weight: bold;">Estoque Baixo</span>';
        } else {
          tdStatus.innerHTML = '<span style="color: var(--sucesso); font-weight: bold;">Ideal</span>';
        }
      }
    }

    function finalizarVenda() {
      var tipo = document.getElementById('tipoVenda').value;
      var textoProduto = '';
      var nomeProdutoAlvo = '';
      var quantidadeBaixa = 0;
      var valorNumerico = calcularTotal();
      var valorAtualStr = document.getElementById('valorTotal').innerText;
      var cpfCliente = document.getElementById('clienteCpf').value.trim() || 'Consumidor Não Identificado';
      var dataHoraAtual = new Date().toLocaleString('pt-BR');

      if (tipo === 'unidade') {
        var sel = document.getElementById('produtoUnidade');
        var qtd = parseInt(document.getElementById('quantidade').value, 10) || 1;
        quantidadeBaixa = qtd;
        nomeProdutoAlvo = sel.options[sel.selectedIndex].getAttribute('data-name') || sel.options[sel.selectedIndex].text.split(' - ')[0];
        textoProduto = qtd + 'x ' + nomeProdutoAlvo;
      } else {
        var sel = document.getElementById('produtoPeso');
        var peso = parseFloat(document.getElementById('pesoKg').value) || 0;
        quantidadeBaixa = peso;
        nomeProdutoAlvo = sel.options[sel.selectedIndex].getAttribute('data-name') || sel.options[sel.selectedIndex].text.split(' - ')[0];
        textoProduto = peso.toFixed(3) + ' kg de ' + nomeProdutoAlvo;
      }

      darBaixaEstoque(nomeProdutoAlvo, quantidadeBaixa, tipo);

      totalVendasAcumulado += 1;
      faturamentoAcumulado += valorNumerico;

      var novaVenda = {
        id: totalVendasAcumulado,
        dataHora: dataHoraAtual,
        cliente: cpfCliente,
        item: textoProduto + ' - Frutos de Goiás',
        valor: valorAtualStr
      };

      listaVendas.push(novaVenda);

      document.getElementById('contadorVendas').innerText = totalVendasAcumulado;
      document.getElementById('faturamentoTotal').innerText = 'R$ ' + faturamentoAcumulado.toFixed(2).replace('.', ',');

      atualizarTabelaNotasFiscais();

      document.getElementById('painel-nf-vazio').style.display = 'none';
      document.getElementById('painel-nf-lista').style.display = 'block';
      document.getElementById('clienteCpf').value = '';

      alert('Venda #' + novaVenda.id + ' finalizada com sucesso!');
    }

    function atualizarTabelaNotasFiscais() {
      var tbody = document.getElementById('tabelaNotasFiscais');
      tbody.innerHTML = '';

      for (var i = 0; i < listaVendas.length; i++) {
        var venda = listaVendas[i];
        var tr = document.createElement('tr');
        tr.innerHTML = `
          <td><strong>#${venda.id}</strong></td>
          <td>${venda.dataHora}</td>
          <td>${venda.cliente}</td>
          <td>${venda.valor}</td>
          <td>
            <button class="btn-principal" style="padding: 6px 12px; font-size: 0.8rem; background-color: #17a2b8;" onclick="abrirPreviewNF(${venda.id})">📄 Gerar PDF / Imprimir</button>
          </td>
        `;
        tbody.appendChild(tr);
      }
    }

    function abrirPreviewNF(idVenda) {
      var vendaEncontrada = null;
      for (var i = 0; i < listaVendas.length; i++) {
        if (listaVendas[i].id === idVenda) {
          vendaEncontrada = listaVendas[i];
          break;
        }
      }

      if (vendaEncontrada) {
        document.getElementById('prevIdNF').innerText = vendaEncontrada.id;
        document.getElementById('prevDataHora').innerText = vendaEncontrada.dataHora;
        document.getElementById('prevCliente').innerText = vendaEncontrada.cliente;
        document.getElementById('prevItem').innerText = vendaEncontrada.item;
        document.getElementById('prevValor').innerText = vendaEncontrada.valor;
        document.getElementById('modal-preview').style.display = 'flex';
      }
    }

    function fecharPreviewNF() { 
      document.getElementById('modal-preview').style.display = 'none'; 
    }

    function gerarPDFNotaFiscal() { 
      window.print();
    }
    
    function confirmarExclusao() {
      if (confirm('Deseja excluir este registro?')) {
        alert('Excluído com sucesso!');
        document.getElementById('idExclusao').value = '';
      }
    }
  </script>
</body>
</html>
"""

if __name__ == '__main__':
    webview.create_window('Frutos de Goiás - Sistema de Gestão', html=HTML_CONTENT, width=1150, height=720)
    webview.start()
