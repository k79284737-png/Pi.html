<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Coleta de Dados Mobile</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
        #info { margin-top: 20px; background: #f0f0f0; padding: 15px; border-radius: 8px; display: none; }
    </style>
</head>
<body>

    <h2>Acesse para coletar dados</h2>
    <p>Toque no botão abaixo para permitir a geolocalização.</p>
    <button onclick="coletarDados()">Coletar Informações</button>

    <div id="info"></div>

    <script>
        async function coletarDados() {
            const infoDiv = document.getElementById('info');
            let dados = {};

            // 1. Coletar Geolocalização (requer permissão do usuário)
            if ("geolocation" in navigator) {
                try {
                    const posicao = await navigator.geolocation.getCurrentPosition();
                    dados.latitude = posicao.coords.latitude;
                    dados.longitude = posicao.coords.longitude;
                } catch (e) {
                    dados.erro_geo = "Geolocalização negada ou indisponível";
                }
            } else {
                dados.erro_geo = "Geolocalização não suportada";
            }

            // 2. Coletar User-Agent (Modelo do celular/SO)
            dados.userAgent = navigator.userAgent;

            // 3. Coletar Informações de Rede (Wi-Fi/4G)
            if ("connection" in navigator) {
                dados.tipo_conexao = navigator.connection.effectiveType; // 4g, wifi, etc.
                dados.downlink = navigator.connection.downlink; // Velocidade estimada
            }

            // 4. Coletar Bateria (se suportado)
            if ("getBattery" in navigator) {
                const bateria = await navigator.getBattery();
                dados.nivel_bateria = Math.round(bateria.level * 100) + "%";
                dados.carregando = bateria.charging ? "Sim" : "Não";
            }

            // Exibir os dados na tela (simulando envio para um servidor)
            infoDiv.style.display = 'block';
            infoDiv.innerHTML = `
                <h3>Dados Coletados:</h3>
                <pre>${JSON.stringify(dados, null, 2)}</pre>
            `;

            // Aqui você enviaria os dados para seu servidor backend (ex: via fetch)
            // await fetch('https://seu-servidor.com/coleta', { method: 'POST', body: JSON.stringify(dados) });
        }
    </script>

</body>
</html>
