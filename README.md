<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Rotisería El Tachi</title>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet"/>
  <style>
    body { font-family: 'Roboto', sans-serif; background: #f8f9fa; margin: 0; padding: 0; color: #333; }
    header { background: #003366; color: white; text-align: center; padding: 1rem; }
    header img { height: 60px; }
    main { max-width: 800px; margin: auto; padding: 1rem; }
    h2 { color: #003366; border-bottom: 2px solid #ffcc00; padding-bottom: 0.3rem; margin-top: 1.5rem; }
    .producto { border-bottom: 1px solid #ccc; padding: 0.5rem 0; display: flex; justify-content: space-between; align-items: center; }
    .producto label { flex-grow: 1; margin-left: 0.5rem; }
    .producto input[type="checkbox"] { transform: scale(1.2); }
    .total, .formulario { margin-top: 1.5rem; }
    input, select { width: 100%; padding: 0.6rem; margin-top: 0.3rem; margin-bottom: 1rem; border: 1px solid #ccc; border-radius: 4px; }
    button { background: #ffcc00; color: #003366; font-weight: bold; border: none; padding: 0.8rem 1.2rem; cursor: pointer; width: 100%; }
    button:hover { background: #e6b800; }
  </style>
</head>
<body>

  <header>
    <img src="https://raw.githubusercontent.com/ludmilasolutions/el-tachi/main/logo.png" alt="Rotisería El Tachi">
    <h1>Rotisería El Tachi</h1>
  </header>

  <main>
    <!-- Categorías y Productos -->
    <div id="menu">
      <h2>Pizzas</h2>
      <div class="producto"><input type="checkbox" data-nombre="Pizza Muzarella" data-precio="1800"><label>Pizza Muzarella - $1800</label></div>
      <div class="producto"><input type="checkbox" data-nombre="Pizza Napolitana" data-precio="2000"><label>Pizza Napolitana - $2000</label></div>

      <h2>Hamburguesas</h2>
      <div class="producto"><input type="checkbox" data-nombre="Hamburguesa Simple" data-precio="1200"><label>Hamburguesa Simple - $1200</label></div>
      <div class="producto"><input type="checkbox" data-nombre="Hamburguesa Doble" data-precio="1500"><label>Hamburguesa Doble - $1500</label></div>

      <h2>Empanadas</h2>
      <div class="producto"><input type="checkbox" data-nombre="Empanada de Carne" data-precio="350"><label>Empanada de Carne - $350</label></div>
      <div class="producto"><input type="checkbox" data-nombre="Empanada de Pollo" data-precio="350"><label>Empanada de Pollo - $350</label></div>

      <h2>Papas Fritas</h2>
      <div class="producto"><input type="checkbox" data-nombre="Papas Clásicas" data-precio="900"><label>Papas Clásicas - $900</label></div>
      <div class="producto"><input type="checkbox" data-nombre="Papas con Cheddar" data-precio="1300"><label>Papas con Cheddar - $1300</label></div>

      <h2>Menú al Plato</h2>
      <div class="producto"><input type="checkbox" data-nombre="Milanesa con Papas" data-precio="2500"><label>Milanesa con Papas - $2500</label></div>

      <h2>Bebidas</h2>
      <div class="producto"><input type="checkbox" data-nombre="Coca-Cola 500ml" data-precio="700"><label>Coca-Cola 500ml - $700</label></div>
      <div class="producto"><input type="checkbox" data-nombre="Agua sin gas" data-precio="500"><label>Agua sin gas - $500</label></div>
    </div>

    <!-- Formulario del pedido -->
    <div class="formulario">
      <h2>Datos del Cliente</h2>
      <label>Nombre:</label>
      <input type="text" id="nombre" required>
      <label>Dirección:</label>
      <input type="text" id="direccion" required>
      <label>Método:</label>
      <select id="metodo">
        <option value="retiro">Retiro en el local</option>
        <option value="envio">Envío a domicilio (+$1000)</option>
      </select>
    </div>

    <!-- Total y botón de pedido -->
    <div class="total">
      <h2>Total: $<span id="total">0</span></h2>
    </div>
    <button onclick="enviarPedido()">Hacer pedido por WhatsApp</button>

  </main>

  <script>
    const checkboxes = document.querySelectorAll('input[type="checkbox"]');
    const totalSpan = document.getElementById('total');

    function actualizarTotal() {
      let total = 0;
      checkboxes.forEach(cb => {
        if (cb.checked) total += parseInt(cb.dataset.precio);
      });
      if (document.getElementById('metodo').value === 'envio') total += 1000;
      totalSpan.textContent = total;
    }

    checkboxes.forEach(cb => cb.addEventListener('change', actualizarTotal));
    document.getElementById('metodo').addEventListener('change', actualizarTotal);

    function enviarPedido() {
      const nombre = document.getElementById('nombre').value.trim();
      const direccion = document.getElementById('direccion').value.trim();
      const metodo = document.getElementById('metodo').value;

      if (!nombre || !direccion) {
        alert("Por favor, completá tus datos.");
        return;
      }

      let mensaje = `Hola, soy ${nombre}. Quiero hacer un pedido:\n`;
      checkboxes.forEach(cb => {
        if (cb.checked) {
          mensaje += `• ${cb.dataset.nombre} - $${cb.dataset.precio}\n`;
        }
      });

      if (metodo === 'envio') {
        mensaje += `\nMétodo: Envío a domicilio\nDirección: ${direccion}\n(+ $1000 de envío)`;
      } else {
        mensaje += `\nMétodo: Retiro en el local`;
      }

      mensaje += `\nTotal: $${totalSpan.textContent}`;
      const url = `https://wa.me/3415923882?text=${encodeURIComponent(mensaje)}`;
      window.open(url, '_blank');
    }
  </script>
</body>
</html>
