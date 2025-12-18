<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Cooperativa CLAF - Pedidos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        body { -webkit-tap-highlight-color: transparent; font-family: 'Inter', sans-serif; background-color: #f3f4f6; }
        input::-webkit-outer-spin-button, input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
        input[type=number] { -moz-appearance: textfield; }
        .item-row:nth-child(even) { background-color: #f9fafb; }
        .input-qtd { height: 22px !important; padding: 0 !important; border-radius: 4px !important; }
    </style>
</head>
<body class="min-h-screen">
    <div class="max-w-md mx-auto bg-white min-h-screen shadow-sm flex flex-col">
        <header class="bg-green-700 pt-2 pb-1.5 px-3 text-center border-b-2 border-yellow-400">
            <h1 class="text-white text-lg font-black uppercase tracking-tighter italic">COOPERATIVA <span class="text-yellow-300">CLAF</span></h1>
            <p class="text-green-100 text-[7px] uppercase tracking-[0.1em] font-bold">Sistema de Entrega</p>
        </header>

        <form id="meuFormulario" class="flex-grow p-2 space-y-1.5">
            <div class="bg-white border border-gray-200 rounded-lg p-2 shadow-sm space-y-1.5">
                <div>
                    <label class="block text-[9px] font-black text-gray-700 uppercase mb-0.5 ml-1">Nome do produtor</label>
                    <input type="text" name="nome" required class="w-full bg-gray-50 border border-gray-200 rounded py-1 px-2 text-xs font-bold outline-none" placeholder="Seu nome">
                </div>
                <div>
                    <label class="block text-[9px] font-black text-gray-700 uppercase mb-0.5 ml-1">Data da Entrega</label>
                    <input type="date" name="data" id="campoData" required class="w-full bg-gray-50 border border-gray-200 rounded py-1 px-2 text-xs font-bold outline-none">
                </div>
            </div>

            <div class="bg-white border border-gray-200 rounded-lg overflow-hidden shadow-sm">
                <div class="bg-gray-100 px-2 py-0.5 border-b border-gray-200 flex justify-between items-center text-[7px] font-black text-gray-500 uppercase">
                    <span>Produto</span><span class="pr-3">Qtd</span>
                </div>
                <div id="listaProdutos" class="divide-y divide-gray-100"></div>
            </div>

            <button type="submit" id="btnEnviar" class="w-full bg-green-600 text-white py-2.5 rounded-lg font-black text-[10px] uppercase tracking-widest flex items-center justify-center gap-2">
                FINALIZAR PEDIDO
            </button>
        </form>
        <footer class="py-1.5 text-center text-[7px] font-black text-gray-300 uppercase italic">Cooperativa CLAF</footer>
    </div>

    <div id="modalSucesso" class="fixed inset-0 bg-green-900/90 hidden items-center justify-center p-4 z-50 backdrop-blur-sm">
        <div class="bg-white p-6 rounded-2xl text-center max-w-xs w-full shadow-2xl border-t-4 border-green-500">
            <h2 class="text-lg font-black text-gray-800 mb-4">ENVIADO!</h2>
            <button onclick="location.reload()" class="w-full bg-gray-900 text-white py-2 rounded-lg font-black text-[10px]">NOVO PEDIDO</button>
        </div>
    </div>

    <script>
        const produtos = ["Abacate", "Abacaxi", "Banana_caturra", "Banana_maça_ou_prata", "Laranja_baiana_lima", "Laranja_Comum", "Maça", "Mamão", "Manga", "Maracujá", "Melancia", "Melão", "Pera", "Pêssego", "Tangerina_poncã", "Almeirão", "Acelga", "Agrião", "Alface", "Couve_Manteiga", "Escarola", "Espinafre", "Quiabo", "Repolho", "Rúcula", "Abobrinha_verde", "Beterraba", "Brócolis", "Cenoura", "Chuchu", "Couve_flor", "Milho_verde", "Pepino", "Tomate", "Vagem", "Leite_Pasteurizado", "Pão_Caseiro", "Alho_poró", "Cebola", "Cebolinha_Verde", "Limão", "Pimentão", "Rabanete", "Salsinha", "Batata_doce", "Batata_inglesa", "Iogurte"];
        document.getElementById('campoData').value = new Date().toISOString().split('T')[0];
        const container = document.getElementById('listaProdutos');
        produtos.forEach((prod) => {
            const div = document.createElement('div');
            div.className = "item-row flex items-center justify-between px-2 py-0.5";
            div.innerHTML = `<span class="text-[8px] font-bold text-gray-600 uppercase tracking-tight">${prod.replace(/_/g, ' ')}</span><input type="number" name="${prod}" inputmode="numeric" class="input-qtd w-10 bg-white border border-gray-300 text-center text-[9px] font-black outline-none" placeholder="0">`;
            container.appendChild(div);
        });

        document.getElementById('meuFormulario').addEventListener('submit', function(e) {
            e.preventDefault();
            const btn = document.getElementById('btnEnviar');
            btn.disabled = true; btn.innerText = "...";
            const formData = new FormData(this);
            const data = { timestamp: new Date().toLocaleString('pt-BR') };
            formData.forEach((value, key) => { if (value && value !== "0" && value !== "") data[key] = value; });
            fetch("https://script.google.com/macros/s/AKfycbwj9Pw6zH5ZVKVjqS5eomVO6kHSZUbtNmcVr6DeA8EmISvqmyTNtwuo-MWlwrdFCl17/exec", { method: 'POST', mode: 'no-cors', body: new URLSearchParams(data) })
            .then(() => { document.getElementById('modalSucesso').style.display = 'flex'; window.scrollTo(0,0); })
            .catch(() => { alert("Erro."); btn.disabled = false; btn.innerText = "FINALIZAR"; });
        });
    </script>
</body>
</html>
