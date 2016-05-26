---
layout: post
title: 각 블록 모양별 생성 방법
type: entryBlock
order: 3
---

### 1. 값 블록  

#### ㄱ. 기본 블록
![기본 블록]({{site.imageurl}}block_create/default_value.png)
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
![기본 블록]({{site.imageurl}}block_create/default_input_value.png)
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

![기본 블록]({{site.imageurl}}block_create/default_multi_input_value.png)
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

---  

#### ㄷ. 중첩 사용자 입력 블록 - 드롭다운 적용

![기본 블록]({{site.imageurl}}block_create/default_dropdown_input_value.png)
{% highlight js %}
Entry.block = {
    default_dropdown_input_value: {
        color: "#FFD974",
        skeleton: "basic_string_field",
        template: "%1 x %2",
        params: [
            {
                type: "Block",
                accept: "string"
            },
            {
                type: "Dropdown",
                // Dropdown 생성 기준은 
                // [["key1", "value1"], ["key2", "value2"]]
                // 처럼 구성이 된다.
                options: [
                    ["1", "1"],
                    ["2", "2"],
                    ["3", "3"],
                    ["4", "4"],
                    ["5", "5"],
                    ["6", "6"],
                    ["7", "7"],
                    ["8", "8"],
                    ["9", "9"]
                ],
                fontSize: 11,
                // 기본 컬러는 EntryStatic.ARROW_COLOR_VARIABLE 이다.
                // 드롭다운 버튼의 컬러를 커스텀하게 변경 할 수 있다.
                arrowColor: "#FFD974"
            }
        ],
        def: {
            params: [
                {
                    type: "number",
                    params: [ "2" ]
                },
                // Dropdown 값의 경우 Value를 세팅하면 초기값이 처리된다.
                "1"
            ],
            type: "test1"
        },
        paramsKeyMap: {
            LEFTHAND: 0,
            RIGHTHAND: 1
        },
        class: "test",
        func: function (sprite, script) {
            var leftValue = script.getNumberValue("LEFTHAND", script);
            // Dropdown의 경우 getField로 값을 가져오고
            // 명식적으로 숫자형으로 가져올땐 getNumberField를 사용해서 가져온다.
            // 가져온 데이터는 Dropdown Option설정시에 지정하였던 Value값이다.
            var rightValue = script.getNumberField("RIGHTHAND", script);
            return  leftValue * rightValue;
        }
    }
}
{% endhighlight %}
사용자 키보드 타이핑 입력이 아닌 정해진 값을 선택해서 입력받는 방식으로 드롭다운 블록을 이용할수 있다. 
기본적으로 드롭다운은 Key, Value방식으로 동작하고 Key는 사용자 에게 보여주는 값이며, Value는 내부적으로 사용되는 값이다.  

> 유사한 블록은 계산블록의 날짜를 가져오는 블록과 하드웨어에서 센서를 골라서 값을 가져오는 기능이다.

---  

### 2. 판단 블록  

#### ㄱ. 기본 판단 블록
![기본 판단 블록]({{site.imageurl}}block_create/default_boolean.png)
{% highlight js %}
Entry.block = {
    default_boolean: {
        // 판단 블록 색생
        // 기본 색상은 #AEB8FF
        color: "#AEB8FF",
        // 판단 블록은 skeleton이 basic_boolean_field이다.
        skeleton: "basic_boolean_field",
        // template에 기본적으로 텍스트를 작성하면 해당 바로 텍스트가 표현되나
        // 아래와 같이 type: "Text" 로도 마찬가지로 설정할수 있다.
        template: "%1",
        params: [
            {
                type: "Text",
                // 보여질 텍스트
                text: "판단 블록",
                // 텍스트의 색상
                color: "#3D3D3D"
            }
        ],
        def: {
            type: "default_boolean"
        },
        class: "test",
        func: function (sprite, script) {
            // 판단 블록은 리턴값이 true, false이어야 한다.
            return true;
        }
    }
}
{% endhighlight %}
IF문을 사용할때 사용하는 파라미터 블럭 이다. 
기본적으로 디지털 값 등 true, false두가지 형태로 나우어지는 데이터에 대해서 구성되어지는 블록이다.  

> 유사한 블록으로는 판단에 마우스를 클릭했는가? 이다.  

---  

#### ㄴ. 중첩 사용자 입력 판단 블록
![기본 판단 블록]({{site.imageurl}}block_create/default_input_boolean.png)
{% highlight js %}
Entry.block = {
    default_input_boolean: {
        color: "#AEB8FF",
        skeleton: "basic_boolean_field",
        template: "%1 == %2",
        params: [{
            type: "Block",
            accept: "string"
        }, {
            type: "Block",
            accept: "string"
        }],
        def: {
            params: [{
                type: "number",
                params: [ "10" ]
            }, {
                type: "number",
                params: [ "33" ]
            }],
            type: "default_input_boolean"
        },
        paramsKeyMap: {
            LEFTHAND: 0,
            RIGHTHAND: 1
        },
        class: "test",
        func: function (sprite, script) {
            var leftValue = script.getNumberValue('LEFTHAND', script);
            var rightValue = script.getNumberValue('RIGHTHAND', script);
            return (leftValue === rightValue);
        }
    }
}
{% endhighlight %}
사용자에게 입력을 받아 값을 비교해 boolean값으로 반환 받는 블록이다. 
하드웨어 센서 값들을 비교해서 처리하는데 이용할 수 있다. 
다만 단순 값비교는 이미 존재하는 판단블록으로도 처리가 가능하다.

> 유사한 블록으로는 판단의 숫자 비교블록 이다.

#### ㄷ. 중첩 사용자 입력 판단 블록
![중첩 사용자 입력 판단 블록]({{site.imageurl}}block_create/default_dropdown_boolean.png)
{% highlight js %}
Entry.block = {
    default_dropdown_boolean: {
        color: "#AEB8FF",
        skeleton: "basic_boolean_field",
        template: "%1이 참이면",
        params: [
            {
                type: "Dropdown",
                options: [
                    [ "첫번째", "0" ],
                    [ "두번째", "1" ],
                    [ "세번째", "2" ]
                ],
                fontSize: 11
            }
        ],
        def: {
            params: [
                "0"
            ],
            type: "default_dropdown_boolean"
        },
        paramsKeyMap: {
            VALUE: 0
        },
        class: "test",
        func: function (sprite, script) {
            var value = script.getNumberValue('VALUE', script);
            var result;
            switch(value) {
                case 0:
                    result = true;
                    break;
                case 1:
                    result = false;
                    break;
                case 2:
                    result = false;
                    break;
            }

            return result;
        }
}
{% endhighlight %}
정해진 목록내에서 입력 값을 받으려면 드롭다운을 사용할수 있다. 
주로 하드웨어의 디지털 입력을 처리 할때 많이 사용한다.

> 유사한 블록으로는 하드웨어 디지털 입력 블록과 판단의 닿았는가 블록이 있다.

