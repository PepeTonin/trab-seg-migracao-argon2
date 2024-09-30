## SISTEMA DE AUTENTICAÇÃO

Este projeto é um sistema de autenticação robusto que implementa um algortimo de criptografia muito seguro, o **AES**, para garantir segurança em cada etapa da comunicação entre cliente e servidor.
O sistema utiliza criptografia end-to-end para garantir a confidencialidade dos dados transmitidos e as senhas dos usuários são armazenadas com o algoritmo **Argon2**, considerado um dos mais seguros para armazenamento de senhas.

### Funcionalidades

- **Criptografia end-to-end:** Todos os dados enviados entre o cliente e o servidor são criptografados utilizando o algoritmo **AES**, garantindo que terceiros não possam interceptar ou manipular os dados.
- **Armazenamento seguro de senhas:** As senhas são armazenadas no servidor utilizando **Argon2**, um dos algoritmos mais avançados e seguros para armazenamento de senhas.
- **Atualização de hashes antigas:** Quando um usuário com uma senha armazenada em um formato de hash antigo faz login, o sistema automaticamente atualiza a senha para o formato Argon2.


### Tecnologias Utilizadas

- **FastAPI:** Framework web para criar APIs rápidas e seguras.
- **MySQL:** Banco de dados relacional usado para armazenar informações dos usuários.
- **Argon2:** Algoritmo de hashing de senhas utilizado para garantir a segurança no armazenamento.
    - implementado utilizando a biblioteca [argon2-cffi: Argon2 for Python](https://pypi.org/project/argon2-cffi/)
- **AES:** Algoritmo de criptografia simétrica usado para proteger as comunicações entre cliente e servidor.
    - implementado utilizando a biblioteca [cryptography](https://pypi.org/project/cryptography/)

### Diagrama do sistema

![diagram do sistema](/assets/diagrama-sistema-argon2.jpg)

### Endpoints, payloads e responses

#### `/signup/{hash_type}`
- Tipo: `POST`
- Descrição: Cria um novo usuário no sistema, armazenando a senha utilizando o algoritmo passado no *path paramenter*
- Payload:
    - Payload enviado na requisição:
      ```json
      {
          "body": <corpo da mensagem encriptado em hex>
          "iv": <iv em texto claro em hex>
      }
      ```
    - Corpo da mensagem antes de encriptar:
      ```json
      {
          "user": {
              "username": <username usario>,
              "password": <senha usuario>
          }
          "metadata": {
              "created_at": <time stamp com a data de criação - formato YYYY-MM-DD HH:MM:SS"
          }
      }
      ```
- Responses:
    - hash type diferente de "argon2" ou "pbkdf2"
      ```json
      {
          "status_code": 400,
          "detail": "Invalid hash type"
      }
      ```
    - problema na conexão com o banco
      ```json
      {
          "status_code": 500,
          "detail": "Failed to connect to the database"
      }
      ```
    - ok
      ```json
      {
          "status_code": 201,
          "message": "user created",
          "id": <user_id>
      }
      ```
- `/login`

      
