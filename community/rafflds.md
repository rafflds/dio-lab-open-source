# Utilitário: Gerador de Card de Perfil do GitHub

Este utilitário é um script Python que gera um card em Markdown com informações do perfil de um usuário do GitHub. Ele utiliza a API do GitHub para obter os dados e formata a saída para ser facilmente inserida em um arquivo README.

## Como Usar

1.  **Requisitos:**
    *   Python 3.6 ou superior instalado.
    *   A biblioteca `requests` instalada. Se não a tiver, você pode instalar usando:
        ```bash
        pip install requests
        ```

2.  **Execução:**
    *   Navegue até o diretório `utils`:
        ```bash
        cd utils
        ```
    *   Execute o script:
        ```bash
        python github_profile_card.py
        ```
    *   O script irá solicitar o nome de usuário do GitHub. Insira o nome desejado e pressione Enter.
    *   O script exibirá o card em Markdown no console.

3.  **Inserir no seu README:**
    *   Copie o resultado gerado pelo script (o código Markdown).
    *   Cole o código Markdown no seu arquivo README.md (ou onde desejar).

## Código

import requests

def generate_github_profile_card(username):
    """Gera um card em Markdown com informações de perfil do GitHub."""

    try:
        response = requests.get(f"https://api.github.com/users/{username}")
        response.raise_for_status()
        user_data = response.json()

        card = f"""
<details>
<summary><b>{user_data['name'] or username}</b></summary>
<p align="center">
    <img src="{user_data['avatar_url']}" width="150" alt="Avatar de {user_data['name'] or username}"/>
</p>
<p align="center">
  <b>{user_data['bio'] or 'Sem bio'}</b> <br>
  Localização: {user_data['location'] or 'Não informado'} <br>
  Seguidores: {user_data['followers']} | Seguindo: {user_data['following']} <br>
  <a href="{user_data['html_url']}">Perfil do GitHub</a>
</p>
</details>
"""
        return card

    except requests.exceptions.RequestException as e:
        return f"Erro ao obter informações do perfil: {e}"
    except Exception as e:
        return f"Erro inesperado: {e}"

if __name__ == "__main__":
    username = input("Digite o nome de usuário do GitHub: ")
    card = generate_github_profile_card(username)
    print(card)
