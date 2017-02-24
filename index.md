##6 모듈

Angular는 많은 모듈의 집합
뼈대와 같은 역할

모듈을 관리하기 위해 @NgModule 장식자 이용한다
하지만 규모가 커질수록 모듈 관리에 드는 비용이 증가한다

비용문제를 해결하기 위해 그룹단위로 구성요소를 분리하고 이에 따르는 모듈도 같이 분리한다

모듈이 무엇인지? 모듈을 구성하는 방법을 알아보자


##6.1 모듈 소개
##6.1.1 모듈이란?

Angular가 제공하는 모듈  : Angular library module 

Angular library module 종류
지시자, 파이프, 장식자, 클래스 , 인터페이스, 함수, Enum, type alias, 상수 (Const)

주요 패키지



각 패키지는 수많은 모듈로 구성되어 있음

원하는 특정 모듈만 선택하여 import 한다

ex)
// @angular/core 패키지중 Component 만 호출
import { Component  } from ‘@angular/core’;         









사용자 정의 모듈 
컴포넌트나 지시자와 같은 장식자를 이용한 모듈
서비스, 함수와 같은 장식자가 없는 모듈


ex)
//장식자가 없는 모듈 예제 (클래스, 값, 함수..)

//클래스
export class Message {
   sayHello(){
      return “Hello!”;
   }
}

//배열변수
class District {
   id : number; name; string;
}
export var DISTRICT: District[] = [
   { id: 1, name: ‘서울’ },   { id: 2, name: ‘부산’ },   { id: 3, name: ‘대구’ }
];

//함수
export function echo(msg:string){
   return msg;
}

외부로 공개할 모듈은 export 이용 선언
export ES6부터 지원
이것은 모듈입니다 알리는것 
다른모듈에서 해당 모듈로 접근 가능한 상태가 된다

위 모듈이 hello.module 일때
//import 방법
import {Message, echo, DISTRICT} from ‘./hello.module’;
//이름 변경 import
import {echo as echo2} from ‘./hello.module’;



##6.1.2 모듈성과 Angular 모듈

모듈성 (modularity) 개발 비용 관련되어 있음
모듈성을 갖춤 = 성능 향상될 수 있게 모듈간 결합 최소화, 모듈 내부의 응집도는 최대화

내부의 응집도를 최대화하고 구성요소간 역할을 명확히 분리하라

규모가 커지는 것을 대비하여
모듈을 그룹단위로 관리하라


 






루트 모듈 (application root module) 을 통해 어플리케이션 모듈을 구성

루트에서 모든것을 설정하는것은 비효율적이기에

특징 모듈 (feature module), 공유 모듈 (shared module), 핵심 모듈 (core module) 로 나눈다

특징 모듈 (feature module)
	단위 애플리케이션 구성 
ex) 게시판 (글쓰기, 리스트 컴포넌트, 여러 서비스 지시자와 함께 하나의 애플리케이션을 구성)

공유 모듈 (shared module)
	다른 모듈에 포함되어 동작하는 모듈
	ex) 반복적으로 선언되는 모듈이 있다면 묶어서 공유 모듈로 정의

핵심 모듈 (core module)
	항상 동작할 필요가 있거나 전체적인 동작에 핵심적인 역할을 하는 모듈
	ex) 타이틀 컴포넌트




##6.2 애플리케이션 루트 모듈

##6.2.1 애플리케이션 루트 모듈 선언

항상 정의되어 있어야하는 모듈

@NgModule({
   imports: [호출할 모듈1, 호출한 모듈2, ...],
   exports: [노출할 모듈1, 노출할 모듈2, ...],
   declarations: [사용할 구성요소1, 사용할 구성요소2, …],
   bootstrap: [애플리케이션 컴포넌트]
})

##NgModule 은 애플리케이션 모듈에 대한 메타데이터 정보를 제공
선언자 (declarations), 호출자 (imports), 제공자 (providers), 부트스트랩 (bootstrap)  속성이 있음



ex) \module\src\app\app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
...

