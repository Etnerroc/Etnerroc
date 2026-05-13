<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Etner Roc - Fundo Comunitário</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <style>
        :root { --primary: #1e40af; --accent: #3b82f6; --bg: #f1f5f9; }
        body { font-family: 'Segoe UI', system-ui, sans-serif; background: var(--bg); margin: 0; display: flex; justify-content: center; padding: 20px; }
        .app-card { background: white; width: 100%; max-width: 400px; border-radius: 20px; shadow: 0 10px 25px rgba(0,0,0,0.1); padding: 25px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); }
        .header { text-align: center; border-bottom: 2px solid #e2e8f0; margin-bottom: 20px; padding-bottom: 10px; }
        .header h1 { color: var(--primary); margin: 0; font-size: 24px; text-transform: uppercase; letter-spacing: 1px; }
        .balance-box { background: var(--primary); color: white; padding: 15px; border-radius: 12px; text-align: center; margin-bottom: 20px; }
        .balance-box span { font-size: 22px; font-weight: bold; }
        label { display: block; margin: 10px 0 5px; font-weight: 600; color: #475569; }
        input, select { width: 100%; padding: 12px; border: 1px solid #cbd5e1; border-radius: 8px; margin-bottom: 15px; box-sizing: border-box; }
        button { width: 100%; padding: 15px; background: var(--primary); color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; font-size: 16px; }
        #receipt { display: none; margin-top: 20px; padding: 15px; border: 2px solid var(--accent); border-radius: 10px; background: #eff6ff; }
        .qrcode-area { text-align: center; margin-top: 15px; }
        #qrcode { display: inline-block; padding: 10px; background: white; border-radius: 10px; }
    </style>
</head>
<body>

<div class="app-card">
    <div class="header">
        <h1>Etner Roc</h1>
        <small>Plataforma de Auxílio Mútuo</small>
    </div>

    <div class="balance-box">
        <small>Saldo do Fundo (100 Membros)</small><br>
        <span>R$ 10.000,00</span>
    </div>

    <label>Valor do Saque (R$)</label>
    <input type="number" id="amount" placeholder="0,00">

    <label>Taxa de Devolução</label>
    <select id="fee">
        <option value="0.15">15% (Retorno Padrão)</option>
        <option value="0.18">18% (Urgência)</option>
    </select>

    <button onclick="generateTransaction()">SOLICITAR CRÉDITO</button>

    <div id="receipt">
        <h4 style="margin:0">Recibo Digital</h4>
        <p id="details" style="font-size: 14px; color: #334155;"></p>
        <div class="qrcode-area">
            <small>QR Code para Devolução em 30 dias</small><br>
            <div id="qrcode"></div>
        </div>
    </div>
</div>

<script>
    function generateTransaction() {
        const val = parseFloat(document.getElementById('amount').value);
        const fee = parseFloat(document.getElementById('fee').value);
        if (!val) return alert("Digite um valor");

        const total = val + (val * fee);
        const deadline = new Date();
        deadline.setDate(deadline.getDate() + 30);

        document.getElementById('receipt').style.display = 'block';
        document.getElementById('details').innerHTML = `
            <b>Saque:</b> R$ ${val.toFixed(2)}<br>
            <b>Acréscimo:</b> R$ ${(val*fee).toFixed(2)}<br>
            <b>Total a devolver:</b> R$ ${total.toFixed(2)}<br>
            <b>Data Limite:</b> ${deadline.toLocaleDateString('pt-BR')}
        `;

        document.getElementById('qrcode').innerHTML = "";
        new QRCode(document.getElementById("qrcode"), {
            text: "DEVOLUCAO-ETNER-ROC-" + total.toFixed(2),
            width: 120, height: 120
        });
    }
</script>

</body>
</html>
