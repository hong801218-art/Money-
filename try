<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>個人財務管理 - 行動優化版</title>
<style>
    :root {
        --primary-color: #1a73e8;
        --income-color: #27ae60;
        --expense-color: #e74c3c;
        --bg-color: #f8f9fa;
    }

    body { 
        font-family: "PingFang TC", "Microsoft JhengHei", sans-serif; 
        background-color: var(--bg-color); 
        padding: 10px; 
        margin: 0;
        line-height: 1.5;
        color: #333;
    }

    .container { 
        max-width: 900px; 
        margin: auto; 
        background: white; 
        padding: 15px; 
        border-radius: 12px; 
        box-shadow: 0 2px 10px rgba(0,0,0,0.05); 
    }

    h2 { text-align: center; color: var(--primary-color); margin-top: 10px; font-size: 1.5rem; }
    h3 { font-size: 1.1rem; margin-bottom: 15px; }

    /* 核心佈局：手機單欄，平板以上雙欄 */
    .grid { 
        display: grid; 
        grid-template-columns: 1fr; 
        gap: 15px; 
    }
    @media (min-width: 768px) {
        .grid { grid-template-columns: 1fr 1fr; }
        body { padding: 20px; }
        .container { padding: 30px; }
    }

    .section { 
        padding: 15px; 
        border: 1px solid #edf2f7; 
        border-radius: 12px; 
        background: #fff;
    }

    /* 輸入列優化：手機上標籤與輸入框分行或適度拉開 */
    .item-row { 
        display: flex; 
        justify-content: space-between; 
        align-items: center; 
        margin-bottom: 12px; 
    }
    
    label { font-weight: 500; color: #4a5568; font-size: 0.95rem; }
    
    input[type="number"], input[type="month"] { 
        width: 120px; 
        padding: 10px; 
        border: 1px solid #cbd5e0; 
        border-radius: 8px; 
        text-align: right; 
        font-size: 1rem;
        -webkit-appearance: none; /* 移除 iOS 預設樣式 */
    }
    
    input:focus { border-color: var(--primary-color); outline: none; box-shadow: 0 0 0 3px rgba(26, 115, 232, 0.1); }

    /* 分類統計卡片 */
    .result-box { 
        background: #fff; 
        padding: 20px; 
        border-radius: 12px; 
        margin-top: 20px; 
        border: 1px solid #e2e8f0;
        box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
    }
    
    .highlight { font-size: 1.5rem; font-weight: 800; }

    /* 圖表條 */
    .chart-bar-wrap { 
        height: 28px; 
        width: 100%; 
        background: #edf2f7; 
        border-radius: 14px; 
        overflow: hidden; 
        display: flex; 
        margin: 15px 0; 
    }
    .bar { transition: width 0.6s ease; display: flex; align-items: center; justify-content: center; color: white; font-size: 11px; font-weight: bold; }
    .bar-essential { background: #f56565; } 
    .bar-savings { background: #48bb78; }   
    .bar-optional { background: #ed8936; }  

    .legend { 
        display: grid; 
        grid-template-columns: repeat(3, 1fr); 
        gap: 8px; 
    }
    .legend-item { 
        display: flex; 
        flex-direction: column; 
        align-items: center; 
        font-size: 0.8rem; 
        background: #f7fafc; 
        padding: 8px; 
        border-radius: 8px;
    }

    /* 按鈕優化 */
    .btn-group { margin-top: 20px; }
    button { 
        width: 100%; 
        padding: 15px; 
        border: none; 
        border-radius: 10px; 
        cursor: pointer; 
        font-size: 1rem; 
        font-weight: bold; 
        transition: transform 0.1s;
        background: var(--income-color); 
        color: white;
    }
    button:active { transform: scale(0.98); }

    /* 表格滾動優化 */
    .table-wrapper { 
        overflow-x: auto; 
        margin-top: 15px; 
        border-radius: 8px;
        border: 1px solid #eee;
    }
    table { min-width: 600px; width: 100%; border-collapse: collapse; font-size: 0.85rem; }
    th, td { padding: 12px 8px; border-bottom: 1px solid #edf2f7; }
    th { background: #f8f9fa; position: sticky; left: 0; }
    td:first-child { position: sticky; left: 0; background: #fff; font-weight: bold; z-index: 1; }
</style>
</head>
<body>

<div class="container">
    <h2>💰 財務小管家</h2>

    <div style="text-align: center; margin-bottom: 20px;">
        <label>選擇月份：</label>
        <input type="month" id="monthPicker" onchange="loadData()">
    </div>

    <div class="grid">
        <div class="section">
            <h3 style="color: var(--income-color);">＋ 收入來源</h3>
            <div id="incomeFields"></div>
            <div class="item-row" style="margin-top:10px; padding-top:10px; border-top: 2px solid #eee;">
                <strong>總計</strong>
                <span id="subtotalInc" style="color: var(--income-color); font-weight: bold;">0</span>
            </div>
        </div>

        <div class="section">
            <h3 style="color: var(--expense-color);">－ 支出與儲蓄</h3>
            <div id="expenseFields"></div>
            <div class="item-row" style="margin-top:10px; padding-top:10px; border-top: 2px solid #eee;">
                <strong>總計</strong>
                <span id="subtotalExp" style="color: var(--expense-color); font-weight: bold;">0</span>
            </div>
        </div>
    </div>

    <div class="result-box">
        <div class="item-row">
            <span>本月結餘</span>
            <span id="balance" class="highlight">0</span>
        </div>
        
        <div class="chart-bar-wrap">
            <div id="barEssential" class="bar bar-essential" style="width: 0%"></div>
            <div id="barSavings" class="bar bar-savings" style="width: 0%"></div>
            <div id="barOptional" class="bar bar-optional" style="width: 0%"></div>
        </div>

        <div class="legend">
            <div class="legend-item"><span>必要</span><span id="valEssential">0</span></div>
            <div class="legend-item"><span>儲蓄</span><span id="valSavings">0</span></div>
            <div class="legend-item"><span>非必要</span><span id="valOptional">0</span></div>
        </div>
    </div>

    <div class="btn-group">
        <button onclick="saveData()">💾 儲存本月帳目</button>
    </div>

    <h3 style="margin-top: 40px; text-align: center;">📊 <span id="yearLabel"></span> 年度回顧</h3>
    <div class="table-wrapper">
        <table id="yearTable">
            <thead><tr id="tableHeader"><th>項目</th></tr></thead>
            <tbody id="tableBody"></tbody>
        </table>
    </div>
</div>

<script>
const incomeItems = ["薪資收入", "其他收入"];
const expenseCategories = {
    essential: ["房租", "生活費", "電話費", "信用卡支出"],
    savings: ["郵局存款", "股票存入"],
    optional: ["出遊旅費", "孝親費"]
};
const expenseItems = [...expenseCategories.essential, ...expenseCategories.savings, ...expenseCategories.optional];

function init() {
    const today = new Date();
    document.getElementById('monthPicker').value = today.toISOString().slice(0, 7);
    renderInputs('incomeFields', incomeItems, 'inc-input');
    renderInputs('expenseFields', expenseItems, 'exp-input');
    loadData();
}

function renderInputs(containerId, list, className) {
    const container = document.getElementById(containerId);
    container.innerHTML = '';
    list.forEach(item => {
        const div = document.createElement('div');
        div.className = 'item-row';
        div.innerHTML = `<label>${item}</label>
                         <input type="number" class="val-input ${className}" data-name="${item}" oninput="calculate()" placeholder="0">`;
        container.appendChild(div);
    });
}

function calculate() {
    let tInc = 0, tExp = 0, sEss = 0, sSav = 0, sOpt = 0;
    document.querySelectorAll('.inc-input').forEach(i => tInc += Number(i.value) || 0);
    document.querySelectorAll('.exp-input').forEach(i => {
        const v = Number(i.value) || 0;
        const n = i.dataset.name;
        tExp += v;
        if (expenseCategories.essential.includes(n)) sEss += v;
        else if (expenseCategories.savings.includes(n)) sSav += v;
        else if (expenseCategories.optional.includes(n)) sOpt += v;
    });

    const bal = tInc - tExp;
    document.getElementById('subtotalInc').innerText = tInc.toLocaleString();
    document.getElementById('subtotalExp').innerText = tExp.toLocaleString();
    document.getElementById('balance').innerText = bal.toLocaleString();
    document.getElementById('balance').style.color = bal >= 0 ? "#27ae60" : "#e74c3c";
    document.getElementById('valEssential').innerText = sEss.toLocaleString();
    document.getElementById('valSavings').innerText = sSav.toLocaleString();
    document.getElementById('valOptional').innerText = sOpt.toLocaleString();

    if (tExp > 0) {
        document.getElementById('barEssential').style.width = (sEss/tExp*100) + "%";
        document.getElementById('barSavings').style.width = (sSav/tExp*100) + "%";
        document.getElementById('barOptional').style.width = (sOpt/tExp*100) + "%";
    }
}

function saveData() {
    const m = document.getElementById('monthPicker').value;
    const data = {};
    document.querySelectorAll('.val-input').forEach(i => data[i.dataset.name] = i.value);
    localStorage.setItem(`fin_${m}`, JSON.stringify(data));
    alert('✅ 資料儲存成功！');
    renderYearTable();
}

function loadData() {
    const m = document.getElementById('monthPicker').value;
    const data = JSON.parse(localStorage.getItem(`fin_${m}`) || "{}");
    document.querySelectorAll('.val-input').forEach(i => i.value = data[i.dataset.name] || "");
    calculate();
    renderYearTable();
}

function renderYearTable() {
    const yr = document.getElementById('monthPicker').value.split('-')[0];
    document.getElementById('yearLabel').innerText = yr;
    const header = document.getElementById('tableHeader');
    const body = document.getElementById('tableBody');

    header.innerHTML = '<th>項目</th>' + Array.from({length: 12}, (_, i) => `<th>${i+1}月</th>`).join('') + '<th>總計</th>';
    body.innerHTML = '';

    [...incomeItems, ...expenseItems, "月餘"].forEach(name => {
        let row = `<td>${name}</td>`, total = 0;
        for (let m = 1; m <= 12; m++) {
            const data = JSON.parse(localStorage.getItem(`fin_${yr}-${m.toString().padStart(2, '0')}`) || "{}");
            let v = 0;
            if (name === "月餘") {
                v = incomeItems.reduce((s, k) => s + (Number(data[k]) || 0), 0) - expenseItems.reduce((s, k) => s + (Number(data[k]) || 0), 0);
            } else { v = Number(data[name]) || 0; }
            row += `<td>${v ? v.toLocaleString() : '-'}</td>`;
            total += v;
        }
        body.innerHTML += `<tr ${name === "月餘" ? 'style="background:#e8f0fe;font-weight:bold"' : ''}>${row}<td>${total.toLocaleString()}</td></tr>`;
    });
}
init();
</script>
</body>
</html>
