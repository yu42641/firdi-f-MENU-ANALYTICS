 1 <!DOCTYPE html>
     2 <html lang="zh-TW">
     3 <head>
     4     <meta charset="UTF-8">
     5     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     6     <title>雪坊精品 | 銷售數據動態分析中心</title>
     7     <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
     8     <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap"
       rel="stylesheet">
     9     <style>
    10         :root {
    11             --primary-color: #003366;
    12             --accent-color: #c5a059;
    13             --bg-color: #fcfcfc;
    14             --text-color: #333333;
    15             --border-color: #e0e0e0;
    16             --card-shadow: 0 4px 20px rgba(0,0,0,0.05);
    17             --base-font-size: 15px;
    18         }
    19
    20         * {
    21             box-sizing: border-box;
    22             font-family: 'Noto Sans TC', sans-serif;
    23             font-size: var(--base-font-size);
    24         }
    25
    26         body {
    27             margin: 0;
    28             background-color: var(--bg-color);
    29             color: var(--text-color);
    30             line-height: 1.6;
    31         }
    32
    33         .navbar {
    34             background-color: #ffffff;
    35             padding: 40px 0;
    36             text-align: center;
    37             border-bottom: 1px solid var(--border-color);
    38             position: sticky;
    39             top: 0;
    40             z-index: 1000;
    41         }
    42
    43         .navbar .logo {
    44             font-size: 32px;
    45             font-weight: 700;
    46             color: var(--primary-color);
    47             letter-spacing: 6px;
    48             text-transform: uppercase;
    49             margin-bottom: 5px;
    50         }
    51
    52         .navbar .subtitle {
    53             font-size: 14px;
    54             color: var(--accent-color);
    55             letter-spacing: 8px;
    56             font-weight: 300;
    57         }
    58
    59         .container {
    60             max-width: 1200px;
    61             margin: 50px auto;
    62             padding: 0 30px;
    63         }
    64
    65         .filter-section {
    66             display: flex;
    67             justify-content: center;
    68             align-items: center;
    69             margin-bottom: 70px;
    70             gap: 20px;
    71         }
    72
    73         .filter-label {
    74             font-weight: 400;
    75             color: #777;
    76             letter-spacing: 1px;
    77         }
    78
    79         select {
    80             appearance: none;
    81             background-color: #fff;
    82             border: 1px solid var(--border-color);
    83             padding: 12px 60px 12px 30px;
    84             border-radius: 0;
    85             cursor: pointer;
    86             transition: all 0.3s;
    87             color: var(--primary-color);
    88             letter-spacing: 1px;
    89             background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12'
       viewBox='0 0 12 12'%3E%3Cpath fill='%23003366' d='M10.293 3.293L6 7.586 1.707 3.293A1 1 0 00.293 4.707l5 5a1 1 0 001.414
       0l5-5a1 1 0 10-1.414-1.414z'/%3E%3C/svg%3E");
    90             background-repeat: no-repeat;
    91             background-position: right 25px center;
    92         }
    93
    94         select:focus {
    95             outline: none;
    96             border-color: var(--accent-color);
    97         }
    98
    99         .dashboard-grid {
   100             display: grid;
   101             grid-template-columns: 1fr;
   102             gap: 60px;
   103             margin-bottom: 100px;
   104         }
   105
   106         .card {
   107             background: #ffffff;
   108             padding: 50px;
   109             border: 1px solid #f2f2f2;
   110             border-radius: 4px;
   111             box-shadow: var(--card-shadow);
   112         }
   113
   114         .card-title {
   115             font-size: 22px;
   116             font-weight: 500;
   117             color: var(--primary-color);
   118             margin-bottom: 40px;
   119             text-align: center;
   120             letter-spacing: 3px;
   121             position: relative;
   122         }
   123
   124         .card-title::after {
   125             content: '';
   126             display: block;
   127             width: 40px;
   128             height: 2px;
   129             background: var(--accent-color);
   130             margin: 18px auto 0;
   131         }
   132
   133         .table-container {
   134             border-top: 1px solid #eee;
   135             padding-top: 60px;
   136         }
   137
   138         table {
   139             width: 100%;
   140             border-collapse: collapse;
   141             background: white;
   142         }
   143
   144         th {
   145             background-color: #fcfcfc;
   146             color: var(--primary-color);
   147             font-weight: 600;
   148             text-align: left;
   149             padding: 25px;
   150             border-bottom: 2px solid var(--primary-color);
   151             letter-spacing: 2px;
   152             text-transform: uppercase;
   153         }
   154
   155         td {
   156             padding: 25px;
   157             border-bottom: 1px solid #f5f5f5;
   158             color: #444;
   159         }
   160
   161         tr:hover td {
   162             color: var(--primary-color);
   163             background-color: #fafafa;
   164         }
   165
   166         .badge {
   167             padding: 8px 20px;
   168             border-radius: 0;
   169             font-size: 13px;
   170             font-weight: 400;
   171             letter-spacing: 1px;
   172             display: inline-block;
   173         }
   174
   175         .badge-hot {
   176             background-color: var(--primary-color);
   177             color: #ffffff;
   178             font-weight: 500;
   179         }
   180
   181         .badge-hot .crown {
   182             font-size: 18px;
   183             margin-right: 5px;
   184             vertical-align: middle;
   185         }
   186
   187         .badge-cold {
   188             background-color: #f4f4f4;
   189             color: #888;
   190             border: 1px solid #eee;
   191         }
   192
   193         .badge-premium {
   194             background-color: #fdf6e9;
   195             color: var(--accent-color);
   196             border: 1px solid #faebcc;
   197         }
   198
   199         footer {
   200             text-align: center;
   201             padding: 80px 20px;
   202             color: #bbb;
   203             font-size: 12px;
   204             letter-spacing: 2px;
   205             border-top: 1px solid #eee;
   206             background: #fff;
   207         }
   208
   209         .canvas-wrapper {
   210             position: relative;
   211             margin: auto;
   212             width: 100%;
   213         }
   214     </style>
   215 </head>
   216 <body>
   217
   218 <nav class="navbar">
   219     <div class="logo">SNOW FACTORY</div>
   220     <div class="subtitle">精品優格銷售數據動態報告</div>
   221 </nav>
   222
   223 <div class="container">
   224     <div class="filter-section">
   225         <span class="filter-label">門市據點篩選</span>
   226         <select id="storeFilter" onchange="updateDashboard()">
   227             <option value="全部">所有門市總覽</option>
   228             <option value="高雄店">高雄旗艦店</option>
   229             <option value="信義店">信義門市</option>
   230             <option value="新竹店">新竹門市</option>
   231             <option value="桃園店">桃園門市</option>
   232             <option value="台中一店">台中一店</option>
   233             <option value="板橋店">板橋門市</option>
   234             <option value="內湖店">內湖門市</option>
   235             <option value="台南店">台南門市</option>
   236         </select>
   237     </div>
   238
   239     <div class="dashboard-grid">
   240         <div class="card">
   241             <div class="card-title">銷售數量對比</div>
   242             <div class="canvas-wrapper">
   243                 <canvas id="qtyChart"></canvas>
   244             </div>
   245         </div>
   246         <div class="card">
   247             <div class="card-title">營收價值對比</div>
   248             <div class="canvas-wrapper">
   249                 <canvas id="amtChart"></canvas>
   250             </div>
   251         </div>
   252
   253         <div class="card table-container">
   254             <div class="card-title">銷售品項與數據</div>
   255             <div style="display: flex; justify-content: flex-end; margin-bottom: 25px; gap: 20px; align-items: center;">
   256                 <span class="filter-label">篩選標籤類別：</span>
   257                 <select id="statusFilter" onchange="updateDashboard()">
   258                     <option value="全部">全部顯示</option>
   259                     <option value="熱銷首選">👑 熱銷首選</option>
   260                     <option value="核心產品">✨ 核心產品</option>
   261                     <option value="建議觀察">⚠️ 建議觀察</option>
   262                     <option value="穩定">⚪ 穩定品項</option>
   263                 </select>
   264             </div>
   265             <table id="summaryTable">
   266                 <thead>
   267                     <tr>
   268                         <th>銷售品項</th>
   269                         <th>銷售總數</th>
   270                         <th>銷售金額</th>
   271                         <th>市場評估</th>
   272                     </tr>
   273                 </thead>
   274                 <tbody>
   275                 </tbody>
   276             </table>
   277         </div>
   278     </div>
   279 </div>
   280
   281 <footer>
   282     &copy; 2026 SNOW FACTORY DATA ANALYSIS SYSTEM. ALL RIGHTS RESERVED.
   283 </footer>
   284
   285 <script>
   286     const rawData = [
   287         {date:"2026-01-01",store:"高雄店",item:"沙拉醬",qty:435,amt:44476},
   288         {date:"2026/2/2",store:"高雄店",item:"冷凍水餃",qty:69,amt:17563},
   289         {date:"20260303",store:"信義店",item:"能量棒",qty:259,amt:9137},
   290         {date:"20260404",store:"新竹店",item:"蛋捲",qty:90,amt:17245},
   291         {date:"2026/5/5",store:"桃園店",item:"能量棒",qty:170,amt:28589},
   292         {date:"2026/6/6",store:"新竹店",item:"湯包",qty:478,amt:16412},
   293         {date:"20260707",store:"台中一店",item:"布丁",qty:117,amt:26949},
   294         {date:"20260808",store:"板橋店",item:"湯包",qty:438,amt:35337},
   295         {date:"2026/9/9",store:"高雄店",item:"即食雞胸",qty:326,amt:21056},
   296         {date:"2026/10/10",store:"新竹店",item:"杯裝優格",qty:59,amt:25965},
   297         {date:"2026-11-11",store:"內湖店",item:"蛋捲",qty:302,amt:4182},
   298         {date:"2026-12-12",store:"板橋店",item:"冷凍水餃",qty:36,amt:5978},
   299         {date:"13 Feb 2026",store:"高雄店",item:"蛋捲",qty:126,amt:39221},
   300         {date:"2026-02-14",store:"內湖店",item:"沙拉醬",qty:309,amt:8748},
   301         {date:"15 Apr 2026",store:"信義店",item:"即食雞胸",qty:325,amt:10652},
   302         {date:"16 May 2026",store:"桃園店",item:"布丁",qty:202,amt:48481},
   303         {date:"2026/5/17",store:"台南店",item:"沙拉醬",qty:6,amt:7791},
   304         {date:"2026-06-18",store:"內湖店",item:"杯裝優格",qty:355,amt:36869},
   305         {date:"2026-07-19",store:"內湖店",item:"氣泡飲",qty:0,amt:48441},
   306         {date:"2026-08-20",store:"高雄店",item:"氣泡飲",qty:393,amt:47527},
   307         {date:"20260921",store:"桃園店",item:"即食雞胸",qty:422,amt:42469},
   308         {date:"2026/10/22",store:"台南店",item:"冷凍水餃",qty:303,amt:30605},
   309         {date:"20261123",store:"台南店",item:"即食雞胸",qty:150,amt:42971},
   310         {date:"2026/12/24",store:"板橋店",item:"鮮奶茶",qty:67,amt:16079},
   311         {date:"2026/1/25",store:"高雄店",item:"即食雞胸",qty:389,amt:7414},
   312         {date:"2026/2/26",store:"台中一店",item:"蛋捲",qty:259,amt:13879},
   313         {date:"20260327",store:"台南店",item:"杯裝優格",qty:58,amt:41974},
   314         {date:"28 May 2026",store:"板橋店",item:"泡芙",qty:5,amt:48370},
   315         {date:"1 Jun 2026",store:"內湖店",item:"蛋捲",qty:166,amt:40248},
   316         {date:"2026/6/2",store:"台南店",item:"果醬",qty:376,amt:31548},
   317         {date:"20260703",store:"桃園店",item:"即食雞胸",qty:150,amt:5117},
   318         {date:"2026/8/4",store:"桃園店",item:"巧克力餅乾",qty:209,amt:3117},
   319         {date:"5 Apr 2026",store:"板橋店",item:"冷凍水餃",qty:463,amt:12285},
   320         {date:"20261006",store:"桃園店",item:"沙拉醬",qty:100,amt:10787},
   321         {date:"20261107",store:"台中一店",item:"杯裝優格",qty:36,amt:18131},
   322         {date:"2026-12-08",store:"信義店",item:"布丁",qty:333,amt:12731},
   323         {date:"20260109",store:"台南店",item:"沙拉醬",qty:270,amt:6192},
   324         {date:"2026/2/10",store:"內湖店",item:"堅果包",qty:115,amt:3264},
   325         {date:"20260311",store:"桃園店",item:"氣泡飲",qty:94,amt:1619},
   326         {date:"20260412",store:"新竹店",item:"杯裝優格",qty:466,amt:21394},
   327         {date:"2026-05-13",store:"新竹店",item:"杯裝優格",qty:181,amt:13434},
   328         {date:"20260614",store:"板橋店",item:"湯包",qty:485,amt:34867},
   329         {date:"2026-07-15",store:"高雄店",item:"杯裝優格",qty:322,amt:7988},
   330         {date:"2026-08-16",store:"桃園店",item:"堅果包",qty:226,amt:26361},
   331         {date:"2026/9/17",store:"高雄店",item:"蛋捲",qty:499,amt:6636},
   332         {date:"2026/10/18",store:"新竹店",item:"蛋捲",qty:339,amt:4418},
   333         {date:"2026-11-19",store:"台中一店",item:"果醬",qty:231,amt:38491},
   334         {date:"2026/12/20",store:"台南店",item:"布丁",qty:208,amt:29721},
   335         {date:"2026/1/21",store:"高雄店",item:"即食雞胸",qty:300,amt:22729},
   336         {date:"20260222",store:"新竹店",item:"堅果包",qty:455,amt:22878},
   337         {date:"23 Apr 2026",store:"桃園店",item:"氣泡飲",qty:2800,amt:34824},
   338         {date:"2026/4/24",store:"信義店",item:"蛋捲",qty:272,amt:17993},
   339         {date:"2026/5/25",store:"板橋店",item:"泡芙",qty:437,amt:2616},
   340         {date:"2026-06-26",store:"高雄店",item:"湯包",qty:228,amt:10614},
   341         {date:"27 Feb 2026",store:"台南店",item:"泡芙",qty:301,amt:6467},
   342         {date:"2026-08-28",store:"新竹店",item:"能量棒",qty:38,amt:14359},
   343         {date:"20260901",store:"台中一店",item:"鮮奶茶",qty:170,amt:39579},
   344         {date:"2 May 2026",store:"台南店",item:"鮮奶茶",qty:462,amt:49570},
   345         {date:"2026-11-03",store:"板橋店",item:"蛋捲",qty:151,amt:44571},
   346         {date:"4 Jan 2026",store:"新竹店",item:"湯包",qty:75,amt:9499},
   347         {date:"2026-01-05",store:"台中一店",item:"杯裝優格",qty:7,amt:49552},
   348         {date:"2026/2/6",store:"高雄店",item:"沙拉醬",qty:432,amt:6314},
   349         {date:"2026-03-07",store:"台南店",item:"巧克力餅乾",qty:461,amt:16062},
   350         {date:"8 May 2026",store:"板橋店",item:"杯裝優格",qty:133,amt:27045},
   351         {date:"9 Jun 2026",store:"桃園店",item:"鮮奶茶",qty:138,amt:37586},
   352         {date:"10 Jan 2026",store:"新竹店",item:"鮮奶茶",qty:342,amt:6890},
   353         {date:"2026-07-11",store:"新竹店",item:"氣泡飲",qty:463,amt:2393},
   354         {date:"2026-08-12",store:"台中一店",item:"果醬",qty:185,amt:24389},
   355         {date:"13 Apr 2026",store:"內湖店",item:"冷凍水餃",qty:489,amt:13750},
   356         {date:"2026/10/14",store:"新竹店",item:"果醬",qty:65,amt:47734},
   357         {date:"15 Jun 2026",store:"信義店",item:"蛋捲",qty:103,amt:27552},
   358         {date:"20261216",store:"台南店",item:"沙拉醬",qty:289,amt:4762},
   359         {date:"17 Feb 2026",store:"信義店",item:"氣泡飲",qty:202,amt:34018},
   360         {date:"18 Mar 2026",store:"桃園店",item:"即食雞胸",qty:462,amt:38148},
   361         {date:"19 Apr 2026",store:"台南店",item:"冷凍水餃",qty:130,amt:22144},
   362         {date:"2026/4/20",store:"台中一店",item:"泡芙",qty:236,amt:5447},
   363         {date:"2026/5/21",store:"內湖店",item:"布丁",qty:436,amt:16599},
   364         {date:"2026/6/22",store:"桃園店",item:"氣泡飲",qty:148,amt:8399},
   365         {date:"20260723",store:"板橋店",item:"原味吐司",qty:65,amt:32240},
   366         {date:"2026-08-24",store:"高雄店",item:"氣泡飲",qty:333,amt:24886},
   367         {date:"2026-09-25",store:"台南店",item:"即食雞胸",qty:128,amt:10501},
   368         {date:"2026/10/26",store:"內湖店",item:"湯包",qty:335,amt:19192},
   369         {date:"27 Jun 2026",store:"台中一店",item:"沙拉醬",qty:132,amt:33246},
   370         {date:"28 Jan 2026",store:"內湖店",item:"果醬",qty:427,amt:34513},
   371         {date:"1 Feb 2026",store:"桃園店",item:"巧克力餅乾",qty:240,amt:48765},
   372         {date:"2 Mar 2026",store:"高雄店",item:"冷凍水餃",qty:1400,amt:19869},
   373         {date:"2026/3/3",store:"台中一店",item:"堅果包",qty:297,amt:4585},
   374         {date:"",store:"高雄店",item:"氣泡飲",qty:64,amt:13379},
   375         {date:"2026-05-05",store:"新竹店",item:"能量棒",qty:287,amt:6297},
   376         {date:"2026/6/6",store:"台中一店",item:"即食雞胸",qty:140,amt:39293},
   377         {date:"20260707",store:"桃園店",item:"湯包",qty:326,amt:22642},
   378         {date:"8 Mar 2026",store:"信義店",item:"原味吐司",qty:364,amt:17633},
   379         {date:"9 Apr 2026",store:"板橋店",item:"堅果包",qty:166,amt:22801},
   380         {date:"2026/10/10",store:"台南店",item:"即食雞胸",qty:363,amt:2970},
   381         {date:"2026/11/11",store:"信義店",item:"即食雞胸",qty:190,amt:30604},
   382         {date:"12 Jan 2026",store:"板橋店",item:"能量棒",qty:423,amt:4966},
   383         {date:"20260113",store:"內湖店",item:"堅果包",qty:484,amt:30570},
   384         {date:"20260214",store:"信義店",item:"氣泡飲",qty:116,amt:10824},
   385         {date:"2026-03-15",store:"桃園店",item:"原味吐司",qty:85,amt:47966},
   386         {date:"16 May 2026",store:"高雄店",item:"鮮奶茶",qty:255,amt:24385},
   387         {date:"2026-05-17",store:"高雄店",item:"沙拉醬",qty:434,amt:17669},
   388         {date:"2026-06-18",store:"板橋店",item:"布丁",qty:185,amt:20424},
   389         {date:"19 Feb 2026",store:"新竹店",item:"沙拉醬",qty:300,amt:49248},
   390         {date:"2026/8/20",store:"高雄店",item:"湯包",qty:431,amt:24196},
   391         {date:"20260921",store:"桃園店",item:"杯裝優格",qty:0,amt:14579},
   392         {date:"20261022",store:"台南店",item:"泡芙",qty:458,amt:5563},
   393         {date:"2026-11-23",store:"板橋店",item:"能量棒",qty:47,amt:39408},
   394         {date:"24 Jan 2026",store:"高雄店",item:"堅果包",qty:428,amt:34683},
   395         {date:"25 Feb 2026",store:"高雄店",item:"堅果包",qty:104,amt:25503},
   396         {date:"20260226",store:"高雄店",item:"即食雞胸",qty:327,amt:1502},
   397         {date:"27 Apr 2026",store:"台南店",item:"鮮奶茶",qty:310,amt:48490},
   398         {date:"2026-04-28",store:"台南店",item:"氣泡飲",qty:468,amt:45814},
   399         {date:"2026/5/1",store:"內湖店",item:"即食雞胸",qty:146,amt:11431},
   400         {date:"20260602",store:"高雄店",item:"原味吐司",qty:266,amt:15409},
   401         {date:"2026-07-03",store:"信義店",item:"布丁",qty:484,amt:16750},
   402         {date:"2026/8/4",store:"信義店",item:"鮮奶茶",qty:436,amt:8986},
   403         {date:"5 Apr 2026",store:"高雄店",item:"蛋捲",qty:114,amt:16462},
   404         {date:"2026-10-06",store:"台中一店",item:"巧克力餅乾",qty:340,amt:13174},
   405         {date:"2026-11-07",store:"板橋店",item:"蛋捲",qty:29,amt:47720},
   406         {date:"2026/12/8",store:"高雄店",item:"湯包",qty:122,amt:48627},
   407         {date:"9 Feb 2026",store:"內湖店",item:"冷凍水餃",qty:147,amt:13785},
   408         {date:"2026/2/10",store:"高雄店",item:"果醬",qty:325,amt:0},
   409         {date:"20260311",store:"信義店",item:"杯裝優格",qty:456,amt:31223},
   410         {date:"2026-04-12",store:"新竹店",item:"",qty:113,amt:48220},
   411         {date:"",store:"內湖店",item:"泡芙",qty:158,amt:9557},
   412         {date:"14 Jan 2026",store:"台南店",item:"湯包",qty:11,amt:18354},
   413         {date:"20260715",store:"新竹店",item:"氣泡飲",qty:380,amt:36897},
   414         {date:"20260816",store:"板橋店",item:"能量棒",qty:181,amt:9695},
   415         {date:"2026-09-17",store:"新竹店",item:"能量棒",qty:212,amt:8472},
   416         {date:"20261018",store:"台南店",item:"蛋捲",qty:331,amt:27068},
   417         {date:"20261119",store:"板橋店",item:"堅果包",qty:336,amt:43855},
   418         {date:"2026/12/20",store:"板橋店",item:"果醬",qty:489,amt:16743},
   419         {date:"",store:"高雄店",item:"泡芙",qty:490,amt:6903},
   420         {date:"2026-02-22",store:"內湖店",item:"杯裝優格",qty:231,amt:30347},
   421         {date:"2026-03-23",store:"板橋店",item:"堅果包",qty:110,amt:849},
   422         {date:"2026/4/24",store:"板橋店",item:"冷凍水餃",qty:91,amt:32361},
   423         {date:"2026/5/25",store:"板橋店",item:"冷凍水餃",qty:407,amt:37559},
   424         {date:"2026-06-26",store:"信義店",item:"蛋捲",qty:472,amt:2950},
   425         {date:"27 Feb 2026",store:"內湖店",item:"原味吐司",qty:31,amt:31534},
   426         {date:"20260828",store:"信義店",item:"布丁",qty:324,amt:20258},
   427         {date:"2026/9/1",store:"台中一店",item:"沙拉醬",qty:237,amt:33026},
   428         {date:"2 May 2026",store:"桃園店",item:"鮮奶茶",qty:305,amt:43802},
   429         {date:"2026-11-03",store:"桃園店",item:"果醬",qty:97,amt:34514},
   430         {date:"2026/12/4",store:"板橋店",item:"即食雞胸",qty:90,amt:13811},
   431         {date:"20260105",store:"新竹店",item:"杯裝優格",qty:127,amt:25358},
   432         {date:"2026/2/6",store:"桃園店",item:"鮮奶茶",qty:430,amt:40786},
   433         {date:"2026-03-07",store:"高雄店",item:"能量棒",qty:281,amt:7914},
   434         {date:"20260408",store:"高雄店",item:"泡芙",qty:441,amt:48778},
   435         {date:"9 Jun 2026",store:"台南店",item:"能量棒",qty:373,amt:29486},
   436         {date:"2026-06-10",store:"新竹店",item:"鮮奶茶",qty:24,amt:7328},
   437         {date:"2026/1/21",store:"高雄店",item:"即食雞胸",qty:300,amt:22729}
   438     ];
   439
   440     let qtyChart, amtChart;
   441
   442     function updateDashboard() {
   443         const storeFilter = document.getElementById('storeFilter').value;
   444         const filtered = storeFilter === '全部' ? rawData : rawData.filter(d => d.store === storeFilter);
   445         const aggregation = {};
   446
   447         filtered.forEach(d => {
   448             if (!d.item) return;
   449             if (!aggregation[d.item]) {
   450                 aggregation[d.item] = { qty: 0, amt: 0 };
   451             }
   452             aggregation[d.item].qty += d.qty;
   453             aggregation[d.item].amt += d.amt;
   454         });
   455
   456         const labels = Object.keys(aggregation);
   457         const qtys = labels.map(l => aggregation[l].qty);
   458         const amts = labels.map(l => aggregation[l].amt);
   459
   460         renderCharts(labels, qtys, amts);
   461         updateTable(aggregation);
   462     }
   463
   464     function renderCharts(labels, qtys, amts) {
   465         const colors = ['#FFB7B2', '#FFDAC1', '#E2F0CB', '#B5EAD7', '#C7CEEA', '#A0E7E5', '#FF9AA2', '#F8B195',
       '#FDFD96', '#FAC898'];
   466         if (qtyChart) qtyChart.destroy();
   467         if (amtChart) amtChart.destroy();
   468         const commonOptions = { indexAxis: 'y', responsive: true, maintainAspectRatio: true, aspectRatio: 2.2, plugins:
       { legend: { display: false } }, scales: { x: { beginAtZero: true, grid: { display: false } }, y: { grid: { display:
       false }, ticks: { font: { weight: '500' } } } } };
   469         qtyChart = new Chart(document.getElementById('qtyChart'), { type: 'bar', data: { labels: labels, datasets: [{
       data: qtys, backgroundColor: colors, borderRadius: 4 }] }, options: { ...commonOptions, plugins: {
       ...commonOptions.plugins, title: { display: true, text: '單位：份', align: 'end' } } } });
   470         amtChart = new Chart(document.getElementById('amtChart'), { type: 'bar', data: { labels: labels, datasets: [{
       data: amts, backgroundColor: colors, borderRadius: 4 }] }, options: { ...commonOptions, plugins: {
       ...commonOptions.plugins, title: { display: true, text: '單位：元', align: 'end' } } } });
   471     }
   472
   473     function updateTable(agg) {
   474         const tbody = document.querySelector('#summaryTable tbody');
   475         const statusFilter = document.getElementById('statusFilter').value;
   476         tbody.innerHTML = '';
   477         const colors = ['#FFB7B2', '#FFDAC1', '#E2F0CB', '#B5EAD7', '#C7CEEA', '#A0E7E5', '#FF9AA2', '#F8B195',
       '#FDFD96', '#FAC898'];
   478         const sortedItems = Object.keys(agg).sort((a, b) => agg[b].qty - agg[a].qty);
   479         const maxQty = agg[sortedItems[0]]?.qty || 0;
   480
   481         sortedItems.forEach((item, index) => {
   482             let statusText = '穩定';
   483             let badgeHtml = '<span class="badge">穩定</span>';
   484             if (agg[item].qty === maxQty && maxQty > 0) {
   485                 statusText = '熱銷首選';
   486                 badgeHtml = '<span class="badge badge-hot"><span class="crown">👑</span>熱銷首選</span>';
   487             } else if (item.includes('優格')) {
   488                 statusText = '核心產品';
   489                 badgeHtml = '<span class="badge badge-premium">核心產品</span>';
   490             } else if (agg[item].qty < 1000) {
   491                 statusText = '建議觀察';
   492                 badgeHtml = '<span class="badge badge-cold">建議觀察</span>';
   493             }
   494             if (statusFilter !== '全部' && statusText !== statusFilter) return;
   495             const row = document.createElement('tr');
   496             const itemColor = colors[index % colors.length];
   497             row.innerHTML = `<td><span style="display:inline-block; width:12px; height:12px;
       background-color:${itemColor}; margin-right:10px;
       border-radius:2px;"></span><strong>${item}</strong></td><td>${agg[item].qty.toLocaleString()}</td><td>$${agg[item].amt.t
       oLocaleString()}</td><td>${badgeHtml}</td>`;
   498             tbody.appendChild(row);
   499         });
   500     }
   501     updateDashboard();
   502 </script>
   503 </body>
   504 </html>
