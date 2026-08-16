<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Identity Generator</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    font-family: Arial, Helvetica, sans-serif;
    background: radial-gradient(circle at top, #222 0%, #0b0b0b 45%, #050505 100%);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.container {
    width: 100%;
    max-width: 440px;
}

.logo {
    text-align: center;
    margin-bottom: 25px;
}

.logo-icon {
    width: 65px;
    height: 65px;
    margin: auto;
    border-radius: 18px;
    background: #171717;
    border: 1px solid #333;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 30px;
    box-shadow: 0 10px 35px #000;
}

.logo h1 {
    margin: 15px 0 5px;
    font-size: 27px;
}

.logo p {
    margin: 0;
    color: #888;
    font-size: 14px;
}

.app {
    background: rgba(18,18,18,.94);
    border: 1px solid #292929;
    border-radius: 25px;
    padding: 22px;
    box-shadow: 0 25px 70px rgba(0,0,0,.7);
}

.generate {
    width: 100%;
    border: 0;
    border-radius: 15px;
    padding: 16px;
    background: white;
    color: #050505;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: .2s;
}

.generate:hover {
    transform: translateY(-2px);
}

.card {
    margin-top: 16px;
    padding: 17px;
    background: #111;
    border: 1px solid #292929;
    border-radius: 17px;
}

.label {
    color: #777;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 8px;
}

.name {
    font-size: 21px;
    font-weight: bold;
}

.email {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    background: #181818;
    border: 1px solid #292929;
    border-radius: 12px;
    padding: 11px;
    margin-top: 9px;
}

.email span {
    font-size: 14px;
    color: #ddd;
    word-break: break-all;
}

.copy {
    flex-shrink: 0;
    border: 1px solid #333;
    background: #222;
    color: white;
    border-radius: 9px;
    padding: 8px 11px;
    cursor: pointer;
}

.copy:hover {
    background: #303030;
}

.footer {
    text-align: center;
    color: #555;
    font-size: 11px;
    margin-top: 18px;
}
</style>
</head>

<body>

<div class="container">

    <div class="logo">

        <div class="logo-icon">👤</div>

        <h1>email Generator</h1>

        <p>Gerador de emails</p>

    </div>

    <div class="app">

        <button class="generate" onclick="gerar()">
            ✦ GERAR NOVA IDENTIDADE
        </button>

        <div class="card">

            <div class="label">
                
            </div>

            <div id="nome" class="name">
                Clique no botão acima
            </div>

        </div>

        <div class="card">

            <div class="label">
                E-mails 
            </div>

            <div id="emails">

                <div class="email">
                    <span>Nenhum e-mail gerado</span>
                </div>

            </div>

        </div>

    </div>

    <div class="footer">
        Apenas sugestões fictícias • Não cria contas Gmail
    </div>

</div>

<script>

const nomes = [
"Lucas","Rafael","Gabriel","Matheus","Pedro","Gustavo","Felipe","João","Miguel","Arthur",
"Bruno","Caio","Henrique","Daniel","Leonardo","Vitor","Enzo","Nicolas","Davi","Samuel",
"Theo","Murilo","Luan","Vinicius","Eduardo","André","Thiago","Diego","Ryan","Alex",

"Adrian","Alan","Alexandre","Alisson","Anderson","André","Antônio","Augusto","Benjamin","Bernardo",
"Breno","Bryan","Cauã","César","Christian","Cristiano","Danilo","Darlan","Davi","David",
"Denis","Diego","Douglas","Elias","Eliseu","Emerson","Erick","Estevão","Evan","Everton",
"Fabrício","Fábio","Fernando","Flávio","Francisco","Frederico","Gael","Geovane","Gilberto","Giovanni",
"Guilherme","Heitor","Hugo","Igor","Isaac","Isaque","Ivan","Jair","Jeferson","Jefferson",
"Joaquim","Jonas","Jonathan","Jorge","José","Juan","Juliano","Kaique","Kevin","Leandro",
"Lucas","Luciano","Luiz","Márcio","Marco","Marcos","Marcelo","Marlon","Martin","Martins",
"Mateus","Maurício","Max","Maxwell","Michael","Nathan","Nathaniel","Nelson","Noah","Otávio",
"Pablo","Patrick","Paulo","Raul","Renan","Renato","Ricardo","Roberto","Rodrigo","Rogério",
"Ruan","Rubens","Sandro","Saulo","Sérgio","Tales","Tarcísio","Tomás","Valentim","Valter",
"Vicente","Victor","Wagner","Wellington","William","Yago","Yuri","Zaqueu","Adriel","Afonso",
"Alberto","Alec","Alexis","Alfredo","Álvaro","Amaro","Américo","Anselmo","Arnaldo","Breno",
"Celso","Cláudio","Clemente","Conrado","Dênis","Edgar","Edson","Emanuel","Emanuelly","Estevão",
"Ezequiel","Fausto","Felipe","Floriano","Gaspar","Geraldo","Gerson","Gustavo","Hélio","Horácio",
"Ismael","Júlio","Lázaro","Lourenço","Manoel","Mário","Matias","Moisés","Natan","Norberto",
"Osvaldo","Otto","Pascoal","Ramon","Reinaldo","Rômulo","Salvador","Sebastião","Silas","Tadeu",
"Teodoro","Timóteo","Ubirajara","Valdir","Valdomiro","Vanderlei","Washington","Wesley","Willian","Wilson",
"Zion","Abel","Abner","Adam","Ariel","Caleb","Dylan","Ethan","Henry","Ian"
];

const sobrenomes = [
    "Almeida",
    "Souza",
    "Silva",
    "Oliveira",
    "Santos",
    "Costa",
    "Pereira",
    "Rodrigues",
    "Ferreira",
    "Gomes",
    "Martins",
    "Barbosa",
    "Ribeiro",
    "Carvalho",
    "Mendes",
    "Teixeira",
    "Moreira",
    "Nunes",
    "Lopes",
    "Moura",
    "Dias",
    "Castro",
    "Pinto",
    "Freitas",
    "Vieira",
    "Campos",
    "Cardoso",
    "Ramos",
    "Araujo",
    "Monteiro"
];

function escolher(lista) {
    return lista[Math.floor(Math.random() * lista.length)];
}

function limpar(texto) {
    return texto
        .normalize("NFD")
        .replace(/[\u0300-\u036f]/g, "")
        .toLowerCase();
}

function numero() {
    return Math.floor(100 + Math.random() * 900);
}

function gerar() {

    const nome = escolher(nomes);
    const sobrenome = escolher(sobrenomes);

    const n = limpar(nome);
    const s = limpar(sobrenome);

    document.getElementById("nome").textContent =
        nome + " " + sobrenome;

    const emails = [
    `${n}.${s}${numero()}@gmail.com`,
    `${n}${s}${numero()}@gmail.com`,
    `${n}.${s}${Math.floor(10 + Math.random() * 90)}@gmail.com`,
    `${n}_${s}${numero()}@gmail.com`,
    `${n}${Math.floor(1000 + Math.random() * 9000)}@gmail.com`,
    `${n}.${s}${Math.floor(100 + Math.random() * 900)}@gmail.com`
];

    document.getElementById("emails").innerHTML =
        emails.map(email => `

            <div class="email">

                <span>${email}</span>

                <button
                    class="copy"
                    onclick="copiar('${email}')">

                    Copiar

                </button>

            </div>

        `).join("");
}

function copiar(email) {

    navigator.clipboard.writeText(email)
        .then(() => {
            alert("E-mail copiado!");
        })
        .catch(() => {
            alert(email);
        });

}

</script>

</body>
</html>
