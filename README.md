<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Cooperativa CLAF - Pedidos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&family=Roboto+Mono:wght@500&display=swap');
        
        body {
            -webkit-tap-highlight-color: transparent;
            font-family: 'Inter', sans-serif;
        }

        /* Estilo para remover setas do input number */
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        input[type=number] {
            -moz-appearance: textfield;
        }

        .item-row:nth-child(even) {
            background-color: #f9fafb;
        }

        /* Ajuste fino para inputs de quantidade muito pequenos */
        .input-qtd {
            padding-top: 2px !important;
            padding-bottom: 2px !important;
            height: 28px !important;
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">

    <div class="max-w-md mx-auto bg-white min-h-screen shadow-xl flex flex-col">
        
        <!-- Cabeçalho -->
        <header class="bg-green-700 pt-5 pb-3 px-4 text-center border-b-4 border-yellow-400">
            <h1 class="text-white text-2xl font-black uppercase tracking-tight italic">
                COOPERATIVA <span class="text-yellow-300">CLAF</span>
            </h1>
            <p class="text-green-100 text-[10px] uppercase tracking-[0.2em] font-bold mt-1">
                Sistema de Entrega de Produtos
            </p>
        </header>

        <form id="meuFormulario" class="flex-grow p-3 space-y-3">
            
            <!-- Bloco de Identificação com Fontes Maiores -->
            <div class="bg-white border border-gray-200 rounded-xl p-3 shadow-sm space-y-3">
                <div>
                    <label class="block text-xs font-black text-gray-700 uppercase mb-1 ml-1">Nome do produtor</label>
                    <input type="text" name="nome" required
                           class="w-full bg-gray-50 border border-gray-200 rounded-lg py-2.5 px-3 text-sm font-bold focus:ring-2 focus:ring-green-500 focus:bg-white outline-none transition-all" 
                           placeholder="Digite seu nome completo">
                </div>
                <div>
                    <label class="block text-xs font-black text-gray-700 uppercase mb-1 ml-1">Data da Entrega</label>
                    <input type="date" name="data" id="campoData" required
                           class="w-full bg-gray-50 border border-gray-200 rounded-lg py-2.5 px-3 text-sm font-bold focus:ring-2 focus:ring-green-500 focus:bg-white outline-none transition-all">
                </div>
            </div>

            <!-- Tabela de Produtos - ULTRA COMPACTA -->
            <div class="bg-white border border-gray-200 rounded-xl overflow-hidden shadow-sm">
                <div class="bg-gray-100 px-3 py-1 border-b border-gray-200 flex justify-between items-center">
                    <span class="text-[9px] font-black text-gray-500 uppercase tracking-wider">Produto</span>
                    <span class="text-[9px] font-black text-gray-500 uppercase tracking-wider pr-5">Qtd</span>
                </div>
                
                <div id="listaProdutos" class="divide-y divide-gray-100">
                    <!-- Itens gerados via JavaScript -->
                </div>
            </div>

            <!-- Botão de Envio -->
            <div class="pt-1">
                <button type="submit" id="btnEnviar" 
                        class="w-full bg-green-600 hover:bg-green-700 text-white py-4 rounded-xl font-black text-sm shadow-lg transform active:scale-[0.98] transition-all uppercase tracking-widest flex items-center justify-center gap-3">
                    <span>Finalizar Pedido</span>
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M10.293 3.293a1 1 0 011.414 0l6 6a1 1 0 010 1.414l-6 6a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-4.293-4.293a1 1 0 010-1.414z" clip-rule="evenodd" />
                    </svg>
                </button>
            </div>
        </form>

        <footer class="py-4 text-center">
            <span class="text-[9px] font-black text-gray-300 uppercase tracking-widest italic">Cooperativa CLAF</span>
        </footer>
    </div>

    <!-- Modal de Sucesso -->
    <div id="modalSucesso" class="fixed inset-0 bg-green-900/90 hidden items-center justify-center p-6 z-50 backdrop-blur-md">
        <div class="bg-white p-8 rounded-3xl text-center max-w-xs w-full shadow-2xl border-t-8 border-green-500">
            <div class="w-16 h-16 bg-green-100 text-green-600 rounded-full flex items-center justify-center mx-auto mb-4">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7" />
                </svg>
            </div>
            <h2 class="text-xl font-black text-gray-800 uppercase mb-2">Pedido Enviado!</h2>
            <p class="text-sm text-gray-500 mb-6">Os dados foram registrados com sucesso.</p>
            <button onclick="location.reload()" class="w-full bg-gray-900 text-white py-3 rounded-xl font-black uppercase text-xs tracking-widest">Novo Pedido</button>
        </div>
    </div>

    <script>
        const produtos = ["Abacate", "Abacaxi", "Banana_caturra", "Banana_maça_ou_prata", "Laranja_baiana_lima", "Laranja_Comum", "Maça", "Mamão", "Manga", "Maracujá", "Melancia", "Melão", "Pera", "Pêssego", "Tangerina_poncã", "Almeirão", "Acelga", "Agrião", "Alface", "Couve_Manteiga", "Escarola", "Espinafre", "Quiabo", "Repolho", "Rúcula", "Abobrinha_verde", "Beterraba", "Brócolis", "Cenoura", "Chuchu", "Couve_flor", "Milho_verde", "Pepino", "Tomate", "Vagem", "Leite_Pasteurizado", "Pão_Caseiro", "Alho_poró", "Cebola", "Cebolinha_Verde", "Limão", "Pimentão", "Rabanete", "Salsinha", "Batata_doce", "Batata_inglesa", "Iogurte"];

        document.getElementById('campoData').value = new Date().toISOString().split('T')[0];

        const container = document.getElementById('listaProdutos');
        produtos.forEach((prod) => {
            const div = document.createElement('div');
            // py-1 torna a linha bem fina
            div.className = "item-row flex items-center justify-between px-3 py-1";
            div.innerHTML = `
                <span class="text-[10px] font-bold text-gray-600 uppercase tracking-tight leading-tight">${prod.replace(/_/g, ' ')}</span>
                <input type="number" name="${prod}" inputmode="numeric" 
                       class="input-qtd w-12 bg-white border border-gray-300 rounded py-0.5 text-center text-xs font-black focus:border-green-500 focus:ring-1 focus:ring-green-500 outline-none transition-all" 
                       placeholder="0" min="0">
            `;
            container.appendChild(div);
        });

        document.getElementById('meuFormulario').addEventListener('submit', function(e) {
            e.preventDefault();
            const URL_DO_SCRIPT = "https://script.google.com/macros/s/AKfycbwj9Pw6zH5ZVKVjqS5eomVO6kHSZUbtNmcVr6DeA8EmISvqmyTNtwuo-MWlwrdFCl17/exec"; 
            
            const btn = document.getElementById('btnEnviar');
            btn.disabled = true;
            btn.innerHTML = `<span class="animate-pulse italic">ENVIANDO...</span>`;

            const formData = new FormData(this);
            const data = { timestamp: new Date().toLocaleString('pt-BR') };
            
            formData.forEach((value, key) => { 
                if (value && value !== "0" && value !== "") data[key] = value; 
            });

            fetch(URL_DO_SCRIPT, {
                method: 'POST',
                mode: 'no-cors',
                body: new URLSearchParams(data)
            }).then(() => {
                document.getElementById('modalSucesso').style.display = 'flex';
                window.scrollTo(0,0);
            }).catch(() => {
                alert("Erro ao enviar pedido.");
                btn.disabled = false;
                btn.innerHTML = `<span>Finalizar Pedido</span>`;
            });
        });
    </script>
</body>
</html>