@NgModule({
  imports: [
    BrowserModule,CommonModule, FormsModule,    
    ...
  ],
  providers: [],
  declarations: [
    AppComponent,
    ...
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

imports 속성
BrowserModule : Angular가 브라우저 상에서 동작한다면 반드시 포함
CommonModule : 템플릿에서 사용하는 ngIf , ngFor 와 관련된 기능 포함
            	                  (공통모듈은 브라우져 모듈이 선언되어 있다면 선언하지 않아도 된다)
FormsModule : NgModel 지시자나 내장 검중기 지시자등을 포함

providers 속성
애플리케이션 전역에서 사용할 서비스를 등록합니다. (logger 서비스와 같은 )

declarations 속성
	애플리케이션 레벨에서 사용하는 컴포넌트, 지시자 파이프 를 선언

bootstrap 속성
	최상위 컴포넌트인 애플리케이션 컴포넌트를 등록
	ex) 이때 선언되는 애플리케이션 컴포넌트
\module\src\app\app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `...
    <router-outlet></router-outlet>`
})
export class AppComponent { }



router-outlet 에 표시할 컴포넌트가 있다면 애플리케이션 라우팅 모듈 설정에 등록한다

애플리케이션 라우팅 모듈 : url 입력에 대응하는 컴포넌트를 router-outlet 에 표시해 준다




ex) \module\src\app\app-routing.module.ts

import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { IntroComponent } from './intro.component';
import { HelloComponent } from './hello/hello.component';
...

const appRoutes: Routes = [
  { path: '', component: IntroComponent },
  { path: 'hello', component: HelloComponent },
  ...
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}


appRoutes  변수는 라우팅 설정을 담고 있으며 입력 URL 에 대응하는 컴포넌트로 라우팅 되게 한다


## 애플리케이션 루트 모듈에 라우팅 모듈 등록

/* 어플리케이션 라우팅 모듈 */
import { AppRoutingModule } from './app-routing.module';

@NgModule({
  imports: [
     ....
     AppRoutingModule
  ],



##6.2.2 핵심 모듈

루트모듈에 한번 설정하면 전역에서 사용하능한 모듈

애플리케이션 시작될때 한번 호출해서 전역사용함
ex) 타이틀 컴포넌트

핵심 모듈 정의
/app/core 디렉토리 추가 (core 는 임의로 변경가능)

핵심 모듈 등록 대상 (컴포넌트, 지시자, 파이프, 서비스)
컴포넌트, 서비스를 등록해보자





ex)  
\module\src\app\core\title.component.ts

import { Component, Input } from '@angular/core';
import { UserService } from '../core/user.service';

@Component({
  selector: 'app-title',
  template: `
  <h1 highlight>{{title}}</h1>
  by <b>{{user}}</b>`
})
export class TitleComponent {
  @Input() title = '';
  user = '';

  constructor(userService: UserService) {
    this.user = userService.nickName;
  }
}


ex) 
\module\src\app\app-routing.module.ts

import { Injectable, Optional } from '@angular/core';

export class UserServiceConfig {
  nickName = '';
}

//UserService 서비스는 루트모듈에 등록 되기 때문에 싱글턴으로 존재하며 전역 서비스가 된다
@Injectable()
export class UserService {
  private _nickName = '';

  //@Optional : 주입객체가 있다면 해당 객체 받고 없으면 null
  //조건문을 통해 객체가 있다면 서비스에 선언된 _nickName 프로퍼티를 갱신한다
  constructor( @Optional() config: UserServiceConfig) {
    if (config) { this._nickName = config.nickName; }
  }

  // obj.nickName 으로 접근가능하게함
  get nickName() {
    return this._nickName;
  }
}









ex) 마지막으로 핵심 모듈 설정
\module\src\app\core\core.module.ts

