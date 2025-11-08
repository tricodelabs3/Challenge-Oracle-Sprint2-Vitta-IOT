![Banner de Saúde e Tecnologia](https://storage.googleapis.com/gemini-prod-us-public/images/20240507/115502_healthcare_tech_banner.png)
# CHALLENGE ORACLE 2025: Projeto Vitta (Sprint 2)
## Disruptive Architectures: IoT, IoB & Generative IA

O Projeto Vitta é uma aplicação de saúde e bem-estar construída na plataforma Oracle Cloud. O objetivo desta Sprint 2 é demonstrar um protótipo funcional que aplica os conceitos de **IoB (Internet of Behaviors)** e **IoT (Internet of Things)**, monitorando o bem-estar diário e gerando insights proativos para o usuário.

### Integrantes do Grupo
* Luann Noqueli Klochko / RM: 560313
* Jhonatta Sandes De Oliveira /RM: 560277
* Lucas Higuti Fontanezi / RM: 561120

---

## 1. Evolução do Projeto em Relação à Sprint 1

O projeto evoluiu significativamente da fase de **concepção teórica** (Sprint 1) para a implementação de um **protótipo funcional** (Sprint 2).

Enquanto a primeira sprint focou em definir o problema (dificuldade em manter hábitos saudáveis) e a solução macro, a Sprint 2 concentrou-se em **materializar essas ideias** em uma aplicação funcional.

A principal mudança foi a **adoção do Oracle APEX** como ferramenta "low-code" para construir tanto o **Frontend (UI)** quanto o **Backend (Processos e Lógica)**. Isso nos permitiu prototipar e testar as regras de negócio (IoB) e a integração da API (IoT) de forma muito mais ágil.

**O que foi implementado na Sprint 2:**
* **Banco de Dados:** Criamos as tabelas `HABITOS` e `ALERTAS` para armazenar os dados diários e as mensagens geradas pela lógica IoB.
* **Lógica de Negócio (IoB):** O "cérebro" do projeto foi implementado no procedure `ANALISAR_HABITO_IOB`, que compara dados atuais com dias anteriores.
* **Integração (IoT):** Construímos um endpoint de API RESTful (ORDS) para permitir que dispositivos externos (simulados pelo Postman) enviem dados para a aplicação.

---

## 2. Ferramentas e Tecnologias Oracle Exploradas

Para o desenvolvimento do protótipo, as seguintes ferramentas e tecnologias do ecossistema Oracle foram exploradas e implementadas:

* **Oracle APEX (Application Express):**
    Utilizado para a criação rápida e eficiente de toda a interface web do protótipo. Com ele, construímos:
    * `Página 10`: Um formulário para o registro manual de hábitos (sono, passos, água).
    * `Página 11`: Um relatório (dashboard) para exibir os alertas gerados para o usuário.

* **Oracle Database (Banco de Dados Integrado):**
    O banco de dados Oracle é utilizado não apenas para a persistência dos dados (`USUARIOS`, `HABITOS`, `ALERTAS`), mas também para processar toda a lógica de negócio.

* **PL/SQL:**
    Foi a linguagem usada para criar o "cérebro" do nosso projeto: o procedure `ANALISAR_HABITO_IOB`. Ele contém todas as regras de **IoB (Internet of Behaviors)**, analisando os hábitos registrados e decidindo quando um alerta deve ser gerado.

* **ORDS (Oracle REST Data Services):**
    Utilizado para criar o endpoint de API REST (`.../iot/registrar`). Essa é a "porta de entrada" para a **IoT**, permitindo que qualquer dispositivo externo (um sensor real, ou o Postman para simulação) envie dados diretamente para o nosso banco de dados e acione a lógica de IoB.

---

## 3. Funcionalidades do Protótipo
<img width="1867" height="850" alt="image" src="https://github.com/user-attachments/assets/01ee7b54-b9d7-4366-9e10-3b3f7e449b75" />

O protótipo atual, desenvolvido no Oracle APEX, já possui as seguintes funcionalidades implementadas e em execução:

* **Formulário de Registro de Hábitos:** Uma interface web (Página 10) permite que o usuário insira manualmente seus dados de sono, passos, água, etc.
    * <img width="1863" height="804" alt="image" src="https://github.com/user-attachments/assets/13314303-111c-4ad3-9657-d0c420195643" />



* **Lógica de Análise de Comportamento (IoB):** Um procedure PL/SQL (`ANALISAR_HABITO_IOB`) é executado sempre que um novo dado é inserido (manual ou via API). Ele verifica padrões como:
    * Passos abaixo de 3000 (Sedentarismo).
    * Sono abaixo de 6 horas.
    * Hidratação baixa por dias seguidos (lógica IoB).

* **Dashboard de Alertas e Recomendações:** Uma interface web (Página 11) exibe todos os alertas gerados pela lógica de IoB para o usuário.
    * <img width="1865" height="717" alt="image" src="https://github.com/user-attachments/assets/8783df9f-09e2-4978-8a1d-8f4880ed9dd9" />


* **Endpoint de API para IoT:** Um endpoint `GET` (`/ords/projetovitta/iot/registrar`) está funcional e pronto para receber dados de sensores.

---

## 4. Integrações Testadas e Implementadas

A principal integração implementada e testada com sucesso nesta sprint foi:

* **Integração APEX ➔ PL/SQL ➔ Banco de Dados:** A conexão entre a interface do protótipo e o banco de dados está 100% funcional. O formulário da Página 10 chama o *Processo APEX* `Gerar_alerta_IoB`, que por sua vez executa o *Procedure PL/SQL*, validando o fluxo de lógica interna.

* **API REST (IoT) ➔ PL/SQL (IoB) ➔ APEX (UI):**
    Evidenciamos a integração com "sensores simulados" (conforme o requisito) usando o **Postman**.
    1.  Disparamos uma requisição `GET` para o endpoint `.../iot/registrar` com os parâmetros `usuario=1` e `passos=500`.
    2.  O Postman recebeu `Status 200 OK`.
    3.  Imediatamente, o alerta ("Alerta de Atividade: Menos de 3000 passos...") apareceu no dashboard do APEX (Página 11).
    
    * `[ARRASTE AQUI A IMAGEM DO POSTMAN MOSTRANDO O 200 OK, COMO A image_cc296a.png]`

* **Node-RED e MQTT (Requisito):**
    O alicerce para esta integração (a API REST) está concluído e funcional. A implementação do fluxo visual no Node-RED e a configuração do broker MQTT não foram finalizadas nesta sprint, sendo um dos próximos passos.

---

## 5. Próximos Passos (Evolução do Projeto)

Para as próximas sprints, os passos planejados para a evolução do Vitta são:
1.  **Concluir a Integração IoT:** Implementar o fluxo **Node-RED** e o broker **MQTT** para que os dados de sensores sejam enviados automaticamente para nossa API, substituindo a simulação via Postman.
2.  **Expandir a Lógica IoB:** Adicionar regras mais complexas de análise comportamental (ex: cruzar dados de sono ruim com alimentação).
3.  **Explorar IA Generativa:** Utilizar a IA Generativa (como a OCI Generative AI) para criar as mensagens de alerta e recomendação, tornando-as mais humanas, personalizadas e motivacionais.
4.  **Integração Real:** Conectar a aplicação com APIs de wearables reais (ex: Apple Health, Google Fit).

---

## 6. Fluxo Técnico do Protótipo (Sprint 2)

O fluxo de dados atual funciona de duas maneiras:

**Fluxo 1 (Manual):**
`APEX (Pág 10)` $\rightarrow$ `Botão Salvar` $\rightarrow$ `Procedure (ANALISAR_HABITO_IOB)` $\rightarrow$ `Tabela ALERTAS` $\rightarrow$ `APEX (Pág 11)`

**Fluxo 2 (IoT Simulado):**
`Postman (GET)` $\rightarrow$ `API REST (ORDS)` $\rightarrow$ `Procedure (ANALISAR_HABITO_IOB)` $\rightarrow$ `Tabela ALERTAS` $\rightarrow$ `APEX (Pág 11)`

---

## 7. Vídeo de Demonstração

Segue abaixo o link do vídeo apresentando a evolução do projeto e demonstrando o funcionamento dos dois fluxos do protótipo:

**[COLE AQUI O SEU LINK DO YOUTUBE]**

