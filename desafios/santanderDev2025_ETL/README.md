# 📘 ETL com Google Sheets + OpenAI
Adaptação do desafio Santander Dev Week 2023

Este projeto recria o fluxo ETL (Extract, Transform, Load) apresentado no desafio original da Santander Dev Week 2023, mas com uma adaptação fundamental:
👉 como não temos acesso à API original do Santander, utilizamos o Google Sheets como fonte e destino dos dados.

Isso permite simular com fidelidade um pipeline de dados real, usando ferramentas acessíveis e gratuitas — ideal para estudos.

## 🚀 Objetivo do Projeto

Criar um fluxo ETL completo utilizando:

Google Sheets → Fonte e destino dos dados

Python + Google Colab → Ambiente de execução

OpenAI GPT (gpt-4o-mini) → Camada de inteligência para gerar mensagens personalizadas

Simulação de API → Já que a API original não está disponível

O resultado final é:
✔ Buscar IDs de clientes de uma planilha
✔ Simular os dados desses clientes
✔ Criar mensagens de marketing personalizadas via IA
✔ Gravar essas mensagens de volta na planilha

## 🧩 Por que usar o Google Sheets?

A escolha pelo Google Sheets foi feita por três motivos práticos:

🔹 1. A API original não está disponível

O desafio da Santander Dev Week utiliza endpoints reais, mas que não funcionam fora do evento.
Para simular o fluxo, é preciso outra fonte de dados — e o Sheets cumpre bem esse papel.

🔹 2. Facilidade de edição e visualização

A planilha permite:

editar manualmente IDs

visualizar resultados rapidamente

manter um histórico claro da transformação

Sem necessidade de bancos de dados ou servidores.

🔹 3. Integração imediata com o Google Colab

O Colab tem suporte nativo ao Google Sheets via APIs.
Não exige chaves extras, instalação de banco de dados, nem configuração complicada.

Resultado:
✔ Uma solução leve
✔ Gratuita
✔ Perfeita para estudo e prototipagem

🛠 Tecnologias Utilizadas
Tecnologia	Função
Python	Linguagem principal
Google Colab	Ambiente de execução
Google Sheets API	Armazenamento e leitura dos dados
OpenAI API (gpt-4o-mini)	Geração das mensagens
gspread	Leitura/escrita da planilha
oauth2client	Autenticação com Google
📂 Estrutura Geral do Fluxo ETL
1. Extract — Extração

Ler IDs do Google Sheets

Criar usuários simulados

2. Transform — Transformação

Enviar dados dos clientes para o modelo de IA

Gerar mensagens personalizadas de marketing

3. Load — Carga

Registrar as mensagens na mesma planilha

📜 Passo a Passo Completo
1️⃣ Instalar dependências
!pip install gspread oauth2client openai

2️⃣ Autenticação com Google
from google.colab import auth
auth.authenticate_user()

import gspread
from oauth2client.client import GoogleCredentials

gc = gspread.authorize(GoogleCredentials.get_application_default())

3️⃣ Conectar à planilha

Crie uma planilha com a coluna UserID

Copie o ID da planilha

sheet_id = "COLE_AQUI_O_ID_DA_SUA_PLANILHA"

sheet = gc.open_by_key(sheet_id).sheet1
user_ids = sheet.col_values(1)[1:]
user_ids = [int(x) for x in user_ids]

4️⃣ Configurar OpenAI
from openai import OpenAI
client = OpenAI(api_key="SUA_API_KEY_AQUI")

5️⃣ Simular dados dos clientes
def get_user_simulado(id):
    nomes = ["Ana", "Bruno", "Carla", "Diego", "Eduarda"]
    return {
        "id": id,
        "name": nomes[(id - 1) % len(nomes)],
        "account": {"balance": round(1000 * id * 0.77, 2)}
    }

6️⃣ Gerar mensagens personalizadas
def gerar_mensagem(user):
    prompt = (
        f"Crie uma mensagem curta (máx 100 caracteres) "
        f"para {user['name']} sobre a importância dos investimentos."
    )

    completion = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": "Você é especialista em marketing bancário."},
            {"role": "user", "content": prompt}
        ]
    )
    return completion.choices[0].message.content

7️⃣ Gravar na planilha
sheet.update("B1", "Name")
sheet.update("C1", "News")

for idx, user in enumerate(users, start=2):
    sheet.update(f"B{idx}", user["name"])
    sheet.update(f"C{idx}", user["news"])

📦 Resultado Final

Ao final do processo, você terá uma planilha assim:

UserID	Name	News
1	Ana	"Ana, investir hoje garante tranquilidade amanhã."
2	Bruno	"Bruno, faça seu dinheiro trabalhar por você!"
3	Carla	...

Isso simula perfeitamente o mesmo fluxo do desafio original — mas usando ferramentas acessíveis para qualquer pessoa.

🎯 Conclusão

Este projeto mostra como é possível recriar um pipeline ETL completo, mesmo sem acesso às APIs originais, utilizando:

Google Sheets como banco de dados

OpenAI como camada inteligente

Python + Colab como facilitadores

O resultado é um fluxo funcional, moderno, escalável e ideal para estudos.