import { ModuleWithProviders, NgModule, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';

import { TitleComponent } from './title.component';
import { UserService } from './user.service';
import { UserServiceConfig } from './user.service';

@NgModule({
  imports: [CommonModule],
  declarations: [TitleComponent],
  exports: [TitleComponent], 
  providers: [UserService]
})
export class CoreModule {
  constructor( @Optional() @SkipSelf() parentModule: CoreModule) {
    if (parentModule) {
      throw new Error(
        'CoreModule이 이미 로딩되었습니다.');
    }
  }

  static forRoot(config: UserServiceConfig): ModuleWithProviders {
    return {
      ngModule: CoreModule,
      providers: [
        { provide: UserServiceConfig, useValue: config }
      ]
    };
  }
}


// 핵심 모듈을 루트모듈에 추가하기 위해 
TitleComponent 선언하고 @NgModule exports 에 선언

//부모 주입기에 대한 의존성을 확인
@SkipSelf

부모 주입기에 객체가 존재하는지 확인후 객체가 존재하면 객체를 받기 위해 @Optional() 이용해 주입받는다

@Optional 장식자는 객체가 존재하면 해당 객체를 전달하고 없으면 null 전달












##루트모듈에 핵심 모듈 등록

ex) \module\src\app\app.module.ts

//핵심 모듈 import
import { CoreModule } from './core/core.module';

@NgModule({
  imports: [
            …
	// CoreModule imports 에 선언, forRoot() 메서드로 값 전달
CoreModule.forRoot({nickName: 'Happy'}),
...
  ],
})
export class AppModule { }


##핵심모듈 테스트

ex) \module\src\app\core-test\core-test.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'core-test',
  template: `
  <app-title [title]="title"></app-title>`
})
export class CoreTestComponent {

  title = '반갑습니다! Core Module!';

}

##테스트 컴포넌트를 라우터에 등록해서 url 로 접근 가능하게 만듬

ex)
\module\src\app\app-routing.module.ts

import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const appRoutes: Routes = [
    ...
    { path: 'core-test', component: CoreTestComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}




## 6.3 특징 모듈

## 6.3.1 하위 모듈의 필요성

루트모듈로 애플리케이션을 구성할 수 있지만

Angular 애플리케이션 규모가 커지면 기능이 비슷한 모듈이 그룹단위로 묶이게 된다
규모가 커지는 대로 루트에서 모든 모듈을 구성하려고 한다면 복잡해진다
또한 모듈간에 이름이 충돌하는 문제가 생긴다
비슷한 모듈을 그룹단위로 관리하기 위해선 루트와 별개로 하위 모듈을 만들 필요가 있다

하위로 분리하는 모듈을 특징 모듈 (feature module) 이라고 한다

특징 모듈
기능적으로 관심이 비슷한 모듈을 묶어 상위에 제공
기능이 비슷하기 때문에 하나의 특징 그룹이된다

비슷한 모듈 그룹의 예
ex)
유틸리티 모듈 그룹
라우팅 관련 모듈 그룹
파이프 모듈 그룹



특성
루트모듈 아래 위치 (즉 루트에서 특징모듈 import)
특징 모듈에 선언한 모듈은 다른 모듈에 노출(export) 하거나 숨길 수 있음
루트와 특징 모듈 각각 라우팅 설정을 포함할 수 있음
라우팅에서 특징모듈로 라우팅 가능 (게으른 모듈 설정과 관련)









##6.3.2 특징 모듈에 선언할 구성요소 추가

하이라이트 지시자 구성요소 추가 예제

ex) 
\module\src\app\member\highlight.directive.ts

import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[highlight]'
})
export class HighlightDirective {

  private el: HTMLElement;
  constructor(el: ElementRef) {
    this.el = el.nativeElement;
    this.el.style.fontSize = "30px";
  }
  @HostListener('mousemove') onMouseMove() {
    this.el.style.backgroundColor = "blue";
    this.el.style.color ="white";
  }
  @HostListener('mouseleave') onMouseLeave() {
    this.el.style.backgroundColor = null;
    this.el.style.color ="black";
  }
}

위 지시자는 하이라이트 기능이 있고
 @HostListener를 통해 등록된 이벤트에 따라 선언영역에 마우스 색갈 바꾸는 기능

그다음 서비스 정의

ex)
\module\src\app\member\member.service.ts

import { Injectable } from '@angular/core';

export class Member {
  constructor(public name: string,public age: number) { }
}

const MEMBERS: Member[] = [
  new Member('유비',30),
  new Member('관우',29),
  new Member('장비',28)
];

// members 변수에 담긴 리스트 목록 데이터를 컴포넌트에 제공
//
@Injectable()
export class MemberService {

  getMembers() {
    return new Promise<Member[]>(resolve => {
      setTimeout(() => { resolve(MEMBERS); }, 500);
    });
  }

