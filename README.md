<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cardápio - Cozinha Criativa</title>
    <style>
        :root {
            --roxo: #a64d92;
            --amarelo: #fbb034;
            --branco: #ffffff;
            --cinza-claro: #f4f4f4;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--cinza-claro);
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            padding-bottom: 100px; /* Espaço para o botão fixo */
        }

        .cardapio-container {
            background-color: var(--branco);
            width: 100%;
            max-width: 400px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            overflow: hidden;
            text-align: center;
        }

        .header {
            background-color: var(--roxo);
            padding: 30px 20px;
            color: var(--amarelo);
        }

        .logo {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            border: 4px solid var(--amarelo);
            margin-bottom: 15px;
            object-fit: cover;
        }

        h1 { margin: 10px 0 5px; font-size: 24px; color: var(--amarelo); }
        .subtitle { font-style: italic; font-size: 14px; color: white; margin-bottom: 10px; }

        .menu-items { padding: 20px; }

        .item {
            background: white;
            border: 1px solid #ddd;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .info { text-align: left; }
        .gramas { display: block; font-weight: bold; color: #555; font-size: 18px; }
        .preco { color: var(--roxo); font-weight: 900; font-size: 20px; }

        .btn-add {
            background-color: var(--amarelo);
            border: none;
            color: #333;
            padding: 10px 15px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
        }

        .btn-add:active { transform: scale(0.9); }

        /* Estilo do Carrinho Fixo */
        .cart-footer {
            position: fixed;
            bottom: 20px;
            width: 90%;
            max-width: 380px;
            background-color: var(--roxo);
            color: white;
            padding: 15px;
            border-radius: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 -5px 15px rgba(0,0,0,0.2);
            cursor: pointer;
            z-index: 100;
        }

        /* Modal do Carrinho */
        #modal-carrinho {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.7);
            z-index: 200;
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background: white;
            width: 90%;
            max-width: 350px;
            padding: 20px;
            border-radius: 15px;
            text-align: left;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            border-bottom: 1px solid #eee;
            padding-bottom: 5px;
        }

        .btn-finalizar {
            background-color: #25d366;
            color: white;
            border: none;
            width: 100%;
            padding: 15px;
            border-radius: 8px;
            font-weight: bold;
            font-size: 16px;
            margin-top: 15px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<div class="cardapio-container">
    <div class="header">
        <img src="WhatsApp Image 2026-02-12 at 18.42.15.jpeg" alt="Logo" class="logo">
        <h1>Marmitas Fitness</h1>
        <p class="subtitle">Feito com Amor - Fátima Silva</p>
    </div>

    <div class="menu-items">
        <div class="item">
            <div class="info">
                <span class="gramas">Marmita 250g</span>
                <span class="preco">R$ 20,60</span>
            </div>
            <button class="btn-add" onclick="addAoCarrinho('250g', 20.60)">Adicionar +</button>
        </div>

        <div class="item">
            <div class="info">
                <span class="gramas">Marmita 350g</span>
                <span class="preco">R$ 22,60</span>
            </div>
            <button class="btn-add" onclick="addAoCarrinho('350g', 22.60)">Adicionar +</button>
        </div>

        <div class="item">
            <div class="info">
                <span class="gramas">Marmita 400g</span>
                <span class="preco">R$ 24,60</span>
            </div>
            <button class="btn-add" onclick="addAoCarrinho('400g', 24.60)">Adicionar +</button>
        </div>

        <div class="item">
            <div class="info">
                <span class="gramas">Marmita 500g</span>
                <span class="preco">R$ 26,65</span>
            </div>
            <button class="btn-add" onclick="addAoCarrinho('500g', 26.65)">Adicionar +</button>
        </div>
    </div>
</div>

<div class="cart-footer" onclick="abrirModal()">
    <span>🛒 Itens: <strong id="cart-count">0</strong></span>
    <span>Ver Carrinho: <strong id="cart-total">R$ 0,00</strong></span>
</div>

<div id="modal-carrinho">
    <div class="modal-content">
        <h3>Seu Pedido:</h3>
        <div id="lista-itens"></div>
        <hr>
        <strong>Total: <span id="modal-total">R$ 0,00</span></strong>
        <button class="btn-finalizar" onclick="enviarWhatsApp()">Enviar Pedido para WhatsApp</button>
        <button onclick="fecharModal()" style="border:none; background:none; color:red; width:100%; margin-top:10px; cursor:pointer;">Continuar Comprando</button>
    </div>
</div>

<script>
    let carrinho = [];
    let total = 0;

    function addAoCarrinho(nome, preco) {
        carrinho.push({nome, preco});
        total += preco;
        atualizarInterface();
    }

    function atualizarInterface() {
        document.getElementById('cart-count').innerText = carrinho.length;
        document.getElementById('cart-total').innerText = `R$ ${total.toFixed(2)}`;
        document.getElementById('modal-total').innerText = `R$ ${total.toFixed(2)}`;
    }

    function abrirModal() {
        if(carrinho.length === 0) return alert("Adicione itens primeiro!");
        const lista = document.getElementById('lista-itens');
        lista.innerHTML = "";
        carrinho.forEach(item => {
            lista.innerHTML += `<div class="cart-item"><span>${item.nome}</span> <span>R$ ${item.preco.toFixed(2)}</span></div>`;
        });
        document.getElementById('modal-carrinho').style.display = "flex";
    }

    function fecharModal() {
        document.getElementById('modal-carrinho').style.display = "none";
    }

    function enviarWhatsApp() {
        let mensagem = "Olá Fátima! Gostaria de fazer um pedido:\n\n";
        carrinho.forEach(item => {
            mensagem += `• Marmita ${item.nome} - R$ ${item.preco.toFixed(2)}\n`;
        });
        mensagem += `\n*Total: R$ ${total.toFixed(2)}*`;
        
        const fone = "5511982168054";
        const link = `https://wa.me/${fone}?text=${encodeURIComponent(mensagem)}`;
        window.open(link, '_blank');
    }
</script>

</body>
</html>
