# Chapter 1: Compilation

Angular的核心是一个复杂的编译器，它采用NgModule类型并生成NgModuleFactory。

NgModule中声明了组件。在创建模块工厂时，编译器将获取模块中每个组件的模板，并使用有关声明的组件和管道的信息，生成一个组件工厂。组件工厂是一个JavaScript类，框架可以使用它来删除组件。

## 1. JIT and AOT

Angular1是一个复杂的HTML编译器，在运行时生成代码。Angular的新版本也有这个选项：它们可以在运行时生成代码，或者实时（just in time:JIT）生成代码。在这种情况下，编译是在应用程序引导时(is being bootstrapped)进行的。但是他们还有另一个选择：他们可以作为应用程序构建的一部分运行编译器，或者提前（ahead of time:AOT）。

## 2. Why would I want to do it?

提前编译应用程序有以下好处：
- 我们不再需要将编译器发送给客户端。因此，编译器是框架中最大的部分。所以它对下载大小有积极的影响。
- 由于编译后的应用程序没有任何HTML，而是生成的TypeScript代码，因此TypeScript编译器可以对其进行分析以产生类型错误。换句话说，模板是类型安全的。
- Bundlers（例如，WebPack、Rollup）可以将应用程序中未使用的所有内容都从树中删除。这意味着您不再需要创建50行节点模块来减少应用程序的下载大小。bundler将确定使用了哪些组件，其余的将从bundle中移除。
- 最后，由于应用程序引导（bootstrap）过程中最昂贵的步骤是编译，因此提前编译可以显著缩短引导时间（bootstrap time）。

总之，使用AOT编译可以使应用程序包更小、更快、更安全。

## 3. How is it possible?

为什么我们以前不做呢，在Angular1？要使AOT工作，应用程序必须对应用程序中的静态和动态数据进行清晰的分离。编译器必须以一种只依赖于静态数据的方式构建。在设计和建造Angular时，我们花了很多精力来做到这一点。新版本的JavaScript和TypeScript所支持的类和装饰器等原语(primitives)使它变得更简单。

要了解这种分离在实践中是如何工作的，让我们看看下面的示例。这里，decorator中的信息是静态的。Angular知道talk组件的选择器和模板。它还知道组件有一个称为talk的输入和一个称为rate的输出。但是框架不知道构造函数或onRate函数做什么。

```js
@Component({
    selector: 'talk-cmp',
    template: `
        {{talk.title}} {{talk.speaker}}
        Rating: {{ talk.rating | formatRating }}
        <watch-button [talk]="talk"></watch-button>
        <rate-button [talk]="talk" (click)="onRate()"></rate-button>
    `
})
class TalkCmp {
    @Input() talk: Talk;
    @Output() rate: EventEmitter;
    constructor() {
    // some initialization logic
    }
    onRate() {
    // reacting to a rate event
    } 
}

```

由于Angular提前知道所有必要的信息，因此它可以编译这个组件，而不需要实际执行任何应用程序代码，作为构建步骤。

## 4. Trade-offs

由于AOT的优越性，我们建议在生产中使用它。但是，和所有事情一样，也有取舍。为了能够提前编译应用程序，元数据必须是可静态分析的。例如，以下代码在AOT模式下不起作用：

```js
@Component({
    selector: 'talk-cmp',
    template: () => window.hide ? 'hidden' : `
        {{talk.title}} {{talk.speaker}}
        Rating: {{ talk.rating | formatRating }}
        <watch-button [talk]="talk"></watch-button>
        <rate-button [talk]="talk" (click)="onRate()"></rate-button>
    `
})
class TalkCmp {
    //...
}
```

这个 window.hide属性将不会被定义。因此，编译将无法指出错误。为了使编译器更智能，已经做了大量的工作，因此它可以理解您在构建应用程序时将使用的大多数日常模式。但某些事情永远不会奏效，就像上面的例子。


## 5. Let's Recap

- Angular的核心部分是它的编译器。
- 编译可以即时JIT（在运行时）和提前AOT（作为构建步骤）完成。
- AOT编译创建更小的包，树震动死代码（ tree shakes dead code），使你的模板类型安全（type-safe），提高了应用程序的引导时间（bootstrap time）。
- AOT编译需要静态地知道某些元数据，因此编译可能在没有实际执行代码的情况下发生。