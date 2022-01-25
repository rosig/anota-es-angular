# Anotações

### 1. O **Decorator** é um padrão de projeto estrutural que permite que você acople novos comportamentos para objetos ao colocá-los dentro de invólucros de objetos que contém os comportamentos.

> @Component: permite marcar uma classe como um componente Angular e fornecer metadados adicionais que determinam como o componente deve ser processado, instanciado e usado. Os componentes são o bloco de construção mais básico de uma interface do usuário em um aplicativo Angular.

> @Injectable: a classe poderá ser injentada e utilizada em outra classe.

> @NgModule: a classe passará a ser usada como um módulo

> @Input('nome_para_receber_valor'[opcional]): expõe uma propriedade para receber dados do componente pai

> @ViewChild(''[opcional]): usado para acessar uma diretiva, componente filho ou um elemento DOM, através de referência

> @Directive: usado para criar uma diretiva

```
Exemplo

No componente.html:

<input type="text" readonly [value]="valor" #campoInput/>

No componente.ts:

 @ViewChild('campoInput') campoValorInput: ElementRef; // campoValorInput é uma referência para a tag <input />

incrementa() {
  this.campoValorInput.nativeElement.value++;
}
```

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

### 3. O código dentro de um **componente** deve ser responsável apenas por mostrar as informações para o usuário e interarir que o mesmo.

### 4. Um **serviço** é responsável por fazer a comunicação com o servidor ou outras tarefas que não sejam a função do componente.

### 5. **Injeção de depedência** é fazer com que o angular já forneça uma instância de uma classe pronta, sem precisar usar _new NomeClasse_ em todos os lugares que forem utilizar _NomeClasse_. A instância é criada no construtor e a depender da versão do angular, deve ser adicionada a definição da classe no _providers_ do módulo.

```
  constructor(private apiService: ApiService) {}
```

### 6. **Data binding** é uma forma de associar informações que estão no componente para o template e virse-versa.

> **Interpolation** : component -> template

```
<p>{{ nome-da-variavel }}</p>
<p>{{ 1 + 1 }}</p>
<p>{{ 1 + getValor() }}</p>
<p>{{ isAngular && true }}</p>
```

> **Property binding** : component -> template

```
<p [propriedade]="data"></p>
<p bind-propriedade="data"></p>
<p propriedade={{ data }}></p>

Quando não existe uma propriedade no elemento, usa-se [attr.nomePropriedade]
```

```
<article>
  <h3>Class & Style Binding</h3>
  <div>
    Selecione uma classe:
    <select #classe (change)="0">
      <option value="alert-success">success</option>
      <option value="alert-danger">danger</option>
    </select>

    <div class="alert {{ classe.value }}" role="alert"></div>

    <div class="alert" role="alert" [class.alert-success]="classe.value === 'alert-success'">success</div>
    <div class="alert" role="alert" [class.alert-danger]="classe.value === 'alert-danger'">danger</div>

    <div class="alert alert-danger" role="alert" [style.display]="classe.value === 'alert-danger' ? 'block' : 'none'">Esse texto só aparece em caso de erro!</div>
  </div>
</article>
```

> **Event binding** : template -> component

```
<p (evento)="handler"></p>
```

> **Two-way binding** : template -> component | component -> template ( obs: não esquecer do módulo _FormsModule_ para o two-way data binding funcionar.)

```
<p [(ngModel)]="data"></p>
```

> **Input properties** : componente pai -> data -> componente filho

```
No componente.ts do filho:

@Input('nome-opcional-alternativo-para-usar-no-lugar-de-nomeVariavel') nomeVariavel: type = '';

No componente.html do pai:

<componente-filho [nomeVariavel]="data"></componente-filho>
```

> **Output properties** : componente filho -> (evento com ou sem dados) -> componente pai

```
No componente.ts do filho:

@Output() nomeDoEvento = new EventEmitter();

this.nomeDoEvento.emit({ data: this.valor }); [dispara evento para ser ouvido no componente pai, através do parâmetro $event]

No componente.html do pai:

<componente-filho (nomeDoEvento)="onNomeDoEvento($event)"></componente-filho>

onNomeDoEvento(evento: any) {
  console.log(evento);
}
```

### 7. **Ciclo de vida** (fases que vão da criação à destruição do componente)

> ngOnChanges: chamado uma vez na criação do componente e sempre que houver alteração em uma de suas propriedades de entrada. Ou seja, mudanças no Input() decorator e no property binding.

> ngOnInit: chamado uma única vez quando o componente é inicializado (logo após o primeiro ngOnChanges).

> ngDoCheck: chamado a cada ciclo de detecção de alterações (processo que percorre o componente atrás de mudanças). Portanto use ao invés do ngOnChanges para alterações que o Angular não detecta.

> ngOnDestroy: chamado antes do Angular destruir o componente.

_Além disso existem outros 4 ganchos dentro do ngDoCheck:_

> ngAfterContentInit: chamado depois que o conteúdo externo é inserido no componente (por exemplo, vindo do ng-content).

> ngAfterContentChecked: invocado após a verificação do conteúdo externo.

> ngAfterViewInit: chamado logo após o conteúdo do próprio componente e de seus filhos ser inicializado.

> ngAfterViewChecked: chamado cada vez que o conteúdo do componente é verificado pelo mecanismo de detecção de alterações do Angular.

_Se não tiver input propertie, podemos usar ngOnInit, se tiver, utilizar ngOnChanges é uma opção, porque ambos são disparados._

### 8. **Diretivas**

1. Diretivas são uma forma de passar instruções para o template.

2. Diretivas estruturais (*ngFor; *ngIf): interagem com a view e modificam a estrutura do DOM e/ou código HTML.

3. Diretivas de atributos (*ng-class; *ng-style): não modificam a estrutura do dom, apenas interagem com o elemento em que foram aplicadas, modificam a aparência.

> \*ngIf (estrutural)

```
<div *ngIf="conditionExpression"></div>
```

> \*ngSwitch (estrutural)

```
<div [ngSwitch]="conditionExpression">
  <p *ngSwitchCase="condition1">condition1 content</p>
  <p *ngSwitchCase="condition2">condition2 content</p>
  <p *ngSwitchDefault>default content</p>
</div>
```

Será mostrado o conteúdo cujo valor do ngSwitchCase for igual ao valor do conditionExpression

> \*ngFor (estrutural)

```
<ul>
  <li *ngFor="let item of lista, let i = index">
    {{ i + 1 }} - {{ item }}
  </li>
</ul>
```

> ngClass (atributo)

```
<h1>
  <i class="glyphicon"
  [ngClass]="{
    'glyphicon-star-empty': !condition,
    'glyphicon-star': condition
  }"
  ></i>
</h1>
```

> ngStyle (atributo)

```
<button
  [ngStyle]="{
    'backgroundColor': (ativo ? 'blue' : 'gray'),
    'color': (ativo ? 'white' : 'black'),
    'fontWeight': (ativo ? 'bold' : 'normal'),
    'fontSize': tamanhoFonte + 'px'
  }"
  (click)="mudarAtivo()"
>
Mudar atributo 'ativo'
</button>
```

> Elvis

```
<p>Descrição: {{ tarefa.desc }}</p>
<!--p>Responsável: {{ tarefa.responsavel.nome }}</p-->
<!--p>Responsável: {{ tarefa.responsavel != null ? tarefa.responsavel.nome : '' }}</p-->
<p>Responsável: {{ tarefa.responsavel?.usuario?.nome }}</p>

```

> ngContent (similar ao _children_ do react, o conteúdo dentro da tag do template que tem a diretiva _ng-content_ será colocado dentro na mesma. O uso do select e class são opcionais, para o caso de ser necessário derecionar um conteúdo para um lugar específico do template)

```
<div class="panel panel-default">
  <div class="panel-heading">
    <ng-content select=".titulo"></ng-content>
  </div>
  <div class="panel-body">
    <ng-content select=".corpo"></ng-content>
  </div>
</div>

<app-exemplo-ng-content>
  <div class="titulo">Título do Painel TITULO</div>
  <div class="corpo">
    Conteúdo passado para o componente.
  </div>
  <div class="corpo">
    Conteúdo passado para o componente 2.
  </div>
</app-exemplo-ng-content>
```

> Diretivas customizadas

```
Criação:

import { Directive, ElementRef, Renderer2 } from "@angular/core";

@Directive({
  selector: "p[fundoAmarelo]",
})
export class FundoAmareloDirective {
  constructor(private _elementRef: ElementRef, private _renderer: Renderer2) {
    this._renderer.setStyle(
      this._elementRef.nativeElement,
      "background-color",
      "yellow"
    );
  }
}

Uso:

<p fundoAmarelo >
  Texto com fundo amarelo.
</p>

```

> @HostListener (ouve eventos no hospedeiro da diretiva).

> @HostBinding (permite fazer associação de um atributo ou classe do html, para uma variável).

```

import { Directive, HostListener, HostBinding, ElementRef, Renderer2 } from '@angular/core';

@Directive({
  selector: '[highlightMouse]'
})
export class HighlightMouseDirective {

  @HostListener('mouseenter') onMouseOver(){
      this.backgroundColor = 'yellow';
  }

  @HostListener('mouseleave') onMouseLeave(){
      this.backgroundColor = 'white';
  }

  //@HostBinding('style.backgroundColor') backgroundColor: string;
  @HostBinding('style.backgroundColor') get setColor(){
    //codigo extra;
    return this.backgroundColor;
  }
  private backgroundColor: string;

  constructor() { }
}


Uso:

<p highlightMouse>
  Texto com highlight quando passo o mouse.
</p>

```

> É possível utilizar input property + property binding em diretivas customizadas, para passar informações customizadas para a diretiva

```
import { Directive, HostListener, HostBinding,
  Input } from '@angular/core';

@Directive({
  selector: '[highlight]'
})
export class HighlightDirective {

  @HostListener('mouseenter') onMouseOver(){
      this.backgroundColor = this.highlightColor;
  }

  @HostListener('mouseleave') onMouseLeave(){
      this.backgroundColor = this.defaultColor;
  }

  @HostBinding('style.backgroundColor') backgroundColor: string;

  @Input() defaultColor: string = 'white';
  @Input('highlight') highlightColor: string = 'yellow';

  constructor() { }

  ngOnInit(){
    this.backgroundColor = this.defaultColor;
  }

}
```

> Exemplo de diretiva de estrutural customizada (ngElse)

```
import { Directive, Input,
  TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[ngElse]'
})
export class NgElseDirective {

  @Input() set ngElse(condition: boolean){
    if (!condition){
      this._viewContainerRef.createEmbeddedView(this._templateRef);
    } else {
      this._viewContainerRef.clear();
    }
  }

  constructor(
    private _templateRef: TemplateRef<any>,
    private _viewContainerRef: ViewContainerRef
  ) { }

}
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

### 5. Criar diretiva

```
ng g d path/directivename
```
