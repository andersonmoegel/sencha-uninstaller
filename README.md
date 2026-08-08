# 🗑️ Script de Desinstalação do Sencha Cmd (Usuário Atual)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=flat&logo=windows&logoColor=white)

## 📋 Documentação

Este script é responsável por desinstalar o Sencha Cmd de uma instalação localizada em `C:\Users\<usuario>\bin\Sencha`. Ele remove os arquivos e pastas relacionados e registra o processo de remoção em um arquivo de log.

## ✨ Funcionalidades

- **Detecta o diretório de instalação do Sencha Cmd para o usuário atual:** verifica se o diretório onde o Sencha Cmd está instalado existe no perfil do usuário atual.
- **Exclui todos os arquivos e pastas:** remove todo o conteúdo dentro do diretório de instalação do Sencha Cmd.
- **Registra a remoção:** grava a mensagem de sucesso ("Sencha removido com sucesso") em um arquivo de log ao concluir.
- **Caminho do log:** o arquivo de log é armazenado em `C:\Windows\Temp\Sencha_Uninstall_Log.txt`.

## 🧩 Estrutura do Código

### 1. Importação de Bibliotecas

```python
import os
import shutil
import time
import getpass
```

Essas bibliotecas são usadas para manipular diretórios e arquivos, e obter o nome do usuário atual de forma segura:

- `os`: para verificação de caminhos de arquivos e diretórios.
- `shutil`: para exclusão recursiva de diretórios.
- `time`: para registrar a data e hora da remoção.
- `getpass`: para obter o nome do usuário atual do sistema com segurança.

### 2. Variáveis de Configuração

**Caminho de instalação do Sencha Cmd**

```python
base_path = f"C:\\Users\\{getpass.getuser()}\\bin\\Sencha"
```

Este é o diretório onde o Sencha Cmd está instalado para o usuário atual. O caminho é gerado dinamicamente usando `getpass.getuser()` para tornar o script independente do usuário.

**Caminho do arquivo de log**

```python
log_path = r"C:\Windows\Temp\Sencha_Uninstall_Log.txt"
```

Este é o arquivo onde o script registra os detalhes da remoção, incluindo data e hora.

### 3. Funções

**`log_remocao()`**

```python
def log_remocao():
    """Registra a mensagem 'Sencha removido com sucesso' no arquivo de log"""
    try:
        with open(log_path, "a") as log_file:
            log_file.write(f"{time.strftime('%Y-%m-%d %H:%M:%S')} - Sencha removido com sucesso\n")
        print("Sencha CMD removido com sucesso!")  # Exibe no console
    except Exception:
        pass  # Ignora erros de log
```

Esta função adiciona uma mensagem de sucesso ao arquivo de log, incluindo a data e hora atuais. Erros durante o registro são ignorados silenciosamente.

**`excluir_arquivos(directory)`**

```python
def excluir_arquivos(directory):
    """Exclui todos os arquivos e pastas dentro do diretório informado"""
    removed = False
    for root, dirs, files in os.walk(directory, topdown=False):
        for name in files:
            try:
                os.remove(os.path.join(root, name))
                removed = True
            except Exception:
                pass
        for name in dirs:
            try:
                shutil.rmtree(os.path.join(root, name))
                removed = True
            except Exception:
                pass
    return removed
```

Esta função percorre recursivamente o diretório e remove todo o seu conteúdo. Retorna `True` se algum arquivo ou pasta foi removido, ou `False` caso contrário.

**`desinstalar_sencha()`**

```python
def desinstalar_sencha():
    """Desinstala o Sencha Cmd excluindo seus arquivos"""
    if os.path.exists(base_path):
        if excluir_arquivos(base_path):
            log_remocao()
    else:
        print("Diretório não encontrado. O Sencha Cmd pode não estar instalado.")  # Nenhum log é criado
```

Esta função verifica se o diretório de instalação existe. Se existir, tenta excluir seu conteúdo e registra o resultado. Se o diretório não for encontrado, exibe um aviso e não gera log.

### 4. Execução do Script

```python
if __name__ == "__main__":
    desinstalar_sencha()
```

Garante que o script só será executado quando chamado diretamente (não quando importado). Ele dispara o processo de desinstalação.

## 🚀 Uso

- **Execute o script:** rode-o em um ambiente Python. Ele verificará a instalação do Sencha Cmd no diretório do usuário atual e a removerá, se encontrada.
- **Verifique o log:** após a execução, revise o log em `C:\Windows\Temp\Sencha_Uninstall_Log.txt` para confirmar a desinstalação.

## 🚧 Possíveis Melhorias

- **Registro de erros aprimorado:** em vez de ignorar erros silenciosamente, o script poderia registrar exceções detalhadas para facilitar a depuração.
- **Verificação de permissões:** o script poderia ser expandido para verificar permissões do usuário antes de tentar exclusões.

## ✅ Conclusão

Este script automatiza a desinstalação do Sencha Cmd para o usuário atual. Ele garante uma remoção limpa dos arquivos e registra a ação para fins de rastreabilidade e auditoria.
