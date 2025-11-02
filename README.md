# 🤖 Projeto DIO: Chatbot IA para Análise de TCC com Azure AI

Este projeto foi desenvolvido como solução para o desafio "Criando um Chat Interativo com IA Generativa para Análise de Documentos" da DIO.

O objetivo foi construir um sistema de chat inteligente capaz de analisar uma base de documentos (PDFs e PPTXs), permitindo ao usuário fazer perguntas e receber respostas baseadas **exclusivamente** no conteúdo desses arquivos.

## 1. 🎯 O Desafio (Cenário)

O cenário proposto foi o de um estudante de Engenharia de Software prestes a escrever seu TCC. O estudante possui diversos artigos científicos em PDF, mas encontra dificuldade em correlacionar informações e extrair ideias-chave de múltiplos textos.

A solução foi criar um sistema de busca inteligente (RAG - Retrieval-Augmented Generation) para interpretar os PDFs, organizar as informações e gerar respostas relevantes com base no conteúdo carregado.

## 2. ✅ Objetivos do Projeto (Entregues)

Conforme a descrição do desafio, o projeto atingiu os seguintes objetivos:

* ✅ **Carregar arquivos PDF:** Realizamos o upload de múltiplos arquivos `.pdf` e `.pptx` contendo a base de conhecimento sobre Machine Learning.
* ✅ **Implementar um sistema de busca vetorial:** Utilizamos o **Azure AI Search** para indexar o conteúdo dos documentos e permitir a busca vetorial.
* ✅ **Utilizar inteligência artificial para gerar respostas:** Usamos o modelo **`gpt-4o`** do Azure AI Studio para entender a pergunta e gerar uma resposta.
* ✅ **Desenvolver um chat interativo:** O **Playground do AI Foundry** foi usado para conectar a IA à busca vetorial (RAG) e validar o chat em tempo real, confirmando que as respostas eram baseadas nos documentos.

## 3. 🛠️ Arquitetura e Ferramentas

* **Orquestração:** Azure AI Foundry (Azure AI Studio)
* **Modelo de Geração (LLM):** `gpt-4o` (OpenAI)
* **Modelo de Vetorização:** `text-embedding-3-large` (OpenAI)
* **Armazenamento de Dados:** Azure Blob Storage
* **Busca Vetorial (Indexação):** Azure AI Search (Camada "Basic")

## 4. 📄 Documentação do Processo (Passo a Passo)

Abaixo está o fluxo de trabalho detalhado, desde a criação dos recursos até o teste do chatbot.

### 4.1. Criação dos Recursos Principais

O primeiro passo foi provisionar os serviços no portal do Azure. Criamos o **AI Foundry** (`projetochatbot...`) e o serviço de **Azure AI Search** (`diosearchsystem`).

####
<img width="1286" height="340" alt="image" src="https://github.com/user-attachments/assets/cf141e7b-7e32-49c6-9f09-9065eed869b5" />
*(Print mostrando a criação do recurso AI Foundry)*

####
<img width="1285" height="596" alt="image" src="https://github.com/user-attachments/assets/a24b0307-9353-4b3c-9c95-7a7097ce1918" />
*(Print mostrando a criação do serviço Azure AI Search)*
####

### 4.2. Conexão de Dados (Ingestão)

No Playground do AI Foundry, iniciamos o assistente para "Adicionar dados" e fizemos o upload dos arquivos que serviriam como base de conhecimento (nossos "artigos de TCC").

####
<img width="1008" height="689" alt="image" src="https://github.com/user-attachments/assets/5c58d6a2-7705-4db5-aa24-199798f708aa" />
*(Print da tela de upload de arquivos PDF e PPTX)*
####

### 4.3. ⚠️ Insight 1: Limitação da Camada Gratuita (AI Search)

Encontramos o primeiro bloqueio: a camada **Gratuita (Free)** do Azure AI Search **não é suportada** pelo assistente do AI Foundry.

