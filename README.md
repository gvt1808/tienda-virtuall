<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ericaldos - Sopa de Mote</title>

<style>
:root{
  --primary:#ff6b00;
  --primary-dark:#e85d00;
  --accent:#00c853;
  --bg:#fff7ed;
  --card:#ffffff;
  --text:#1f2937;
  --muted:#6b7280;
}

*{box-sizing:border-box}

body{
  font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial;
  margin:0;
  background:var(--bg);
  color:var(--text);
  font-size:18px; /* BASE MÁS GRANDE */
}

/* HEADER */
header{
  background:linear-gradient(135deg,var(--primary),var(--primary-dark));
  color:white;
  padding:22px;
  box-shadow:0 6px 20px rgba(0,0,0,.2);
}

.brand h1{
  margin:0;
  font-size:32px; /* MÁS GRANDE */
}

.brand p{
  margin:4px 0 0;
  font-size:16px;
}

/* MAIN */
.main{
  max-width:1100px;
  margin:20px auto;
  padding:0 16px;
  display:grid;
  grid-template-columns:1fr 320px;
  gap:18px;
}

/* PRODUCTO */
.product{
  background:var(--card);
  border-radius:18px;
  box-shadow:0 12px 30px rgba(0,0,0,.1);
  padding:22px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  gap:14px;
}

.product .info h2{
  margin:0 0 10px;
  font-size:26px; /* MÁS GRANDE */
}

.product .desc{
  font-size:16px; /* MÁS GRANDE */
  line-height:1.6;
}

.product .price{
  font-size:26px;
  font-weight:700;
  color:var(--primary);
  margin-top:10px;
}

/* BOTÓN */
.btn-add{
  background:var(--accent);
  color:#fff;
  padding:14px 18px;
  border:none;
  border-radius:12px;
  cursor:pointer;
  font-weight:700;
  font-size:16px; /* MÁS GRANDE */
}

/* CARRITO */
.cart{
  background:var(--card);
  border-radius:18px;
  box-shadow:0 12px 30px rgba(0,0,0,.1);
  padding:18px;
  position:sticky;
  top:12px;
}

.cart h3{
  margin:0 0 12px;
  font-size:22px; /* MÁS GRANDE */
}

.cart-item{
  padding:12px 0;
  border-bottom:1px solid #eee;
  font-size:16px;
}

.row{
  display:flex;
  justify-content:space-between;
}

.qty-controls{
  display:flex;
  gap:8px;
  margin-top:6px;
}

.qty-controls button{
  border:none;
  padding:8px 12px;
  border-radius:8px;
  cursor:pointer;
  font-weight:bold;
  font-size:16px;
}

.del{
  background:#ff3b30;
  color:white;
}

.total{
  display:flex;
  justify-content:space-between;
  margin-top:14px;
  font-weight:bold;
  font-size:20px; /* MÁS GRANDE */
}

.checkout{
  width:100%;
  margin-top:14px;
  padding:16px;
  background:var(--accent);
  color:white;
  border:none;
  border-radius:12px;
  cursor:pointer;
  font-weight:700;
  font-size:18px; /* MÁS GRANDE */
}

@media (max-width:800px){
  .main{grid-template-columns:1fr}
}
</style>
</head>

<body>

<header>
  <div class="header-wrap">
    <div class="brand">
      <h1>Ericaldos</h1>
      <p>Sabor casero que enamora 🍲</p>
    </div>
  </div>
</header>

<div class="main">

  <div>
    <div class="product">
      <div class="info">
        <h2>Sopa de Mote</h2>
        <div class="desc">
          Disfruta de nuestra exquisita sopa de mote, preparada con una receta tradicional
          que resalta lo mejor de la cocina casera. Cocinada lentamente para lograr un
          sabor intenso y reconfortante, cada cucharada combina ingredientes frescos,
          mote suave y un caldo delicioso que te llenará de energía. Ideal para cualquier
          momento del día, perfecta para combatir el frío o simplemente darte un gusto
          con un plato nutritivo, sabroso y hecho con dedicación.
        </div>
        <div class="price">S/ 17</div>
      </div>
      <div class="actions">
        <button class="btn-add" onclick="agregar('Sopa de Mote')">
          Agregar
        </button>
      </div>
    </div>
  </div>

  <aside class="cart">
    <h3>🛒 Carrito</h3>
    <div id="carrito"></div>
    <div class="total"><span>Total</span><span id="total">S/ 0</span></div>
    <button class="checkout" onclick="pedir()">Pedir por WhatsApp</button>
  </aside>

</div>

<script>
let carrito = JSON.parse(localStorage.getItem('carrito')) || [];

function agregar(nombre){
  let item = carrito.find(p=>p.nombre===nombre);
  if(item){item.cantidad++}else{carrito.push({nombre,precio:17,cantidad:1})}
  actualizar();
}

function cambiarCantidad(index,delta){
  carrito[index].cantidad += delta;
  if(carrito[index].cantidad<=0){carrito.splice(index,1)}
  actualizar();
}

function eliminar(index){
  carrito.splice(index,1);
  actualizar();
}

function actualizar(){
  localStorage.setItem('carrito',JSON.stringify(carrito));
  let cont = document.getElementById('carrito');
  cont.innerHTML='';
  let total=0;

  carrito.forEach((item,i)=>{
    const subtotal = item.precio*item.cantidad;
    cont.innerHTML += `
      <div class="cart-item">
        <div class="row">
          <strong>${item.nombre}</strong>
          <span>S/ ${subtotal}</span>
        </div>
        <div class="qty-controls">
          <button onclick="cambiarCantidad(${i},1)">+</button>
          <button onclick="cambiarCantidad(${i},-1)">-</button>
          <button class="del" onclick="eliminar(${i})">X</button>
        </div>
      </div>`;
    total += subtotal;
  });

  document.getElementById('total').innerText=`S/ ${total}`;
}

function pedir(){
  if(carrito.length===0){alert('Agrega productos');return}
  let telefono='51953874298';
  let msg='Hola, quiero pedir:%0A';
  let total=0;

  carrito.forEach(item=>{
    const subtotal = item.precio*item.cantidad;
    msg+=`- ${item.nombre} x${item.cantidad} (S/ ${subtotal})%0A`;
    total+=subtotal;
  });

  msg+=`%0ATotal: S/ ${total}`;
  window.open(`https://wa.me/${telefono}?text=${msg}`,'_blank');
}

actualizar();
</script>

</body>
</html>
