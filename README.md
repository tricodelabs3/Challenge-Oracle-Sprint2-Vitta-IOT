# Challenge-Oracle-Sprint2-Vitta-IOT
#  CHALLENGE ORACLE 2025: Projeto Vitta (Sprint 2)
## Disruptive Architectures: IoT, IoB & Generative IA

### Integrantes do Grupo
* Jhonatta Sandes De Oliveira / RM560277
* Luann Noqueli Klochko / RM560313
* Lucas Higuti Fontanezi / RM561120

---

## 1. Visão Geral do Projeto Vitta

O Projeto Vitta é um protótipo de aplicação inteligente focado no monitoramento e incentivo de hábitos saudáveis, construído inteiramente na plataforma **Oracle Cloud**.

O objetivo desta Sprint 2 é demonstrar um protótipo funcional que aplica os conceitos de **IoB (Internet of Behaviors)** e **IoT (Internet of Things)**. A aplicação monitora o bem-estar diário (sono, hidratação, passos) e usa lógica de back-end para transformar esses dados em alertas e recomendações proativas para o usuário.

### Funcionalidades Implementadas (Sprint 2):
* **Registro de Hábitos:** O usuário pode registrar seus dados manualmente através da interface do APEX.
* **API de Integração IoT:** A aplicação expõe uma API REST (via ORDS) capaz de receber dados de "sensores simulados" (testado via Postman).
* **Lógica de IoB:** Um procedimento PL/SQL (`ANALISAR_HABITO_IOB`) atua como o "cérebro" do sistema, analisando os dados e identificando padrões de comportamento.
* **Alertas e Recomendações:** O sistema gera alertas dinâmicos (ex: "Alerta de Sedentarismo") com base nas regras de IoB.

---

## 2. Evolução do Projeto (Sprint 1 ➔ Sprint 2)

### Proposta Inicial (Sprint 1)
Na Sprint 1, o foco foi a **concepção e arquitetura teórica** do Vitta. O problema identificado era a dificuldade que as pessoas têm em monitorar seus hábitos de forma prática e motivadora. A solução proposta foi um assistente digital que usaria IoT, IoB e IA.

A arquitetura original previa um frontend mobile (React Native) separado de um backend (API REST) e do Oracle Database.

### Protótipo Funcional (Sprint 2)
Nesta Sprint 2, **focamos em construir um protótipo rápido e funcional (MVP)** para validar a lógica de negócio principal (o IoB).

A principal mudança foi a **adoção do Oracle APEX** como ferramenta de "low-code" para construir tanto o **Frontend (UI)** quanto o **Backend (Processos e Lógica)**. Isso nos permitiu prototipar e testar as regras de negócio e a integração da API de forma muito mais ágil.

**O que mudou e foi implementado:**
* **Frontend:** Saímos de uma proposta em React Native para uma aplicação web funcional construída em **Oracle APEX** (Páginas 10 e 11).
* **Backend (IoB):** A "IA de regras simples" foi implementada como um **Procedure PL/SQL** (`ANALISAR_HABITO_IOB`) no banco de dados, que contém toda a lógica de análise comportamental.
* **Integração (IoT):** A API REST não é mais apenas um conceito. Nós a implementamos usando **Oracle REST Data Services (ORDS)**, criando um *Handler* `GET` (`/iot/registrar`) que recebe dados de sensores simulados e aciona o procedure de IoB.

---

## 3. Ferramentas Oracle Exploradas

Para esta Sprint, as seguintes ferramentas do Oracle Cloud foram exploradas e utilizadas:

| Camada | Ferramenta Oracle | Função no Projeto |
| :--- | :--- | :--- |
| **Aplicação e UI** | **Oracle APEX** | (Evolução da S1) Usado para construir o protótipo funcional completo: as páginas de registro de hábitos e o dashboard de alertas. |
| **Banco de Dados** | **Oracle Database Cloud** | Armazenamento central dos dados nas tabelas `USUARIOS`, `HABITOS` e `ALERTAS`. |
| **Lógica de Negócio (IoB)** | **PL/SQL** | (Evolução da S1) Usado para criar o procedure `ANALISAR_HABITO_IOB`, que é o "cérebro" da aplicação e contém as regras de IoB. |
| **Integração / API** | **ORDS (Oracle REST Data Services)**| (Novo na S2) Usado para expor a lógica PL/SQL como uma API REST segura, permitindo a integração com dispositivos IoT. |

---

## 4. Funcionalidades e Testes de Integração

O protótipo foi validado através de dois fluxos funcionais:

### Fluxo 1: Registro Manual (Validando a Lógica IoB)
Este fluxo demonstra a lógica de IoB sendo acionada pelo próprio usuário.

1.  **Ação:** O usuário acessa a **Página 10 (Registro de hábitos)** no app Vitta.
2.  **Processo:** Ele preenche o formulário com dados que disparam uma regra (ex: `Passos = 500`).
3.  **Resultado:** Ao salvar, o processo `Gerar_alerta_IoB` da página chama o procedure `ANALISAR_HABITO_IOB`. O procedure identifica o sedentarismo e insere uma linha na tabela `ALERTAS`.
4.  **Prova:** O usuário é redirecionado para a **Página 11 (Alertas e Recomendações)** e vê a mensagem "Alerta de Atividade: Menos de 3000 passos..."

### Fluxo 2: Consumo de API (Simulação de Sensor IoT)
Este fluxo demonstra que a aplicação está pronta para integrações externas.

1.  **Ação:** Um "sensor simulado" (o software **Postman**) é usado para enviar dados de IoT.
2.  **Processo:** Uma requisição `GET` é disparada para o endpoint do ORDS: `.../ords/projetovitta/iot/registrar?usuario=1&passos=500`.
3.  **Resultado:** O *Handler* `GET` no APEX recebe os dados, atualiza a tabela `HABITOS` e chama o mesmo procedure `ANALISAR_HABITO_IOB`, que gera o alerta.
4.  **Prova:** O Postman recebe `Status 200 OK` e o alerta de atividade aparece imediatamente no aplicativo APEX.

---

## 5. Integração Node-RED e MQTT (Objetivo Específico)

Um dos objetivos era integrar o APEX com um fluxo Node-RED e MQTT.

Nesta Sprint, **o alicerce para essa integração foi construído e testado com sucesso:** a API REST (`.../iot/registrar`).

O Postman funcionou como o *simulador* do Node-RED (cumprindo o requisito de "sensor simulado" e "consumo de API"). A implementação física do fluxo Node-RED e a configuração do broker MQTT não foram concluídas nesta sprint e ficam como objetivo para a próxima evolução do projeto.

---

## 6. Próximos Passos (Visão Futura)

Para as próximas Sprints, o Vitta pretende evoluir para:
* **Concluir a integração** com **Node-RED** e **MQTT**, substituindo o Postman por um fluxo automatizado.
* **Integração real** com dispositivos IoT (ex: Apple Health, Google Fit).
* **Aplicar IA (Machine Learning)** para substituir as "regras simples" por recomendações preditivas e verdadeiramente personalizadas.
* **Implementar IA Generativa** para criar os textos das recomendações em linguagem natural e mais humana.
* **Criar um painel** em **Oracle Analytics Cloud** para acompanhamento visual dos dados coletivos.
