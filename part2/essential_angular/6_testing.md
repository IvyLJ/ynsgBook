# Chapter 6: Testing

Angular的设计目标之一是使测试变得简单。这就是为什么框架依赖于依赖注入，将用户代码与框架代码分离，并附带一组用于编写和运行测试的工具。在本章中，我将介绍四种测试Angular组件的方法：隔离测试、浅层测试、集成测试和量角器测试(isolated tests, shallow tests, integration tests, and protractor tests.)。

## 1. Isolated Tests

在不渲染复杂组件的情况下测试它们通常很有用。想知道怎么做，让我们为以下组件编写测试：

```js
@Component({
    selector: 'filters-cmp',
    templateUrl: './filters.component.html',
    styleUrls: ['./filters.component.css']
})

export class FiltersCmp {
    @Output() change = new EventEmitter();
    filters = new FormGroup({
        speaker: new FormControl(),
        title: new FormControl(), highRating: new FormControl(false),
    });
    constructor(@Inject('createFiltersObject') createFilters: Function) {
         this.filters.valueChanges.debounceTime(200).subscribe((value) => {
            this.change.next(createFilters(value)); 
        });
    }
}

```
filters.component.html:

```html
<div [formGroup]="filters"> 
    <md-input-container>
        <input md-input formControlName="title" placeholder="Title">
    </md-input-container>
    <md-input-container>
        <input md-input formControlName="speaker" placeholder="Speaker">
    </md-input-container>
    <md-checkbox formControlName="highRating"> 
        High Rating
    </md-checkbox> 
</div>
```

本例中有几点值得注意：
- 我们在该组件的模板中使用反应形式。这就要求我们在component类中手动创建一个form对象，这有一个很好的结果：我们可以在不呈现模板的情况下测试输入处理。
- 我们将监听所有form更改，使用RxJS debounceTime operator对其进行去抖动，然后发出(emit)更改事件(change even)。
- 最后，我们注入一个函数，在form外创建一个filters对象。

现在，让我们看看测试。

```js
describe('FiltersCmp', () => {
    describe("when filters change", () => {
        it('should fire a change event after 200 millis', fakeAsync(() => { 
            const component = new FiltersCmp((v) => v);

            const events = [];
            component.change.subscribe(v => events.push(v));

            component.filters.controls['title'].setValue('N');
            setTimeout(() => { component.filters.controls['title'].setValue('Ne'); }, 150);
            setTimeout(() => { component.filters.controls['title'].setValue('New'); },200);

            expect(events).toEqual([]);

            tick(1000);
            
            // only one item because of debouncing
            expect(events).toEqual([
                {title: 'New', speaker: null, highRating: false}
            ]);

            component.filters.controls['title'].setValue('New!');
            
            tick(1000);
            
            expect(events).toEqual([
                {title: 'New', speaker: null, highRating: false},
                {title: 'New!', speaker: null, highRating: false}
            ]);
        }));
    });
});

```
如您所见，单独测试Angular组件与测试任何其他JavaScript对象没有区别。我们不使用任何特定于用户界面的工具。但是，我们使用fakeAsync。这是由zone.js提供的工具，利用它我们可以控制时间，这对于测试消除抖动很方便（which is handy for testing the debouncing）。此外，此测试不使用此组件的模板。模板也可以是空的–测试仍然会通过。

## 2. Shallow Testing

测试组件类而不呈现它们的模板可以在某些场景中工作，但不能在所有场景中都工作。有时，只有呈现组件的模板，我们才能编写有意义的测试。我们可以这样做，但仍然保持测试隔离。我们只需要渲染模板而不渲染组件的子级。这就是俗称的浅层测试。

让我们看看这个方法的实际效果。

```js
@Component({
    selector: 'talks-cmp',
    template: '<talk-cmp *ngFor="let t of talks" [talk]="t"></talk-cmp>'
})
export class TalksCmp {
    @Input() talks: Talk[];
}
```
这个组件只是呈现一个TalkCmp的集合

现在让我们看看它的测试。

```js
import { async, ComponentFixture, TestBed } from '@angular/core/testing'; 
import { By } from '@angular/platform-browser';
import { DebugElement, NO_ERRORS_SCHEMA } from '@angular/core';

import { TalksCmp } from './talks.component';

describe('TalksCmp', () => {
    let component: TalksCmp;
    let fixture: ComponentFixture<TalksCmp>;

    beforeEach(async(() => {
        TestBed.configureTestingModule({
            declarations: [TalksCmp],
            schemas: [NO_ERRORS_SCHEMA]
        })
        .compileComponents();
    }));

    beforeEach(() => {
        fixture = TestBed.createComponent(TalksCmp);
        component = fixture.componentInstance;
        fixture.detectChanges();
    });

    it('should render a list of talks', () => {
        component.talks = <any>[
            { title: 'Are we there yet?' },
            { title: 'The Value of Values' }
        ];

        fixture.detectChanges();

        const s = fixture.debugElement.nativeElement;
        const ts = s.querySelectorAll("talk-cmp");

        expect(ts.length).toEqual(2);
        expect(ts[0].talk.title).toEqual('Are we there yet?');
        expect(ts[1].talk.title).toEqual('The Value of Values');
    }); 
});
```
首先，看看我们是如何配置测试模块的。我们只声明了TalksCmp，没有别的。这意味着模板中的所有元素都将被视为简单的DOM节点，并且只应用公共指令（例如ngIf和ngFor）。这正是我们想要的。第二，传递NO_ERRORS_SCHEMA 告诉编译器不要在未知元素和属性上出错，这是浅层测试所需要的。结果将创建一个我们在测试中检查过的DOM元素列表。未创建TalkCmp实例。

