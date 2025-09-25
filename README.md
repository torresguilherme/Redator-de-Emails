#🤖 Assistente de E-mail Inteligente com n8n e OpenAI# 
Este projeto é um fluxo de trabalho (workflow) avançado construído na plataforma n8n, projetado para funcionar como um assistente de e-mail inteligente. Ele monitora uma caixa de entrada do Gmail, utiliza um modelo de linguagem da OpenAI para classificar se o remetente é um cliente e, em caso afirmativo, usa um agente de IA para redigir uma resposta e salvá-la como rascunho para revisão humana.

✨ Visão Geral do Fluxo
O objetivo é automatizar a triagem e a resposta inicial a e-mails de clientes, economizando tempo e garantindo que as respostas sejam consistentes e bem elaboradas. A etapa final de salvar como rascunho (em vez de enviar diretamente) mantém um humano no controle, combinando o melhor da automação com a supervisão necessária.

(Nota: Substitua o link acima pelo link da sua imagem no repositório, se desejar)

⚙️ Detalhamento do Fluxo
O workflow é dividido em uma sequência lógica de processamento e decisão:

Gmail Trigger (Gatilho):

Tipo: Gatilho do Gmail.

Função: O fluxo é iniciado sempre que um novo e-mail chega à caixa de entrada ou a uma pasta/label específica monitorada no Gmail.

É Cliente? (Nó de Classificação com IA):

Tipo: Nó de Modelo de Chat da OpenAI.

Função: Este é o primeiro passo de inteligência. O nó recebe o conteúdo do e-mail (remetente, assunto, corpo) e o envia para um modelo da OpenAI com um prompt específico, como: "Analisando o seguinte e-mail, determine se o remetente é um cliente. Responda apenas com 'true' ou 'false'.". O resultado dessa análise é usado para a tomada de decisão.

Condicional (Nó de Lógica):

Tipo: IF (Se).

Função: Este nó direciona o fluxo com base na saída do passo anterior.

Ramo true: Se o e-mail foi classificado como sendo de um cliente, o fluxo continua para a etapa de redação da resposta.

Ramo false: Se o e-mail não é de um cliente, o fluxo segue para um nó "No Operation", encerrando a automação para aquele e-mail.

Redator (Agente de IA para Redação):

Tipo: LangChain Agent ou um nó similar de IA complexa.

Função: Esta é a parte mais sofisticada do workflow. É um agente de IA configurado para gerar uma resposta de e-mail coerente e contextual. Seus componentes são:

Model (4.1-mini): Utiliza um modelo de linguagem eficiente (provavelmente o GPT-4o mini da OpenAI) como o "cérebro" para gerar o texto.

Tool (Think): Possui ferramentas que permitem ao agente "pensar" ou realizar subtarefas antes de formular a resposta final, garantindo maior qualidade.

Structured Output Parser: Garante que a saída do modelo de IA seja sempre em um formato estruturado (ex: JSON com os campos subject e body). Isso é crucial para que o próximo nó possa usar os dados de forma confiável, sem erros de formatação.

Chat Model Memory: Permite que o agente se lembre do contexto da conversa (neste caso, o e-mail original) ao formular a resposta.

Salvar Resposta como Rascunho (Ação Final):

Tipo: Nó do Gmail.

Função: Utiliza a saída estruturada do agente "Redator" para criar um novo rascunho (create: draft) na sua conta do Gmail. O assunto e o corpo do rascunho são preenchidos dinamicamente com o texto gerado pela IA.

🛠️ Tecnologias Utilizadas
n8n: Plataforma de automação que orquestra todo o fluxo.

API do Gmail: Para ler e-mails recebidos e criar rascunhos.

API da OpenAI: Para alimentar os modelos de linguagem (LLMs) que realizam a classificação e a redação.

Conceitos de LangChain: A estrutura do nó "Redator" (com modelos, ferramentas e parsers) é fortemente inspirada na biblioteca LangChain para construção de aplicações com LLMs.

🔧 Pré-requisitos
Uma instância do n8n (seja no n8n.cloud ou auto-hospedada).

Uma conta da OpenAI com uma chave de API válida.

Uma conta do Google com as credenciais da API do Gmail configuradas no n8n.

🚀 Como Configurar
Importe o Fluxo: Copie o código JSON do workflow e importe-o para a sua instância do n8n.

Configure o Gatilho do Gmail: Selecione sua credencial do Gmail e configure o gatilho para monitorar a caixa de entrada ou a pasta desejada.

Configure os Nós de IA:

Adicione sua chave de API da OpenAI às credenciais do n8n.

Associe a credencial aos nós "É Cliente?" e "Redator".

Crucial: Revise e ajuste os prompts dentro desses nós para alinhar com o tom de voz da sua empresa e a especificidade das tarefas. A qualidade da automação depende diretamente da qualidade dos seus prompts!

Configure a Ação do Gmail:

No nó "Salvar Resposta como Rascunho", selecione sua credencial do Gmail.

Mapeie os campos de saída do nó "Redator" para os campos do rascunho (ex: {{ $json.output.subject }} para o assunto e {{ $json.output.body }} para o corpo do e-mail).

Ative o Workflow: Salve as alterações e ative o fluxo.

💡 Considerações Importantes
Privacidade: Lembre-se que o conteúdo dos e-mails será processado pela API da OpenAI. Certifique-se de que isso esteja em conformidade com suas políticas de privacidade e as dos seus clientes.

Custos: O uso da API da OpenAI gera custos por token processado. Monitore seu uso para evitar surpresas na fatura.

Prompt Engineering: O sucesso deste fluxo depende 90% da qualidade dos seus prompts. Teste e refine os comandos dados à IA para obter os melhores resultados. Comece com instruções claras e dê exemplos se necessário.

<img width="1596" height="696" alt="RETADOR EMAIL 1" src="https://github.com/user-attachments/assets/7e1b0fa6-b515-4248-9048-056e9ce2be12" />
<img width="628" height="620" alt="RETADOR EMAIL 2" src="https://github.com/user-attachments/assets/6c4c8394-5016-470e-96a6-38eb1f61b82b" />
<img width="512" height="187" alt="RETADOR EMAIL 3" src="https://github.com/user-attachments/assets/53016428-4997-4e5c-b493-6eb7ec4c6233" />


