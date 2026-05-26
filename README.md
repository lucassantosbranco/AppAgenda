📱 AppAgenda - Sistema de Contatos com Banco de Dados
Um aplicativo Android desenvolvido em Java que permite gerenciar uma agenda de contatos com persistência de dados em SQLite.

🎯 Objetivo
Criar uma aplicação completa de CRUD (Create, Read, Update, Delete) onde o usuário pode:

✅ Cadastrar novos contatos (Nome, CPF, Telefone)
✅ Listar todos os contatos salvos
✅ Pesquisar contatos por nome em tempo real
✅ Atualizar dados de contatos existentes
✅ Excluir contatos do banco de dados


🏗️ Arquitetura do Projeto
Estrutura de Pastas
AppAgenda/
├── app/src/main/
│   ├── java/br/com/senacrs/appAgenda/
│   │   ├── MainActivity.java          (Tela de Cadastro)
│   │   ├── ListarPessoasActitivity.java (Tela de Listagem)
│   │   ├── Pessoa.java                (Entidade/Modelo)
│   │   ├── Conexao.java               (Gerenciador BD)
│   │   └── PessoaDAO.java             (Operações CRUD)
│   └── res/
│       ├── layout/
│       │   ├── activity_main.xml
│       │   └── activity_listar_pessoa_activity.xml
│       ├── menu/
│       │   ├── menu_principal.xml
│       │   └── menu_contexto.xml
│       └── AndroidManifest.xml

📋 Componentes Principais
1️⃣ Pessoa.java - Entidade de Dados
javapublic class Pessoa implements Serializable {
    private Integer id;
    private String nome;
    private String cpf;
    private String telefone;
    // Getters e Setters
}
Responsabilidade: Representa um contato no banco de dados.

implements Serializable permite enviar a pessoa entre telas via Intent.


2️⃣ Conexao.java - Gerenciador do Banco de Dados
javapublic class Conexao extends SQLiteOpenHelper
Responsabilidade: Criar e gerenciar a conexão com o banco SQLite.
Características:

Nome do banco: banco.db
Versão: 1
Tabela criada: pessoa com colunas (id, nome, cpf, telefone)

Método onCreate(): Executa o SQL de criação da tabela na primeira vez.

3️⃣ PessoaDAO.java - Data Access Object (CRUD)
Implementa todas as operações do banco de dados:
🟢 CREATE - Inserir
javapublic long inserir(Pessoa pessoa)
Adiciona um novo contato ao banco.
🔵 READ - Consultar
javapublic List<Pessoa> obterTodos()
Retorna todos os contatos salvos.
🟡 UPDATE - Atualizar
javapublic void atualizar(Pessoa pessoa)
Modifica dados de um contato existente.
🔴 DELETE - Excluir
javapublic void excluir(Pessoa pessoa)
Remove um contato do banco de dados.

4️⃣ MainActivity.java - Tela de Cadastro
Layout: activity_main.xml
Componentes:

EditText para Nome
EditText para CPF
EditText para Telefone
Button para Salvar

Funcionalidades:

Detecta se é novo cadastro ou atualização
Valida campos
Insere ou atualiza dados no banco
Exibe mensagens de sucesso com Toast


5️⃣ ListarPessoasActitivity.java - Tela de Listagem
Layout: activity_listar_pessoa_activity.xml
Componentes:

ListView para exibir contatos

Funcionalidades:

Exibe todos os contatos ao abrir
SearchView: Busca em tempo real por nome
Menu Contexto: Opções ao clicar em um contato (Excluir/Atualizar)
Menu Principal: Botão para cadastrar novo contato

Métodos principais:
javaprocuraPessoa(String nome)    // Filtra contatos por nome
cadastrar(MenuItem item)       // Abre tela de cadastro
excluir(MenuItem item)         // Remove contato (com confirmação)
atualizar(MenuItem item)       // Abre tela de edição

