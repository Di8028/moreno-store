
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MORENO STORE</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f5f5f5;color:#171717}
header{background:#111;color:white;padding:22px 16px;text-align:center}
header h1{margin:0;font-size:27px;letter-spacing:3px}
header p{color:#bbb;font-size:12px;letter-spacing:2px}
nav{display:flex;justify-content:center;gap:20px;padding:14px;background:white}
nav a{color:#222;text-decoration:none;font-size:13px}
.hero{background:#191919;color:white;text-align:center;padding:48px 18px}
.hero h2{font-size:30px;margin:0 0 12px}
.hero p{color:#ccc;font-size:14px}
.btn{display:inline-block;background:white;color:#111;padding:13px 22px;text-decoration:none;border:0;border-radius:5px;font-weight:bold;cursor:pointer}
main{max-width:1000px;margin:auto;padding:25px 15px}
.search{width:100%;padding:14px;border:1px solid #ddd;border-radius:7px;font-size:15px;margin-bottom:22px}
h2.section{font-size:22px}
.categories{display:flex;gap:9px;overflow:auto;margin-bottom:22px}
.categories button{padding:10px 15px;border:1px solid #ddd;background:white;border-radius:20px;white-space:nowrap}
.categories button.active{background:#111;color:white}
.products{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:13px}
.card{background:white;border-radius:9px;overflow:hidden;border:1px solid #eee}
.photo{height:125px;background:#eaeaea;display:grid;place-items:center;font-size:55px}
.info{padding:13px}
.info h3{font-size:15px;margin:0 0 8px}
.price{font-weight:bold;font-size:17px}
.info button{width:100%;margin-top:12px;background:#111;color:white;border:0;padding:12px;border-radius:5px}
.cart{background:white;padding:17px;margin-top:25px;border-radius:9px}
.cart button{background:#111;color:white;border:0;padding:13px;border-radius:5px;width:100%;margin-top:12px}
footer{background:#111;color:white;text-align:center;padding:25px;font-size:13px;margin-top:30px}
@media(min-width:650px){.products{grid-template-columns:repeat(3,1fr)}}
</style>
</head>
<body>
<header>
<h1>MORENO STORE</h1>
<p>TECNOLOGÍA · GADGETS · ACCESORIOS</p>
</header>
<nav>
<a href="#inicio">Inicio</a>
<a href="#productos">Productos</a>
<a href="#carrito">Mi carrito</a>
</nav>
<section class="hero" id="inicio">
<h2>Bienvenido a Moreno Store</h2>
<p>Encuentra tus productos favoritos en un solo lugar.</p>
<a class="btn" href="#productos">VER CATÁLOGO</a>
</section>
<main>
<h2 class="section" id="productos">Nuestros productos</h2>
<input class="search" id="search" placeholder="Buscar productos...">
<div class="categories">
<button class="active" onclick="filter('Todos',this)">Todos</button>
<button onclick="filter('Audio',this)">Audio</button>
<button onclick="filter('Accesorios',this)">Accesorios</button>
<button onclick="filter('Tecnología',this)">Tecnología</button>
</div>
<div class="products" id="products"></div>
<section class="cart" id="carrito">
<h2>🛒 Mi carrito</h2>
<div id="items">Tu carrito está vacío.</div>
<p><b>Total: $<span id="total">0</span></b></p>
<button onclick="checkout()">REALIZAR PEDIDO POR WHATSAPP</button>
</section>
</main>
<footer>
<b>MORENO STORE</b>
<p>Gracias por confiar en nosotros.</p>
<p>© 2026 Moreno Store</p>
</footer>
<script>
const WHATSAPP_NUMBER = "573XXXXXXXXX";
const products = [
{name:"Audífonos inalámbricos",price:89900,category:"Audio",emoji:"🎧"},
{name:"Parlante Bluetooth",price:69900,category:"Audio",emoji:"🔊"},
{name:"Cargador rápido",price:39900,category:"Accesorios",emoji:"🔌"},
{name:"Soporte para celular",price:29900,category:"Accesorios",emoji:"📱"},
{name:"Smartwatch",price:119900,category:"Tecnología",emoji:"⌚"},
{name:"Teclado inalámbrico",price:79900,category:"Tecnología",emoji:"⌨️"}
];
let cart=[];
let category="Todos";
const money=n=>n.toLocaleString("es-CO");
function render(){
const term=document.getElementById("search").value.toLowerCase();
document.getElementById("products").innerHTML=products.filter(p=>(category==="Todos"||p.category===category)&&p.name.toLowerCase().includes(term)).map(p=>`
<div class="card"><div class="photo">${p.emoji}</div><div class="info">
<h3>${p.name}</h3><div class="price">$${money(p.price)}</div>
<button onclick="add('${p.name}')">Agregar al carrito</button>
</div></div>`).join("");
}
function filter(c,b){
category=c;
document.querySelectorAll(".categories button").forEach(x=>x.classList.remove("active"));
b.classList.add("active");render();
}
function add(name){cart.push(name);renderCart();}
function renderCart(){
document.getElementById("items").innerHTML=cart.length?cart.map((n,i)=>`<p>${n} <button style="width:auto;padding:5px 9px" onclick="removeItem(${i})">Quitar</button></p>`).join(""):"Tu carrito está vacío.";
document.getElementById("total").textContent=money(cart.reduce((s,n)=>s+products.find(p=>p.name===n).price,0));
}
function removeItem(i){cart.splice(i,1);renderCart();}
function checkout(){
if(!cart.length){alert("Agrega productos primero.");return;}
if(WHATSAPP_NUMBER.includes("X")){alert("Primero debes configurar tu número de WhatsApp en el código.");return;}
const total=cart.reduce((s,n)=>s+products.find(p=>p.name===n).price,0);
const msg="Hola, quiero realizar este pedido en Moreno Store:\n"+cart.join("\n")+"\nTotal: $"+money(total);
window.open("https://wa.me/"+WHATSAPP_NUMBER+"?text="+encodeURIComponent(msg),"_blank");
}
document.getElementById("search").addEventListener("input",render);
render();
</script>
</body>
</html>