  getMember(name: string) {
    return this.getMembers()
      .then(members => members.find(member => member.name == name));
  }
  
}



ex) 출력하는 테스트 컴포넌트
\module\src\app\member\member-list.component.ts

import { Component } from '@angular/core';

import { Member, MemberService } from './member.service';

@Component({
  selector: 'member-list',
  template: `  
  {{name}}의 나이 : {{age}}<br>
  <div *ngFor='let m of members' highlight (mouseover)="setAge(m.name)">
    {{m.name}} {{m.age}}      
  </div>`
})
export class MemberListComponent {
  members: Member[];
  age: number;
  name: string;
  
  constructor(private memberService: MemberService) {}

  ngOnInit() {
    this.memberService.getMembers().then(members => {
      this.members = members;
    });
    this.setAge("유비");
  }

  setAge(name: string){
    this.name = name;
    this.memberService.getMember(name).then(member => {
      this.age = member.age;
    });
  }
}

컴포넌트 생명주기인 ngOnInit 을 통해 
초기화 할때  memberService 로부터 데이터를 받아온다
이때 .getMembers() 메서드는 프로미스 객체를 반환하기 때문에 then() 메서드를 이용해 결과를 받아온다




## 6.3.3 특징 모듈 선언

특징 모듈에 사용할 구성요소는 준비끝
이제 특징 모듈을 어떻게 구성하는지 살펴보자

특징 모듈은 루트 모듈의 하위 모듈이며 루트와 유사하게 구성

ex)
\module\src\app\member\member.module.ts

import { NgModule }           from '@angular/core';
import { CommonModule }       from '@angular/common';
import { FormsModule }        from '@angular/forms';

import { MemberListComponent }   from './member-list.component';
import { HighlightDirective } from './highlight.directive';
import { MemberRoutingModule }   from './member-routing.module';
import { MemberService }     from './member.service';

@NgModule({
  imports:      [ CommonModule, FormsModule, MemberRoutingModule ],
  declarations: [ MemberListComponent, HighlightDirective ],
  providers:    [ MemberService ]
})
export class MemberModule { }

관심사가 동일한 모듈을 대상으로 구현한다는 것을 다시한번 상기

위 특징 모듈은 멤버리스트를 어떻게 출력할 것인지에 대한 관심사를 갖고 있음

##특징 모듈 라우터 설정

ex) member-list 주소가 요청되면 MemberListComponent 출력하기위한 라우팅 모듈

\module\src\app\member\member-routing.module.ts

import { NgModule }            from '@angular/core';
import { RouterModule }        from '@angular/router';

import { MemberListComponent }    from './member-list.component';

@NgModule({
  imports: [RouterModule.forChild([
    { path: 'member-list', component: MemberListComponent}
  ])],
  exports: [RouterModule]
})
export class MemberRoutingModule {}







ex) 특징 모듈을 루트에 등록

\module\src\app\app.module.ts

import { NgModule } from '@angular/core';

/* 특징 모듈 */
import { MemberModule } from './member/member.module';
…

@NgModule({
  imports: [
    ...
    MemberModule,
  ],
  providers: [],
  declarations: [
    AppComponent,
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

특징 모듈과의 차이는 bootstrap 속성이 있냐 없냐의 차이 (루트에만 있음)



## 6.3.4 특징 모듈에 공유 모듈 설정

공유 모듈은 애플리케이션 관점에서 사용되는 모듈
자주 사용되지만 핵심 모듈처럼 항상 사용되는건 아니고 
특징 모듈을 구성할 때 자주 반복적으로 선언 되는 모듈 그룹


공유 모듈 비슷한 성격의 모듈
ex)
ngIf , ngFor 포함하는 BrowerModule 의 경우 [(ngModel)] (양방향 지원) 과 함께 자주 사용함 
따라서
BrowerModule, FormsModule 을 공유 모듈로 묶고
특징 모듈로 제공하기 위해 재노출합니다.




목적은 모듈 선언의 중복을 피하기 위해

공유모듈은 shared 디렉토리에 위치
/app/shared


##공유 모듈에서 사용할 구성요소 정의

ex) 
하이라이트 지시자는 영역에 마우스 커서 들어오면 배경 빨강으로 바꾸는 지시자
\module\src\app\shared\highlight.directive.ts

import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[highlight]'
})
export class HighlightDirective {

