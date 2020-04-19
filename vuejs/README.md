# VueJS 2

## Overview

- [Introduction](#introduction)
  - [Installation Vue CLI](#installation-vue-cli)
  - [What is Vue.js?](#what-is-vue.js?)
  - [Declarative Rendering](#declarative-rendering)
  - [Conditionals and Loops](#conditionals-and-loops)
  - [Handling User Input](#handling-user-input)
  - [Components](#components)
- [The Vue Instance](#the-vue-instance)
  - [Creating a Vue Instance](#creating-a-vue-instance)
  - [Data and Methods](#data-and-methods)
  - [Instance Lifecycle Hooks](#instance-lifecycle-hooks)
  - [Lifecycle Diagram](#lifecycle-diagram)
- [Template Syntax](#Template-syntax)
- [Computed Properties and Watchers](#computed-properties-and-watchers)
  - [Computed Caching vs Methods](#computed-caching-vs-methods)
  - [Computed vs Watched Property](#computed-vs-watched-property)
  - [Watchers](#watchers)


## Introduction

### Installation Vue CLI
`npm install -g @vue/cli @vue/cli-service-global`
`vue create hello-world`

### What is Vue.js?

Vue is a progressive framework for building user interfaces.

The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects.

### Declarative Rendering

```html
<div id="app" v-bind:title="title">
  {{ message }}
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!',
    title: 'You loaded this page on ' + new Date().toLocaleString()
  }
})
</script>
```

### Conditionals and Loops

```html
<span v-if="seen">Now you see me</span>

<li v-for="todo in todos">
  {{ todo.text }}
</li>
```

### Handling User Input

```html
<button v-on:click="reverseMessage">
    Reverse Message
</button>

<script>
...
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
...
</script>
```

```html
<p>{{ message }}</p>
<input v-model="message">

<script>
...
  data: {
    message: 'Hello Vue!'
  }
...
</script>
```

### Components

```html
<script>
export default {
  props: {
    buttonLabel: String
  }
}
</script>
```

```html
<template>
  <div>
    <PrimaryButton
      :buttonLabel="buttonText"
    />
  </div>
</template>

<script>
import PrimaryButton from '@/components/Button/PrimaryButton.vue'

export default {
  components: {
    PrimaryButton
  }
}
</script>

<style lang="scss">

</style>
```

## The Vue Instance

### Creating a Vue Instance

Every Vue application starts by creating a new Vue instance:

```js
var vm = new Vue({
  // options
})
```

Vue's design was partly inspired by the MVVM pattern. The variable `vm` (short for ViewModel) to refer to the Vue instance.

You can pass in an **options object**.

A Vue application consists of a **root Vue instance** created with `new Vue`, organized into a tree of nested, reusable components.

Vue components are also Vue instances, and so accept the same options object (in most of the cases).

### Data and Methods

When a Vue instance is created, it adds all the properties found in its `data` object to Vue's **reactivity system**. When the values of those properties change, the view will "react", updating to match the new values.

If you know you will need a property later, but it starts out empty, you will need to set some initial value:
```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

Except when using `Object.freeze()`.

In addition to data properties, Vue instances expose a number of instance properties and methods. These are prefixed with `$` to differentiate them from user-defined properties:

```js
vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
  // This callback will be called when `vm.a` changes
})
```

### Instance Lifecycle Hooks

Each Vue instance goes through a series or initialization steps when it's created - for example it needs to:
- set up data observation
- compile the template
- mount the instance to the DOM
- update the DOM when data changes

Along the way, it also runs functions called **lifecycle hooks**:
- `created`
- `mounted`
- `updated`
- `destroyed`

Arraow function should not be use!!

### Lifecycle Diagram

![bwop](https://vuejs.org/images/lifecycle.png)

## Template Syntax

Mustache: `<span>Message: {{ msg }}</span>`

Mustache once: `<span v-once>This will never change: {{ msg }}</span>`

Raw HTML: `<p><span v-html="rawHtml"></span></p>`

Attributes: `<div v-bind:id="dynamicId"></div>`

Boolean attributes: `<button v-bind:disabled="isButtonDisabled">Button</button>`

JavaScript Expressions: 
- `{{ number + 1 }}`
- `{{ ok ? 'YES' : 'NO' }}`
- `{{ message.split('').reverse().join('') }}`
- `<div v-bind:id="'list-' + id"></div>`

Directives (attributes with the `v-` prefix):
- `<p v-if="seen">Now you see me</p>`
- `<a v-bind:href="url"> ... </a>`
- `<a v-on:click="doSomething"> ... </a>`
- `<a v-bind:[attributeName]="url"> ... </a>`
- `<a v-on:[eventName]="doSomething"> ... </a>`
- `<form v-on:submit.prevent="onSubmit"> ... </form>`

Shorthands:

`v-bind`:
- `<a v-bind:href="url"> ... </a>`
- `<a :href="url"> ... </a>`
- `<a :[key]="url"> ... </a>`

`v-on`:
- `<a v-on:click="doSomething"> ... </a>`
- `<a @click="doSomething"> ... </a>`
- `<a @[event]="doSomething"> ... </a>`

## Computed Properties and Watchers

In-template expressions are meant for simple operations. For any complex logic, you should use a **computed property**.

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

<script>
...
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
...
</script>
```

### Computed Caching vs Methods

We can achieve the same by invoking a method in the expression:

```html
<p>Reversed message: "{{ reverseMessage() }}"</p>

<script>
...
  methods: {
    reverseMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
...
</script>
```

For the end result, the two approaches are indeed exactly the same. However, the difference is that **computed properties are cached based on their reactive dependencies**.

### Computed vs Watched Property

Vue does provide a more generic way to observe and react to data changes on a Vue instance: **watch properties**. However it is often a better idea to use a computed property.

```html
<div id="demo">{{ fullName }}</div>

<script>
...
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
...

// Instead we should do:
...
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
...
</script>
```

### Watchers

This is most useful when you want to perform asynchronous or expensive operations in response to changing data.

```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>

<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // _.debounce is a function provided by lodash to limit how
    // often a particularly expensive operation can be run.
    // In this case, we want to limit how often we access
    // yesno.wtf/api, waiting until the user has completely
    // finished typing before making the ajax request. To learn
    // more about the _.debounce function (and its cousin
    // _.throttle), visit: https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```
