
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Porsche Sales Dashboard</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{font-family:Arial;background:#0d0d0d;color:white;margin:0}
.sidebar{width:250px;position:fixed;height:100%;background:#151515;padding:20px}
.main{margin-left:270px;padding:20px}
.kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:15px}
.card{background:#1d1d1d;padding:20px;border-radius:15px}
select{width:100%;padding:10px;margin:10px 0;background:#222;color:white}
canvas{background:#1d1d1d;border-radius:15px;padding:15px;margin-top:20px}
h1{color:#b8963c}
</style>
</head>
<body>
<div class="sidebar">
<h2>Porsche</h2>
<select id="model"></select>
<select id="city"></select>
<select id="status"></select>
</div>

<div class="main">
<h1>Porsche Sales Analytics</h1>

<div class="kpis">
<div class="card"><h3>Total Vendas</h3><div id="total"></div></div>
<div class="card"><h3>Faturamento</h3><div id="revenue"></div></div>
<div class="card"><h3>Ticket Médio</h3><div id="ticket"></div></div>
<div class="card"><h3>Modelo Líder</h3><div id="leader"></div></div>
</div>

<canvas id="models"></canvas>
<canvas id="statusChart"></canvas>

<div class="card">
<h3>Insight Automático</h3>
<p id="insight"></p>
</div>

</div>

<script>
const data=[{"PorscheModelSanitized": "718 Cayman", "CitySanitized": "Boston", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 79500.0}, {"PorscheModelSanitized": "911 Turbo S", "CitySanitized": "Seattle", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 235000.0}, {"PorscheModelSanitized": "Cayenne Coupe", "CitySanitized": "Austin", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 112750.0}, {"PorscheModelSanitized": "Macan S", "CitySanitized": "Denver", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 68900.0}, {"PorscheModelSanitized": "Taycan 4S", "CitySanitized": "Los Angeles", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 121000.0}, {"PorscheModelSanitized": "Panamera 4", "CitySanitized": "Miami", "DeliveryStatusSanitized": "Cancelled", "SalesPriceSanitized": 104500.0}, {"PorscheModelSanitized": "911 Carrera S", "CitySanitized": "New York", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 96300.0}, {"PorscheModelSanitized": "Cayenne E-Hybrid", "CitySanitized": "San Diego", "DeliveryStatusSanitized": "Pending Approval", "SalesPriceSanitized": 89750.0}, {"PorscheModelSanitized": "718 Boxster", "CitySanitized": "Chicago", "DeliveryStatusSanitized": "Shipped", "SalesPriceSanitized": 73500.0}, {"PorscheModelSanitized": "Macan GTS", "CitySanitized": "Phoenix", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 95000.0}, {"PorscheModelSanitized": "Taycan Turbo", "CitySanitized": "Dallas", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 153200.5}, {"PorscheModelSanitized": "911 GT3", "CitySanitized": "Las Vegas", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 241000.0}, {"PorscheModelSanitized": "Panamera Turbo S", "CitySanitized": "San Jose", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 132000.0}, {"PorscheModelSanitized": "Cayenne Turbo GT", "CitySanitized": "Houston", "DeliveryStatusSanitized": "Awaiting Delivery", "SalesPriceSanitized": 188000.0}, {"PorscheModelSanitized": "911 Carrera Cabriolet", "CitySanitized": "Atlanta", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 127800.0}, {"PorscheModelSanitized": "Macan", "CitySanitized": "Orlando", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 58900.0}, {"PorscheModelSanitized": "718 Spyder RS", "CitySanitized": "Portland", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 164000.0}, {"PorscheModelSanitized": "Taycan Cross Turismo", "CitySanitized": "Charlotte", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 118500.0}, {"PorscheModelSanitized": "Cayenne S", "CitySanitized": "Nashville", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 91300.0}, {"PorscheModelSanitized": "911 Targa 4S", "CitySanitized": "Minneapolis", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 158750.0}, {"PorscheModelSanitized": "Panamera", "CitySanitized": "Philadelphia", "DeliveryStatusSanitized": "Cancelled", "SalesPriceSanitized": 72000.0}, {"PorscheModelSanitized": "Macan Electric", "CitySanitized": "San Antonio", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 86500.0}, {"PorscheModelSanitized": "911 Dakar", "CitySanitized": "Salt Lake City", "DeliveryStatusSanitized": "Awaiting Pickup", "SalesPriceSanitized": 270000.0}, {"PorscheModelSanitized": "Taycan GTS", "CitySanitized": "Raleigh", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 139000.0}, {"PorscheModelSanitized": "Cayenne", "CitySanitized": "Detroit", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 76800.0}, {"PorscheModelSanitized": "718 Cayman GT4 RS", "CitySanitized": "Columbus", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 173600.0}, {"PorscheModelSanitized": "911 Carrera GTS", "CitySanitized": "Indianapolis", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 119900.0}, {"PorscheModelSanitized": "Panamera 4 E-Hybrid", "CitySanitized": "Fort Worth", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 109250.0}, {"PorscheModelSanitized": "Macan T", "CitySanitized": "Jacksonville", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 82000.0}, {"PorscheModelSanitized": "Taycan Turbo S", "CitySanitized": "San Diego", "DeliveryStatusSanitized": "Pending Review", "SalesPriceSanitized": 214000.0}, {"PorscheModelSanitized": "911 Carrera", "CitySanitized": "Tampa", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 124500.0}, {"PorscheModelSanitized": "Cayenne S", "CitySanitized": "Sacramento", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 98200.0}, {"PorscheModelSanitized": "Macan", "CitySanitized": "Cleveland", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 67500.0}, {"PorscheModelSanitized": "Taycan", "CitySanitized": "Milwaukee", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 116900.0}, {"PorscheModelSanitized": "Panamera 4S", "CitySanitized": "Kansas City", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 112000.0}, {"PorscheModelSanitized": "718 Boxster", "CitySanitized": "Omaha", "DeliveryStatusSanitized": "Cancelled", "SalesPriceSanitized": 74000.0}, {"PorscheModelSanitized": "911 Turbo", "CitySanitized": "Albuquerque", "DeliveryStatusSanitized": "Awaiting Delivery", "SalesPriceSanitized": 198300.0}, {"PorscheModelSanitized": "Cayenne Coupe", "CitySanitized": "Tucson", "DeliveryStatusSanitized": "Pending Approval", "SalesPriceSanitized": 103750.0}, {"PorscheModelSanitized": "Macan GTS", "CitySanitized": "Fresno", "DeliveryStatusSanitized": "Shipped", "SalesPriceSanitized": 93600.0}, {"PorscheModelSanitized": "Taycan 4S", "CitySanitized": "Virginia Beach", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 129000.0}, {"PorscheModelSanitized": "Panamera Turbo", "CitySanitized": "Colorado Springs", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 136000.0}, {"PorscheModelSanitized": "911 GT3 RS", "CitySanitized": "Arlington", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 286500.0}, {"PorscheModelSanitized": "Cayenne E-Hybrid", "CitySanitized": "Bakersfield", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 92800.0}, {"PorscheModelSanitized": "Macan T", "CitySanitized": "Mesa", "DeliveryStatusSanitized": "Awaiting Pickup", "SalesPriceSanitized": 72400.0}, {"PorscheModelSanitized": "Taycan Turbo", "CitySanitized": "Atlanta", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 158500.0}, {"PorscheModelSanitized": "718 Cayman", "CitySanitized": "Long Beach", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 69900.0}, {"PorscheModelSanitized": "911 Targa 4", "CitySanitized": "Oakland", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 141250.0}, {"PorscheModelSanitized": "Panamera", "CitySanitized": "Tulsa", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 71500.0}, {"PorscheModelSanitized": "Cayenne Turbo", "CitySanitized": "Wichita", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 146800.0}, {"PorscheModelSanitized": "Macan Electric", "CitySanitized": "New Orleans", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 89700.0}, {"PorscheModelSanitized": "911 Carrera S", "CitySanitized": "Honolulu", "DeliveryStatusSanitized": "Cancelled", "SalesPriceSanitized": 104600.0}, {"PorscheModelSanitized": "Taycan GTS", "CitySanitized": "Anaheim", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 142000.0}, {"PorscheModelSanitized": "Cayenne", "CitySanitized": "Henderson", "DeliveryStatusSanitized": "Awaiting Review", "SalesPriceSanitized": 78400.0}, {"PorscheModelSanitized": "718 Spyder RS", "CitySanitized": "Lexington", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 169000.0}, {"PorscheModelSanitized": "911 Dakar", "CitySanitized": "Riverside", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 268900.0}, {"PorscheModelSanitized": "Panamera 4", "CitySanitized": "Corpus Christi", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 101300.0}, {"PorscheModelSanitized": "Macan S", "CitySanitized": "St. Louis", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 66750.0}, {"PorscheModelSanitized": "Taycan Cross Turismo", "CitySanitized": "Pittsburgh", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 127900.0}, {"PorscheModelSanitized": "Cayenne Turbo GT", "CitySanitized": "Cincinnati", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 200000.0}, {"PorscheModelSanitized": "911 Carrera Cabriolet", "CitySanitized": "Anchorage", "DeliveryStatusSanitized": "Pending Review", "SalesPriceSanitized": 132000.0}, {"PorscheModelSanitized": "718 Cayman GT4 RS", "CitySanitized": "Plano", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 176400.0}, {"PorscheModelSanitized": "Panamera 4 E-Hybrid", "CitySanitized": "Newark", "DeliveryStatusSanitized": "Cancelled", "SalesPriceSanitized": 108500.0}, {"PorscheModelSanitized": "Macan", "CitySanitized": "Greensboro", "DeliveryStatusSanitized": "Awaiting Delivery", "SalesPriceSanitized": 59000.0}, {"PorscheModelSanitized": "Taycan Turbo S", "CitySanitized": "Lincoln", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 218000.0}, {"PorscheModelSanitized": "Cayenne S", "CitySanitized": "Jersey City", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 99950.0}, {"PorscheModelSanitized": "911 Carrera GTS", "CitySanitized": "Chandler", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 121750.0}, {"PorscheModelSanitized": "718 Boxster GTS", "CitySanitized": "Reno", "DeliveryStatusSanitized": "Shipped", "SalesPriceSanitized": 91500.0}, {"PorscheModelSanitized": "Panamera Turbo S", "CitySanitized": "Buffalo", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 134000.0}, {"PorscheModelSanitized": "Macan GTS", "CitySanitized": "Durham", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 96800.0}, {"PorscheModelSanitized": "Taycan 4S", "CitySanitized": "Laredo", "DeliveryStatusSanitized": "Pending Approval", "SalesPriceSanitized": 131600.0}, {"PorscheModelSanitized": "Cayenne E-Hybrid", "CitySanitized": "Madison", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 94300.0}, {"PorscheModelSanitized": "911 Turbo S", "CitySanitized": "Lubbock", "DeliveryStatusSanitized": "Awaiting Pickup", "SalesPriceSanitized": 242000.0}, {"PorscheModelSanitized": "718 Cayman S", "CitySanitized": "Toledo", "DeliveryStatusSanitized": "Cancelled", "SalesPriceSanitized": 82750.0}, {"PorscheModelSanitized": "Macan Electric", "CitySanitized": "Irvine", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 91300.0}, {"PorscheModelSanitized": "Panamera", "CitySanitized": "Garland", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 79900.0}, {"PorscheModelSanitized": "Cayenne Coupe", "CitySanitized": "Irving", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 111000.0}, {"PorscheModelSanitized": "911 Targa 4S", "CitySanitized": "Chesapeake", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 156500.0}, {"PorscheModelSanitized": "Taycan", "CitySanitized": "Scottsdale", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 119900.0}, {"PorscheModelSanitized": "Macan T", "CitySanitized": "Norfolk", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 73200.0}, {"PorscheModelSanitized": "911 GT3", "CitySanitized": "Boise", "DeliveryStatusSanitized": "Awaiting Delivery", "SalesPriceSanitized": 224000.0}, {"PorscheModelSanitized": "911 Carrera", "CitySanitized": "Orlando", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 126900.0}, {"PorscheModelSanitized": "Cayenne", "CitySanitized": "San Jose", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 84500.0}, {"PorscheModelSanitized": "Macan S", "CitySanitized": "Tampa", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 69800.0}, {"PorscheModelSanitized": "Taycan 4S", "CitySanitized": "Denver", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 132700.0}, {"PorscheModelSanitized": "Panamera", "CitySanitized": "Austin", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 81000.0}, {"PorscheModelSanitized": "718 Cayman", "CitySanitized": "Seattle", "DeliveryStatusSanitized": "Cancelled", "SalesPriceSanitized": 78900.0}, {"PorscheModelSanitized": "911 Turbo S", "CitySanitized": "Boston", "DeliveryStatusSanitized": "Awaiting Delivery", "SalesPriceSanitized": 249300.0}, {"PorscheModelSanitized": "Cayenne Coupe", "CitySanitized": "Phoenix", "DeliveryStatusSanitized": "Pending Approval", "SalesPriceSanitized": 108750.0}, {"PorscheModelSanitized": "Macan Electric", "CitySanitized": "Chicago", "DeliveryStatusSanitized": "Shipped", "SalesPriceSanitized": 92600.0}, {"PorscheModelSanitized": "Taycan Turbo", "CitySanitized": "Dallas", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 164000.0}, {"PorscheModelSanitized": "Panamera 4S", "CitySanitized": "San Francisco", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 119000.0}, {"PorscheModelSanitized": "911 GT3", "CitySanitized": "Las Vegas", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 232500.0}, {"PorscheModelSanitized": "Cayenne E-Hybrid", "CitySanitized": "Charlotte", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 96800.0}, {"PorscheModelSanitized": "Macan T", "CitySanitized": "Mesa", "DeliveryStatusSanitized": "Awaiting Pickup", "SalesPriceSanitized": 74400.0}, {"PorscheModelSanitized": "Taycan GTS", "CitySanitized": "Atlanta", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 148500.0}, {"PorscheModelSanitized": "718 Boxster", "CitySanitized": "Long Beach", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 71900.0}, {"PorscheModelSanitized": "911 Targa 4", "CitySanitized": "Oakland", "DeliveryStatusSanitized": "In Transit", "SalesPriceSanitized": 143250.0}, {"PorscheModelSanitized": "Panamera Turbo", "CitySanitized": "Tulsa", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 137500.0}, {"PorscheModelSanitized": "Cayenne Turbo GT", "CitySanitized": "Wichita", "DeliveryStatusSanitized": "Pending", "SalesPriceSanitized": 204800.0}, {"PorscheModelSanitized": "911 Dakar", "CitySanitized": "New Orleans", "DeliveryStatusSanitized": "Delivered", "SalesPriceSanitized": 271700.0}];

function fillFilters(){
["PorscheModelSanitized","CitySanitized","DeliveryStatusSanitized"].forEach((field,i)=>{
let sel=document.querySelectorAll("select")[i];
sel.innerHTML='<option value="">Todos</option>';
[...new Set(data.map(x=>x[field]))].forEach(v=>sel.innerHTML+=`<option>${v}</option>`);
});
}

function update(){
let model=modelSel.value;
let city=citySel.value;
let status=statusSel.value;

let filtered=data.filter(x=>
(!model||x.PorscheModelSanitized==model)&&
(!city||x.CitySanitized==city)&&
(!status||x.DeliveryStatusSanitized==status)
);

document.getElementById("total").innerText=filtered.length;
let revenue=filtered.reduce((a,b)=>a+b.SalesPriceSanitized,0);
document.getElementById("revenue").innerText='R$ '+revenue.toLocaleString();
document.getElementById("ticket").innerText='R$ '+(revenue/(filtered.length||1)).toFixed(0);

let counts={};
filtered.forEach(x=>counts[x.PorscheModelSanitized]=(counts[x.PorscheModelSanitized]||0)+1);

let leader=Object.keys(counts).sort((a,b)=>counts[b]-counts[a])[0]||'-';
document.getElementById("leader").innerText=leader;

document.getElementById("insight").innerText=
leader!='-' ? `${leader} apresenta forte aceitação nesta segmentação.` : '';

renderModels(counts);

let statusCount={};
filtered.forEach(x=>statusCount[x.DeliveryStatusSanitized]=(statusCount[x.DeliveryStatusSanitized]||0)+1);
renderStatus(statusCount);
}

let modelsChart,statusChart;

function renderModels(counts){
if(modelsChart) modelsChart.destroy();
modelsChart=new Chart(document.getElementById('models'),{
type:'bar',
data:{labels:Object.keys(counts),datasets:[{label:'Vendas',data:Object.values(counts)}]}
});
}

function renderStatus(counts){
if(statusChart) statusChart.destroy();
statusChart=new Chart(document.getElementById('statusChart'),{
type:'doughnut',
data:{labels:Object.keys(counts),datasets:[{data:Object.values(counts)}]}
});
}

const modelSel=document.getElementById('model');
const citySel=document.getElementById('city');
const statusSel=document.getElementById('status');

fillFilters();
update();

document.querySelectorAll('select').forEach(s=>s.addEventListener('change',update));
</script>

</body>
</html>