  private el: HTMLElement;
  constructor(el: ElementRef) {
    this.el = el.nativeElement;
    this.el.style.fontSize = "30px";
  }
  @HostListener('mousemove') onMouseMove() {
    this.el.style.backgroundColor = "red";
    this.el.style.color = "white";
  }
  @HostListener('mouseleave') onMouseLeave() {
    this.el.style.backgroundColor = null;
    this.el.style.color = "black";
  }
  
}

ex)
파이프 추가 
myupper 파이프 : 입력을  대문자로 변환하는 파이프

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'myupper' })
export class MyUpperPipe implements PipeTransform {
  transform(phrase: string) {
    return phrase.toUpperCase();
  }
}










## 지시자와 파이프를 공유 모듈에 등록

ex)
\module\src\app\shared\shared.module.ts

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

import { MyUpperPipe } from './my-upper.pipe';
import { HighlightDirective } from './highlight.directive';

@NgModule({
  imports: [CommonModule],
  declarations: [MyUpperPipe, HighlightDirective],
  exports: [MyUpperPipe, HighlightDirective,
    CommonModule, FormsModule]
})
export class SharedModule { }

declarations 선언을 통해 모듈로 등록
exports 를 통해 외부로 노출

이렇게 정의하면 간단하게 다른 모듈에 포함될 수 있음


## 공유 모듈을 이용한 모듈 작성과 결과 테스트 

ex)
테스트할 컴포넌트 등록
\module\src\app\player\player.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'player',
  template: `<div highlight>{{"player!!!"|myupper}}</div>`
})
export class PlayerComponent { }

highlight지시자 와 myupper파이프 정의














ex)
라우터정의
\module\src\app\player\player-routing.module.ts


import { NgModule } from '@angular/core';
import { RouterModule } from '@angular/router';

import { PlayerComponent } from './player.component';

@NgModule({
  imports: [RouterModule.forChild([
    { path: '', redirectTo: 'player', pathMatch: 'full' },
    { path: 'player', component: PlayerComponent }
  ])],
  exports: [RouterModule]
})
export class PlayerRoutingModule { }


// path: '',  루트 패스 //redirectTo   설정되어있으면 그쪽으로 라우팅



ex) 


import { NgModule } from '@angular/core';

import { SharedModule } from '../shared/shared.module';

import { PlayerRoutingModule } from './player-routing.module';
import { PlayerComponent } from './player.component';

@NgModule({
  imports: [ PlayerRoutingModule, SharedModule],
  declarations: [PlayerComponent],
  providers: []
})
export class PlayerModule { }

앞서 공유한 shareModule 을 import 
덕분에 PlayerModule 에 선언하는 모듈의 갯수가 줄어들었음













## 6.3.5 게으른 모듈 로딩

기본적으로 모듈을 부지런히(eagerly) 로딩한다 
다시 말하면 애플리케이션이 시작되면 필요한 모듈을 미리 임포트 해놓음

시작시에는 무리를 주지만 전체적인 라우팅 속도는 빠르다 
모듈이 너무 많아서 시작단계 부하를 준다면
게으른 로딩을 사용하라 (lazy loading)

url 요청하는 시점에 
모듈을 비동기적으로 (asynchronously) 불러온다 

설정은 loadChildren 속성이용

{ path: ‘lazy’ , loadchildren: ‘app/player/player.module#PlayerModule’ },

#을 기준으로 모듈파일과 모듈 클래스로 구분

ex)
\module\src\app\app-routing.module.ts

import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const appRoutes: Routes = [
   { path: 'lazy', loadChildren: 'app/player/player.module#PlayerModule' },
 ];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}


// /lazy 요청이 들어오면 app/player/player.module import




앵귤라 소개

1.*
https://docs.angularjs.org/guide/concepts
2.*
https://angular.io/docs/ts/latest/guide/ngmodule.html


promise 
http://exploringjs.com/es6/ch_promises.html


















## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/misarm/misarm.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/misarm/misarm.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
