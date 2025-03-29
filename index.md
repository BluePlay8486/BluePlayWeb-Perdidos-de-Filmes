<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pedidos de Filmes e Séries</title>
    <!-- Importando Firebase -->
    <script type="module" src="https://www.gstatic.com/firebasejs/10.0.0/firebase-app.js"></script>
    <script type="module" src="https://www.gstatic.com/firebasejs/10.0.0/firebase-database.js"></script>
    <style>
        /* Estilo geral da página */
        body {
            font-family: Arial, sans-serif;
            background-color: #121212;
            color: #e0e0e0;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 400px;
            background: #1f1f1f;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
            text-align: center;
        }

        h2 {
            color: #ff7f00; /* Cor laranja para título */
            margin-bottom: 20px;
        }

        input {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 1px solid #333;
            border-radius: 5px;
            background-color: #2c2c2c;
            color: #fff;
            font-size: 16px;
        }

        input:focus {
            outline: none;
            border-color: #ff7f00;
        }

        button {
            width: 100%;
            padding: 12px;
            background-color: #ff7f00; /* Cor laranja para botão */
            color: #fff;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #e76f00; /* Cor laranja mais escuro ao passar o mouse */
        }

        .alert {
            margin-top: 20px;
            padding: 10px;
            background-color: #4caf50;
            color: #fff;
            border-radius: 5px;
        }

        .alert.error {
            background-color: #f44336;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Faça seu Pedido</h2>
        <input type="text" id="nomeFilme" placeholder="Nome do Filme ou Série">
        <input type="text" id="nomePessoa" placeholder="Seu Nome">
        <button onclick="enviarPedido()">Enviar Pedido</button>
        <div id="alertBox"></div> <!-- Área para mensagens de sucesso/erro -->
    </div>

    <script type="module">
        // Inicializa o Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.0.0/firebase-app.js";
        import { getDatabase, ref, push } from "https://www.gstatic.com/firebasejs/10.0.0/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAkmmFDwhkcY6Z4UJMYHPqHrTSPTfvop8s",
            authDomain: "blueplayweb.firebaseapp.com",
            databaseURL: "https://blueplayweb-default-rtdb.firebaseio.com",
            projectId: "blueplayweb",
            storageBucket: "blueplayweb.appspot.com",
            messagingSenderId: "1010274882829",
            appId: "1:1010274882829:web:8188c8a0eb7b02d4d4cc44",
            measurementId: "G-Q5SDTRY084"
        };

        // Inicializa o app Firebase
        const app = initializeApp(firebaseConfig);
        const database = getDatabase(app);

        // Função para enviar o pedido para o Realtime Database
        window.enviarPedido = async function () {
            const nomeFilme = document.getElementById("nomeFilme").value;
            const nomePessoa = document.getElementById("nomePessoa").value;
            const alertBox = document.getElementById("alertBox");

            // Verifica se os campos estão preenchidos
            if (nomeFilme && nomePessoa) {
                try {
                    // Envia os dados para o Firebase
                    await push(ref(database, "pedidos"), {
                        filme: nomeFilme,
                        pessoa: nomePessoa,
                        status: "pendente"
                    });

                    // Exibe mensagem de sucesso
                    alertBox.innerHTML = "<div class='alert'>Pedido enviado com sucesso!</div>";
                    document.getElementById("nomeFilme").value = ''; // Limpa o campo de nome do filme
                    document.getElementById("nomePessoa").value = ''; // Limpa o campo de nome da pessoa

                } catch (error) {
                    // Exibe mensagem de erro
                    alertBox.innerHTML = "<div class='alert error'>Erro ao enviar pedido: " + error + "</div>";
                }
            } else {
                // Alerta caso campos estejam vazios
                alertBox.innerHTML = "<div class='alert error'>Preencha todos os campos!</div>";
            }
        };
    </script>
</body>
</html>
