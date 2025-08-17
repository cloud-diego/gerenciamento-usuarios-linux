# 🐧 Gerenciamento de Usuários e Grupos no Linux (AWS EC2)

Este projeto contém a documentação de um laboratório prático realizado em uma instância **Amazon Linux EC2**, com foco em criação e gerenciamento de **usuários e grupos**, além da verificação de permissões e logs do sistema.

-----

📂 **Estrutura do Projeto**

```
gerenciamento-usuarios-linux/
│
├── imagens/
│
└── README.md
```

-----

## 🎯 Objetivos

  - Criar novos usuários com senha padrão.
  - Criar grupos e associar os usuários correspondentes.
  - Realizar login como diferentes usuários.
  - Entender permissões de acesso e logs do sistema.

-----

## 🚀 Ambiente

  - **Serviço**: Amazon EC2
  - **Tipo de instância**: `t3.micro` (1 vCPU, 1 GiB RAM)
  - **Sistema Operacional**: Amazon Linux 2
  - **Acesso**: SSH (via `.pem` Linux)

-----

## 📌 Etapas:

### 1\. Conexão via SSH

  - **Linux/macOS**:
    ```bash
    # Altera a permissão da chave para somente leitura pelo proprietário
    chmod 400 labsuser.pem

    # Conecta-se à instância usando a chave
    ssh -i labsuser.pem ec2-user@<public-ip>
    ```

<img width="1289" height="689" alt="01-conexao-ssh" src="https://github.com/user-attachments/assets/f4a2d482-cfa8-4003-92b5-61621b5f8b3e" />


-----

### 2\. Criação de Usuários

  - Lista de usuários a serem criados:

| Nome      | Sobrenome | User ID   | Função               | Senha Inicial |
| :-------- | :-------- | :-------- | :------------------- | :------------ |
| Alejandro | Rosalez   | arosalez  | Sales Manager        | senha1234     |
| Efua      | Owusu     | eowusu    | Shipping             | senha1234     |
| Jane      | Doe       | jdoe      | Shipping             | senha1234     |
| Li        | Juan      | ljuan     | HR Manager           | senha1234     |
| Mary      | Major     | mmajor    | Finance Manager      | senha1234     |
| Mateo     | Jackson   | mjackson  | CEO                  | senha1234     |
| Nikki     | Wolf      | nwolf     | Sales Representative | senha1234     |
| Paulo     | Santos    | psantos   | Shipping             | senha1234     |
| Sofia     | Martinez  | smartinez | HR Specialist        | senha1234     |
| Saanvi    | Sarkar    | ssarkar   | Finance Specialist   | senha1234     |

  - **Exemplo de criação de usuário e senha:**

    ```bash
    # Cria o usuário 'arosalez'
    sudo useradd arosalez

    # Define a senha para 'arosalez' (será solicitado interativamente)
    sudo passwd arosalez
    ```

    *(Repita o processo para todos os usuários da lista)*

   <img width="1289" height="689" alt="02-adicionando-usuario" src="https://github.com/user-attachments/assets/15bd11f8-3c95-4564-953a-0b7cd87ec631" />

  - **Validação:**
    Para confirmar que os usuários foram criados, liste-os a partir do arquivo `/etc/passwd`.

    ```bash
    # Lista os nomes de todos os usuários do sistema
    sudo cat /etc/passwd | cut -d: -f1
    ```

<img width="1292" height="731" alt="03-lista-usuarios" src="https://github.com/user-attachments/assets/95f58220-592c-435d-9caf-b517948256cd" />


-----

### 3\. Criação de Grupos

  - Grupos a serem criados:

      - `Sales`
      - `HR`
      - `Finance`
      - `Shipping`
      - `Managers`
      - `CEO`

  - **Exemplo de criação de grupo:**

    ```bash
    sudo groupadd sales
    ```

    *(Repita o processo para todos os grupos da lista)*

  - **Validação:**
    Verifique se os grupos existem no arquivo `/etc/group`.

    ```bash
    cat /etc/group
    ```

<img width="1292" height="731" alt="04-lista-grupos" src="https://github.com/user-attachments/assets/5f3e9578-5723-49ef-8bff-95abc87a6aa2" />

-----

### 4\. Associação de Usuários aos Grupos

  - Associações:

| Grupo    | Usuários                 |
| :------- | :----------------------- |
| Sales    | arosalez, nwolf          |
| HR       | ljuan, smartinez         |
| Finance  | mmajor, ssarkar          |
| Shipping | eowusu, jdoe, psantos    |
| Managers | arosalez, ljuan, mmajor  |
| CEO      | mjackson                 |

  - **Exemplo de associação:**
    O comando `usermod` com a flag `-aG` adiciona um usuário a um grupo.

    ```bash
    sudo usermod -a -G Sales arosalez
    ```

    *(Repita o processo para todas as associações necessárias)*

  - **Validação:**
    Inspecione novamente o arquivo `/etc/group` para ver os usuários associados.

    ```bash
    cat /etc/group
    ```

 <img width="1292" height="731" alt="05-usuarios-grupos" src="https://github.com/user-attachments/assets/13837eba-ac0c-4fde-ba5b-bed881848354" />


-----

### 5\. Testando Login de Usuários

  - **Troca de usuário:**
    Use o comando `su` para assumir a identidade de `arosalez`.

    ```bash
    su arosalez
    ```

  - **Tentativa de criar arquivo sem permissão:**
    Após trocar de usuário, o terminal continua no diretório `/home/ec2-user`. O usuário `arosalez` não tem permissão para escrever neste local.

    ```bash
    touch myFile.txt
    ```

  - **Erro esperado:**
    `touch: cannot touch ‘myFile.txt’: Permission denied`

  - **Tentativa de usar `sudo`:**
    Por padrão, um novo usuário não tem privilégios de administrador.

    ```bash
    sudo touch myFile.txt
    ```

  - **Erro por falta de permissão no `sudo`:**
    O sistema nega a execução e informa que o usuário não está no arquivo `sudoers`, que define quem pode usar `sudo`.

<img width="1292" height="731" alt="06-erro-permissao" src="https://github.com/user-attachments/assets/4d668283-d46e-4678-a60a-c11a12cfce7d" />

-----

### 6\. Logs de Segurança

  - **Visualizar tentativas de uso indevido do `sudo`:**
    O sistema registra tentativas de acesso administrativo no arquivo `/var/log/secure`. Podemos filtrar por eventos relacionados ao usuário `arosalez`.

    ```bash
    sudo cat /var/log/secure | grep arosalez
    ```

  - **Logs:**
    A saída do comando mostrará que a tentativa de `arosalez` usar `sudo` foi registrada como um incidente de segurança, com a mensagem `user NOT in sudoers`.

<img width="1292" height="731" alt="07-logs-seguranca" src="https://github.com/user-attachments/assets/de9f755a-fd57-4530-9a7e-35613852c3d1" />
