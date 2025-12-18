<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Cooperativa CLAF - Pedidos</title>
    <!-- Importação do Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Courier+Prime:wght@400;700&display=swap');
        
        body {
            -webkit-tap-highlight-color: transparent;
        }

        .fonte-cupom { 
            font-family: 'Courier Prime', monospace; 
        }
        
        .cabecalho-tabela { 
            background-color: #ffff00; 
        }

        @keyframes pop { 
            0% { transform: scale(0.8); opacity: 0; } 
            100% { transform: scale(1); opacity: 1; } 
        }
        .animate-pop { 
            animation: pop 0.3s ease-out forwards; 
        }

        input[type="number"] {
            -moz-appearance: textfield;
        }
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        .sombra-mobile {
            box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen font-sans">

    <div class="max-w-md mx-auto bg-white min-h-screen shadow-lg border-x border-gray-200 flex flex-col">
        
        <!-- Cabeçalho Ajustado com Novo Nome -->
        <header class="bg-green-700 p-4 text-white text-center shadow-md">
            <h1 class="text-3xl font-black uppercase tracking-tighter italic leading-none">Cooperativa<br>CLAF</h1>
            <div class="h-1 w-16 bg-yellow-400 mx-auto mt-2"></div>
            <p class="text-[11px] font-bold opacity-90 mt-2 uppercase tracking-widest">Sistema de Entrega de Produtos</p>
        </header>

        <form id="meuFormulario" class="p-2 space-y-2 flex-grow">
            
            <!-- Identificação (Grid de 2 colunas para economizar altura) -->
            <div class="grid grid-cols-2 gap-2 bg-yellow-50/30 p-2 rounded border border-green-100 sombra-mobile">
                <div>
                    <label class="block text-[8px] font-black text-green-800 uppercase tracking-tighter">Produtor</label>
                    <input type="text" name="nome" class="w-full p-1 border border-gray-300 rounded outline-none text-xs font-bold" placeholder="Nome" required>
                </div>
                <div>
                    <label class="block text-[8px] font-black text-green-800 uppercase tracking-tighter">Data Entrega</label>
                    <input type="date" name="data" id="campoData" class="w-full p-1 border border-gray-300 rounded outline-none text-xs font-bold" required>
                </div>
            </div>

            <!-- Tabela Ultra Compacta -->
            <div class="border border-black rounded overflow-hidden sombra-mobile bg-white">
                <table class="w-full border-collapse fonte-cupom">
                    <thead>
                        <tr class="cabecalho-tabela">
                            <th class="border-b border-black px-2 py-1 text-[10px] font-black uppercase text-left w-3/4">Item</th>
                            <th class="border-b border-black px-2 py-1 text-[10px] font-black uppercase text-center w-1/4">Qtd.</th>
                        </tr>
                    </thead>
                    <tbody id="listaProdutos" class="divide-y divide-gray-100">
                        <!-- Produtos via JS -->
                    </tbody>
                </table>
            </div>

            <!-- Botão Fixo/Compacto no Fim -->
            <div class="pt-1 pb-2">
                <button type="submit" id="btnEnviar" class="w-full bg-green-600 text-white py-3 rounded-lg font-black text-sm shadow-sm uppercase flex items-center justify-center gap-2 active:bg-green-800">
                    <span>ENVIAR PEDIDO</span>
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M13 7l5 5m0 0l-5 5m5-5H6" />
                    </svg>
                </button>
            </div>
        </form>

        <footer class="text-center text-gray-300 text-[7px] py-2 uppercase font-bold tracking-widest bg-gray-50">
            --- FIM ---
        </footer>
    </div>

    <!-- Modal de Sucesso -->
    <div id="modalSucesso" class="fixed inset-0 bg-black/80 hidden items-center justify-center p-6 z-50 backdrop-blur-sm">
        <div class="bg-white p-6 rounded-2xl text-center max-w-xs w-full animate-pop border-4 border-green-500 shadow-2xl">
            <div class="w-12 h-12 bg-green-100 text-green-600 rounded-full flex items-center justify-center mx-auto mb-3 text-3xl">✓</div>
            <h2 class="text-lg font-black text-green-800 uppercase mb-1">Enviado!</h2>
            <p class="text-[10px] text-gray-500 font-bold mb-4">Seu pedido foi registrado.</p>
            <button onclick="location.reload()" class="w-full bg-green-700 text-white py-2 rounded-lg font-black uppercase text-xs">NOVO</button>
        </div>
    </div>

    <script>
        const produtos = ["Abacate", "Abacaxi", "Banana_caturra", "Banana_maça_ou_prata", "Laranja_baiana_lima", "Laranja_Comum", "Maça", "Mamão", "Manga", "Maracujá", "Melancia", "Melão", "Pera", "Pêssego", "Tangerina_poncã", "Almeirão", "Acelga", "Agrião", "Alface", "Couve_Manteiga", "Escarola", "Espinafre", "Quiabo", "Repolho", "Rúcula", "Abobrinha_verde", "Beterraba", "Brócolis", "Cenoura", "Chuchu", "Couve_flor", "Milho_verde", "Pepino", "Tomate", "Vagem", "Leite_Pasteurizado", "Pão_Caseiro", "Alho_poró", "Cebola", "Cebolinha_Verde", "Limão", "Pimentão", "Rabanete", "Salsinha", "Batata_doce", "Batata_inglesa", "Iogurte"];

        document.getElementById('campoData').value = new Date().toISOString().split('T')[0];

        const tbody = document.getElementById('listaProdutos');
        produtos.forEach((prod) => {
            const tr = document.createElement('tr');
            tr.innerHTML = `
                <td class="px-2 py-0.5 text-xs font-bold text-gray-700 uppercase leading-none">${prod.replace(/_/g, ' ')}</td>
                <td class="p-0.5 text-center">
                    <input type="number" name="${prod}" inputmode="numeric" pattern="[0-9]*" min="0" 
                           class="w-full max-w-[60px] p-1 border border-gray-100 rounded text-center text-sm font-black bg-gray-50 focus:bg-white focus:border-green-500 outline-none" 
                           placeholder="0">
                </td>
            `;
            tbody.appendChild(tr);
        });

        document.getElementById('meuFormulario').addEventListener('submit', function(e) {
            e.preventDefault();
            const URL_DO_SCRIPT = "https://script.google.com/macros/s/AKfycbwj9Pw6zH5ZVKVjqS5eomVO6kHSZUbtNmcVr6DeA8EmISvqmyTNtwuo-MWlwrdFCl17/exec"; 
            const btn = document.getElementById('btnEnviar');
            btn.disabled = true;
            btn.innerHTML = `<span class="animate-pulse">...</span>`;

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
                alert("Erro ao enviar.");
                btn.disabled = false;
                btn.innerHTML = `<span>ENVIAR PEDIDO</span>`;
            });
        });
    </script>
</body>
</html>
