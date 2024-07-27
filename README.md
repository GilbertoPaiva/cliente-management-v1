
# Projeto de Gerenciamento de Clientes

Este projeto é uma aplicação simples para gerenciar entidades de cliente, utilizando uma interface gráfica para interação com o usuário. O sistema utiliza o padrão de design DAO (Data Access Object) para realizar operações CRUD (Criar, Ler, Atualizar, Excluir) em clientes, armazenando os dados em um `Map` interno.

## Estrutura do Projeto

O projeto está dividido em quatro pacotes principais:

1. **`domain`**: Contém a classe de entidade `Cliente`.
2. **`dao`**: Contém a interface `IClienteDAO` e sua implementação `ClienteMapDAO`.
3. **`br.com.gpaiva`**: Contém a classe principal `App` que inicia a aplicação.

### Pacote `domain`

#### Classe `Cliente`

A classe `Cliente` representa um cliente com os seguintes atributos:

- `nome`: Nome do cliente.
- `cpf`: CPF do cliente (usado como chave única).
- `tel`: Telefone do cliente.
- `end`: Endereço do cliente.
- `numero`: Número da residência.
- `cidade`: Cidade do cliente.
- `estado`: Estado do cliente.

```java
public class Cliente {
    private String nome;
    private Long cpf;
    private Long tel;
    private String end;
    private Integer numero;
    private String cidade;
    private String estado;
    // Construtor, getters e setters
}
```

### Pacote `dao`

#### Interface `IClienteDAO`

Define as operações básicas para manipulação de dados de clientes.

- `cadastrar(Cliente cliente)`: Adiciona um novo cliente.
- `excluir(Long cpf)`: Remove um cliente pelo CPF.
- `alterar(Cliente cliente)`: Altera os dados de um cliente.
- `consultar(Long cpf)`: Consulta um cliente pelo CPF.
- `buscarTodos()`: Retorna todos os clientes cadastrados.

```java
public interface IClienteDAO {
    Boolean cadastrar(Cliente cliente);
    void excluir(Long cpf);
    void alterar(Cliente cliente);
    Cliente consultar(Long cpf);
    Collection<Cliente> buscarTodos();
}
```

#### Classe `ClienteMapDAO`

Implementa a interface `IClienteDAO` utilizando um `HashMap` para armazenamento dos clientes.

```java
public class ClienteMapDAO implements IClienteDAO {
    private Map<Long, Cliente> map;

    public ClienteMapDAO() {
        this.map = new HashMap<>();
    }

    @Override
    public Boolean cadastrar(Cliente cliente){
        if(this.map.containsKey(cliente.getCpf())){
            return false;
        }
        this.map.put(cliente.getCpf(), cliente);
        return true;
    }

    @Override
    public void excluir(Long cpf){
        this.map.remove(cpf);
    }

    @Override
    public void alterar(Cliente cliente){
        this.map.put(cliente.getCpf(), cliente);
    }

    @Override
    public Cliente consultar(Long cpf) {
        return this.map.get(cpf);
    }

    @Override
    public Collection<Cliente> buscarTodos() {
        return this.map.values();
    }
}
```

### Pacote `br.com.gpaiva`

#### Classe `App`

A classe `App` é a entrada principal da aplicação e utiliza a biblioteca `JOptionPane` para interagir com o usuário através de uma interface gráfica simples. A aplicação permite ao usuário cadastrar, consultar, alterar e excluir clientes.

```java
public class App {

    private static IClienteDAO iClienteDAO;

    public static void main(String args[]) {
        iClienteDAO = new ClienteMapDAO();

        String opcao = JOptionPane.showInputDialog(null,
                "Digite 1 para cadastro, 2 para consultar, 3 para exclusão, 4 para alteração ou 5 para sair",
                "Cadastro", JOptionPane.INFORMATION_MESSAGE);

        while (!isOpcaoValida(opcao)) {
            if ("".equals(opcao)) {
                sair();
            }
            opcao = JOptionPane.showInputDialog(null,
                    "Opção inválida digite 1 para cadastro, 2 para consulta, 3 para exclusão, 4 para alteração ou 5 para sair",
                    "Green dinner", JOptionPane.INFORMATION_MESSAGE);
        }

        while (isOpcaoValida(opcao)) {
            if (isOpcaoSair(opcao)) {
                sair();
            } else if (isCadastro(opcao)) {
                String dados = JOptionPane.showInputDialog(null,
                        "Digite os dados do cliente separados por vírgula, conforme exemplo: Nome, CPF, Telefone, Endereço, Número, Cidade e Estado",
                        "Cadastro", JOptionPane.INFORMATION_MESSAGE);
                cadastrar(dados);
            } else if (isConsultar(opcao)) {
                String dados = JOptionPane.showInputDialog(null,
                        "Digite o cpf",
                        "Consultar", JOptionPane.INFORMATION_MESSAGE);
                consultar(dados);
            } else if (isExcluir(opcao)) {
                String dados = JOptionPane.showInputDialog(null,
                        "Digite o cpf",
                        "Excluir", JOptionPane.INFORMATION_MESSAGE);
                excluir(dados);
            } else if (isAlterar(opcao)) {
                String dados = JOptionPane.showInputDialog(null,
                        "Digite os dados do cliente separados por vírgula, conforme exemplo: Nome, CPF, Telefone, Endereço, Número, Cidade e Estado",
                        "Alterar", JOptionPane.INFORMATION_MESSAGE);
                alterar(dados);
            }

            opcao = JOptionPane.showInputDialog(null,
                    "Digite 1 para cadastro, 2 para consulta, 3 para exclusão, 4 para alteração ou 5 para sair",
                    "Green dinner", JOptionPane.INFORMATION_MESSAGE);
        }
    }

    private static void consultar(String dados) {
        Cliente cliente = iClienteDAO.consultar(Long.parseLong(dados));
        if (cliente != null) {
            JOptionPane.showMessageDialog(null, "Cliente encontrado: " + cliente.toString(), "Sucesso", JOptionPane.INFORMATION_MESSAGE);
        } else {
            JOptionPane.showMessageDialog(null, "Cliente não encontrado: ", "Sucesso", JOptionPane.INFORMATION_MESSAGE);
        }
    }


    // Outros métodos auxiliares como isOpcaoValida, isCadastro, sair, etc.
}
```

## Como Executar

1. Clone o repositório.
2. Importe o projeto em sua IDE preferida.
3. Execute a classe `App` para iniciar a aplicação com interface gráfica.

## Contribuições

Sinta-se à vontade para contribuir com melhorias. Faça um fork do projeto, crie uma branch para sua feature e envie um pull request.

---

Essa documentação reflete as funcionalidades atuais do projeto e está formatada para ser visualmente clara e fácil de navegar. Se precisar de mais alguma coisa, estou à disposição!
