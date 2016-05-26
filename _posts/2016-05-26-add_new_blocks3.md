---
layout: post
title: 각 블록 모양별 생성 방법
type: entryBlock
order: 3
---

### 1. 값 블록  

#### ㄱ. 기본 블록
![기본 블록]({{site.base-link-url}}/wiki-image/block_create/default_value.png)
{% highlight js %}
Entry.block = {
    default_value: {
        //블록 생상
        color: "#FFD974",
        // 폰트색상 basic_string_field는 기본 색상이 검정색(#000) 이다.
        fontColor: "#000",
        // 블록 모양 정의
        skeleton: "basic_string_field",
        // 블록 텍스트
        template: "test",
        // 보여질 블록 정의
        def: {
            type: "default_value"
        },
        // 블록 그룹 정의
        class: "test",
        // 블록 기능정의
        func: function (sprite, script) {
            return 'value';
        }
    }
}
{% endhighlight %}
가장 기본적인 블록이다.  
사용자에게 어떤한 입력도 받지 않고 단지 func정의한 return 값만 사용할수 있는 블록이다.  
미리 정의된 변수를 호출하는 것이 가능하다.  
skeleton이 "basic_string_field"로 정의된 블록의 모양이 이와 같다.  
글자색이 제대로 보이지 않는다면 fontColor 프로퍼티를 통해 수정할수 있다.  
template 프로퍼티에 정의한 텍스트가 그대로 나오게 된다.
template 프로퍼티는 작성 되어 있지 않다면 Lang.template 에서 정의된 블록 명칭으로 값을 가져오게 된다.  
**다만, 정의된 블록 명칭이 없을경우 프로그램 자체가 로드되지 않는 버그 있으므로 주의!**

> 유사한 블록으로 초시계 값, 소릿값 블록이 있다.  

---  

#### ㄴ. 기본 사용자 입력 블록
![기본 블록]({{site.base-link-url}}/wiki-image/block_create/default_input_value.png)
{% highlight js %}
Entry.block = {
    default_input_value: {
        color: "#FFD974",
        skeleton: "basic_string_field",
        // 블록 텍스트 - %1은 0번째 파라미터를 뜻한다.
        template: "%1",
        // 블록에 사용할 파라미터
        params: [
            {
                type: "TextInput",
                value: 0
            }
        ],
        def: {
            type: "default_input_value"
        },
        // 파라미터를 사용 할때 쓰는 Key값 정의
        paramsKeyMap: {
            // VALUE라는 key에 0번째 파라미터를 정의 하였다.
            VALUE: 0
        },
        class: "test",
        func: function (sprite, script) {            
            // 해당 값을 getField로 가져오고 
            // 가져 올때 paramsKeyMap에서 
            // 정의한 VALUE라는 키값으로 데이터를 가져온다.
            return script.getField('VALUE', script);
        }
    }
}
{% endhighlight %}
가장 많이 사용되는 블록  
단독으로 쓰이기 보단, 다른 블록의 파라미터로 사용되어지는 블록  
사용자에게 직접 입력받는 블록이다.  
여기서의 핵심은 template의 %1값이다. %1 이라고 정의한 값은 params의 0번째 param속성을 가져오고 
해당 값을 세팅하도록 되어 있다.  

---  

#### ㄷ. 중첩 사용자 입력 블록

![기본 블록]({{site.base-link-url}}/wiki-image/block_create/default_multi_input_value.png)
{% highlight js %}
Entry.block = {
    default_multi_input_value: {
        color: "#FFD974",
        skeleton: "basic_string_field",
        // 블록 텍스트
        // 파라미터가 2개 사용되기 때문에 %1 %2 두개를 사용하였다.
        template: "%1 + %2",
        params: [
            {
                // 중첩되는 Value블록을 만들경우에는 
                // TextInput이 아닌 Block타입으로 생성한다.
                // Block type에는 string, boolean, param 
                // 3가지 종류의 accept가 존재 한다.
                type: "Block",
                accept: "string"
            },
            {
                type: "Block",
                accept: "string"
            }
        ],
        events: {},
        def: {
            // def의 params의 경우는 초기값을 지정할수 있다.
            // TextInput의 경우에도 def > params을 통해 값을 지정할수 있다.
            params: [
                {
                    type: "number",
                    params: [ "0" ]
                },
                {
                    type: "number",
                    params: [ "10" ]
                }
            ],
            type: "default_multi_input_value"
        },
        paramsKeyMap: {
            LEFTHAND: 0,
            RIGHTHAND: 1
        },
        class: "test",
        func: function (sprite, script) {
            // type이 Block의 경우에는 Field가 아닌 Value로 취급해서 가져온다.
            // 일반적으로는 getValue로 값을 가져오고
            // 명시적으로 숫자형으로 가져오고 싶을때에는 getNumberValue를 사용한다.
            var leftValue = script.getNumberValue("LEFTHAND", script);
            var rightValue = script.getNumberValue("RIGHTHAND", script);
            return  leftValue + rightValue;
        }
    }
}
{% endhighlight %}
자료 계산시 또는 하드웨어 아날로그 값을 처리할때 많이 사용하는 블록 형태이다.  
해당 블록에서 핵심은 단일 입력 블록을 만들때와는 다르게 `type: "Block"`를 사용해서 
값을 가져오는 것과 def > params를 통해 초기값을 세팅한다는 점을 꼼꼼히 봐야한다.  

> 유사한 블록은 계산 블록들과 하드웨어에서 아날로그 처리 블록들이다.