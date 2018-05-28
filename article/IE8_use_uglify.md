# IE8에서 예약어를 사용한 소스코드 UglifyJS를 사용하여 Minify하는 방법

## bracket notation(대괄호 표기법) Uglify 하기

객체의 속성을 접근하는 방법은 여러가지가 있다. 그 중에 대괄호 표기법을 사용하여 속성에 접근하는 경우 예약어를 속성의 이름으로 사용하는 경우가 종종 발생한다.
ECMAScript에서도 예약어를 속성의 이름으로 사용하지 않기를 권고 하고 있다. 하지만 사용은 가능하기에 사용하는 분들이 계신다.
이러한 소스코드를 UglifyJS를 사용하여 최소화와 난독화를 하면 문제가 발생한다. 특히 IE8에서 발생한다
그 위에서는 자동으로 예약어를 속성으로 사용하구나 생각하여 자동으로 처리를 한다. 하지만 사용은 안하는 것이 좋다.

아래의 소스코드는 UglifyJS의 기본 설정을 사용하면 Systax Error가 발생한다.

``` javascript
// 원본 소스코드
olleh.INCHES_PER_UNIT["in"] = olleh.INCHES_PER_UNIT.inches;

// Uglify 한 소스코드
olleh.INCHES_PER_UNIT.in = olleh.INCHES_PER_UNIT.inches;

```

`in`이 예약어 포함되어 문제가 있다고 IE8에서는 Syntax Error가 발생한다.([예약어리스트](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords))


이럴 경우 아래의 UglifyJS설정에서 compress의 properties의 값을 false로 변경하여야 한다.

``` javascript
.pipe(minify({
    ext:{'min':'-min.js'},
    exclude: ['tasks'],
    compress:{properties:false} // 기본 설정 true,
}))
```

변경을 후에 Uglify를 다시 진행하면, 아래와 같이 변경이 되지 않고 최소화와 난독화가 된다.

``` javascript
// 원본 소스코드
olleh.INCHES_PER_UNIT["in"] = olleh.INCHES_PER_UNIT.inches;

// Uglify 한 소스코드
olleh.INCHES_PER_UNIT["in"] = olleh.INCHES_PER_UNIT.inches;

```

## String(문자열)을 이용한 객체의 속성 선언

새로운 객체를 만들때 object initializers(객체 초기화) 방법을 사용하여 만들 수 있다. 여기서 객체의 속성의 Key값을 문자열를 사용하여 만들 수 있다.
하지만 문제는 이 문자열이 예약어일 경우 문제가 발생한다. 최근의 브라우저에서는 문제가 되지 않지만, IE8에서는 예약어로 인식하여 문제가 발생한다.

아래의 소스코드는 UglifyJS의 기본 설정을 사용하면 Systax Error가 발생한다.

``` javascript
// 원본 소스코드
olleh.Vector.style = {
    "default": {}
}
// Uglify 한 소스코드
olleh.Vector.style = {
    default: {}
}
```

default는 예약어로 사용이 금지 되지만 사용해도 문제가 없기에 모르고 사용하는 경우가 있다.([예약어리스트](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords))

이럴 경우 아래의 UglifyJS의 설정에서 output의 quote_keys의 값을 true로 변경하여야 한다.

``` javascript
.pipe(minify({
    ext:{'min':'-min.js'},
    exclude: ['tasks'],
    compress:{properties:false},
    output:{quote_keys:true} // 기본값은 false
}))
```

변경을 후에 Uglify를 다시 진행하면, 아래와 같이 변경이 되지 않고 최소화와 난독화가 된다.

``` javascript
// 원본 소스코드
olleh.Vector.style = {
    "default": {}
}
// Uglify 한 소스코드
olleh.Vector.style = {
    "default": {}
}
```

## 마무리

현재 브라우저의 성능이 좋고, 알아서 해주는 부분이 많아 예약어(Reserved Keywords)를 속성으로 사용하는 경우가 많다. Uglify를 하지 않고 소스코드를 그대로 사용하는 경우가 많아 문제가 없다고 생각하여 그대로
실제 프로젝트에 적용하여 사용하는 경우도 많다. 하지만 이것은 잘 못 된 습관으로 JavaScript의 소스코드는 Production에서 사용 시 꼭 Uglify를 해야 하며, 또한 이럴 경우 문제가 발생하지 않도록 예약어를 사용하지 않도록 해야 한다. 또한 IE8이전의 경우 더욱 자동으로 해주는 부분이 없기에 더욱 주의를 기울여야 한다. 혹시 IE8을 지원하면서 Uglify를 해야 한다면 이 포스트가 도움이 되었으면 좋겠다.

## 참고
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors