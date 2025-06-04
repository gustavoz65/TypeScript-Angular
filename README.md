# TypeScript-Angular

# Manual Completo de TypeScript e Angular

## Sumário
1. [Introdução](#introdução)
2. [Preparando o Ambiente de Desenvolvimento](#preparando-o-ambiente-de-desenvolvimento)
3. [Fundamentos do TypeScript](#fundamentos-do-typescript)
4. [Conceitos Básicos do Angular](#conceitos-básicos-do-angular)
5. [Criando uma API RESTful](#criando-uma-api-restful)
6. [Desenvolvendo um Site Angular](#desenvolvendo-um-site-angular)
7. [Integração da API com o Frontend](#integração-da-api-com-o-frontend)
8. [Projeto Completo](#projeto-completo)
9. [Referências Adicionais](#referências-adicionais)

## Introdução

Este manual é um guia passo a passo para começar com TypeScript e Angular, culminando no desenvolvimento de um aplicativo completo com backend API RESTful e frontend em Angular.

### O que é TypeScript?

TypeScript é uma linguagem de programação desenvolvida pela Microsoft que estende o JavaScript adicionando tipagem estática. Isso significa que podemos definir os tipos das variáveis, parâmetros e retornos de funções, possibilitando a detecção de erros durante o desenvolvimento, antes mesmo da execução do código.

### O que é Angular?

Angular é um framework front-end desenvolvido pelo Google para criar aplicações web dinâmicas. É baseado em componentes e oferece recursos avançados como injeção de dependência, vinculação de dados bidirecional, roteamento e muito mais.

## Preparando o Ambiente de Desenvolvimento

Antes de começarmos, precisamos configurar nosso ambiente de desenvolvimento instalando todas as ferramentas necessárias.

### Instalando o Node.js e npm

1. Acesse o site oficial do [Node.js](https://nodejs.org/)
2. Baixe a versão LTS (Long Term Support)
3. Siga as instruções de instalação para seu sistema operacional
4. Para verificar se a instalação foi bem-sucedida, abra o terminal e execute:

```bash
node -v
npm -v
```

### Instalando o TypeScript

Com o npm instalado, podemos instalar o TypeScript globalmente:

```bash
npm install -g typescript
```

Para verificar a instalação:

```bash
tsc --version
```

### Instalando o Angular CLI

O Angular CLI (Command Line Interface) é uma ferramenta essencial que nos ajuda a criar, gerenciar e construir aplicações Angular:

```bash
npm install -g @angular/cli
```

Para verificar a instalação:

```bash
ng version
```

### Configurando um Editor de Código

Recomendo o uso do Visual Studio Code (VS Code) que tem suporte excelente para TypeScript e Angular:

1. Baixe e instale o [VS Code](https://code.visualstudio.com/)
2. Instale as extensões recomendadas:
   - Angular Language Service
   - ESLint
   - Prettier - Code formatter
   - TypeScript Hero

## Fundamentos do TypeScript

Vamos explorar os conceitos básicos do TypeScript para compreender como ele funciona.

### Seu Primeiro Arquivo TypeScript

Crie um arquivo chamado `hello.ts` com o seguinte conteúdo:

```typescript
// hello.ts
function saudacao(nome: string): string {
    return `Olá, ${nome}!`;
}

const mensagem: string = saudacao("Mundo");
console.log(mensagem);
```

Para compilar este arquivo para JavaScript, execute:

```bash
tsc hello.ts
```

Isso gerará um arquivo `hello.js` que pode ser executado com Node.js:

```bash
node hello.js
```

### Tipos Básicos

TypeScript oferece vários tipos para diferentes necessidades:

```typescript
// Tipos básicos
let booleano: boolean = true;
let numero: number = 10;
let texto: string = "TypeScript";
let listaNumeros: number[] = [1, 2, 3];
let tupla: [string, number] = ["idade", 25];

// Enum
enum Cor {
    Vermelho,
    Verde,
    Azul
}
let corFavorita: Cor = Cor.Azul;

// Any (qualquer tipo)
let variavel: any = "qualquer valor";
variavel = 100; // válido

// Void (sem retorno)
function log(mensagem: string): void {
    console.log(mensagem);
}

// Null e Undefined
let n: null = null;
let u: undefined = undefined;

// Never (nunca retorna)
function erro(mensagem: string): never {
    throw new Error(mensagem);
}
```

### Interfaces e Tipos

As interfaces definem contratos na sua aplicação:

```typescript
// Interface
interface Pessoa {
    nome: string;
    idade: number;
    email?: string; // Propriedade opcional
    cumprimentar(): void;
}

// Implementando a interface
const usuario: Pessoa = {
    nome: "Maria",
    idade: 30,
    cumprimentar() {
        console.log(`Olá, meu nome é ${this.nome}`);
    }
};

// Type alias
type Coordenada = {
    x: number;
    y: number;
};

const ponto: Coordenada = { x: 10, y: 20 };
```

### Classes

O TypeScript suporta programação orientada a objetos com classes:

```typescript
class Funcionario {
    // Propriedades da classe
    private nome: string;
    protected salario: number;
    readonly departamento: string;

    // Construtor
    constructor(nome: string, salario: number, departamento: string) {
        this.nome = nome;
        this.salario = salario;
        this.departamento = departamento;
    }

    // Métodos
    apresentar(): void {
        console.log(`Sou ${this.nome} do departamento ${this.departamento}`);
    }

    // Getters e setters
    get nomeCompleto(): string {
        return this.nome;
    }

    set nomeCompleto(nome: string) {
        this.nome = nome;
    }
}

// Herança
class Gerente extends Funcionario {
    private equipe: string[];

    constructor(nome: string, salario: number, equipe: string[]) {
        super(nome, salario, "Gerência");
        this.equipe = equipe;
    }

    // Sobrescrevendo método
    apresentar(): void {
        super.apresentar();
        console.log(`Gerencio uma equipe de ${this.equipe.length} pessoas`);
    }
}

const gerente = new Gerente("Carlos", 8000, ["Ana", "Pedro", "Julia"]);
gerente.apresentar();
```

### Generics

Generics permitem criar componentes reutilizáveis:

```typescript
// Função genérica
function primeiroDaLista<T>(lista: T[]): T | undefined {
    return lista[0];
}

const primeiroNumero = primeiroDaLista([1, 2, 3]); // tipo: number
const primeiroNome = primeiroDaLista(["Ana", "João", "Pedro"]); // tipo: string

// Classes genéricas
class Fila<T> {
    private dados: T[] = [];

    adicionar(item: T): void {
        this.dados.push(item);
    }

    remover(): T | undefined {
        return this.dados.shift();
    }
}

const filaNumerica = new Fila<number>();
filaNumerica.adicionar(10);
```

### Decoradores

Decoradores são um recurso experimental que permite adicionar anotações e metaprogramação:

```typescript
// Ativar experimentalDecorators no tsconfig.json
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const metodoOriginal = descriptor.value;
    
    descriptor.value = function(...args: any[]) {
        console.log(`Chamando método ${propertyKey}`);
        return metodoOriginal.apply(this, args);
    };
    
    return descriptor;
}

class ExemploDecorador {
    @log
    metodoComLog(a: number, b: number): number {
        return a + b;
    }
}

const exemplo = new ExemploDecorador();
exemplo.metodoComLog(5, 10);
```

## Conceitos Básicos do Angular

Agora vamos explorar os conceitos fundamentais do Angular.

### Criando um Novo Projeto

Usando o Angular CLI, vamos criar um novo projeto:

```bash
ng new meu-primeiro-app
```

O CLI fará algumas perguntas:
- Would you like to add Angular routing? (Y/n) - Responda Y
- Which stylesheet format would you like to use? - Selecione CSS

Depois que o projeto for criado, navegue até a pasta e inicie a aplicação:

```bash
cd meu-primeiro-app
ng serve --open
```

A flag `--open` abrirá automaticamente seu navegador no endereço `http://localhost:4200/`.

### Estrutura do Projeto Angular

Vamos entender os principais arquivos e pastas:

```
meu-primeiro-app/
├── node_modules/       # Dependências instaladas
├── src/                # Código-fonte da aplicação
│   ├── app/            # Componentes da aplicação
│   ├── assets/         # Recursos estáticos (imagens, etc.)
│   ├── environments/   # Configurações de ambiente
│   ├── index.html      # Página HTML principal
│   ├── main.ts         # Ponto de entrada da aplicação
│   └── styles.css      # Estilos globais
├── angular.json        # Configuração do projeto Angular
├── package.json        # Dependências NPM
└── tsconfig.json       # Configuração do TypeScript
```

### Componentes

Os componentes são blocos de construção fundamentais no Angular:

1. Crie um novo componente:

```bash
ng generate component hello
# ou forma abreviada
ng g c hello
```

2. Examine os arquivos criados em `src/app/hello/`:

```typescript
// hello.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-hello',
  templateUrl: './hello.component.html',
  styleUrls: ['./hello.component.css']
})
export class HelloComponent {
  nome: string = 'Mundo';
}
```

3. Edite o template em `hello.component.html`:

```html
<div class="container">
  <h2>Olá {{ nome }}!</h2>
  <button (click)="trocarNome()">Mudar Nome</button>
</div>
```

4. Adicione um método no componente:

```typescript
export class HelloComponent {
  nome: string = 'Mundo';
  
  trocarNome() {
    this.nome = this.nome === 'Mundo' ? 'Angular' : 'Mundo';
  }
}
```

5. Use o componente no `app.component.html`:

```html
<div class="content">
  <h1>Meu Primeiro App Angular</h1>
  <app-hello></app-hello>
</div>
```

### Interpolação e Data Binding

Angular oferece várias maneiras de conectar os dados entre classe e template:

```html
<!-- Interpolação -->
<h2>{{ titulo }}</h2>

<!-- Property binding -->
<img [src]="imagemUrl">
<button [disabled]="botaoDesativado">Clique</button>

<!-- Event binding -->
<button (click)="onClick()">Clique em mim</button>
<input (input)="onInput($event)">

<!-- Two-way binding -->
<input [(ngModel)]="nome">
<p>Você digitou: {{ nome }}</p>
```

Para usar `ngModel`, precisamos importar `FormsModule` no `app.module.ts`:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
import { HelloComponent } from './hello/hello.component';

@NgModule({
  declarations: [
    AppComponent,
    HelloComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Diretivas

Diretivas permitem manipular o DOM:

```html
<!-- ngIf -->
<div *ngIf="exibir">Este conteúdo será exibido condicionalmente</div>

<!-- ngFor -->
<ul>
  <li *ngFor="let item of itens; let i = index">{{ i }} - {{ item }}</li>
</ul>

<!-- ngClass -->
<div [ngClass]="{'ativo': estaAtivo, 'grande': tamanhoGrande}">
  Classes dinâmicas
</div>

<!-- ngStyle -->
<div [ngStyle]="{'color': cor, 'font-size': tamanho + 'px'}">
  Estilos dinâmicos
</div>

<!-- ngSwitch -->
<div [ngSwitch]="cor">
  <p *ngSwitchCase="'vermelho'">Você escolheu vermelho</p>
  <p *ngSwitchCase="'azul'">Você escolheu azul</p>
  <p *ngSwitchDefault>Cor não reconhecida</p>
</div>
```

### Serviços e Injeção de Dependência

Serviços são usados para compartilhar lógica e dados entre componentes:

1. Crie um serviço:

```bash
ng generate service dados
# ou forma abreviada
ng g s dados
```

2. Implemente o serviço:

```typescript
// dados.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class DadosService {
  private itens: string[] = ['Item 1', 'Item 2', 'Item 3'];

  constructor() { }

  getItens(): string[] {
    return this.itens;
  }

  adicionarItem(item: string): void {
    this.itens.push(item);
  }
}
```

3. Use o serviço em um componente:

```typescript
// lista.component.ts
import { Component, OnInit } from '@angular/core';
import { DadosService } from '../dados.service';

@Component({
  selector: 'app-lista',
  templateUrl: './lista.component.html',
})
export class ListaComponent implements OnInit {
  itens: string[] = [];
  novoItem: string = '';

  constructor(private dadosService: DadosService) { }

  ngOnInit(): void {
    this.carregarItens();
  }

  carregarItens(): void {
    this.itens = this.dadosService.getItens();
  }

  adicionarItem(): void {
    if (this.novoItem.trim()) {
      this.dadosService.adicionarItem(this.novoItem);
      this.novoItem = '';
    }
  }
}
```

4. Template correspondente:

```html
<!-- lista.component.html -->
<div>
  <h2>Lista de Itens</h2>
  <ul>
    <li *ngFor="let item of itens">{{ item }}</li>
  </ul>
  
  <div>
    <input [(ngModel)]="novoItem" placeholder="Novo item">
    <button (click)="adicionarItem()">Adicionar</button>
  </div>
</div>
```

### Roteamento

O roteamento permite navegar entre diferentes componentes:

1. Configure as rotas em `app-routing.module.ts`:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { SobreComponent } from './sobre/sobre.component';
import { ListaComponent } from './lista/lista.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'sobre', component: SobreComponent },
  { path: 'lista', component: ListaComponent },
  { path: '**', redirectTo: '' } // Redireciona rotas não encontradas
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

2. Adicione o `router-outlet` no `app.component.html`:

```html
<div class="container">
  <header>
    <nav>
      <a routerLink="/" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}">Home</a>
      <a routerLink="/sobre" routerLinkActive="active">Sobre</a>
      <a routerLink="/lista" routerLinkActive="active">Lista</a>
    </nav>
  </header>

  <main>
    <router-outlet></router-outlet>
  </main>

  <footer>
    <p>© 2025 Meu Primeiro App Angular</p>
  </footer>
</div>
```

### Formulários

Angular suporta dois tipos de abordagens para formulários: Template-driven e Reactive.

#### Formulário Template-driven:

1. Certifique-se de importar o `FormsModule`:

```typescript
// app.module.ts
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [
    BrowserModule,
    FormsModule
    // ...
  ],
})
export class AppModule { }
```

2. Crie um componente de formulário:

```typescript
// contato.component.ts
import { Component } from '@angular/core';

interface Contato {
  nome: string;
  email: string;
  mensagem: string;
}

@Component({
  selector: 'app-contato',
  templateUrl: './contato.component.html'
})
export class ContatoComponent {
  contato: Contato = {
    nome: '',
    email: '',
    mensagem: ''
  };

  enviarFormulario() {
    console.log('Formulário enviado:', this.contato);
    // Aqui você enviaria os dados para um backend
    alert('Mensagem enviada com sucesso!');
    this.contato = { nome: '', email: '', mensagem: '' };
  }
}
```

3. Template do formulário:

```html
<!-- contato.component.html -->
<div class="formulario-contato">
  <h2>Entre em Contato</h2>
  
  <form #contatoForm="ngForm" (ngSubmit)="enviarFormulario()" novalidate>
    <div class="form-group">
      <label for="nome">Nome</label>
      <input 
        type="text" 
        id="nome" 
        name="nome" 
        [(ngModel)]="contato.nome" 
        required 
        #nome="ngModel">
      <div class="erro" *ngIf="nome.invalid && (nome.dirty || nome.touched)">
        Nome é obrigatório.
      </div>
    </div>
    
    <div class="form-group">
      <label for="email">Email</label>
      <input 
        type="email" 
        id="email" 
        name="email" 
        [(ngModel)]="contato.email" 
        required 
        email
        #email="ngModel">
      <div class="erro" *ngIf="email.invalid && (email.dirty || email.touched)">
        <span *ngIf="email.errors?.['required']">Email é obrigatório.</span>
        <span *ngIf="email.errors?.['email']">Email inválido.</span>
      </div>
    </div>
    
    <div class="form-group">
      <label for="mensagem">Mensagem</label>
      <textarea 
        id="mensagem" 
        name="mensagem" 
        [(ngModel)]="contato.mensagem" 
        required
        #mensagem="ngModel"></textarea>
      <div class="erro" *ngIf="mensagem.invalid && (mensagem.dirty || mensagem.touched)">
        Mensagem é obrigatória.
      </div>
    </div>
    
    <button type="submit" [disabled]="contatoForm.invalid">Enviar</button>
  </form>
</div>
```

#### Formulário Reactive:

1. Importe o `ReactiveFormsModule`:

```typescript
// app.module.ts
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    BrowserModule,
    ReactiveFormsModule
    // ...
  ],
})
export class AppModule { }
```

2. Componente com formulário reativo:

```typescript
// login.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-login',
  templateUrl: './login.component.html'
})
export class LoginComponent implements OnInit {
  loginForm!: FormGroup;
  submitted = false;

  constructor(private formBuilder: FormBuilder) { }

  ngOnInit() {
    this.loginForm = this.formBuilder.group({
      usuario: ['', [Validators.required, Validators.minLength(4)]],
      senha: ['', [Validators.required, Validators.minLength(6)]]
    });
  }

  // Getter para acesso fácil aos campos do formulário
  get f() { return this.loginForm.controls; }

  onSubmit() {
    this.submitted = true;

    // Parar aqui se o formulário for inválido
    if (this.loginForm.invalid) {
      return;
    }

    console.log('Dados de login:', this.loginForm.value);
    // Implementaria a lógica de login aqui
    alert('Login bem-sucedido!');
  }
}
```

3. Template do formulário reativo:

```html
<!-- login.component.html -->
<div class="login-form">
  <h2>Login</h2>
  
  <form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
    <div class="form-group">
      <label for="usuario">Usuário</label>
      <input 
        type="text" 
        formControlName="usuario" 
        id="usuario" 
        [ngClass]="{ 'invalido': submitted && f['usuario'].errors }">
      <div class="erro" *ngIf="submitted && f['usuario'].errors">
        <div *ngIf="f['usuario'].errors['required']">Usuário é obrigatório</div>
        <div *ngIf="f['usuario'].errors['minlength']">Usuário deve ter pelo menos 4 caracteres</div>
      </div>
    </div>
    
    <div class="form-group">
      <label for="senha">Senha</label>
      <input 
        type="password" 
        formControlName="senha" 
        id="senha" 
        [ngClass]="{ 'invalido': submitted && f['senha'].errors }">
      <div class="erro" *ngIf="submitted && f['senha'].errors">
        <div *ngIf="f['senha'].errors['required']">Senha é obrigatória</div>
        <div *ngIf="f['senha'].errors['minlength']">Senha deve ter pelo menos 6 caracteres</div>
      </div>
    </div>
    
    <div class="form-group">
      <button type="submit">Login</button>
    </div>
  </form>
</div>
```

### HTTP e Comunicação com API

Angular fornece o `HttpClient` para fazer requisições HTTP:

1. Importe o `HttpClientModule`:

```typescript
// app.module.ts
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [
    BrowserModule,
    HttpClientModule
    // ...
  ],
})
export class AppModule { }
```

2. Crie um serviço para API:

```typescript
// api.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, retry } from 'rxjs/operators';

export interface Produto {
  id?: number;
  nome: string;
  preco: number;
  descricao: string;
}

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private apiUrl = 'http://localhost:3000/produtos'; // URL da nossa API

  httpOptions = {
    headers: new HttpHeaders({ 'Content-Type': 'application/json' })
  };

  constructor(private http: HttpClient) { }

  // GET todos os produtos
  getProdutos(): Observable<Produto[]> {
    return this.http.get<Produto[]>(this.apiUrl)
      .pipe(
        retry(1),
        catchError(this.handleError)
      );
  }

  // GET produto por id
  getProduto(id: number): Observable<Produto> {
    return this.http.get<Produto>(`${this.apiUrl}/${id}`)
      .pipe(
        catchError(this.handleError)
      );
  }

  // POST novo produto
  criarProduto(produto: Produto): Observable<Produto> {
    return this.http.post<Produto>(this.apiUrl, produto, this.httpOptions)
      .pipe(
        catchError(this.handleError)
      );
  }

  // PUT atualizar produto
  atualizarProduto(id: number, produto: Produto): Observable<Produto> {
    return this.http.put<Produto>(`${this.apiUrl}/${id}`, produto, this.httpOptions)
      .pipe(
        catchError(this.handleError)
      );
  }

  // DELETE produto
  deletarProduto(id: number): Observable<any> {
    return this.http.delete(`${this.apiUrl}/${id}`, this.httpOptions)
      .pipe(
        catchError(this.handleError)
      );
  }

  // Tratamento de erro
  private handleError(error: any) {
    let errorMessage = '';
    if (error.error instanceof ErrorEvent) {
      // Erro do lado do cliente
      errorMessage = `Erro: ${error.error.message}`;
    } else {
      // Erro do lado do servidor
      errorMessage = `Código: ${error.status}\nMensagem: ${error.message}`;
    }
    console.error(errorMessage);
    return throwError(() => new Error(errorMessage));
  }
}
```

3. Use o serviço em um componente:

```typescript
// produtos.component.ts
import { Component, OnInit } from '@angular/core';
import { ApiService, Produto } from '../api.service';

@Component({
  selector: 'app-produtos',
  templateUrl: './produtos.component.html'
})
export class ProdutosComponent implements OnInit {
  produtos: Produto[] = [];
  novoProduto: Produto = { nome: '', preco: 0, descricao: '' };
  carregando = false;
  erro = '';

  constructor(private apiService: ApiService) { }

  ngOnInit(): void {
    this.carregarProdutos();
  }

  carregarProdutos(): void {
    this.carregando = true;
    this.apiService.getProdutos().subscribe({
      next: (data) => {
        this.produtos = data;
        this.carregando = false;
      },
      error: (e) => {
        this.erro = 'Erro ao carregar produtos: ' + e.message;
        this.carregando = false;
      }
    });
  }

  adicionarProduto(): void {
    if (this.novoProduto.nome.trim() && this.novoProduto.preco > 0) {
      this.apiService.criarProduto(this.novoProduto).subscribe({
        next: (produto) => {
          this.produtos.push(produto);
          this.novoProduto = { nome: '', preco: 0, descricao: '' };
          alert('Produto adicionado com sucesso!');
        },
        error: (e) => {
          this.erro = 'Erro ao adicionar produto: ' + e.message;
        }
      });
    }
  }

  deletarProduto(id: number): void {
    if (confirm('Tem certeza que deseja excluir este produto?')) {
      this.apiService.deletarProduto(id).subscribe({
        next: () => {
          this.produtos = this.produtos.filter(p => p.id !== id);
          alert('Produto excluído com sucesso!');
        },
        error: (e) => {
          this.erro = 'Erro ao excluir produto: ' + e.message;
        }
      });
    }
  }
}
```

4. Template para exibir e gerenciar produtos:

```html
<!-- produtos.component.html -->
<div class="produtos-container">
  <h2>Gerenciamento de Produtos</h2>
  
  <div *ngIf="erro" class="erro-mensagem">
    {{ erro }}
  </div>
  
  <div class="form-produto">
    <h3>Adicionar Produto</h3>
    <form (ngSubmit)="adicionarProduto()">
      <div>
        <label for="nome">Nome:</label>
        <input type="text" id="nome" [(ngModel)]="novoProduto.nome" name="nome" required>
      </div>
      
      <div>
        <label for="preco">Preço:</label>
        <input type="number" id="preco" [(ngModel)]="novoProduto.preco" name="preco" min="0" step="0.01" required>
      </div>
      
      <div>
        <label for="descricao">Descrição:</label>
        <textarea id="descricao" [(ngModel)]="novoProduto.descricao" name="descricao"></textarea>
      </div>
      
      <button type="submit">Adicionar Produto</button>
    </form>
  </div>
  
  <div class="lista-produtos">
    <h3>Lista de Produtos</h3>
    
    <div *ngIf="carregando" class="carregando">
      Carregando produtos...
    </div>
    
    <table *ngIf="!carregando && produtos.length > 0">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Preço</th>
          <th>Descrição</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let produto of produtos">
          <td>{{ produto.id }}</td>
          <td>{{ produto.nome }}</td>
          <td>{{ produto.preco | currency:'BRL' }}</td>
          <td>{{ produto.descricao }}</td>
          <td>
            <button (click)="deletarProduto(produto.id!)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
    
    <div *ngIf="!carregando && produtos.length === 0" class="sem-dados">
      Nenhum produto cadastrado.
    </div>
  </div>
</div>
```

## Criando uma API RESTful

Vamos criar uma API RESTful usando Node.js, Express e TypeScript para ser consumida pelo nosso frontend Angular.

### Configurando o Projeto da API

1. Crie uma pasta para o projeto da API:

```bash
mkdir api-produtos
cd api-produtos
npm init -y
```

2. Instale as dependências necessárias:

```bash
npm install express cors body-parser morgan
npm install --save-dev typescript ts-node @types/express @types/cors @types/body-parser @types/morgan @types/node nodemon
```

3. Crie um arquivo `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"]
}
```

4. Crie um script no `package.json`:

```json
{
  "scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon src/index.ts",
    "build": "tsc"
  }
}
```

### Implementando a Estrutura da API

1. Crie a pasta `src` e o arquivo principal:

```
mkdir -p src
touch src/index.ts
```

2. Defina os modelos e interfaces:

```typescript
// src/models/produto.model.ts
export interface Produto {
  id: number;
  nome: string;
  preco: number;
  descricao: string;
}
```

3. Crie um serviço para gerenciar produtos:

```typescript
// src/services/produto.service.ts
import { Produto } from '../models/produto.model';

// Simula um banco de dados com uma lista em memória
export class ProdutoService {
  private produtos: Produto[] = [
    { id: 1, nome: 'Laptop', preco: 5000, descricao: 'Laptop com 16GB RAM' },
    { id: 2, nome: 'Smartphone', preco: 2500, descricao: 'Smartphone última geração' },
    { id: 3, nome: 'Fones de Ouvido', preco: 500, descricao: 'Fones sem fio com cancelamento de ruído' }
  ];

  private proximoId = 4;

  listarTodos(): Produto[] {
    return this.produtos;
  }

  obterPorId(id: number): Produto | undefined {
    return this.produtos.find(p => p.id === id);
  }

  criar(produto: Omit<Produto, 'id'>): Produto {
    const novoProduto: Produto = {
      ...produto,
      id: this.proximoId++
    };
    this.produtos.push(novoProduto);
    return novoProduto;
  }

  atualizar(id: number, produto: Omit<Produto, 'id'>): Produto | undefined {
    const index = this.produtos.findIndex(p => p.id === id);
    if (index === -1) {
      return undefined;
    }

    const produtoAtualizado: Produto = {
      ...produto,
      id
    };
    this.produtos[index] = produtoAtualizado;
    return produtoAtualizado;
  }

  excluir(id: number): boolean {
    const tamanhoAnterior = this.produtos.length;
    this.produtos = this.produtos.filter(p => p.id !== id);
    return tamanhoAnterior > this.produtos.length;
  }
}
```

4. Crie os controladores:

```typescript
// src/controllers/produto.controller.ts
import { Request, Response } from 'express';
import { ProdutoService } from '../services/produto.service';
import { Produto } from '../models/produto.model';

export class ProdutoController {
  private produtoService = new ProdutoService();

  listarTodos = (req: Request, res: Response): void => {
    try {
      const produtos = this.produtoService.listarTodos();
      res.json(produtos);
    } catch (error) {
      res.status(500).json({ message: 'Erro ao listar produtos', error });
    }
  };

  obterPorId = (req: Request, res: Response): void => {
    try {
      const id = parseInt(req.params.id);
      const produto = this.produtoService.obterPorId(id);
      
      if (!produto) {
        res.status(404).json({ message: 'Produto não encontrado' });
        return;
      }
      
      res.json(produto);
    } catch (error) {
      res.status(500).json({ message: 'Erro ao buscar produto', error });
    }
  };

  criar = (req: Request, res: Response): void => {
    try {
      const { nome, preco, descricao } = req.body;
      
      // Validações básicas
      if (!nome || !preco) {
        res.status(400).json({ message: 'Nome e preço são obrigatórios' });
        return;
      }
      
      const produto = this.produtoService.criar({ nome, preco, descricao });
      res.status(201).json(produto);
    } catch (error) {
      res.status(500).json({ message: 'Erro ao criar produto', error });
    }
  };

  atualizar = (req: Request, res: Response): void => {
    try {
      const id = parseInt(req.params.id);
      const { nome, preco, descricao } = req.body;
      
      // Validações básicas
      if (!nome || !preco) {
        res.status(400).json({ message: 'Nome e preço são obrigatórios' });
        return;
      }
      
      const produtoAtualizado = this.produtoService.atualizar(id, { nome, preco, descricao });
      
      if (!produtoAtualizado) {
        res.status(404).json({ message: 'Produto não encontrado' });
        return;
      }
      
      res.json(produtoAtualizado);
    } catch (error) {
      res.status(500).json({ message: 'Erro ao atualizar produto', error });
    }
  };

  excluir = (req: Request, res: Response): void => {
    try {
      const id = parseInt(req.params.id);
      const excluido = this.produtoService.excluir(id);
      
      if (!excluido) {
        res.status(404).json({ message: 'Produto não encontrado' });
        return;
      }
      
      res.status(204).send();
    } catch (error) {
      res.status(500).json({ message: 'Erro ao excluir produto', error });
    }
  };
}
```

5. Configure as rotas:

```typescript
// src/routes/produto.routes.ts
import { Router } from 'express';
import { ProdutoController } from '../controllers/produto.controller';

export const produtoRoutes = Router();
const produtoController = new ProdutoController();

produtoRoutes.get('/', produtoController.listarTodos);
produtoRoutes.get('/:id', produtoController.obterPorId);
produtoRoutes.post('/', produtoController.criar);
produtoRoutes.put('/:id', produtoController.atualizar);
produtoRoutes.delete('/:id', produtoController.excluir);
```

6. Implemente o arquivo principal da API:

```typescript
// src/index.ts
import express from 'express';
import bodyParser from 'body-parser';
import cors from 'cors';
import morgan from 'morgan';
import { produtoRoutes } from './routes/produto.routes';

const app = express();
const PORT = process.env.PORT || 3000;

// Middlewares
app.use(cors());
app.use(bodyParser.json());
app.use(morgan('dev'));

// Rotas
app.use('/produtos', produtoRoutes);

// Rota padrão
app.get('/', (req, res) => {
  res.send('API de Produtos - Online');
});

// Tratamento para rotas não encontradas
app.use((req, res) => {
  res.status(404).json({ message: 'Rota não encontrada' });
});

// Inicialização do servidor
app.listen(PORT, () => {
  console.log(`Servidor rodando na porta ${PORT}`);
  console.log(`http://localhost:${PORT}`);
});
```

### Executando a API

Agora você pode iniciar a API com:

```bash
npm run dev
```

A API estará rodando em `http://localhost:3000`. Você pode testar as rotas usando uma ferramenta como Postman ou Insomnia:

- GET http://localhost:3000/produtos - Listar todos os produtos
- GET http://localhost:3000/produtos/1 - Obter produto com ID 1
- POST http://localhost:3000/produtos - Criar um novo produto
- PUT http://localhost:3000/produtos/1 - Atualizar o produto com ID 1
- DELETE http://localhost:3000/produtos/1 - Excluir o produto com ID 1

## Desenvolvendo um Site Angular

Vamos criar um site Angular completo que consuma nossa API de produtos.

### Configurando a Estrutura do Projeto

Primeiro, vamos organizar melhor o projeto Angular com componentes, serviços e módulos:

```bash
# Gerar componentes
ng g c components/home
ng g c components/produtos/lista-produtos
ng g c components/produtos/adicionar-produto
ng g c components/produtos/editar-produto
ng g c components/sobre
ng g c components/layout/cabecalho
ng g c components/layout/rodape
ng g c components/shared/loading

# Gerar serviços
ng g s services/produto
ng g s services/mensagem

# Gerar interfaces
ng g interface models/produto
```

### Configurando as Rotas

Atualize o arquivo `app-routing.module.ts`:

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './components/home/home.component';
import { ListaProdutosComponent } from './components/produtos/lista-produtos/lista-produtos.component';
import { AdicionarProdutoComponent } from './components/produtos/adicionar-produto/adicionar-produto.component';
import { EditarProdutoComponent } from './components/produtos/editar-produto/editar-produto.component';
import { SobreComponent } from './components/sobre/sobre.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'produtos', component: ListaProdutosComponent },
  { path: 'produtos/adicionar', component: AdicionarProdutoComponent },
  { path: 'produtos/editar/:id', component: EditarProdutoComponent },
  { path: 'sobre', component: SobreComponent },
  { path: '**', redirectTo: '' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

### Implementando a Interface do Produto

```typescript
// src/app/models/produto.ts
export interface Produto {
  id?: number;
  nome: string;
  preco: number;
  descricao: string;
}
```

### Criando o Serviço de Produto

```typescript
// src/app/services/produto.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Produto } from '../models/produto';
import { environment } from '../../environments/environment';

@Injectable({
  providedIn: 'root'
})
export class ProdutoService {
  private apiUrl = `${environment.apiUrl}/produtos`;

  constructor(private http: HttpClient) { }

  listarTodos(): Observable<Produto[]> {
    return this.http.get<Produto[]>(this.apiUrl);
  }

  obterPorId(id: number): Observable<Produto> {
    return this.http.get<Produto>(`${this.apiUrl}/${id}`);
  }

  adicionar(produto: Produto): Observable<Produto> {
    return this.http.post<Produto>(this.apiUrl, produto);
  }

  atualizar(id: number, produto: Produto): Observable<Produto> {
    return this.http.put<Produto>(`${this.apiUrl}/${id}`, produto);
  }

  excluir(id: number): Observable<any> {
    return this.http.delete(`${this.apiUrl}/${id}`);
  }
}
```

### Criando o Serviço de Mensagens

```typescript
// src/app/services/mensagem.service.ts
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

export interface Mensagem {
  texto: string;
  tipo: 'sucesso' | 'erro' | 'info';
}

@Injectable({
  providedIn: 'root'
})
export class MensagemService {
  private mensagemSubject = new Subject<Mensagem | null>();
  mensagens$ = this.mensagemSubject.asObservable();

  constructor() { }

  exibir(texto: string, tipo: 'sucesso' | 'erro' | 'info' = 'info'): void {
    this.mensagemSubject.next({ texto, tipo });
    
    // Limpa a mensagem após 5 segundos
    setTimeout(() => {
      this.limpar();
    }, 5000);
  }

  sucesso(texto: string): void {
    this.exibir(texto, 'sucesso');
  }

  erro(texto: string): void {
    this.exibir(texto, 'erro');
  }

  limpar(): void {
    this.mensagemSubject.next(null);
  }
}
```

### Configurando o Ambiente

Crie ou atualize o arquivo `src/environments/environment.ts`:

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000'
};
```

### Implementando os Componentes

#### Home Component

```typescript
// src/app/components/home/home.component.ts
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent {
  constructor(private router: Router) {}

  navegarParaProdutos(): void {
    this.router.navigate(['/produtos']);
  }
}
```

```html
<!-- src/app/components/home/home.component.html -->
<div class="home-container">
  <div class="hero">
    <h1>Bem-vindo à nossa Loja de Produtos</h1>
    <p>Gerencie seus produtos com facilidade usando nossa aplicação Angular + TypeScript</p>
    <button (click)="navegarParaProdutos()" class="btn-primary">Ver Produtos</button>
  </div>
  
  <div class="features">
    <div class="feature-card">
      <h3>Cadastro Simples</h3>
      <p>Adicione novos produtos em apenas alguns cliques</p>
    </div>
    
    <div class="feature-card">
      <h3>Gestão Completa</h3>
      <p>Edite, exclua e visualize todos os seus produtos</p>
    </div>
    
    <div class="feature-card">
      <h3>Interface Amigável</h3>
      <p>Design responsivo para facilitar o uso em qualquer dispositivo</p>
    </div>
  </div>
</div>
```

#### Lista de Produtos Component

```typescript
// src/app/components/produtos/lista-produtos/lista-produtos.component.ts
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { Produto } from '../../../models/produto';
import { ProdutoService } from '../../../services/produto.service';
import { MensagemService } from '../../../services/mensagem.service';

@Component({
  selector: 'app-lista-produtos',
  templateUrl: './lista-produtos.component.html',
  styleUrls: ['./lista-produtos.component.css']
})
export class ListaProdutosComponent implements OnInit {
  produtos: Produto[] = [];
  carregando = true;

  constructor(
    private produtoService: ProdutoService,
    private mensagemService: MensagemService,
    private router: Router
  ) {}

  ngOnInit(): void {
    this.carregarProdutos();
  }

  carregarProdutos(): void {
    this.carregando = true;
    this.produtoService.listarTodos().subscribe({
      next: (produtos) => {
        this.produtos = produtos;
        this.carregando = false;
      },
      error: (erro) => {
        this.mensagemService.erro('Erro ao carregar produtos: ' + erro.message);
        this.carregando = false;
      }
    });
  }

  excluirProduto(id: number): void {
    if (confirm('Tem certeza que deseja excluir este produto?')) {
      this.produtoService.excluir(id).subscribe({
        next: () => {
          this.produtos = this.produtos.filter(p => p.id !== id);
          this.mensagemService.sucesso('Produto excluído com sucesso!');
        },
        error: (erro) => {
          this.mensagemService.erro('Erro ao excluir produto: ' + erro.message);
        }
      });
    }
  }

  adicionarProduto(): void {
    this.router.navigate(['/produtos/adicionar']);
  }

  editarProduto(id: number): void {
    this.router.navigate(['/produtos/editar', id]);
  }
}
```

```html
<!-- src/app/components/produtos/lista-produtos/lista-produtos.component.html -->
<div class="produtos-container">
  <div class="header">
    <h2>Lista de Produtos</h2>
    <button (click)="adicionarProduto()" class="btn-add">Adicionar Produto</button>
  </div>

  <div *ngIf="carregando" class="loading-container">
    <app-loading></app-loading>
  </div>

  <div *ngIf="!carregando && produtos.length === 0" class="no-data">
    <p>Nenhum produto cadastrado.</p>
  </div>

  <div *ngIf="!carregando && produtos.length > 0" class="lista">
    <div class="produto-card" *ngFor="let produto of produtos">
      <div class="produto-info">
        <h3>{{ produto.nome }}</h3>
        <p class="preco">{{ produto.preco | currency:'BRL' }}</p>
        <p class="descricao">{{ produto.descricao }}</p>
      </div>
      
      <div class="produto-acoes">
        <button (click)="editarProduto(produto.id!)" class="btn-edit">Editar</button>
        <button (click)="excluirProduto(produto.id!)" class="btn-delete">Excluir</button>
      </div>
    </div>
  </div>
</div>
```

#### Adicionar Produto Component

```typescript
// src/app/components/produtos/adicionar-produto/adicionar-produto.component.ts
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { Router } from '@angular/router';
import { Produto } from '../../../models/produto';
import { ProdutoService } from '../../../services/produto.service';
import { MensagemService } from '../../../services/mensagem.service';

@Component({
  selector: 'app-adicionar-produto',
  templateUrl: './adicionar-produto.component.html',
  styleUrls: ['./adicionar-produto.component.css']
})
export class AdicionarProdutoComponent {
  produtoForm: FormGroup;
  enviando = false;

  constructor(
    private fb: FormBuilder,
    private produtoService: ProdutoService,
    private mensagemService: MensagemService,
    private router: Router
  ) {
    this.produtoForm = this.fb.group({
      nome: ['', [Validators.required, Validators.minLength(3)]],
      preco: ['', [Validators.required, Validators.min(0)]],
      descricao: ['']
    });
  }

  get f() { return this.produtoForm.controls; }

  onSubmit(): void {
    if (this.produtoForm.invalid) {
      // Marca todos os campos como tocados para mostrar os erros
      Object.keys(this.produtoForm.controls).forEach(key => {
        const control = this.produtoForm.get(key);
        control?.markAsTouched();
      });
      return;
    }

    this.enviando = true;
    const produto: Produto = this.produtoForm.value;

    this.produtoService.adicionar(produto).subscribe({
      next: () => {
        this.mensagemService.sucesso('Produto adicionado com sucesso!');
        this.router.navigate(['/produtos']);
      },
      error: (erro) => {
        this.mensagemService.erro('Erro ao adicionar produto: ' + erro.message);
        this.enviando = false;
      }
    });
  }

  cancelar(): void {
    this.router.navigate(['/produtos']);
  }
}
```

```html
<!-- src/app/components/produtos/adicionar-produto/adicionar-produto.component.html -->
<div class="form-container">
  <h2>Adicionar Produto</h2>

  <form [formGroup]="produtoForm" (ngSubmit)="onSubmit()">
    <div class="form-group">
      <label for="nome">Nome</label>
      <input type="text" id="nome" formControlName="nome" [class.invalid]="f['nome'].touched && f['nome'].invalid">
      <div class="error-message" *ngIf="f['nome'].touched && f['nome'].errors">
        <span *ngIf="f['nome'].errors['required']">Nome é obrigatório</span>
        <span *ngIf="f['nome'].errors['minlength']">Nome deve ter pelo menos 3 caracteres</span>
      </div>
    </div>

    <div class="form-group">
      <label for="preco">Preço</label>
      <input type="number" id="preco" formControlName="preco" step="0.01" min="0" [class.invalid]="f['preco'].touched && f['preco'].invalid">
      <div class="error-message" *ngIf="f['preco'].touched && f['preco'].errors">
        <span *ngIf="f['preco'].errors['required']">Preço é obrigatório</span>
        <span *ngIf="f['preco'].errors['min']">Preço deve ser maior que zero</span>
      </div>
    </div>

    <div class="form-group">
      <label for="descricao">Descrição</label>
      <textarea id="descricao" formControlName="descricao" rows="3"></textarea>
    </div>

    <div class="form-actions">
      <button type="button" (click)="cancelar()" class="btn-cancel">Cancelar</button>
      <button type="submit" class="btn-submit" [disabled]="enviando">
        {{ enviando ? 'Salvando...' : 'Salvar' }}
      </button>
    </div>
  </form>
</div>
```

#### Editar Produto Component

```typescript
// src/app/components/produtos/editar-produto/editar-produto.component.ts
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { ActivatedRoute, Router } from '@angular/router';
import { Produto } from '../../../models/produto';
import { ProdutoService } from '../../../services/produto.service';
import { MensagemService } from '../../../services/mensagem.service';

@Component({
  selector: 'app-editar-produto',
  templateUrl: './editar-produto.component.html',
  styleUrls: ['./editar-produto.component.css']
})
export class EditarProdutoComponent implements OnInit {
  produtoForm: FormGroup;
  enviando = false;
  carregando = true;
  produtoId!: number;

  constructor(
    private fb: FormBuilder,
    private route: ActivatedRoute,
    private router: Router,
    private produtoService: ProdutoService,
    private mensagemService: MensagemService
  ) {
    this.produtoForm = this.fb.group({
      nome: ['', [Validators.required, Validators.minLength(3)]],
      preco: ['', [Validators.required, Validators.min(0)]],
      descricao: ['']
    });
  }

  ngOnInit(): void {
    this.route.params.subscribe(params => {
      this.produtoId = +params['id'];
      this.carregarProduto(this.produtoId);
    });
  }

  carregarProduto(id: number): void {
    this.carregando = true;
    this.produtoService.obterPorId(id).subscribe({
      next: (produto) => {
        this.produtoForm.patchValue({
          nome: produto.nome,
          preco: produto.preco,
          descricao: produto.descricao
        });
        this.carregando = false;
      },
      error: (erro) => {
        this.mensagemService.erro('Erro ao carregar produto: ' + erro.message);
        this.router.navigate(['/produtos']);
      }
    });
  }

  get f() { return this.produtoForm.controls; }

  onSubmit(): void {
    if (this.produtoForm.invalid) {
      // Marca todos os campos como tocados para mostrar os erros
      Object.keys(this.produtoForm.controls).forEach(key => {
        const control = this.produtoForm.get(key);
        control?.markAsTouched();
      });
      return;
    }

    this.enviando = true;
    const produto: Produto = this.produtoForm.value;

    this.produtoService.atualizar(this.produtoId, produto).subscribe({
      next: () => {
        this.mensagemService.sucesso('Produto atualizado com sucesso!');
        this.router.navigate(['/produtos']);
      },
      error: (erro) => {
        this.mensagemService.erro('Erro ao atualizar produto: ' + erro.message);
        this.enviando = false;
      }
    });
  }

  cancelar(): void {
    this.router.navigate(['/produtos']);
  }
}
```

```html
<!-- src/app/components/produtos/editar-produto/editar-produto.component.html -->
<div class="form-container">
  <h2>Editar Produto</h2>

  <div *ngIf="carregando" class="loading-container">
    <app-loading></app-loading>
  </div>

  <form *ngIf="!carregando" [formGroup]="produtoForm" (ngSubmit)="onSubmit()">
    <div class="form-group">
      <label for="nome">Nome</label>
      <input type="text" id="nome" formControlName="nome" [class.invalid]="f['nome'].touched && f['nome'].invalid">
      <div class="error-message" *ngIf="f['nome'].touched && f['nome'].errors">
        <span *ngIf="f['nome'].errors['required']">Nome é obrigatório</span>
        <span *ngIf="f['nome'].errors['minlength']">Nome deve ter pelo menos 3 caracteres</span>
      </div>
    </div>

    <div class="form-group">
      <label for="preco">Preço</label>
      <input type="number" id="preco" formControlName="preco" step="0.01" min="0" [class.invalid]="f['preco'].touched && f['preco'].invalid">
      <div class="error-message" *ngIf="f['preco'].touched && f['preco'].errors">
        <span *ngIf="f['preco'].errors['required']">Preço é obrigatório</span>
        <span *ngIf="f['preco'].errors['min']">Preço deve ser maior que zero</span>
      </div>
    </div>

    <div class="form-group">
      <label for="descricao">Descrição</label>
      <textarea id="descricao" formControlName="descricao" rows="3"></textarea>
    </div>

    <div class="form-actions">
      <button type="button" (click)="cancelar()" class="btn-cancel">Cancelar</button>
      <button type="submit" class="btn-submit" [disabled]="enviando">
        {{ enviando ? 'Salvando...' : 'Salvar' }}
      </button>
    </div>
  </form>
</div>
```

#### Componentes de Layout

```html
<!-- src/app/components/layout/cabecalho/cabecalho.component.html -->
<header class="header">
  <div class="logo">
    <a routerLink="/">
      <h1>CatalogApp</h1>
    </a>
  </div>
  <nav class="main-nav">
    <ul>
      <li><a routerLink="/" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}">Home</a></li>
      <li><a routerLink="/produtos" routerLinkActive="active">Produtos
