Browser: Versão 145.0.7632.160 (Versão oficial) 64 bits;
SO:  Windows 11 Pro;
Resolução: 1920x1080.

# Teste Técnico – QA Tester - Processo Seletivo – 4blue

### Resumo de Severidade e Prioridade

Abaixo, apresento meu quadro consolidado com a classificação dos problemas identificados durante o ciclo de testes:

| Severidade | Quantidade | Exemplos Principais | Prioridade Predominante |
| :--- | :---: | :--- | :--- |
| **Crítico** | 2 | - Login com campos vazios (Issue 6)<br>- Confirmação de senha diferente (Issue 13) | **Alta** |
| **Alto** | 7 | - Brute Force (Issue 5)<br>- Criação de contas vazias/duplicadas (Issues 7 e 8)<br>- E-mail/senha inválidos aceitos (Issues 11 e 12)<br>- Vulnerabilidade XSS (Issue 14) | **Alta** |
| **Médio** | 5 | - Login com e-mail inválido (Issue 4)<br>- Campos com valores inválidos (Issues 9 e 10)<br>- Quebra de layout<br>- Falta de confirmação de e-mail | **Média/Alta** |
| **Baixo** | 3 | - Regra de senha na tela errada (Issue 1)<br>- Sem visualização de senha (Issue 3)<br>- Mensagem de erro inesperada (Tela de Sucesso) | **Baixa/Média** |

##  Tela de Login

### 1. Regra de criação de senha exibida na tela de login
**Descrição:** A tela de login exibe a regra de criação de senha (“mínimo de 8 caracteres e 1 caractere especial”). Essa regra é relevante apenas para o cadastro de conta, não para a tela de login.

**Passos para reproduzir:**
1. Acessar o sistema.
2. Observar a mensagem exibida abaixo do campo de senha.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| A regra de criação de senha aparece na tela de login. | A regra de criação de senha deveria aparecer apenas na tela de criação de conta. |

**Severidade:** Baixo | **Prioridade:** Baixa

---

### 2. Sistema permite tentativa de login sem preencher os campos
**Descrição:** O sistema permite clicar no botão Entrar mesmo sem preencher os campos de e-mail e senha.

**Passos para reproduzir:**
1. Acessar a tela de login.
2. Não preencher os campos de e-mail e senha.
3. Clicar em Entrar.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema tenta realizar a validação de login mesmo sem dados preenchidos. | O sistema deveria bloquear a ação e informar que os campos são obrigatórios. |

**Severidade:** Médio | **Prioridade:** Alta

---

### 3. Campo de senha não possui opção de visualização
**Descrição:** O campo de senha não possui opção para mostrar ou ocultar o conteúdo digitado, o que pode prejudicar a usabilidade.

**Passos para reproduzir:**
1. Digitar uma senha no campo de senha.
2. Tentar visualizar o conteúdo digitado.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| Não existe opção de visualizar a senha digitada. | O sistema deveria oferecer um botão para mostrar ou ocultar a senha. |

**Severidade:** Baixo | **Prioridade:** Baixa

---

### 4. Sistema tenta validar login com e-mail em formato inválido
**Descrição:** O sistema não valida corretamente o formato do e-mail antes de tentar realizar o login.

**Passos para reproduzir:**
1. Acessar a tela de login.
2. No campo e-mail inserir: `teste`
3. Inserir qualquer senha e clicar em Entrar.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema tenta realizar a validação de login. | O sistema deveria validar o formato do e-mail e impedir a tentativa de login. |

**Severidade:** Médio | **Prioridade:** Média

---

### 5. Sistema permite múltiplas tentativas de login sem bloqueio (Brute Force)
**Descrição:** O sistema permite múltiplas tentativas consecutivas de login sem qualquer mecanismo de proteção contra ataques de força bruta.

**Passos para reproduzir:**
1. Inserir qualquer e-mail e senha.
2. Clicar em Entrar diversas vezes seguidas.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema continua permitindo tentativas de login sem bloqueio ou limitação. | O sistema deveria limitar tentativas consecutivas ou aplicar mecanismos de proteção (bloqueio/captcha). |

**Severidade:** Alto | **Prioridade:** Alta

---

### 6. Sistema permite login com campos vazios caso exista conta criada sem dados
**Descrição:** Caso uma conta seja criada sem preencher os campos, o sistema permite realizar login com campos vazios.

