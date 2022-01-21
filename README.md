# Anotações

### 1. O **Decorator** é um padrão de projeto estrutural que permite que você acople novos comportamentos para objetos ao colocá-los dentro de invólucros de objetos que contém os comportamentos.

> Component:

> Injectable: a classe poderá ser injentada e utilizada em outra classe.

### 2. Um **módulo** (name.module.ts) é um arquivo para organizar e modularizar a aplicação, dividir responsabilidades.

> BrowserModule: prepara a aplicação para rodar em um browser.

> FormsModule: two-way-data-binding e formulários.

> HttpModule: contém o objeto http para fazer requisições para api.

> declarations: componentes, diretivas e pipes.

> imports: outros módulos para utilizar no módulo ou dentro de componentes que pertencem ao módulo.

> providers: serviços disponíveis para os componentes do módulo.

> bootstrap: só tem no módulo raiz (AppModule), define o componente container do projeto.

> Um módulo de funcionalidade não importa o BrowserModule, mas sim o CommonModule que contém as diretivas e pipes mais comuns para usar na aplicação.

> É preciso usar o metadado exports no módulo de funcionalidade para poder tornar um componente visível para módulos externos

### 3. **Interpolação** serve para usar dados do componente.ts no template, component.html

```
<p>{{nome-da-variavel}}</p>
```

### 4. O código dentro de um **componente** deve ser responsável apenas por mostrar as informações para o usuário e interarir que o mesmo.

### 5. Um **serviço** é responsável por fazer a comunicação com o servidor ou outras tarefas que não sejam a função do componente.

### 6. **Injeção de depedência** é fazer com que o angular já forneça uma instância de uma classe pronta, sem precisar usar _new NomeClasse_ em todos os lugares que forem utilizar _NomeClasse_. A instância é criada no construtor e a depender da versão do angular, deve ser adicionada a definição da classe no _providers_ do módulo.

```
  constructor(private apiService: ApiService) {}
```

# Comandos

### 1. Criar aplicação angular

```
ng new appname
```

### 2. Criar componente

```
ng g c path/componentname
```

### 3. Criar módulo

```
ng g m path/modulename
```

### 4. Criar serviço

```
ng g s path/servicename
```
