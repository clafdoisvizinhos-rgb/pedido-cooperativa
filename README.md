<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Cooperativa CLAF - Pedidos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        
        /* Reset para evitar transbordamento de texto e elementos */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body { 
            -webkit-tap-highlight-color: transparent; 
            font-family: 'Inter', sans-serif; 
            background-color: #f3f4f6;
            overflow-x: hidden; /* Impede rolagem lateral */
            width: 100vw;
        }

        /* Estética dos inputs */
        input::-webkit-outer-spin-button, input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
        input[type=number] { -moz-appearance: textfield; }
        
        .item-row:nth-child(even) { background-color: #f9fafb; }
        
        /* Força a altura e remove paddings extras que podem "empurrar" o texto */
        .input-qtd { 
            height: 24px !important; 
            padding: 0 !important; 
            border-radius: 4px !important;
            border: 1px solid #d1d5db !important;
            font-size: 11px !important;
            line-height: 24px !important;
        }

        /* Container que trava a largura para evitar que as coisas fiquem "enormes" */
        .app-container {
            width: 100%;
            max-width: 400px; /* Tamanho padrão de celular */
            margin: 0 auto;
            background-color: white;
            min-h: 100vh;
        }
    </style>
</head>
<body class="min-h-screen">
    <div class="app-container shadow-sm flex flex-col">
        
        <!-- Cabeçalho Compacto -->
        <header class="bg-green-700 pt-3 pb-2 px-3 text-center border-b-2 border-yellow-400">
            <h1 class="text-white text-lg font-black uppercase tracking-tighter italic">COOPERATIVA <span class="text-yellow-300">CLAF</span></h1>
            <p class="text-green-100 text-[8px] uppercase tracking-[0.1em] font-bold">Sistema de Entrega</p>
        </header>

        <form id="meuFormulario" class="flex-grow p-3 space-y-2">
            
            <!-- Dados do Produtor -->
            <div class="border border-gray-200 rounded-lg p-2.5 space-y-2 bg-gray-50/30">
                <div>
                    <label class="block text-[10px] font-black text-gray-600 uppercase mb-1 ml-1">Nome do produtor</label>
                    <input type="text" name="nome" required 
                           class="w-full bg-white border border-gray-300 rounded py-1.5 px-2 text-xs font-bold outline-none focus:border-green-600 transition-all" 
                           placeholder="Seu nome">
                </div>
                <div>
                    <label class="block text-[10px] font-black text-gray-600 uppercase mb-1 ml-1">Data da Entrega</label>
                    <input type="date" name="data" id="campoData" required 
                           class="w-full bg-white border border-gray-300 rounded py-1.5 px-2 text-xs font-bold outline-none focus:border-green-600 transition-all">
                </div>
            </div>

            <!-- Tabela de Produtos Compacta -->
            <div class="border border-gray-200 rounded-lg overflow-hidden shadow-sm">
                <div class="bg-gray-100 px-2 py-1 border-b border-gray-200 flex justify-between items-center text-[8px] font-black text-gray-500 uppercase">
                    <span>Produto</span>
                    <span class="pr-3">Qtd</span>
                </div>
                <div id="listaProdutos" class="divide-y divide-gray-100">
                    <!-- Conteúdo via JS -->
                </div>
            </div>

            <!-- Botão de Ação -->
            <div class="pt-2">
                <button type="submit" id="btnEnviar" 
                        class="w-full bg-green-600 text-white py-3 rounded-lg font-black text-[11px] uppercase tracking-widest shadow-md active:bg-green-700 transition-all">
                    Finalizar Pedido
                </button>
            </div>
        </form>

        <footer class="py-2 text-center border-t border-gray-100">
            <span class="text-[8px] font-black text-gray-300 uppercase italic">Cooperativa CLAF</span>
        </footer>
    </div>

    <!-- Modal de Sucesso -->
    <div id="modalSucesso" class="fixed inset-0 bg-black/60 hidden items-center justify-center p-4 z-50 backdrop-blur-sm">
        <div class="bg-white p-6 rounded-2xl text-center max-w-xs w-full shadow-2xl border-t-4 border-green-500">
            <h2 class="text-lg font-black text-gray-800 mb-4">PEDIDO ENVIADO!</h2>
            <button onclick="location.reload()" class="w-full bg-gray-900 text-white py-2 rounded-lg font-black text-[10px]">NOVO PEDIDO</button>
        </div>
    </div>

    <script>
        const produtos = ["Abacate", "Abacaxi", "Banana_caturra", "Banana_maça_ou_prata", "Laranja_baiana_lima", "Laranja_Comum", "Maça", "Mamão", "Manga", "Maracujá", "Melancia", "Melão", "Pera", "Pêssego", "Tangerina_poncã", "Almeirão", "Acelga", "Agrião", "Alface", "Couve_Manteiga", "Escarola", "Espinafre", "Quiabo", "Repolho", "Rúcula", "Abobrinha_verde", "Beterraba", "Brócolis", "Cenoura", "Chuchu", "Couve_flor", "Milho_verde", "Pepino", "Tomate", "Vagem", "Leite_Pasteurizado", "Pão_Caseiro", "Alho_poró", "Cebola", "Cebolinha_Verde", "Limão", "Pimentão", "Rabanete", "Salsinha", "Batata_doce", "Batata_inglesa", "Iogurte"];
        
        document.getElementById('campoData').value = new Date().toISOString().split('T')[0];
        
        const container = document.getElementById('listaProdutos');
        produtos.forEach((prod) => {
            const div = document.createElement('div');
            div.className = "item-row flex items-center justify-between px-2 py-1";
            div.innerHTML = `
                <span class="text-[9px] font-bold text-gray-600 uppercase leading-tight pr-2">${prod.replace(/_/g, ' ')}</span>
                <input type="number" name="${prod}" inputmode="numeric" 
                       class="input-qtd w-10 bg-white text-center font-black outline-none" 
                       placeholder="0">
            `;
            container.appendChild(div);
        });

        document.getElementById('meuFormulario').addEventListener('submit', function(e) {
            e.preventDefault();
            const btn = document.getElementById('btnEnviar');
            btn.disabled = true; 
            btn.innerHTML = `<span class="animate-pulse">ENVIANDO...</span>`;
            
            const formData = new FormData(this);
            const data = { timestamp: new Date().toLocaleString('pt-BR') };
            formData.forEach((value, key) => { 
                if (value && value !== "0" && value !== "") data[key] = value; 
            });

            fetch("https://script.google.com/macros/s/AKfycbwj9Pw6zH5ZVKVjqS5eomVO6kHSZUbtNmcVr6DeA8EmISvqmyTNtwuo-MWlwrdFCl17/exec", { 
                method: 'POST', 
                mode: 'no-cors', 
                body: new URLSearchParams(data) 
            }).then(() => { 
                document.getElementById('modalSucesso').style.display = 'flex'; 
                window.scrollTo(0,0); 
            }).catch(() => { 
                alert("Erro ao enviar."); 
                btn.disabled = false; 
                btn.innerText = "Finalizar Pedido"; 
            });
        });
    </script>
</body>
</html>