**Passos para reproduzir:**
1. Criar uma conta sem preencher os campos na tela de cadastro.
2. Retornar à tela de login.
3. Clicar em Entrar sem preencher e-mail e senha.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema permite realizar login. | O sistema deveria impedir a criação de contas vazias e bloquear logins sem dados. |

**Severidade:** Crítico | **Prioridade:** Alta

---

### 18. Mensagem de erro inadequada ao inserir senha incorreta
**Descrição:** Ao tentar realizar o login com um e-mail cadastrado, mas inserindo a senha incorreta, o sistema exibe a mensagem “Conta não encontrada. Crie uma conta primeiro”. Esta mensagem é tecnicamente falsa (pois a conta existe) e perigosa por permitir a confirmação de e-mails cadastrados na base.

**Passos para reproduzir:**
1. Possuir uma conta já cadastrada no sistema.
2. Acessar a tela de login.
3. Inserir o e-mail correto da conta.
4. Inserir uma **senha incorreta**.
5. Clicar em Entrar.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema exibe: “Conta não encontrada. Crie uma conta primeiro.” | O sistema deve exibir uma mensagem genérica como: “E-mail ou senha inválidos.” |

**Severidade:** Médio | **Prioridade:** Média

---

##  Tela de Criação de Conta

### 7. Sistema permite criar conta com campos vazios
**Descrição:** O sistema permite criar uma conta mesmo sem preencher os campos obrigatórios.

**Passos para reproduzir:**
1. Acessar a tela Criar conta.
2. Clicar diretamente em Criar conta.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| A conta é criada sem validação de dados. | O sistema deveria exigir o preenchimento dos campos obrigatórios antes de permitir o cadastro. |

**Severidade:** Alto | **Prioridade:** Alta

---

### 8. Sistema permite criação de contas duplicadas
**Descrição:** O sistema permite criar múltiplas contas utilizando os mesmos dados de cadastro.

**Passos para reproduzir:**
1. Criar uma conta com dados válidos.
2. Repetir o cadastro utilizando os mesmos dados.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema cria múltiplas contas com os mesmos dados. | O sistema deveria impedir a criação de contas duplicadas. |

**Severidade:** Alto | **Prioridade:** Alta

---

### 9. Campo nome aceita caracteres inválidos
**Descrição:** O campo Nome completo aceita números e caracteres especiais.

**Passos para reproduzir:**
1. No campo nome inserir: `123456` ou `@@@@@`
2. Preencher os demais campos e clicar em Criar conta.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema aceita os valores inseridos. | O sistema deveria validar o campo e aceitar apenas caracteres alfabéticos. |

**Severidade:** Médio | **Prioridade:** Média

---

### 10. Campo telefone aceita valores inválidos
**Descrição:** O campo telefone aceita letras e sequências de números fora do formato padrão.

**Passos para reproduzir:**
1. No campo telefone inserir: `abcabcabc` ou `999999999999999999`
2. Clicar em Criar conta.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema aceita os valores inválidos. | O sistema deveria validar o formato do telefone. |

**Severidade:** Médio | **Prioridade:** Média

---

### 11. Sistema aceita e-mail inválido no cadastro
**Descrição:** O sistema permite criar conta com e-mail em formato inválido.

**Passos para reproduzir:**
1. Inserir no campo e-mail: `teste` ou `teste@`
2. Clicar em Criar conta.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| A conta é criada com e-mail inválido. | O sistema deveria validar o formato do e-mail antes do cadastro. |

**Severidade:** Alto | **Prioridade:** Alta

---

### 12. Sistema aceita senha sem os requisitos informados
**Descrição:** O sistema permite cadastrar senhas que não atendem às regras exibidas.

**Passos para reproduzir:**
1. Inserir senha: `12345678` ou `aaa`
2. Confirmar a senha e clicar em Criar conta.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| A conta é criada com senha fora dos requisitos. | O sistema deveria validar os requisitos de senha antes de permitir o cadastro. |

**Severidade:** Alto | **Prioridade:** Alta

---

### 13. Sistema aceita confirmação de senha diferente
**Descrição:** O sistema permite criar conta mesmo quando a senha e a confirmação são diferentes.

**Passos para reproduzir:**
1. Inserir senha: `Senha@123` e confirmação: `Senha@124`
2. Clicar em Criar conta.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| Conta criada com senhas diferentes. | O sistema deveria informar que as senhas não coincidem e bloquear o cadastro. |

**Severidade:** Crítico | **Prioridade:** Alta

---

### 14. Possível vulnerabilidade XSS nos campos de cadastro
**Descrição:** Os campos aceitam inserção de código JavaScript (Cross-Site Scripting).