* **Erro:** "Não há suporte para a camada gratuita. Selecione outro recurso."
* **Solução:** Foi necessário provisionar um novo serviço de AI Search em uma camada paga (ex: "Basic", que utilizei como `searchdiopay`) para prosseguir.
####
<img width="834" height="607" alt="image" src="https://github.com/user-attachments/assets/762f3d95-ffa4-4ad0-be4d-39e1d34cb3de" />
*(Print do erro ao tentar usar o AI Search gratuito)*
####

### 4.4. ⚠️ Insight 2: Permissões de Acesso (IAM)

Após criar o serviço de Search pago, o pipeline de ingestão falhou. Isso ocorreu porque o AI Foundry não tinha permissão para escrever no índice do AI Search.

* **Solução:** Foi preciso acessar o recurso de Search, ir até **"IAM (Controle de acesso)"** e atribuir a função de **"Contribuidor do Serviço de Pesquisa"** (Search Service Contributor) para a identidade gerenciada do AI Foundry.
####
<img width="1366" height="766" alt="image" src="https://github.com/user-attachments/assets/47750c2d-dad3-4f58-896a-8393e551bc15" />
*(Print da tela de atribuição de função IAM)*
####

### 4.5. Indexação e Processamento

Com as permissões corrigidas, o AI Foundry iniciou a ingestão, processando e indexando os 23 documentos com sucesso.

####
<img width="290" height="223" alt="image" src="https://github.com/user-attachments/assets/402d6da9-85e6-4633-a2ca-d8459fa4799f" />
*(Print mostrando o status "Ingestão em andamento")*
####

<img width="1353" height="638" alt="image" src="https://github.com/user-attachments/assets/0416fb36-3e4e-4684-8cdd-1142b50fb0d6" />
*(Print do índice "exercicio" criado com 23 documentos no AI Search)*
####


### 4.6. Resultado Final: Chat RAG em Funcionamento

No Playground de Chat, conectamos tudo:
1.  **Modelo:** `gpt-4o`
2.  **Fonte de Dados:** Nosso índice `exercicio` do AI Search.
3.  **Prompt do Sistema:** "Você é um especialista em Marchining Learning..."

Fizemos a pergunta: "como funciona o macnine learning".

O modelo respondeu corretamente e, o mais importante, **citou as 4 referências dos documentos** que havíamos carregado. Isso confirma que o padrão RAG (Busca Aumentada por Recuperação) funcionou perfeitamente.

####
<img width="855" height="626" alt="image" src="https://github.com/user-attachments/assets/9451ec3f-bea3-4e1b-820f-35b3c9ccee13" />
*(Print da pergunta do usuário e o início da resposta do chat)*
####

<img width="852" height="638" alt="image" src="https://github.com/user-attachments/assets/78f122a7-0251-4f03-9af0-3481100d47b8" />
*(Print do final da resposta, mostrando as "4 referências" aos arquivos originais)*
####

## 5. 💡 Insights e Possibilidades

O principal insight deste projeto foi entender as limitações das contas *Free Trial* do Azure para implantação de IA.

Embora o **Playground funcione perfeitamente para prototipação e validação** (como provado acima), a publicação do chatbot como um Aplicativo Web independente foi bloqueada.

Ao tentar implantar usando o plano de preços `Free (F1)`, o assistente de implantação trava, pois a camada gratuita não oferece flexibilidade de região, impedindo o provisionamento dos recursos.
####
<img width="544" height="667" alt="image" src="https://github.com/user-attachments/assets/83e69bbf-c47f-4b51-94ba-adaf23e935c4" />
*(Print do bloqueio final ao tentar implantar o App Web com campo 'local' bloqueado)*
####

## **Conclusão:** O Azure AI Foundry é uma ferramenta extremamente poderosa para criar soluções RAG complexas em minutos. A conta gratuita é excelente para aprender e prototipar, mas qualquer implantação real (seja API ou Web App) exigirá recursos pagos, como um plano "Basic" do AI Search e um App Service Plan pago.
