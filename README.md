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

        /* Correção para visibilidade do número digitado */
        input[type="number"] {
            -moz-appearance: textfield;
            appearance: textfield;
        }

        input[type="number"]::-webkit-inner-spin-button,
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        /* Garante que o texto digitado seja preto e visível */
        input {
            color: #000 !important;
            opacity: 1 !important;
        }

        @keyframes pop {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-pop { animation: pop 0.3s ease-out forwards; }
        
        /* Sticky button adjustment */
        .btn-shadow {
            box-shadow: 0 8px 0 rgb(21, 128, 61);
        }
        .btn-shadow:active {
            box-shadow: none;
            transform: translateY(4px);
        }
    </style>
</head>
<body class="bg-gray-200 min-h-screen pb-10 font-sans">

    <div class="max-w-md mx-auto bg-white min-h-screen shadow-2xl border-x border-gray-300">
        
        <div class="bg-green-700 p-6 text-white text-center shadow-md">
            <h1 class="text-3xl font-black uppercase tracking-tighter leading-none italic">Cooperativa<br>Familiar</h1>
            <div class="mt-2 h-1 w-20 bg-yellow-400 mx-auto"></div>
            <p class="text-[10px] font-bold opacity-90 mt-2 uppercase tracking-widest">Sistema de Entrega de Produtos</p>
        </div>

        <form id="meuFormulario" class="p-4 space-y-6">
            
            <div class="space-y-4 bg-gray-50 p-4 rounded-xl border-2 border-dashed border-gray-400">
                <div>
                    <label class="block text-sm font-black text-gray-700 mb-1 uppercase italic">
                        1. Qual o seu Nome?
                    </label>
                    <input 
                        type="text" 
                        name="nome"
                        class="w-full p-4 border-2 border-gray-300 rounded-lg focus:border-green-600 outline-none text-xl font-bold shadow-sm placeholder-gray-300 text-black"
                        placeholder="Escreva aqui seu nome"
                        required
                    >
                </div>
                <div>
                    <label class="block text-sm font-black text-gray-700 mb-1 uppercase italic">
                        2. Dia da Entrega:
                    </label>
                    <input 
                        type="date" 
                        name="data"
                        id="campoData"
                        class="w-full p-4 border-2 border-gray-300 rounded-lg focus:border-green-600 outline-none text-xl font-bold shadow-sm text-black"
                        required
                    >
                </div>
            </div>

            <div class="flex items-center gap-3 py-3 px-4 bg-yellow-100 border-l-4 border-yellow-500 rounded-r-lg">
                <span class="text-2xl">✍️</span>
                <p class="text-xs font-bold text-yellow-900 uppercase">
                    Digite a quantidade ao lado do produto que vai entregar.
                </p>
            </div>

            <div class="border-2 border-black rounded-lg overflow-hidden shadow-lg bg-white">
                <table class="w-full border-collapse fonte-cupom">
                    <thead>
                        <tr class="cabecalho-tabela">
                            <th class="border-b-2 border-black p-3 text-sm font-black uppercase text-left w-2/3">PRODUTO</th>
                            <th class="border-b-2 border-black p-3 text-sm font-black uppercase text-center w-1/3">QTD.</th>
                        </tr>
                    </thead>
                    <tbody id="listaProdutos" class="divide-y divide-gray-200">
                    </tbody>
                </table>
            </div>

            <div class="sticky bottom-4 px-2 pb-4">
                <button 
                    type="submit"
                    id="btnEnviar"
                    class="w-full bg-green-600 hover:bg-green-700 text-white py-6 rounded-2xl font-black text-2xl btn-shadow transition-all transform uppercase flex items-center justify-center gap-3"
                >
                    <span>ENVIAR AGORA</span>
                    <span class="text-3xl">➔</span>
                </button>
            </div>
        </form>

        <div class="text-center text-gray-400 text-[9px] py-10 uppercase font-bold tracking-widest">
            *** FIM DO FORMULÁRIO ***<br>
            VERSÃO PARA IMPRESSÃO TÉRMICA 2024
        </div>
    </div>

    <!-- Modal Sucesso -->
    <div id="modalSucesso" class="fixed inset-0 bg-black/90 hidden items-center justify-center p-6 z-50">
        <div class="bg-white p-8 rounded-3xl text-center max-w-xs w-full border-4 border-green-500 shadow-2xl animate-pop">
            <div class="w-20 h-20 bg-green-100 text-green-600 rounded-full flex items-center justify-center mx-auto mb-4 text-5xl font-bold">
                ✓
            </div>
            <h2 class="text-2xl font-black text-green-700 uppercase mb-2">Tudo Certo!</h2>
            <p class="text-gray-600 font-bold mb-6 text-sm">O seu pedido de entrega foi recebido pela cooperativa.</p>
            <button onclick="fecharModal()" class="w-full bg-green-600 text-white py-4 rounded-xl font-black uppercase tracking-widest text-lg shadow-lg active:scale-95 transition-transform">
                CONCLUÍDO
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
            tr.className = "border-b border-gray-100 items-center hover:bg-gray-50 transition-colors";
            const idPlanilha = prod.normalize('NFD').replace(/[\u0300-\u036f]/g, "").replace(/\s+/g, '_');
            
            tr.innerHTML = `
                <td class="p-3">
                    <label class="flex items-center gap-3 cursor-pointer select-none py-2">
                        <input type="checkbox" id="check_${index}" class="w-6 h-6 rounded border-gray-300 text-green-600 focus:ring-green-500">
                        <span class="text-base font-bold text-gray-800 uppercase">${prod}</span>
                    </label>
                </td>
                <td class="p-2">
                    <input type="number" name="${idPlanilha}" min="0" inputmode="numeric"
                           class="w-full p-3 border-2 border-gray-200 rounded-lg text-center text-xl font-black bg-gray-50 focus:bg-white focus:border-green-500 outline-none text-black"
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
            
            // Link do seu Apps Script
            const URL_DO_SCRIPT = "https://script.google.com/macros/s/AKfycbwj9Pw6zH5ZVKVjqS5eomVO6kHSZUbtNmcVr6DeA8EmISvqmyTNtwuo-MWlwrdFCl17/exec";

            btn.disabled = true;
            btn.innerHTML = `<span class="animate-pulse">ENVIANDO...</span>`;

            const formData = new FormData(this);
            const params = new URLSearchParams();
            
            formData.forEach((value, key) => {
                if (value && value !== "0" && value !== "") {
                    params.append(key, value);
                }
            });

            // Adiciona timestamp se não houver
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
                btn.innerHTML = `<span>ENVIAR AGORA</span><span class="text-3xl">➔</span>`;
                document.getElementById('campoData').value = new Date().toISOString().split('T')[0];
            })
            .catch(() => {
                alert("Erro ao enviar! Verifique sua conexão.");
                btn.disabled = false;
                btn.innerHTML = `<span>ENVIAR AGORA</span><span class="text-3xl">➔</span>`;
            });
        });

        function fecharModal() {
            modal.style.display = 'none';
        }
    </script>
</body>
</html>
