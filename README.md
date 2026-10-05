# **Resumo Estruturado — Módulo 4: Testando e Automatizando Testes de API**

# **1\. Mentalidade de Testes em APIs**

A transição dos testes de interface web para o nível de serviços (API) exige a validação estrutural de contratos e dados:

* **Ausência de Interface Gráfica:** A interação ocorre diretamente via protocolo HTTP entre cliente e servidor, focando em *requests*, *responses*, *headers*, métodos HTTP e *status codes*.  
* **Testes Atômicos e Fragmentados:** Foco em isolar *endpoints* para validar regras de negócio, em contraste com cenários *End-to-End* (E2E) predominantemente visuais.  
* **Validação Estrutural:** Verificação rigorosa de chaves/atributos JSON, tipagem de dados e cumprimento de regras de negócio.  
* **Gerenciamento de Autenticação (*Stateless*):** Requisições protegidas exigem o envio explícito do token de autorização (ex: Bearer JWT).  
* **Adoção do Shift-Left:** Habilidade de testar a camada de integração e validar contratos antes do desenvolvimento da interface do usuário.

# **2\. A Heurística VADER Aplicada ao Contexto Bancário**

Guia mnemônico para criação sistemática de cenários de teste em APIs financeiras:

* **V — Verbs (Verbos HTTP):**  
  * `POST`: Criação e execução de ações (ex: `/login` e `/transferencias`).  
  * `GET`: Consultas e leituras seguras e idempotentes (ex: `/transferencias`, `/contas/{id}`).  
  * `PUT` e `PATCH`: Atualizações integrais e parciais de recursos.  
  * `DELETE`: Remoção ou cancelamento com foco em *Safe Delete* (*Soft Delete*) para garantir conformidade e auditoria contábil.  
* **A — Authorization (Autorização e Segurança):**  
  * Proteção de rotas contra acesso anônimo (401 Unauthorized) e permissões insuficientes (403 Forbidden).  
  * Testes para prevenir falhas de acesso horizontal (*IDOR \- Insecure Direct Object Reference*).  
* **D — Data (Dados e Contratos):**  
  * Validação de formatos, tipos de dados e limites do payload (ex: rejeição de valores negativos, auto-transferência e campos obrigatórios ausentes).  
* **E — Errors (Tratamento de Exceções):**  
  * Respostas com mensagens JSON estruturadas e claras (ex: 404 Not Found, 400 Bad Request / 422 Unprocessable Entity), evitando erros genéricos e vazamento de informações.  
* **R — Responsiveness (Performance e Estabilidade):**  
  * Monitoramento de tempo de resposta (SLA), paginação em listagens grandes e controle de concorrência com *Rate Limiting* (429 Too Many Requests).

# **3\. Erros Comuns em Testes de APIs**

1. **Uso Incorreto dos Verbos HTTP:** Executar mutações de estado utilizando o método `GET`.  
2. **Falhas de Autenticação/Autorização:** Permitir acesso sem token válido ou quebra de isolamento entre contas (IDOR).  
3. **Validação Inexistente ou Frágil de Dados:** Confiar no payload enviado sem validar tipos, limites ou injeções maliciosas (SQLi/XSS).  
4. **Mensagens de Erro Genéricas:** Retornar mensagens sem clareza quanto à regra violada.  
5. **Uso Incorreto de Status Codes:** Retornar `200 OK` com payload de erro interno ou `500 Internal Server Error` para falhas do cliente.  
6. **Desempenho Indevido e Falta de Paginação:** Degradação em consultas sem limite e concorrência sem bloqueio transacional.  
7. **Exposição de Stack Trace:** Exibir informações internas da arquitetura e servidor em falhas não tratadas.

# **4\. Arquitetura e Configuração do Projeto de Automação**

## **Stack Tecnológica (Node.js)**

* **Mocha:** *Test Runner* responsável pela organização de blocos (`describe`, `it`) e ganchos de ciclo de vida (`before`, `beforeEach`).  
* **SuperTest:** Cliente HTTP para envio de requisições e validação das respostas.  
* **Chai:** Biblioteca para asserções no estilo BDD (`expect`).  
* **Mochawesome:** Gerador de relatórios gráficos e visuais em formato HTML.  
* **Dotenv:** Gerenciamento centralizado de variáveis de ambiente.

## **Boas Práticas e Padrões Implementados**

* **Variáveis de Ambiente:** Remoção de URLs e dados *hardcoded* com `.env` (preservando credenciais fora do Git via `.gitignore` e disponibilizando `.env.example`).  
* **Helpers:** Centralização de rotinas repetitivas (ex: geração de tokens JWT) aplicando o princípio DRY.  
* **Hooks (`beforeEach`):** Isolamento e renovação de contexto a cada teste individual.  
* **Fixtures:** Armazenamento desacoplado de massas de dados em arquivos estáticos `.json` (ex: `transferencias.json`).  
* **Inspeção de Response Body:** Validação de atributos, tipos e valores em objetos e arrays retornados pela API.  
* **Documentação Técnica:** Elaboração de um `README.md` detalhado e profissional para apresentação no GitHub.


