 1 <!DOCTYPE html>
     2 <html lang="zh-TW">
     3 <head>
     4     <meta charset="UTF-8">
     5     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     6     <title>雪坊精品 | 銷售數據分析</title>
     7     <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
     8     <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap"
       rel="stylesheet">
     9     <style>
    10         :root { --primary-color: #003366; --accent-color: #c5a059; --bg-color: #fcfcfc; --card-shadow: 0 4px 20px
       rgba(0,0,0,0.05); }
    11         * { box-sizing: border-box; font-family: 'Noto Sans TC', sans-serif; font-size: 15px; }
    12         body { margin: 0; background-color: var(--bg-color); color: #333; line-height: 1.6; }
    13         .navbar { background: #fff; padding: 40px 0; text-align: center; border-bottom: 1px solid #e0e0e0; position:
       sticky; top: 0; z-index: 1000; }
    14         .navbar .logo { font-size: 32px; font-weight: 700; color: var(--primary-color); letter-spacing: 6px; }
    15         .navbar .subtitle { font-size: 14px; color: var(--accent-color); letter-spacing: 8px; font-weight: 300; }
    16         .container { max-width: 1200px; margin: 50px auto; padding: 0 30px; }
    17         .filter-section { display: flex; justify-content: center; align-items: center; margin-bottom: 70px; gap: 20px; }
    18         select { appearance: none; background: #fff; border: 1px solid #e0e0e0; padding: 12px 60px 12px 30px; cursor:
       pointer; color: var(--primary-color); }
    19         .dashboard-grid { display: grid; grid-template-columns: 1fr; gap: 60px; margin-bottom: 100px; }
    20         .card { background: #fff; padding: 50px; border: 1px solid #f2f2f2; border-radius: 4px; box-shadow:
       var(--card-shadow); }
    21         .card-title { font-size: 22px; font-weight: 500; color: var(--primary-color); margin-bottom: 40px; text-align:
       center; letter-spacing: 3px; position: relative; }
    22         .card-title::after { content: ''; display: block; width: 40px; height: 2px; background: var(--accent-color);
       margin: 18px auto 0; }
    23         table { width: 100%; border-collapse: collapse; background: white; }
    24         th { background: #fcfcfc; color: var(--primary-color); font-weight: 600; text-align: left; padding: 25px;
       border-bottom: 2px solid var(--primary-color); }
    25         td { padding: 25px; border-bottom: 1px solid #f5f5f5; }
    26         .badge { padding: 8px 20px; font-size: 13px; display: inline-block; }
    27         .badge-hot { background-color: var(--primary-color); color: #fff; }
    28         .badge-hot .crown { font-size: 18px; margin-right: 5px; vertical-align: middle; }
    29         .badge-cold { background: #f4f4f4; color: #888; border: 1px solid #eee; }
    30         .badge-premium { background: #fdf6e9; color: var(--accent-color); border: 1px solid #faebcc; }
    31         footer { text-align: center; padding: 80px 0; color: #bbb; font-size: 12px; border-top: 1px solid #eee; }
    32     </style>
    33 </head>
    34 <body>
    35 <nav class="navbar"><div class="logo">SNOW FACTORY</div><div class="subtitle">精品優格銷售數據報表</div></nav>
    36 <div class="container">
    37     <div class="filter-section">
    38         <span>門市據點：</span>
    39         <select id="storeFilter" onchange="updateDashboard()">
    40             <option value="全部">所有門市總覽</option>
    41             <option value="高雄店">高雄旗艦店</option>
    42             <option value="信義店">信義門市</option>
    43             <option value="新竹店">新竹門市</option>
    44             <option value="桃園店">桃園門市</option>
    45             <option value="台中一店">台中一店</option>
    46             <option value="板橋店">板橋門市</option>
    47             <option value="內湖店">內湖門市</option>
    48             <option value="台南店">台南門市</option>
    49         </select>
    50     </div>
    51     <div class="dashboard-grid">
    52         <div class="card"><div class="card-title">銷售數量對比</div><canvas id="qtyChart"></canvas></div>
    53         <div class="card"><div class="card-title">營收價值對比</div><canvas id="amtChart"></canvas></div>
    54         <div class="card">
    55             <div class="card-title">銷售品項與數據</div>
    56             <div style="display: flex; justify-content: flex-end; margin-bottom: 25px; gap: 20px; align-items: center;">
    57                 <span>篩選：</span>
    58                 <select id="statusFilter" onchange="updateDashboard()">
    59                     <option value="全部">全部顯示</option>
    60                     <option value="熱銷首選">👑 熱銷首選</option>
    61                     <option value="核心產品">✨ 核心產品</option>
    62                     <option value="建議觀察">⚠️ 建議觀察</option>
    63                     <option value="穩定">⚪ 穩定品項</option>
    64                 </select>
    65             </div>
    66             <table id="summaryTable">
    67                 <thead><tr><th>銷售品項</th><th>銷售總數</th><th>銷售金額</th><th>市場評估</th></tr></thead>
    68                 <tbody></tbody>
    69             </table>
    70         </div>
    71     </div>
    72 </div>
    73 <footer>&copy; 2026 SNOW FACTORY ALL RIGHTS RESERVED.</footer>
    74 <script>
    75     const rawData = [
    76
       {store:"高雄店",item:"沙拉醬",qty:435,amt:44476},{store:"高雄店",item:"冷凍水餃",qty:69,amt:17563},{store:"信義店",item:
       "能量棒",qty:259,amt:9137},{store:"新竹店",item:"蛋捲",qty:90,amt:17245},{store:"桃園店",item:"能量棒",qty:170,amt:28589
       },{store:"新竹店",item:"湯包",qty:478,amt:16412},{store:"台中一店",item:"布丁",qty:117,amt:26949},{store:"板橋店",item:"
       湯包",qty:438,amt:35337},{store:"高雄店",item:"即食雞胸",qty:326,amt:21056},{store:"新竹店",item:"杯裝優格",qty:59,amt:2
       5965},{store:"內湖店",item:"蛋捲",qty:302,amt:4182},{store:"板橋店",item:"冷凍水餃",qty:36,amt:5978},{store:"高雄店",ite
       m:"蛋捲",qty:126,amt:39221},{store:"內湖店",item:"沙拉醬",qty:309,amt:8748},{store:"信義店",item:"即食雞胸",qty:325,amt:
       10652},{store:"桃園店",item:"布丁",qty:202,amt:48481},{store:"台南店",item:"沙拉醬",qty:6,amt:7791},{store:"內湖店",item
       :"杯裝優格",qty:355,amt:36869},{store:"內湖店",item:"氣泡飲",qty:0,amt:48441},{store:"高雄店",item:"氣泡飲",qty:393,amt:
       47527},{store:"桃園店",item:"即食雞胸",qty:422,amt:42469},{store:"台南店",item:"冷凍水餃",qty:303,amt:30605},{store:"台
       南店",item:"即食雞胸",qty:150,amt:42971},{store:"板橋店",item:"鮮奶茶",qty:67,amt:16079},{store:"高雄店",item:"即食雞胸"
       ,qty:389,amt:7414},{store:"台中一店",item:"蛋捲",qty:259,amt:13879},{store:"台南店",item:"杯裝優格",qty:58,amt:41974},{s
       tore:"板橋店",item:"泡芙",qty:5,amt:48370},{store:"內湖店",item:"蛋捲",qty:166,amt:40248},{store:"台南店",item:"果醬",qt
       y:376,amt:31548},{store:"桃園店",item:"即食雞胸",qty:150,amt:5117},{store:"桃園店",item:"巧克力餅乾",qty:209,amt:3117},{
       store:"板橋店",item:"冷凍水餃",qty:463,amt:12285},{store:"桃園店",item:"沙拉醬",qty:100,amt:10787},{store:"台中一店",ite
       m:"杯裝優格",qty:36,amt:18131},{store:"信義店",item:"布丁",qty:333,amt:12731},{store:"台南店",item:"沙拉醬",qty:270,amt:
       6192},{store:"內湖店",item:"堅果包",qty:115,amt:3264},{store:"桃園店",item:"氣泡飲",qty:94,amt:1619},{store:"新竹店",ite
       m:"杯裝優格",qty:466,amt:21394},{store:"新竹店",item:"杯裝優格",qty:181,amt:13434},{store:"板橋店",item:"湯包",qty:485,a
       mt:34867},{store:"高雄店",item:"杯裝優格",qty:322,amt:7988},{store:"桃園店",item:"堅果包",qty:226,amt:26361},{store:"高
       雄店",item:"蛋捲",qty:499,amt:6636},{store:"新竹店",item:"蛋捲",qty:339,amt:4418},{store:"台中一店",item:"果醬",qty:231,
       amt:38491},{store:"台南店",item:"布丁",qty:208,amt:29721},{store:"高雄店",item:"即食雞胸",qty:300,amt:22729},{store:"新
       竹店",item:"堅果包",qty:455,amt:22878},{store:"桃園店",item:"氣泡飲",qty:2800,amt:34824},{store:"信義店",item:"蛋捲",qty
       :272,amt:17993},{store:"板橋店",item:"泡芙",qty:437,amt:2616},{store:"高雄店",item:"湯包",qty:228,amt:10614},{store:"台
       南店",item:"泡芙",qty:301,amt:6467},{store:"新竹店",item:"能量棒",qty:38,amt:14359},{store:"台中一店",item:"鮮奶茶",qty:
       170,amt:39579},{store:"台南店",item:"鮮奶茶",qty:462,amt:49570},{store:"板橋店",item:"蛋捲",qty:151,amt:44571},{store:"
       新竹店",item:"湯包",qty:75,amt:9499},{store:"台中一店",item:"杯裝優格",qty:7,amt:49552},{store:"高雄店",item:"沙拉醬",qt
       y:432,amt:6314},{store:"台南店",item:"巧克力餅乾",qty:461,amt:16062},{store:"板橋店",item:"杯裝優格",qty:133,amt:27045},
       {store:"桃園店",item:"鮮奶茶",qty:138,amt:37586},{store:"新竹店",item:"鮮奶茶",qty:342,amt:6890},{store:"新竹店",item:"
       氣泡飲",qty:463,amt:2393},{store:"台中一店",item:"果醬",qty:185,amt:24389},{store:"內湖店",item:"冷凍水餃",qty:489,amt:1
       3750},{store:"新竹店",item:"果醬",qty:65,amt:47734},{store:"信義店",item:"蛋捲",qty:103,amt:27552},{store:"台南店",item:
       "沙拉醬",qty:289,amt:4762},{store:"信義店",item:"氣泡飲",qty:202,amt:34018},{store:"桃園店",item:"即食雞胸",qty:462,amt:
       38148},{store:"台南店",item:"冷凍水餃",qty:130,amt:22144},{store:"台中一店",item:"泡芙",qty:236,amt:5447},{store:"內湖店
       ",item:"布丁",qty:436,amt:16599},{store:"桃園店",item:"氣泡飲",qty:148,amt:8399},{store:"板橋店",item:"原味吐司",qty:65,
       amt:32240},{store:"高雄店",item:"氣泡飲",qty:333,amt:24886},{store:"台南店",item:"即食雞胸",qty:128,amt:10501},{store:"
       內湖店",item:"湯包",qty:335,amt:19192},{store:"台中一店",item:"沙拉醬",qty:132,amt:33246},{store:"內湖店",item:"果醬",qt
       y:427,amt:34513},{store:"桃園店",item:"巧克力餅乾",qty:240,amt:48765},{store:"高雄店",item:"冷凍水餃",qty:1400,amt:19869
       },{store:"台中一店",item:"堅果包",qty:297,amt:4585},{store:"高雄店",item:"氣泡飲",qty:64,amt:13379},{store:"新竹店",item
       :"能量棒",qty:287,amt:6297},{store:"台中一店",item:"即食雞胸",qty:140,amt:39293},{store:"桃園店",item:"湯包",qty:326,amt
       :22642},{store:"信義店",item:"原味吐司",qty:364,amt:17633},{store:"板橋店",item:"堅果包",qty:166,amt:22801},{store:"台南
       店",item:"即食雞胸",qty:363,amt:2970},{store:"信義店",item:"即食雞胸",qty:190,amt:30604},{store:"板橋店",item:"能量棒",q
       ty:423,amt:4966},{store:"內湖店",item:"堅果包",qty:484,amt:30570},{store:"信義店",item:"氣泡飲",qty:116,amt:10824},{stor
       e:"桃園店",item:"原味吐司",qty:85,amt:47966},{store:"高雄店",item:"鮮奶茶",qty:255,amt:24385},{store:"高雄店",item:"沙拉
       醬",qty:434,amt:17669},{store:"板橋店",item:"布丁",qty:185,amt:20424},{store:"新竹店",item:"沙拉醬",qty:300,amt:49248},{
       store:"高雄店",item:"湯包",qty:431,amt:24196},{store:"桃園店",item:"杯裝優格",qty:0,amt:14579},{store:"台南店",item:"泡
       芙",qty:458,amt:5563},{store:"板橋店",item:"能量棒",qty:47,amt:39408},{store:"高雄店",item:"堅果包",qty:428,amt:34683},{
       store:"高雄店",item:"堅果包",qty:104,amt:25503},{store:"高雄店",item:"即食雞胸",qty:327,amt:1502},{store:"台南店",item:"
       鮮奶茶",qty:310,amt:48490},{store:"台南店",item:"氣泡飲",qty:468,amt:45814},{store:"內湖店",item:"即食雞胸",qty:146,amt:
       11431},{store:"高雄店",item:"原味吐司",qty:266,amt:15409},{store:"信義店",item:"布丁",qty:484,amt:16750},{store:"信義店"
       ,item:"鮮奶茶",qty:436,amt:8986},{store:"高雄店",item:"蛋捲",qty:114,amt:16462},{store:"台中一店",item:"巧克力餅乾",qty:
       340,amt:13174},{store:"板橋店",item:"蛋捲",qty:29,amt:47720},{store:"高雄店",item:"湯包",qty:122,amt:48627},{store:"內湖
       店",item:"冷凍水餃",qty:147,amt:13785},{store:"高雄店",item:"果醬",qty:325,amt:0},{store:"信義店",item:"杯裝優格",qty:45
       6,amt:31223},{store:"新竹店",item:"",qty:113,amt:48220},{store:"內湖店",item:"泡芙",qty:158,amt:9557},{store:"台南店",it
       em:"湯包",qty:11,amt:18354},{store:"新竹店",item:"氣泡飲",qty:380,amt:36897},{store:"板橋店",item:"能量棒",qty:181,amt:9
       695},{store:"新竹店",item:"能量棒",qty:212,amt:8472},{store:"台南店",item:"蛋捲",qty:331,amt:27068},{store:"板橋店",item
       :"堅果包",qty:336,amt:43855},{store:"板橋店",item:"果醬",qty:489,amt:16743},{store:"高雄店",item:"泡芙",qty:490,amt:6903
       },{store:"內湖店",item:"杯裝優格",qty:231,amt:30347},{store:"板橋店",item:"堅果包",qty:110,amt:849},{store:"板橋店",item
       :"冷凍水餃",qty:91,amt:32361},{store:"板橋店",item:"冷凍水餃",qty:407,amt:37559},{store:"信義店",item:"蛋捲",qty:472,amt
       :2950},{store:"內湖店",item:"原味吐司",qty:31,amt:31534},{store:"信義店",item:"布丁",qty:324,amt:20258},{store:"台中一店
       ",item:"沙拉醬",qty:237,amt:33026},{store:"桃園店",item:"鮮奶茶",qty:305,amt:43802},{store:"桃園店",item:"果醬",qty:97,a
       mt:34514},{store:"板橋店",item:"即食雞胸",qty:90,amt:13811},{store:"新竹店",item:"杯裝優格",qty:127,amt:25358},{store:"
       桃園店",item:"鮮奶茶",qty:430,amt:40786},{store:"高雄店",item:"能量棒",qty:281,amt:7914},{store:"高雄店",item:"泡芙",qty
       :441,amt:48778},{store:"台南店",item:"能量棒",qty:373,amt:29486},{store:"新竹店",item:"鮮奶茶",qty:24,amt:7328},{store:"
       高雄店",item:"即食雞胸",qty:300,amt:22729}
    77     ];
    78
    79     let qtyChart, amtChart;
    80     function updateDashboard() {
    81         const storeFilter = document.getElementById('storeFilter').value;
    82         const statusFilter = document.getElementById('statusFilter').value;
    83         const filtered = storeFilter === '全部' ? rawData : rawData.filter(d => d.store === storeFilter);
    84         const aggregation = {};
    85         filtered.forEach(d => {
    86             if (!d.item) return;
    87             if (!aggregation[d.item]) aggregation[d.item] = { qty: 0, amt: 0 };
    88             aggregation[d.item].qty += d.qty;
    89             aggregation[d.item].amt += d.amt;
    90         });
    91         const labels = Object.keys(aggregation);
    92         const qtys = labels.map(l => aggregation[l].qty);
    93         const amts = labels.map(l => aggregation[l].amt);
    94         renderCharts(labels, qtys, amts);
    95         updateTable(aggregation, statusFilter);
    96     }
    97
    98     function renderCharts(labels, qtys, amts) {
    99         const colors = ['#FFB7B2', '#FFDAC1', '#E2F0CB', '#B5EAD7', '#C7CEEA', '#A0E7E5', '#FF9AA2', '#F8B195',
       '#FDFD96', '#FAC898'];
   100         if (qtyChart) qtyChart.destroy();
   101         if (amtChart) amtChart.destroy();
   102         const opt = { indexAxis: 'y', responsive: true, maintainAspectRatio: true, aspectRatio: 2.2, plugins: { legend:
       { display: false } }, scales: { x: { beginAtZero: true, grid: { display: false } }, y: { grid: { display: false } } } };
   103         qtyChart = new Chart(document.getElementById('qtyChart'), { type: 'bar', data: { labels, datasets: [{ data:
       qtys, backgroundColor: colors, borderRadius: 4 }] }, options: opt });
   104         amtChart = new Chart(document.getElementById('amtChart'), { type: 'bar', data: { labels, datasets: [{ data:
       amts, backgroundColor: colors, borderRadius: 4 }] }, options: opt });
   105     }
   106
   107     function updateTable(agg, statusFilter) {
   108         const tbody = document.querySelector('#summaryTable tbody');
   109         tbody.innerHTML = '';
   110         const colors = ['#FFB7B2', '#FFDAC1', '#E2F0CB', '#B5EAD7', '#C7CEEA', '#A0E7E5', '#FF9AA2', '#F8B195',
       '#FDFD96', '#FAC898'];
   111         const sorted = Object.keys(agg).sort((a, b) => agg[b].qty - agg[a].qty);
   112         const maxQty = agg[sorted[0]]?.qty || 0;
   113
   114         sorted.forEach((item, index) => {
   115             let sText = '穩定', bHtml = '<span class="badge">穩定</span>';
   116             if (agg[item].qty === maxQty && maxQty > 0) { sText = '熱銷首選'; bHtml = '<span class="badge
       badge-hot"><span class="crown">👑</span>熱銷首選</span>'; }
   117             else if (item.includes('優格')) { sText = '核心產品'; bHtml = '<span class="badge badge-premium">✨
       核心產品</span>'; }
   118             else if (agg[item].qty < 1000) { sText = '建議觀察'; bHtml = '<span class="badge badge-cold">⚠️
       建議觀察</span>'; }
   119
   120             if (statusFilter !== '全部' && sText !== statusFilter) return;
   121             const row = document.createElement('tr');
   122             row.innerHTML = `<td><span
       style="display:inline-block;width:12px;height:12px;background:${colors[index%colors.length]};margin-right:10px;border-ra
       dius:2px;"></span><strong>${item}</strong></td><td>${agg[item].qty.toLocaleString()}</td><td>$${agg[item].amt.toLocaleSt
       ring()}</td><td>${bHtml}</td>`;
   123             tbody.appendChild(row);
   124         });
   125     }
   126     updateDashboard();
   127 </script>
   128 </body>
   129 </html>