📱 Fluxo da Aplicação
┌─────────────────────────────┐
│  ListarPessoasActitivity    │ (Tela Principal)
│  - Lista todos os contatos   │
│  - SearchView para buscar    │
└──────────┬──────────────────┘
           │
     ┌─────┴──────┬─────────────┐
     │             │             │
  Cadastrar    Atualizar      Excluir
     │             │             │
     └──────┬──────┘             │
            │                    │
     ┌──────▼────────┐           │
     │ MainActivity  │           │
     │ (Formulário)  │           │
     └──────┬────────┘           │
            │                    │
     ┌──────▼──────────────┐     │
     │    PessoaDAO        │     │
     │  (CRUD Operations)  │─────┘
     └─────────┬──────────┘
               │
     ┌─────────▼────────┐
     │ SQLite (banco.db)│
     │  Tabela pessoa   │
     └──────────────────┘

🔄 Operações CRUD Detalhadas
✨ CREATE (Inserir)

Usuário preenche formulário em MainActivity
Clica em "Salvar"
PessoaDAO.inserir() insere no banco
Toast exibe: "Pessoa inserida no ID de nº: [id]"
Volta para lista e exibe novo contato

📖 READ (Listar)

App abre ListarPessoasActitivity
PessoaDAO.obterTodos() retorna todos os contatos
ArrayAdapter exibe na ListView
SearchView filtra em tempo real

✏️ UPDATE (Atualizar)

Usuário clica em contato → Menu contexto
Seleciona "Atualizar"
MainActivity abre com dados preenchidos (via Intent + Serializable)
Usuário modifica dados
PessoaDAO.atualizar() salva alterações
Toast confirma: "[Nome], atualizado com sucesso !!!"

❌ DELETE (Excluir)

Usuário clica em contato → Menu contexto
Seleciona "Excluir"
AlertDialog pede confirmação
PessoaDAO.excluir() remove do banco
ListView atualiza automaticamente


🛠️ Dependências
gradledependencies {
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
}

⚙️ Configuração do AndroidManifest.xml
xml<activity
    android:name=".ListarPessoasActitivity"
    android:theme="@style/Theme.AppCompat.Light.DarkActionBar"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>

<activity
    android:name=".MainActivity"
    android:exported="true" />
Observação: ListarPessoasActitivity é a Activity de inicialização (LAUNCHER).

📲 Menus
Menu Principal (menu_principal.xml)

Cadastrar: Ícone de adicionar (+)
Consultar: SearchView para buscar contatos

Menu Contexto (menu_contexto.xml)

Excluir: Remove o contato com confirmação
Atualizar: Abre formulário para editar


🔐 Persistência de Dados

Banco de Dados: SQLite (banco.db)
Localização: Armazenamento interno do app
Tabela: pessoa (id, nome, cpf, telefone)
Durabilidade: Dados persistem mesmo após fechar o app


📝 Como Usar
1. Cadastrar novo contato:

Clique no ícone "+" no menu superior
Preencha Nome, CPF e Telefone
Clique em "Salvar"

2. Pesquisar contato:

Na tela inicial, clique na lupa "Consultar"
Digite o nome (busca em tempo real)

3. Atualizar contato:

Pressione e segure o contato na lista
Selecione "Atualizar"
Modifique os dados
Clique em "Salvar"

4. Excluir contato:

Pressione e segure o contato na lista
Selecione "Excluir"
Confirme a exclusão


🎓 Conceitos Aprendidos
✅ SQLiteOpenHelper - Gerenciamento de banco de dados
✅ SQLiteDatabase - Operações CRUD com SQL
✅ ContentValues - Inserção e atualização de dados
✅ Cursor - Recuperação de dados de consultas
✅ DAO Pattern - Separação de lógica de dados
✅ Intent e Serializable - Envio de objetos entre telas
✅ ListView e ArrayAdapter - Exibição de listas
✅ SearchView - Busca em tempo real
✅ AlertDialog - Caixas de diálogo
✅ Menu Principal e Contexto - Menus no Android

🐛 Possíveis Melhorias

 Adicionar validação mais robusta de CPF
 Implementar RecyclerView no lugar de ListView
 Adicionar animações de transição
 Criar tela de detalhes do contato
 Implementar backup automático
 Adicionar foto do contato
 Migrar para Room Database (mais moderno)


👨‍💻 Autor
Projeto Educacional - Sistema de Agenda com SQLite
Desenvolvido para aprender CRUD em Android.

📄 Licença
Este projeto é de caráter educacional.
