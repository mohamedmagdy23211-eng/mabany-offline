<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>حساب تكلفة المباني - Offline</title>
<style>
body {
    font-family: Arial;
    direction: rtl;
    background: #f4f4f4;
    padding: 15px;
}
.box {
    background: #fff;
    padding: 15px;
    margin-bottom: 15px;
    border-radius: 8px;
}
h2 {
    margin-top: 0;
    color: #333;
}
input {
    width: 100%;
    padding: 8px;
    margin: 5px 0 10px;
}
.result {
    background: #e8f5e9;
    padding: 10px;
    margin-top: 10px;
    border-radius: 6px;
}
button {
    padding: 10px;
    width: 100%;
    background: #2e7d32;
    color: #fff;
    border: none;
    font-size: 16px;
    border-radius: 6px;
}
</style>
</head>

<body>

<div class="box">
<h2>🔹 المدخلات</h2>

<label>مساحة العمل (م²)</label>
<input id="area">

<label>طول الطوبة (م)</label>
<input id="length">

<label>ارتفاع الطوبة (م)</label>
<input id="height">

<label>عرض الطوبة (م)</label>
<input id="width">

<label>سعر ألف طوبة</label>
<input id="brickPrice">

<label>سعر شيكارة الأسمنت</label>
<input id="cementPrice">

<label>سعر المتر المكعب رمل</label>
<input id="sandPrice">

<label>يومية الصنايعي</label>
<input id="daily">

<button onclick="calc()">احسب</button>
</div>

<div class="box result" id="output"></div>

<script>
function calc() {

let area = Number(areaEl = document.getElementById("area").value);
let L = Number(document.getElementById("length").value);
let H = Number(document.getElementById("height").value);
let brickPrice = Number(document.getElementById("brickPrice").value);
let cementPrice = Number(document.getElementById("cementPrice").value);
let sandPrice = Number(document.getElementById("sandPrice").value);
let daily = Number(document.getElementById("daily").value);

// ثوابت
let waste = 1.05;
let productivity = 1500; // طوبة / يوم
let cementPer1000 = 4;
let sandPer1000 = 0.07;

// عدد الطوب
let bricksPerM2 = (1 / (L * H)) * waste;
let totalBricks = bricksPerM2 * area;

// الخامات
let cementBags = (totalBricks / 1000) * cementPer1000;
let sandQty = (totalBricks / 1000) * sandPer1000;

// الزمن
let days = totalBricks / productivity;

// التكاليف
let brickCost = (brickPrice / 1000) * totalBricks;
let cementCost = cementBags * cementPrice;
let sandCost = sandQty * sandPrice;
let laborCost = days * daily;

let materials = brickCost + cementCost + sandCost;
let totalCost = materials + laborCost;

let cost15 = totalCost * 1.15;
let cost25 = totalCost * 1.25;

let companyLabor = ((cost15 + cost25) / 2) - materials;
let companyCost = materials + companyLabor;

document.getElementById("output").innerHTML = `
<b>🔢 النتائج:</b><br><br>
عدد الطوب: ${totalBricks.toFixed(0)} طوبة<br>
عدد الشكاير: ${cementBags.toFixed(2)} شيكارة<br>
كمية الرمل: ${sandQty.toFixed(3)} م³<br>
عدد الأيام: ${days.toFixed(2)} يوم<br><br>

<b>💰 التكاليف:</b><br>
تكلفة بدون مكسب: ${totalCost.toFixed(0)} ج<br>
تكلفة مقاول +15%: ${cost15.toFixed(0)} ج<br>
تكلفة مقاول +25%: ${cost25.toFixed(0)} ج<br><br>

<b>🏢 تكلفة على الشركة:</b><br>
${companyCost.toFixed(0)} ج
`;
}
</script>

</body>
</html>
