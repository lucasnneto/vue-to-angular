# Vue to Angular

## Props e Events | Inputs e Outputs

### Vue
Props | Vue.Js

Events | Vue.Js

#### Props
```vue
<script setup>
const props = defineProps(['foo'])

// definition of types
defineProps({
  title: String,
  likes?: Number
})
</script>

<!-- send props in component -->
<MyComponent title="hello" />
```

#### Events
```vue
<script setup>
const emit = defineEmits(['inFocus', 'submit'])

// definition of types
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()

function buttonClick() {
  emit('submit')
}
</script>

<!-- send props in component -->
<MyComponent @in-focus="increaseCount" />
```

### Angular
Input and Output | Angular

#### Input
> Props de componentes no angular são os inputs

```typescript
import { Component, Input } from '@angular/core';

export class ItemDetailComponent {
  @Input() title = '';
}

<!-- send props in component -->
<app-my-component title="hello" />
```

#### Outputs
> Events de componentes no angular são os outputs

```typescript
import { Output, EventEmitter } from '@angular/core';

export class ItemDetailComponent {
  @Output() newItemEvent = new EventEmitter<string>();

  addNewItem(value: string) {
    this.newItemEvent.emit(value);
  }
}

<!-- send props in component -->
<app-item-output (newItemEvent)="addItem($event)"></app-item-output>
```

## Slots | Content projection

### Vue
Slots | Vue.Js

#### Slot Simples
```vue
<button class="fancy-btn">
  <slot></slot>
</button>

<FancyButton>
  Click me! <!-- slot content -->
</FancyButton>
```

#### Slot Nomeado
```vue
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
</div>

<BaseLayout>
  <!-- content for the header default -->
  <template v-slot:header>
    <!-- content for the header slot -->
  </template>
  <!-- content for the header default -->
</BaseLayout>
```

### Angular
Content projection | Angular

#### Slot Simples
> Para angular o `<slot>` é substituído pelo `<ng-content>`

```html
<button class="fancy-btn">
  <ng-content></ng-content>
</button>

<app-fancy-button>
  Click me! <!-- slot content -->
</app-fancy-button>
```

#### Slot Nomeado
> Para os nomeados utilize o atributo `select` no `<ng-content>`

> Na utilização coloque na `tag` selecionada o valor selecionado para o slot

```html
<div class="container">
  <header>
    <ng-content select="[header]"></ng-content>
  </header>
  <main>
    <ng-content></ng-content>
  </main>
</div>

<app-base-layout>
  <!-- content for the header default -->
  <div header>
    <!-- content for the header slot -->
  </div>
  <!-- content for the header default -->
</app-base-layout>
```

## Watch | Subjective and Observable

### Vue
Watchers | Vue.Js

```javascript
const valores = ref(0)
watch(valores, async (newValor, oldValor) => {
  console.log(newValor, oldValor)
})

valores.value = 1;
valores.value = 2;

// Log: 0 1
// Log: 1 2
```

### Angular
Subjective | Angular|RxJs

> Angular faz uso do RxJs como uma lib nativa para trabalhar com reatividade

```typescript
import { Subject } from 'rxjs';

const subject = new Subject<number>();

subject.subscribe({
  next: (v) => console.log(v), 
  error: (err) => console.error(err),
  complete: () => console.log('complete'),
});

// Alternativa simples
subject.subscribe((v) => console.log(v));

subject.next(1);
subject.next(2);

// Log: 1
// Log: 2
```

> Outra alternativa ao subject é o Observable sendo ele `unicast` diferente do Subject que é `multicast`

```typescript
import { Observable } from 'rxjs';

const observable = new Observable((subscriber) => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.complete();
});

observable.subscribe({
  next(x) {
    console.log('got value ' + x);
  },
  error(err) {
    console.error('something wrong occurred: ' + err);
  },
  complete() {
    console.log('done');
  },
});
```

## Condicionais | ngIf e v-if

### Vue
- **v-if**: Diretiva para renderização condicional de elementos.

```vue
<template>
  <div v-if="isVisible">Este elemento é visível</div>
</template>

<script setup>
const isVisible = ref(true);
</script>
```

### Angular
- **ngIf**: Diretiva estrutural para renderização condicional de elementos.

```html
<div *ngIf="isVisible">Este elemento é visível</div>

<script>
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  isVisible = true;
}
</script>
```

## Loops | ngFor e v-for

### Vue
- **v-for**: Diretiva para renderização de listas.

```vue
<template>
  <ul>
    <li v-for="item in items" :key="item.id">{{ item.name }}</li>
  </ul>
</template>

<script setup>
const items = ref([
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' },
  { id: 3, name: 'Item 3' }
]);
</script>
```

### Angular
- **ngFor**: Diretiva estrutural para renderização de listas.

```html
<ul>
  <li *ngFor="let item of items">{{ item.name }}</li>
</ul>

<script>
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  items = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ];
}
</script>
```

## Ciclo de Vida dos Componentes | Lifecycle Hooks

### Vue
- **Lifecycle Hooks**: Métodos de ciclo de vida dos componentes.

```vue
<script>
export default {
  created() {
    console.log('Componente criado');
  },
  mounted() {
    console.log('Componente montado');
  },
  updated() {
    console.log('Componente atualizado');
  },
  destroyed() {
    console.log('Componente destruído');
  }
}
</script>
```

### Angular
- **Lifecycle Hooks**: Métodos de ciclo de vida dos componentes.

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit, OnDestroy {
  ngOnInit() {
    console.log('Componente inicializado');
  }

  ngOnDestroy() {
    console.log('Componente destruído');
  }
}
```

## Rotas | Routing

### Vue
- **Rotas**: Configurando rotas com Vue Router.

```javascript
// router.js
import { createRouter, createWebHistory } from 'vue-router';
import Home from './components/Home.vue';
import About from './components/About.vue';

const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

export default router;

// main.js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';

createApp(App).use(router).mount('#app');
```

### Angular
- **Rotas**: Configurando rotas com Angular Router.

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}

// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    AboutComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```
