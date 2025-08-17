# Gerenciamento de Usuários e Grupos no Linux (AWS EC2)

Este projeto contém um script em Bash que automatiza a criação e gerenciamento de usuários em sistemas Linux. Ele permite adicionar múltiplos usuários de forma prática, atribuindo senhas iniciais, grupos e permissões.

---

📂 Estrutura do Projeto

gerenciamento-usuarios-linux/

│── imagens

│── README.md

---

## 🎯 Objetivos

- Criar novos usuários com senha padrão  
- Criar grupos e associar os usuários correspondentes  
- Realizar login como diferentes usuários  
- Entender permissões de acesso e logs do sistema  

---

## 🚀 Ambiente

- **Serviço**: Amazon EC2  
- **Tipo de instância**: `t3.micro` (1 vCPU, 1 GiB RAM)  
- **Sistema Operacional**: Amazon Linux 2  
- **Acesso**: SSH (via `.pem` Linux)  

---

## 📌 Etapas:

### 1. Conexão via SSH

- **Linux/macOS**:
  ```bash
  chmod 400 labsuser.pem
  ssh -i labsuser.pem ec2-user@<public-ip>

<img width="1289" height="689" alt="01-conexao-ssh" src="https://github.com/user-attachments/assets/9846d596-a16f-4a19-a3df-e44742fa21d9" />

---

# 2. Criação de Usuários

- Lista de usuários criados:

| Nome      | Sobrenome | User ID   | Função               | Senha Inicial  |
| --------- | --------- | --------- | -------------------- | -------------- |
| Alejandro | Rosalez   | arosalez  | Sales Manager        | P\@ssword1234! |
| Efua      | Owusu     | eowusu    | Shipping             | P\@ssword1234! |
| Jane      | Doe       | jdoe      | Shipping             | P\@ssword1234! |
| Li        | Juan      | ljuan     | HR Manager           | P\@ssword1234! |
| Mary      | Major     | mmajor    | Finance Manager      | P\@ssword1234! |
| Mateo     | Jackson   | mjackson  | CEO                  | P\@ssword1234! |
| Nikki     | Wolf      | nwolf     | Sales Representative | P\@ssword1234! |
| Paulo     | Santos    | psantos   | Shipping             | P\@ssword1234! |
| Sofia     | Martinez  | smartinez | HR Specialist        | P\@ssword1234! |
| Saanvi    | Sarkar    | ssarkar   | Finance Specialist   | P\@ssword1234! |

- Exemplo de criação:

``sudo useradd arosalez``

``sudo passwd arosalez``

<img width="1289" height="689" alt="02-adicionando-usuario" src="https://github.com/user-attachments/assets/9cb82145-db92-4d3c-a082-7d06cefc48a4" />

- Validação:

``sudo cat /etc/passwd | cut -d: -f1``

<img width="1292" height="731" alt="03-lista-usuarios" src="https://github.com/user-attachments/assets/aa61ead7-a2d9-47c0-8191-c10eaeeee76a" />



---

# 3. Criação de Grupos

Grupos criados:

• Sales 

• HR

• Finance

• Shipping

• Managers

• CEO

- Exemplo:

  ``sudo groupadd Sales``

- Validação:

  ``cat /etc/group``

  <img width="1292" height="731" alt="04-lista-grupos" src="https://github.com/user-attachments/assets/b75d328f-de2b-4b1e-b1fd-144ecaa235c8" />


  ---

  # 4. Associação de Usuários aos Grupos

| Grupo     | Usuários                                                           |
| --------- | ------------------------------------------------------------------ |
| Sales     | arosalez, nwolf                                                    |
| HR        | ljuan, smartinez                                                   |
| Finance   | mmajor, ssarkar                                                    |
| Shipping  | eowusu, jdoe, psantos                                              |
| Managers  | arosalez, ljuan, mmajor                                            |
| CEO       | mjackson                                                           |

- Exemplo:
- 
  `sudo usermod -a -G Sales arosalez`
  
- Validação:
  
  ``cat /etc/group``

  <img width="1292" height="731" alt="05-usuarios-grupos" src="https://github.com/user-attachments/assets/0a03e5f7-6873-44d7-8460-1bf42b60c99e" />


  ---

# 5. Testando Login de Usuários

- Troca de usuário:

  ``su arosalez``

- Tentativa de criar arquivo sem permissão:

  `touch myFile.txt`

- Erro esperado:

  `touch: cannot touch ‘myFile.txt’: Permission denied`

- Tentativa com sudo:

  `sudo touch myFile.txt`

- Erro causado:

  <img width="1292" height="731" alt="06-erro-permissao" src="https://github.com/user-attachments/assets/60a0396f-bdc7-4772-930d-1302d8d8db86" />


  ---

# 6. Logs de Segurança

- Visualizar tentativas de uso indevido do sudo:

  ``sudo cat /var/log/secure``

- Logs:

<img width="1292" height="731" alt="07-logs-seguranca" src="https://github.com/user-attachments/assets/20e9afa3-3fb0-45e3-9972-0e206c8af6c3" />



  

  ---
  










