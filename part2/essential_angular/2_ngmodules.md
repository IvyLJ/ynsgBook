# Chapter 2: NgModules

## 1. Declarations,Imports,and Exports

**NgModules是编译（compilation）和分发（distribution）Angular组件和管道（components and pipes）的单元。在许多方面，它们与ES6模块相似，因为它们有声明、导入和导出。**

## 2. Bootstrap and Entry Components

让我们看看这个例子：

```js
@NgModule({
    declarations: [FormattedRatingPipe, WatchButtonCmp, RateButtonCmp, TalkCmp, TalksCmp],
    exports: [TalksCmp]
})
class TalksModule {}

@NgModule({
    declarations: [AppCmp],
    imports: [BrowserModule, TalksModule],
    bootstrap: [AppCmp]
})
class AppModule {}
```
这里我们定义了两个模块：TalksModule和AppModule。TalksModule拥有在应用程序中执行实际工作的所有组件和管道，而AppModule只有AppCmp，这是一个薄薄的应用程序外壳。

TalksModule声明了四个组件和一个管道。这四个组件可以在它们的模板中相互使用，类似于ES模块中定义的类在它们的方法中如何相互引用。此外，所有组件都可以使用FormattedRatingPipe。因此NgModule是其组件的编译上下文，也就是说，它告诉Angular应该如何编译这些组件。与ES一样，一个组件只能在一个模块中声明。

在本例中，TalksModule仅导出TalksCmp–其余为私有。这意味着只有TalksCmp被添加到AppModule的编译上下文中。同样，这类似于export关键字在JavaScript中的工作方式。

### summary

- 为了提高效率，Angular将静态（声明性）使用的组件与动态（强制）使用的组件分开。
- Angular直接实例化静态使用的组件；不需要额外的抽象。
- Angular为entryComponents中列出的每个组件生成一个组件工厂，因此它们可以被强制实例化。

## 3. Providers

我将在第4章介绍提供者（providers）和依赖注入（dependency injection）。这里我只想指出，NgModules可以包含提供者。导入模块的提供者从左到右与目标模块的提供者合并，即，如果多个导入模块定义同一提供者，则最后一个模块获胜。

```js
@NgModule({
    providers: [
        Repository
    ] 
})
class TalksModule {}

@NgModule({
    imports: [TalksModule]
})
class AppModule {}
```

## 4. Injecting NgModules and Module Initialization

Angular实例化NgModules并用依赖注入注册它们。这意味着您可以将模块注入其他模块或组件，如下所示：

```js
@NgModule({
    imports: [TalksModule]
})
class AppModule {
    constructor(t: TalksModule) {}
}
```

这对于协调多个模块的初始化非常有用，如下所示：

```js
@NgModule({
  imports: [ModuleA, ModuleB]
})
class AppModule {
  constructor(a: ModuleA, b: ModuleB) {
    a.initialize().then(() => b.initialize());
} }

```

## 5. Bootstrap

要在JIT模式下引导(bootstrap) Angular 应用程序，需要将一个模块传递给bootstrapModule。

```js
import {platformBrowserDynamic} from '@angular/platform-browser-dynamic'; 
import {AppModule} from './app';

platformBrowserDynamic().bootstrapModule(AppModule);
```

这将把AppModule编译成一个模块工厂，然后使用工厂来实例化模块。

如果您使用AOT，您可能需要自己提供工厂。
```js
import {platformBrowser} from '@angular/platform-browser'; 
import {AppModuleNgFactory} from './app.ngfactory';

platformBrowser().bootstrapModuleFactory(AppModuleNgFactory);
```

我说“可能需要”，因为CLI和WebPack插件会帮你处理。必要时，它们将bootstrapModuleFactory替换bootstrapModule调用。

## 6. Lazy Loading

正如我前面提到的，NgModules不仅仅是编译单元，它们也是分发单元。这就是为什么我们引导(bootstrap)一个NgModule，而不是一个组件，我们不分发组件，而是分发模块。这也是为什么我们也懒加载NgModules。

```js
import {NgModuleFactoryLoader, Injector} from '@angular/core';

class MyService {
    constructor(loader: NgModuleFactoryLoader, injector: Injector) {
        loader.load("mymodule").then((f: NgModuleFactory) => {
            const moduleRef = f.create(injector);
            moduleRef.injector; // module injector
            moduleRef.componentFactoryResolver; // all the components factories of the lazy-loaded module
        });
    } 
}
```

如果应用程序是在JIT模式下运行的，而不是在AOT模式下运行的，则加载程序编译模块。默认的loader@angular/core附带使用SystemJS，但是，与angular中的大多数东西一样，您可以提供自己的。


## 7. Let's Recap

- NgModules是编译单元。它们告诉Angular应该如何编译组件。
- 与ES模块类似，它们有声明、导入和导出。
- 每个组件（component ）都属于一个NgModules。
- 引导（Bootstrap）和入口组件（entry components）在NgModules中配置。
- NgModules配置依赖注入。
- NgModules用于引导应用程序（bootstrap applications）。
- NgModules用于懒加载代码。
