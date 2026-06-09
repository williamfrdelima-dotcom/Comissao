<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Simulador Beta - Formatação Automática</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Roboto, sans-serif; }
        body { background: #f4f4f9; padding-bottom: 130px; }
        header { background: #DA291C; color: white; padding: 15px; text-align: center; font-weight: bold; border-bottom: 4px solid #F1C400; font-size: 1.2rem; }
        .container { padding: 15px; max-width: 600px; margin: 0 auto; }
        .card { background: white; border-radius: 12px; padding: 15px; margin-bottom: 15px; border-left: 5px solid #DA291C; }
        .card-title { font-size: 1.1rem; font-weight: bold; margin-bottom: 12px; color: #DA291C; border-bottom: 1px solid #eee; padding-bottom: 6px; }
        .input-group { display: flex; gap: 12px; margin-bottom: 12px; flex-wrap: wrap; }
        .input-box { flex: 1; display: flex; flex-direction: column; min-width: 110px; }
        label { font-size: 0.75rem; font-weight: 600; color: #666; margin-bottom: 4px; }
        input, select { padding: 12px; border: 1px solid #ccc; border-radius: 10px; font-size: 0.95rem; width: 100%; background: white; }
        input:focus, select:focus { outline: none; border-color: #DA291C; box-shadow: 0 0 0 2px rgba(218,41,28,0.1); }
        .res { display: flex; justify-content: space-between; margin-top: 12px; padding-top: 8px; border-top: 1px dashed #ddd; font-weight: bold; flex-wrap: wrap; }
        .result-value { color: #2e7d32; font-size: 1.1rem; }
        .info-faixa { background: #E8F5E9; padding: 6px 12px; border-radius: 8px; font-size: 11px; color: #2e7d32; margin-top: 8px; }
        .footer-total { position: fixed; bottom: 0; width: 100%; background: white; padding: 14px 20px; border-top: 4px solid #DA291C; }
        .btn-zap { background: #25D366; color: white; padding: 14px; text-align: center; border-radius: 40px; display: block; font-weight: bold; margin-top: 10px; cursor: pointer; border: none; width: 100%; font-size: 1rem; }
        .btn-limpar { background: #6c757d; color: white; padding: 10px; border-radius: 40px; font-weight: bold; cursor: pointer; border: none; font-size: 0.9rem; margin-bottom: 10px; width: 100px; }
        .header-buttons { display: flex; justify-content: flex-end; margin-bottom: 5px; }
        .tatico-note { font-size: 11px; color: #1976D2; text-align: center; background: #E3F2FD; padding: 5px; border-radius: 6px; margin-top: 8px; }
    </style>
</head>
<body>
    <header>🛢️ Simulador - BETA 🛢️</header>
    <div class="container">
        <div class="header-buttons"><button class="btn-limpar" onclick="limparTudo()">🗑️ Limpar</button></div>
        
        <div class="card">
            <div class="card-title">🏆 Pontuação</div>
            <div class="input-group">
                <div class="input-box"><label>Meta (pontos)</label><input type="text" id="p_m" placeholder="Ex: 130.260" oninput="formatNumber(this)" onkeyup="formatNumber(this)"></div>
                <div class="input-box"><label>Realizado</label><input type="text" id="p_r" placeholder="Ex: 150.000" oninput="formatNumber(this)" onkeyup="formatNumber(this)"></div>
            </div>
            <div class="res"><span>🎯 Atingimento: <span id="p_perc">0%</span></span><span>💵 Comissão: <span class="result-value" id="res_p">R$ 0,00</span></span></div>
            <div class="info-faixa" id="p_faixa_info">📊 Faixa: digite a meta</div>
        </div>

        <div class="card">
            <div class="card-title">🤝 Clientes</div>
            <div class="input-group">
                <div class="input-box"><label>Meta</label><input type="number" id="e_m" placeholder="Ex: 90" oninput="calc()"></div>
                <div class="input-box"><label>Realizado</label><input type="number" id="e_r" placeholder="Ex: 99" oninput="calc()"></div>
            </div>
            <div class="res"><span>📊 Atingimento: <span id="e_perc">0%</span></span><span>💰 Comissão: <span class="result-value" id="res_e">R$ 0,00</span></span></div>
            <div class="info-faixa" id="e_faixa_info">📊 Faixa: digite a meta</div>
        </div>

        <div class="card">
            <div class="card-title">🔧 Tático 1 (50%)</div>
            <div class="input-group">
                <div class="input-box"><label>Meta</label><input type="number" id="t1_m" placeholder="Meta" oninput="calc()"></div>
                <div class="input-box"><label>Realizado</label><input type="number" id="t1_r" placeholder="Realizado" oninput="calc()"></div>
            </div>
            <div class="res"><span>📈 Atingimento: <span id="t1_perc">0%</span></span><span>🏆 Comissão: <span class="result-value" id="res_t1">R$ 0,00</span></span></div>
            <div class="tatico-note" id="t1_faixa_info">📌 Baseado na faixa de clientes</div>
        </div>

        <div class="card">
            <div class="card-title">⚙️ Tático 2 (50%)</div>
            <div class="input-group">
                <div class="input-box"><label>Meta</label><input type="number" id="t2_m" placeholder="Meta" oninput="calc()"></div>
                <div class="input-box"><label>Realizado</label><input type="number" id="t2_r" placeholder="Realizado" oninput="calc()"></div>
            </div>
            <div class="res"><span>📈 Atingimento: <span id="t2_perc">0%</span></span><span>🏆 Comissão: <span class="result-value" id="res_t2">R$ 0,00</span></span></div>
            <div class="tatico-note" id="t2_faixa_info">📌 Baseado na faixa de clientes</div>
        </div>

        <div class="card">
            <div class="card-title">💰 Faturamento</div>
            <div class="input-group">
                <div class="input-box"><label>%</label><select id="a_perc" onchange="calc()"><option value="0.01">1%</option><option value="0.02">2%</option><option value="0.03">3%</option><option value="0.04">4%</option><option value="0.05">5%</option></select></div>
                <div class="input-box"><label>R$ Realizado</label><input type="text" id="a_r" placeholder="Ex: 50.000" oninput="formatNumber(this)" onkeyup="formatNumber(this)"></div>
            </div>
            <div class="res"><span>📊 Comissão</span><span class="result-value" id="res_a">R$ 0,00</span></div>
        </div>
    </div>

    <div class="footer-total">
        <div class="res"><span>🛢️ TOTAL:</span><span id="res_total" style="font-size: 1.5rem; font-weight: bold; color: #DA291C">R$ 0,00</span></div>
        <button class="btn-zap" onclick="compartilharWhatsApp()">📲 Compartilhar</button>
    </div>

    <script>
        // ========== TABELAS ==========
        const pt_metas = [130260, 173680, 217100, 260520, 303940, 347360, 390780, 434200, 651300, 868400, 1085500, 1302600];
        const pt_percs = [70,80,90,100,110,120,130,140,150,160,170,180,190,200,210,220];
        const pt_grid = [[315.23,415.83,481.96,703.40,781.56,814.13,846.69,859.72,874.04,875.35,876.00,892.28,908.56,924.85,941.13,957.41],[420.31,555.78,642.62,937.87,1042.08,1085.50,1128.92,1146.29,1165.39,1167.13,1168.00,1189.71,1211.42,1233.13,1254.84,1276.55],[525.38,694.72,803.27,1172.34,1302.60,1356.88,1411.15,1432.86,1456.74,1458.91,1460.00,1487.14,1514.27,1541.41,1568.55,1595.69],[630.46,833.66,963.92,1406.81,1563.12,1628.25,1693.38,1719.43,1748.09,1750.69,1752.00,1784.56,1817.13,1849.69,1882.26,1914.82],[735.53,972.61,1124.58,1641.28,1823.64,1899.63,1975.61,2006.00,2039.44,2042.48,2044.00,2081.99,2115.97,2157.97,2195.97,2233.96],[840.61,1111.55,1285.23,1875.74,2084.16,2171.00,2257.84,2292.58,2330.79,2334.26,2336.00,2379.42,2422.84,2466.26,2509.68,2553.10],[945.69,1250.50,1445.89,2110.21,2344.68,2442.38,2540.07,2579.16,2622.13,2626.04,2628.00,2676.69,2725.38,2774.54,2823.39,2872.23],[1050.76,1389.44,1606.54,2344.68,2605.20,2713.75,2822.30,2865.72,2913.48,2917.82,2920.00,2974.27,3028.55,3082.82,3137.10,3191.37],[1576.15,2084.16,2409.81,3517.02,3907.80,4070.63,4233.45,4298.58,4370.22,4376.74,4379.99,4461.41,4542.82,4624.23,4705.64,4787.06],[2101.53,2778.88,3213.08,4689.36,5210.40,5427.50,5644.60,5731.44,5826.96,5835.65,5839.99,5948.54,6057.09,6165.64,6274.19,6382.74],[2626.91,3473.60,4016.35,5861.70,6513.00,6784.38,7055.75,7164.30,7283.71,7294.56,7299.99,7435.68,7571.36,7707.05,7842.74,7978.43],[3152.29,4168.32,4819.62,7034.04,7815.60,8141.25,8466.90,8597.16,8740.45,8753.47,8759.99,8922.81,9085.64,9248.46,9411.29,9574.11]];
        const ef_metas = [49,50,70,80,90,100,110,120,130,140,150];
        const ef_percs = [80,90,100,110,120,130];
        const ef_grid = [[300,420,600,660,726,798.6],[450,630,900,990,1089,1197.9],[550,770,1100,1210,1331,1464.1],[750,1050,1500,1650,1815,1996.5],[1417.5,1575,1750,1925,2100,2275],[1000,1400,2000,2200,2420,2662],[1250,1750,2500,2750,3025,3327.5],[1400,1960,2800,3080,3388,3726.8],[1500,2100,3000,3300,3630,3993],[1650,2310,3300,3630,3993,4392.3],[1750,2450,3500,3850,4235,4658.5]];
        const tat_metas = [10,20,30,40,50,60,70,80,90,100];
        const tat_percs = [100,110,120,130];
        const tat_grid = [[100,110,121,133.1],[200,220,242,266.2],[300,330,363,399.3],[400,440,484,532.4],[500,550,605,665.5],[600,660,726,798.6],[700,770,847,931.7],[800,880,968,1064.8],[900,990,1089,1197.9],[1000,1100,1210,1331]];

        // ========== FUNÇÕES AUXILIARES ==========
        function floor10(p) { return Math.floor(p/10)*10; }
        function findIdx(arr,v) { if(v<=0||v<arr[0])return-1; for(let i=arr.length-1;i>=0;i--)if(v>=arr[i])return i; return-1; }
        function fmt(v) { return 'R$ '+v.toFixed(2).replace('.',',').replace(/\B(?=(\d{3})+(?!\d))/g,'.'); }
        function faixaTxt(metas,v,tipo){ if(v<=0)return'📊 Faixa: digite a meta'; let i=findIdx(metas,v); if(i==-1)return v<metas[0]?'⚠️ Abaixo de '+metas[0]:'📊 Faixa: '+metas[metas.length-1].toLocaleString('pt-BR')+'+'; return'📊 Faixa: '+metas[i].toLocaleString('pt-BR')+(tipo=='pontos'?' pts':(tipo=='clientes'?' clientes':'')); }
        function taticoFaixa(me){ if(me<=0)return'📌 Aguardando meta de clientes'; let i=findIdx(tat_metas,me); if(i==-1)return me<tat_metas[0]?'📌 Abaixo de '+tat_metas[0]+' clientes':'📌 Faixa '+tat_metas[tat_metas.length-1]+'+ clientes (50%)'; return'📌 Baseado na faixa de '+tat_metas[i]+' clientes (50%)'; }

        // ========== FORMATAÇÃO COM PONTOS ==========
        function formatNumber(input) {
            let value = input.value.replace(/\./g, '').replace(/\D/g, '');
            if (value === '') {
                input.value = '';
                calc();
                return;
            }
            let number = parseInt(value, 10);
            if (isNaN(number)) {
                input.value = '';
                calc();
                return;
            }
            input.value = number.toLocaleString('pt-BR');
            calc();
        }

        function getRawValue(id) {
            let el = document.getElementById(id);
            if (!el) return 0;
            let raw = el.value.replace(/\./g, '').replace(/\D/g, '');
            return parseFloat(raw) || 0;
        }

        // ========== CÁLCULOS ==========
        function calcP(){
            let m = getRawValue('p_m');
            let r = getRawValue('p_r');
            document.getElementById('p_faixa_info').innerHTML=faixaTxt(pt_metas,m,'pontos');
            if(m<=0||r<=0)return 0; 
            let p=(r/m)*100, pf=floor10(p); 
            document.getElementById('p_perc').innerHTML=p.toFixed(1)+'% (paga '+pf+'%)';
            if(pf<70||pf>220)return 0; 
            let row=findIdx(pt_metas,m), col=pt_percs.indexOf(pf); 
            if(row==-1||col==-1)return 0; 
            return pt_grid[row][col];
        }
        
        function calcE(){
            let m=parseFloat(document.getElementById('e_m').value)||0, r=parseFloat(document.getElementById('e_r').value)||0;
            document.getElementById('e_faixa_info').innerHTML=faixaTxt(ef_metas,m,'clientes');
            if(m<=0||r<=0)return 0; 
            let p=(r/m)*100, pf=floor10(p); 
            document.getElementById('e_perc').innerHTML=p.toFixed(1)+'% (paga '+pf+'%)';
            if(pf<80||pf>130)return 0; 
            let row=findIdx(ef_metas,m), col=ef_percs.indexOf(pf); 
            if(row==-1||col==-1)return 0; 
            return ef_grid[row][col];
        }
        
        function calcT(me,mt,rt,percId,faixaId){
            document.getElementById(faixaId).innerHTML=taticoFaixa(me);
            if(me<=0||mt<=0||rt<=0){document.getElementById(percId).innerHTML='0%'; return 0;}
            let p=(rt/mt)*100, pf=floor10(p); 
            document.getElementById(percId).innerHTML=p.toFixed(1)+'% (paga '+pf+'%)';
            if(pf<100||pf>130)return 0; 
            let row=findIdx(tat_metas,me), col=tat_percs.indexOf(pf); 
            if(row==-1||col==-1)return 0; 
            return tat_grid[row][col]/2;
        }
        
        function atualizar(){
            let p=calcP(), e=calcE(), me=parseFloat(document.getElementById('e_m').value)||0;
            let t1=calcT(me,parseFloat(document.getElementById('t1_m').value)||0,parseFloat(document.getElementById('t1_r').value)||0,'t1_perc','t1_faixa_info');
            let t2=calcT(me,parseFloat(document.getElementById('t2_m').value)||0,parseFloat(document.getElementById('t2_r').value)||0,'t2_perc','t2_faixa_info');
            let pa=parseFloat(document.getElementById('a_perc').value)||0, va=getRawValue('a_r'), a=va*pa;
            document.getElementById('res_p').innerHTML=fmt(p); document.getElementById('res_e').innerHTML=fmt(e);
            document.getElementById('res_t1').innerHTML=fmt(t1); document.getElementById('res_t2').innerHTML=fmt(t2);
            document.getElementById('res_a').innerHTML=fmt(a); document.getElementById('res_total').innerHTML=fmt(p+e+t1+t2+a);
        }
        
        function limparTudo(){ 
            document.getElementById('p_m').value=''; document.getElementById('p_r').value='';
            document.getElementById('e_m').value=''; document.getElementById('e_r').value='';
            document.getElementById('t1_m').value=''; document.getElementById('t1_r').value='';
            document.getElementById('t2_m').value=''; document.getElementById('t2_r').value='';
            document.getElementById('a_r').value=''; document.getElementById('a_perc').value='0.01';
            atualizar(); 
        }
        
        function compartilharWhatsApp(){ 
            let msg='🛢️ *SIMULADOR - BETA* 🛢️%0A%0A💰 *TOTAL:* '+document.getElementById('res_total').innerHTML+'%0A%0A🏆 *PONTOS:* '+document.getElementById('res_p').innerHTML+'%0A'+document.getElementById('p_faixa_info').innerHTML+'%0A%0A🤝 *CLIENTES:* '+document.getElementById('res_e').innerHTML+'%0A'+document.getElementById('e_faixa_info').innerHTML+'%0A%0A🔧 *T1:* '+document.getElementById('res_t1').innerHTML+'%0A'+document.getElementById('t1_faixa_info').innerHTML+'%0A%0A⚙️ *T2:* '+document.getElementById('res_t2').innerHTML+'%0A'+document.getElementById('t2_faixa_info').innerHTML+'%0A%0A💰 *FATURAMENTO:* '+document.getElementById('res_a').innerHTML; 
            window.open('https://wa.me/?text='+msg,'_blank'); 
        }
        
        function calc() { atualizar(); }
        
        // Eventos
        document.getElementById('p_m').addEventListener('input', function(){ formatNumber(this); });
        document.getElementById('p_r').addEventListener('input', function(){ formatNumber(this); });
        document.getElementById('a_r').addEventListener('input', function(){ formatNumber(this); });
        document.getElementById('e_m').addEventListener('input', calc);
        document.getElementById('e_r').addEventListener('input', calc);
        document.getElementById('t1_m').addEventListener('input', calc);
        document.getElementById('t1_r').addEventListener('input', calc);
        document.getElementById('t2_m').addEventListener('input', calc);
        document.getElementById('t2_r').addEventListener('input', calc);
        document.getElementById('a_perc').addEventListener('change', calc);
        
        atualizar();
    </script>
</body>
</html>