**Passos para reproduzir:**
1. Inserir em um dos campos: `<script>alert(1)</script>`
2. Clicar em Criar conta.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema aceita o código e cria a conta. | O sistema deveria sanitizar ou bloquear entradas contendo código HTML/JS. |

**Severidade:** Alto | **Prioridade:** Alta

---

### 15. Quebra de Layout nos Campos
**Descrição:** Os campos "Telefone" e "Confirmar Senha" ultrapassam o limite lateral do card branco de cadastro sobrepondo outro campos, prejudicando a estética e a usabilidade em resoluções de desktop.

**Passos para reproduzir:**
1. Acessar a tela de criação de conta do sistema.
2. Observar o alinhamento dos campos à direita.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| Os inputs de Telefone e Confirmar Senha estão maiores que os campos da esquerda e vazam para fora do container principal. | Todos os campos de input devem estar contidos dentro do card branco, mantendo o alinhamento e o preenchimento (padding) consistentes. |

**Severidade:** Baixa | **Prioridade:** Média

---

### 16. Falta de campo "Confirmar E-mail" no formulário de cadastro
**Descrição:** O formulário de criação de conta solicita a confirmação da senha, mas não oferece um campo para confirmar o e-mail digitado. Isso permite que erros de digitação passem despercebidos, impossibilitando recuperações de senha futuras ou comunicações do sistema.

**Passos para reproduzir:**
1. Acessar a tela "Crie sua conta".
2. Observar os campos disponíveis para preenchimento.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| Existe apenas um campo para inserção de e-mail. | O formulário deve conter um campo "Confirmar E-mail" com validação de igualdade, ou implementar verificação via OTP/Link. |

**Severidade:** Média | **Prioridade:** Alta

---

##  Tela simples de Sucesso


###  17. Mensagem de “Erro inesperado” exibida após login realizado com sucesso
**Descrição:** Após realizar login com credenciais válidas, o sistema redireciona corretamente para a tela de sucesso informando que o login foi realizado. Porém, simultaneamente é exibida uma notificação no canto inferior direito com a mensagem “Erro inesperado”, gerando inconsistência de feedback para o usuário.

**Passos para reproduzir:**
1. Acessar a página de login do sistema.
2. Inserir um email válido cadastrado.
3. Inserir a senha correta.
4. Clicar em Entrar / Login.
5. Observar a tela após o redirecionamento.

| Resultado Atual | Resultado Esperado |
| :--- | :--- |
| O sistema redireciona para a tela “Login realizado com sucesso”, mas exibe simultaneamente a notificação “Erro inesperado” no canto inferior direito. | Após login bem-sucedido, nenhuma mensagem de erro deve ser exibida. Apenas a confirmação de sucesso deve aparecer. |

**Severidade:** Baixo | **Prioridade:** Baixo

---

## Priorização de Correções (Hotfixes)

**Pergunta: Quais 2 bugs você corrigiria primeiro e por quê?**

**Resposta:**

1. **Bug 7: Sistema permite criar conta com campos vazios**
   * **Por que:** Este bug compromete a base de dados. Permitir que um cliente se cadastre sem dados reais gera "contas fantasmagóricas", impossibilita comunicações futuras (e-mail marketing, recuperação de senha) e distorce completamente as métricas de crescimento e uso do sistema.

2. **Bug 14: Possível vulnerabilidade XSS nos campos de cadastro**
   * **Por que:** Este é o risco mais crítico de segurança. A capacidade de injetar scripts nos campos permite que roubem cookies de sessão, redirecionem usuários para sites maliciosos ou capturem senhas de outros clientes. Corrigir isso é vital para proteger os dados dos usuários e a reputação da empresa e evitar processos por LGPD.


---

##  Sugestões de Melhorias (UX/UI)

1. **Feedback em Tempo Real para Requisitos de Senha:**
   * **Benefício:** Em vez de exibir as regras apenas como texto estático (Issue 1), o sistema deve validar os critérios (8 caracteres, caractere especial, etc.) conforme o usuário digita. 
   * **Impacto:** Melhora a taxa de conversão no cadastro, indicando ao usuário onde está o erro.

2. **Ícones de Visualização de Senha (Show/Hide Password):**
   * **Benefício:** Adicionar um ícone de "olho" nos campos de senha e confirmação de senha.
   * **Impacto:** Permite que o usuário verifique o que digitou, evitando erros de digitação comuns (Issue 3) e reduzindo a frustração de bloqueios por senha incorreta.

---