## 3. Integration Testing

我们还可以编写一个集成测试来测试整个应用程序。

```js
import {TestBed, async, ComponentFixture, inject} from '@angular/core/testing'; import {AppCmp} from './app.component';
import {AppModule} from './app.module';
import {App} from "./app";

describe('AppCmp', () => {
    let component: AppCmp;
    let fixture: ComponentFixture<AppCmp>; 
    let el: Element;

    beforeEach(async(() => {
        TestBed.configureTestingModule({
            imports: [AppModule]
        });
        TestBed.compileComponents();
    }));

    beforeEach(() => {
        fixture = TestBed.createComponent(AppCmp);
        component = fixture.componentInstance;
        fixture.detectChanges();
        el = fixture.debugElement.nativeElement;
    });
    
    it('should filter talks by title', async(inject([App], (app: App) => {
        app.model.talks = [
            {
                "id": 1,
                "title": "Are we there yet?",
                "speaker": "Rich Hickey",
                "yourRating": null,
                "rating": 9.0
            },
            {
                "id": 2,
                "title": "The Value of Values",
                "speaker": "Rich Hickey", 
                "yourRating": null,
                "rating": 8.0
            },
        ];
        fixture.detectChanges();
        
        expect(el.innerHTML).toContain("Are we there yet?");
        expect(el.innerHTML).toContain("The Value of Values");

        component.handleFiltersChange({ 
            title: 'we',
            speaker: null,
            minRating: 0
        });

        fixture.detectChanges();

        expect(el.innerHTML).toContain("Are we there yet?");
        expect(el.innerHTML).not.toContain("The Value of Values");
    })));
});
```
注意这里我们正在导入AppModule，这意味着Angular将创建所有注册的provides并将编译所有注册的组件。测试本身是不言自明的。

尽管浅层测试和集成测试都呈现组件，但这些测试在本质上是非常不同的。在浅层测试中，我们模拟组件的每个依赖项，并且不呈现组件的任何子级。我们的目标是孤立地执行组件树的一个片段。在集成测试中，我们只模拟平台依赖项（例如，location），其余部分使用生产代码。浅层测试是孤立的，因此，可以用来驱动组件的设计。集成测试仅用于检查正确性。

## 4. Protractor Tests

最后，我们可以编写一个量角器测试(Protractor Tests)来测试整个应用程序。

```js
import {browser, element, by} from 'protractor';

export class TalksAppPage { 
    navigateTo() {
        return browser.get('/');
    }
    
    getTitleInput() {
        return element(by.css('input[formcontrolname=title]'));
    }

    getTalks() {
        return element.all(by.css('talk-cmp'));
    }
    
    getTalkText(index: number) {
        return this.getTalks().get(index).geText();
    } 
}

describe('e2e tests', function() { 
    let page: TalksAppPage;
    
    beforeEach(() => {
        page = new TalksAppPage();
    });
    
    it('should filter talks by title', () => {
        page.navigateTo();

        const title = page.getTitleInput(); 
        title.sendKeys("Are we there");

        expect(page.getTalks().count()).toEqual(1);
        expect(page.getTalkText(0)).toContain("Are we there yet?");
    });
});

```
首先，我们创建了一个page对象，这是使测试更加以域为中心的一个好做法，因此它们更多地讨论用户故事，而不是DOM。其次，我们编写了一个量角器测试来验证按标题过滤是否有效。

量角器测试和集成测试（如上所述）都解决了相同的问题——它们验证正确性，也就是说，它们验证特定用例已经实现。我们应该用哪一种？我倾向于使用集成测试来测试大多数行为，并且只在少数冒烟测试中使用量角器，但是它高度依赖于团队的文化。

## 5. Let's Recap

在本章中，我们研究了四种测试角度组件的方法：孤立测试、浅层测试、集成测试和量角器测试。它们都有各自的时间和地点：
- **isolated tests：**隔离测试是测试驱动组件和测试复杂逻辑的一种好方法。
- **shallow tests：**浅层测试是针对类固醇的独立测试，当编写有意义的测试需要呈现组件时，应该使用浅层测试的模板。
- **integration tests & protractor tests：**最后，集成和量角器测试验证了一组组件和服务（即应用程序）可以协同工作。