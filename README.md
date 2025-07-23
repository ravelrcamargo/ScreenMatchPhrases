# ScreenMatchPhrases
📚 ScreenMatchPhrases
Projeto que integra back-end + front-end + banco de dados utilizando Spring Boot, JPA, PostgreSQL e React.

🚀 Tecnologias utilizadas
Java + Spring Boot

Spring Web

Spring Data JPA

PostgreSQL

Spring Boot DevTools

React (front-end)

Postman (testes)

⚙️ Configuração inicial
O projeto foi iniciado com o Spring Initializr, onde configurei o nome do projeto e adicionei as dependências necessárias:

Spring Web

Spring Data JPA

PostgreSQL Driver

Spring Boot DevTools

Em seguida, no arquivo application.properties (localizado em src/main/resources), configurei a conexão com o banco de dados, informando:

URL do banco

Usuário e senha

Dialeto

Estratégias de geração e exibição de queries

Também é possível usar variáveis de ambiente para proteger dados sensíveis.

🧱 Entidade Frase
Foi criada a classe Frase, que é mapeada como uma tabela no banco de dados utilizando:

@Entity: indica que a classe representa uma entidade JPA.

@Table(name = "frases"): define o nome da tabela no banco.

A classe possui atributos como titulo, frase, personagem e poster, além de um campo id com a estratégia de geração:

java
Copiar
Editar
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
A estratégia IDENTITY permite que o banco gere automaticamente o valor do ID a cada novo registro. Se não utilizássemos isso, precisaríamos informar o ID manualmente ao salvar um novo objeto.

🎯 Controller (FraseController)
A classe FraseController é responsável por receber requisições HTTP e retornar respostas para o cliente.

Ela atua como ponte entre o navegador e a lógica de negócio, redirecionando a requisição para o FraseService.

Anotações utilizadas:

@RestController: define que a classe será um controller REST (responde em JSON).

@GetMapping("/series/frases"): mapeia a URL /series/frases para um método GET.

@Autowired: injeta automaticamente o FraseService sem precisar instanciar com new.

Exemplo:

java
Copiar
Editar
@GetMapping("/series/frases")
public FraseDTO obterFraseAleatoria() {
    return servico.obterFraseAleatoria();
}
💼 DTO (FraseDTO)
O FraseDTO é um record (Java 16+) utilizado para transferir apenas os dados necessários para o cliente.

Isso evita o envio de dados desnecessários ou sensíveis da entidade original (Frase), além de manter o código mais seguro e limpo.

Vantagens de usar record:

Imutável

Código reduzido

Ideal para transporte de dados entre camadas

Exemplo:

java
Copiar
Editar
public record FraseDTO(String titulo, String frase, String personagem, String poster) {}
🧠 Service (FraseService)
A classe FraseService contém a lógica de negócio da aplicação.
Ela utiliza o FraseRepository para acessar o banco, obtém a entidade Frase, e a converte em um FraseDTO.

Exemplo:

java
Copiar
Editar
@Service
public class FraseService {

    @Autowired
    private FraseRepository repository;

    public FraseDTO obterFraseAleatoria() {
        Frase frase = repository.buscaFraseAleatoria();
        return new FraseDTO(frase.getTitulo(), frase.getFrase(), frase.getPersonagem(), frase.getPoster());
    }
}
🗄️ Repository (FraseRepository)
O FraseRepository é a interface que acessa o banco de dados.

Ela estende JpaRepository<Frase, Long>, o que já fornece métodos prontos como:

findAll()

findById()

save()

deleteById()

Também permite criar consultas personalizadas com @Query.

Exemplo:

java
Copiar
Editar
@Query("SELECT f FROM Frase f ORDER BY function('RANDOM') LIMIT 1")
Frase buscaFraseAleatoria();
Esse método retorna uma frase aleatória do banco.

🔓 CORS Configuration
A classe de configuração CORS define quais origens podem acessar a API, quais métodos HTTP são permitidos, e quais cabeçalhos são aceitos.

Isso é fundamental para permitir que o front-end (React) acesse a API que está em outro domínio ou porta (ex: localhost:3000 chamando localhost:8080).

✅ Fluxo geral da aplicação
txt
Copiar
Editar
[Front-end] → [Controller] → [Service] → [Repository] → [Banco de Dados]
                                     ↑
                             [DTO usado para resposta]


