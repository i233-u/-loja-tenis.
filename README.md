<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>AbyPonp - Loja de Tênis</title>
<style>
  body { font-family: Arial, sans-serif; margin: 0; padding: 0; background: #f4f4f4; }
  header { background: #333; color: white; padding: 10px; text-align: center; }
  .container { max-width: 900px; margin: auto; padding: 20px; background: white; }
  .product { border: 1px solid #ddd; padding: 15px; margin-bottom: 15px; display: flex; align-items: center; }
  .product img { width: 120px; margin-right: 15px; }
  .product-details { flex-grow: 1; }
  .product-title { font-size: 18px; margin-bottom: 5px; }
  .product-price { color: green; font-weight: bold; margin-bottom: 10px; }
  button { background: #28a745; color: white; border: none; padding: 10px; cursor: pointer; }
  button:hover { background: #218838; }
  #cart { position: fixed; right: 10px; top: 60px; width: 250px; background: white; border: 1px solid #ccc; padding: 10px; max-height: 400px; overflow-y: auto; }
  #cart h3 { margin-top: 0; }
  #chat { position: fixed; right: 10px; bottom: 10px; width: 250px; background: white; border: 1px solid #ccc; }
  #chat-header { background: #333; color: white; padding: 5px; cursor: pointer; }
  #chat-messages { height: 150px; overflow-y: auto; padding: 5px; border-top: 1px solid #ccc; border-bottom: 1px solid #ccc; display: none; }
  #chat-input { width: calc(100% - 12px); padding: 5px; border: none; display: none; }
</style>
</head>
<body>

<header>
  <h1>AbyPonp - Loja de Tênis</h1>
</header>

<div class="container">
  <div class="product" data-id="1" data-name="Nike Air Max 270" data-price="399.99">
    <img src="https://via.placeholder.com/120?text=Nike+Air+Max+270" alt="Nike Air Max 270" />
    <div class="product-details">
      <div class="product-title">Nike Air Max 270</div>
      <div class="product-price">€399,99</div>
      <button onclick="addToCart(1)">Adicionar ao Carrinho</button>
    </div>
  </div>
  <div class="product" data-id="2" data-name="Adidas Ultraboost 22" data-price="299.99">
    <img src="https://via.placeholder.com/120?text=Adidas+Ultraboost+22" alt="Adidas Ultraboost 22" />
    <div class="product-details">
      <div class="product-title">Adidas Ultraboost 22</div>
      <div class="product-price">€299,99</div>
      <button onclick="addToCart(2)">Adicionar ao Carrinho</button>
    </div>
  </div>
  <div class="product" data-id="3" data-name="Puma RS-X³" data-price="199.99">
    <img src="https://via.placeholder.com/120?text=Puma+RS-X3" alt="Puma RS-X³" />
    <div class="product-details">
      <div class="product-title">Puma RS-X³</div>
      <div class="product-price">€199,99</div>
      <button onclick="addToCart(3)">Adicionar ao Carrinho</button>
    </div>
  </div>
  <div class="product" data-id="4" data-name="Jordan Air Retro" data-price="450.00">
    <img src="https://via.placeholder.com/120?text=Jordan+Air+Retro" alt="Jordan Air Retro" />
    <div class="product-details">
      <div class="product-title">Jordan Air Retro</div>
      <div class="product-price">€450,00</div>
      <button onclick="addToCart(4)">Adicionar ao Carrinho</button>
    </div>
  </div>
</div>

<div id="cart">
  <h3>Carrinho</h3>
  <div id="cart-items">Seu carrinho está vazio.</div>
  <div id="cart-total"></div>
  <button onclick="checkout()">Finalizar Compra</button>
</div>

<div id="chat">
  <div id="chat-header" onclick="toggleChat()">Chat de Dúvidas</div>
  <div id="chat-messages"></div>
  <input type="text" id="chat-input" placeholder="Escreva sua dúvida..." onkeydown="if(event.key==='Enter') sendMessage()" />
</div>

<script>
  const products = [
    { id: 1, name: "Nike Air Max 270", price: 399.99 },
    { id: 2, name: "Adidas Ultraboost 22", price: 299.99 },
    { id: 3, name: "Puma RS-X³", price: 199.99 },
    { id: 4, name: "Jordan Air Retro", price: 450.00 }
  ];

  let cart = [];

  function addToCart(id) {
    const product = products.find(p => p.id === id);
    if (!product) return;
    const item = cart.find(i => i.id === id);
    if (item) {
      item.qty++;
    } else {
      cart.push({ ...product, qty: 1 });
    }
    updateCart();
  }

  function updateCart() {
    const cartItems = document.getElementById('cart-items');
    const cartTotal = document.getElementById('cart-total');
    if (cart.length === 0) {
      cartItems.innerHTML = 'Seu carrinho está vazio.';
      cartTotal.innerHTML = '';
      return;
    }
    let html = '';
    let total = 0;
    cart.forEach(item => {
      html += `<div>${item.name} x ${item.qty} - €${(item.price * item.qty).toFixed(2)}</div>`;
      total += item.price * item.qty;
    });
    cartItems.innerHTML = html;
    cartTotal.innerHTML = `<strong>Total: €${total.toFixed(2)}</strong>`;
  }

  function checkout() {
    if (cart.length === 0) {
      alert('Seu carrinho está vazio.');
      return;
    }
    alert('Compra finalizada! Em breve entraremos em contato.');
    cart = [];
    updateCart();
  }

  function toggleChat() {
    const messages = document.getElementById('chat-messages');
    const input = document.getElementById('chat-input');
    if (messages.style.display === 'none' || messages.style.display === '') {
      messages.style.display = 'block';
      input.style.display = 'block';
    } else {
      messages.style.display = 'none';
      input.style.display = 'none';
    }
  }

  function sendMessage() {
    const input = document.getElementById('chat-input');
    const messages = document.getElementById('chat-messages');
    const text = input.value.trim();
    if (!text) return;
    const messageDiv = document.createElement('div');
    messageDiv.style.background = '#e0e0e0';
    messageDiv.style.margin = '5px';
    messageDiv.style.padding = '5px';
    messageDiv.style.borderRadius = '5px';
    messageDiv.textContent = 'Cliente: ' + text;
    messages.appendChild(messageDiv);
    input.value = '';

    // Resposta automática de exemplo
    setTimeout(() => {
      const reply = document.createElement('div');
      reply.style.background = '#28a745';
      reply.style.color = 'white';
      reply.style.margin = '5px';
      reply.style.padding = '5px';
      reply.style.borderRadius = '5px';
      reply.textContent = 'Suporte: Obrigado pela sua dúvida! Vamos responder em breve.';
      messages.appendChild(reply);
      messages.scrollTop = messages.scrollHeight;
    }, 1000);
    messages.scrollTop = messages.scrollHeight;
  }

  updateCart();
</script>

</body>
</html>
