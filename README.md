# Gerenciamento de Usuários e Grupos no Linux (AWS EC2)

Este projeto contém um script em Bash que automatiza a criação e gerenciamento de usuários em sistemas Linux. Ele permite adicionar múltiplos usuários de forma prática, atribuindo senhas iniciais, grupos e permissões.

---

📂 Estrutura do Projeto
gerenciamento-usuarios-linux/
│── script.sh         # Script principal de gerenciamento
│── README.md         # Documentação do projeto

---

## 🎯 Objetivos

- Criar novos usuários com senha padrão  
- Criar grupos e associar os usuários correspondentes  
- Realizar login como diferentes usuários  
- Entender permissões de acesso e logs do sistema  

---

## 🕒 Duração

Aproximadamente **45 minutos**.

---

## 🚀 Ambiente

- **Serviço**: Amazon EC2  
- **Tipo de instância**: `t3.micro` (1 vCPU, 1 GiB RAM)  
- **Sistema Operacional**: Amazon Linux 2  
- **Acesso**: SSH (via `.pem` para Linux/macOS ou `.ppk` para Windows)  

---

## 📌 Passos do Laboratório

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

``sudo useradd arosalez
sudo passwd arosalez``

- Validação:

``sudo cat /etc/passwd | cut -d: -f1``

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
| Personnel | Todos os usuários (via Managers + HR + Finance + Sales + Shipping) |

- Exemplo:
- 
  `sudo usermod -a -G Sales arosalez`
  
- Validação:
  
  ``cat /etc/group``

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

- Erro esperado:

  `arosalez is not in the sudoers file. This incident will be reported.`

  ---

# 6. Logs de Segurança

- Visualizar tentativas de uso indevido do sudo:

  ``sudo cat /var/log/secure``

- Exemplo de log:

  ``Aug  9 14:45:55 ip-10-0-10-217 sudo: arosalez : user NOT in sudoers ; ...``

  ---
  










