<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Cooperativa Familiar - Pedidos</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Courier+Prime:wght@400;700&display=swap');

        .fonte-cupom {
            font-family: 'Courier Prime', monospace;
        }

        .cabecalho-tabela { 
            background-color: #ffff00; 
        }

        /* Redução de escala para mobile */
        input[type="number"] {
            -moz-appearance: textfield;
            appearance: textfield;
        }

        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        /* Garante visibilidade e reduz altura dos campos */
        input {
            color: #000 !important;
            opacity: 1 !important;
        }

        @keyframes pop {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-pop { animation: pop 0.3s ease-out forwards; }
        
        /* Botão mais denso */
        .btn-shadow {
            box-shadow: 0 4px 0 rgb(21, 128, 61);
        }
        .btn-shadow:active {
            box-shadow: none;
            transform: translateY(2px);
        }

        /* Ajuste de densidade das linhas da tabela */
        .item-row td {
            padding-top: 0.25rem !important;
            padding-bottom: 0.25rem !important;
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen pb-6 font-sans">

    <!-- Container com largura máxima reduzida para não esticar em telas maiores -->
    <div class="max-w-[360px] mx-auto bg-white min-h-screen shadow-xl border-x border-gray-200">
        
        <!-- Cabeçalho Compacto -->
        <div class="bg-green-700 p-4 text-white text-center shadow-sm">
            <h1 class="text-2xl font-black uppercase tracking-tighter italic">Cooperativa<br>Familiar</h1>
            <div class="mt-1 h-1 w-12 bg-yellow-400 mx-auto"></div>
            <p class="text-[8px] font-bold opacity-90 mt-1 uppercase tracking-widest">Sistema de Entrega</p>
        </div>

        <form id="meuFormulario" class="p-3 space-y-4">
            
            <!-- Campos de Identificação Menores -->
            <div class="grid grid-cols-2 gap-2 bg-gray-50 p-3 rounded-lg border border-gray-300">
                <div class="col-span-2">
                    <label class="block text-[10px] font-black text-gray-700 mb-0.5 uppercase italic">1. Nome do produtor</label>
                    <input type="text" name="nome" class="w-full p-2 border border-gray-300 rounded focus:border-green-600 outline-none text-sm font-bold text-black" placeholder="Digite o aqui" required>
                </div>
                <div class="col-span-2">
                    <label class="block text-[10px] font-black text-gray-700 mb-0.5 uppercase italic">2. Data</label>
                    <input type="date" name="data" id="campoData" class="w-full p-2 border border-gray-300 rounded focus:border-green-600 outline-none text-sm font-bold text-black" required>
                </div>
            </div>

            <!-- Instrução Compacta -->
            <div class="flex items-center gap-2 py-2 px-3 bg-yellow-50 border-l-4 border-yellow-400 rounded-r">
                <span class="text-lg">✍️</span>
                <p class="text-[9px] font-bold text-yellow-900 uppercase leading-tight">
                    Digite a quantidade ao lado do produto.
                </p>
            </div>

            <!-- Tabela Super Densa -->
            <div class="border border-black rounded overflow-hidden bg-white">
                <table class="w-full border-collapse fonte-cupom">
                    <thead>
                        <tr class="cabecalho-tabela">
                            <th class="border-b border-black p-2 text-[10px] font-black uppercase text-left w-2/3">PRODUTO</th>
                            <th class="border-b border-black p-2 text-[10px] font-black uppercase text-center w-1/3">QTD.</th>
                        </tr>
                    </thead>
                    <tbody id="listaProdutos" class="divide-y divide-gray-100">
                    </tbody>
                </table>
            </div>

            <!-- Botão de Enviar Menor e Fixo -->
            <div class="sticky bottom-2 pt-2">
                <button 
                    type="submit"
                    id="btnEnviar"
                    class="w-full bg-green-600 hover:bg-green-700 text-white py-4 rounded-xl font-black text-lg btn-shadow transition-all transform uppercase flex items-center justify-center gap-2"
                >
                    <span>ENVIAR AGORA</span>
                    <span class="text-xl">➔</span>
                </button>
            </div>
        </form>

        <div class="text-center text-gray-400 text-[8px] py-6 uppercase font-bold tracking-widest">
            *** FIM DO FORMULÁRIO ***
        </div>
    </div>

    <!-- Modal Sucesso -->
    <div id="modalSucesso" class="fixed inset-0 bg-black/80 hidden items-center justify-center p-4 z-50">
        <div class="bg-white p-6 rounded-2xl text-center max-w-[280px] w-full border-4 border-green-500 shadow-2xl animate-pop">
            <div class="w-14 h-14 bg-green-100 text-green-600 rounded-full flex items-center justify-center mx-auto mb-3 text-3xl font-bold">
                ✓
            </div>
            <h2 class="text-xl font-black text-green-700 uppercase mb-1">Enviado!</h2>
            <p class="text-gray-600 font-bold mb-4 text-xs italic text-center">Pedido recebido com sucesso.</p>
            <button onclick="location.reload()" class="w-full bg-green-600 text-white py-3 rounded-lg font-black uppercase text-sm">
                FECHAR
            </button>
        </div>
    </div>

    <script>
        const produtos = [
            "Abacate", "Abacaxi", "Banana caturra", "Banana maçã ou prata", "Laranja baiana lima", 
            "Laranja Comum", "Maçã", "Mamão", "Manga", "Maracujá", "Melancia", "Melão", "Pera", 
            "Pêssego", "Tangerina poncã", "Almeirão", "Acelga", "Agrião", 
            "Alface", "Couve Manteiga", "Escarola", "Espinafre", "Quiabo", "Repolho", 
            "Rúcula", "Abobrinha verde", "Beterraba", "Brócolis", "Cenoura", "Chuchu", 
            "Couve flor", "Milho verde", "Pepino", "Tomate", "Vagem", "Leite Pasteurizado", 
            "Pão Caseiro", "Alho poró", "Cebola", "Cebolinha Verde", "Limão", "Pimentão", 
            "Rabanete", "Salsinha", "Batata doce", "Batata inglesa", "Iogurte"
        ];

        document.getElementById('campoData').value = new Date().toISOString().split('T')[0];

        const tbody = document.getElementById('listaProdutos');
        produtos.forEach((prod, index) => {
            const tr = document.createElement('tr');
            tr.className = "item-row border-b border-gray-100 items-center";
            const idPlanilha = prod.normalize('NFD').replace(/[\u0300-\u036f]/g, "").replace(/\s+/g, '_');
            
            tr.innerHTML = `
                <td class="p-2">
                    <label class="flex items-center gap-2 cursor-pointer select-none">
                        <input type="checkbox" id="check_${index}" class="w-4 h-4 rounded border-gray-300 text-green-600">
                        <span class="text-[11px] font-bold text-gray-800 uppercase tracking-tighter">${prod}</span>
                    </label>
                </td>
                <td class="p-1">
                    <input type="number" name="${idPlanilha}" min="0" inputmode="numeric"
                           class="w-full p-1.5 border border-gray-200 rounded text-center text-sm font-black bg-gray-50 focus:bg-white focus:border-green-500 outline-none text-black"
                           placeholder="0"
                           oninput="document.getElementById('check_${index}').checked = (this.value > 0)">
                </td>
            `;
            tbody.appendChild(tr);
        });

        const form = document.getElementById('meuFormulario');
        const modal = document.getElementById('modalSucesso');
        const btn = document.getElementById('btnEnviar');

        form.addEventListener('submit', function(e) {
            e.preventDefault();
            const URL_DO_SCRIPT = "https://script.google.com/macros/s/AKfycbwj9Pw6zH5ZVKVjqS5eomVO6kHSZUbtNmcVr6DeA8EmISvqmyTNtwuo-MWlwrdFCl17/exec";

            btn.disabled = true;
            btn.innerHTML = `<span class="animate-pulse text-sm">ENVIANDO...</span>`;

            const formData = new FormData(this);
            const params = new URLSearchParams();
            
            formData.forEach((value, key) => {
                if (value && value !== "0" && value !== "") {
                    params.append(key, value);
                }
            });

            if(!params.has('timestamp')) params.append('timestamp', new Date().toLocaleString('pt-BR'));

            fetch(URL_DO_SCRIPT, {
                method: 'POST',
                mode: 'no-cors',
                body: params
            })
            .then(() => {
                modal.style.display = 'flex';
                form.reset();
                btn.disabled = false;
                btn.innerHTML = `<span>ENVIAR AGORA</span><span class="text-xl">➔</span>`;
            })
            .catch(() => {
                alert("Erro ao enviar!");
                btn.disabled = false;
                btn.innerHTML = `<span>ENVIAR AGORA</span>`;
            });
        });

        function fecharModal() {
            modal.style.display = 'none';
        }
    </script>
</body>
</html>
