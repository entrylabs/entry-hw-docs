---
layout: none
title: 엔트리 블록추가
---

### 엔트리 블록추가 하기

엔트리에 블록을 추가하는 과정은 간단하게

>1. 블록 모양 정의
>2. 블록 기능 정의
>3. 블록 삽입

위의 3단계 과정으로 요약할수 있습니다.  
  
 
### 데이터 블록

#### 블록 모양 정의
* 블록 모양은 Blockly.Blocks 에 선언하도록 되어 있습니다.
* 예 ) ![기본블록](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image.png) = ![더하기블록](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B1%5D.png)
* Blockly.Blocks 에 더하기 블록의 경우 위과 같이 작성하도록 되어 있습니다.
* setColour 는 블록의 색을 정의 합니다. (calcBlockColor 의 기본색상은 #FFD974 입니다.)
* appendValueInput은 입력받을수 있는 영역을 나타 냅니다. 
* setCheck는 입력 받을 값에 대한 데이터를 정의합니다.
* appendDummyInput은 더미를 만들어 추가로 블록을 넣는 공간을 확보하는 역할을 수행합니다.
* setOutput은 이 블록의 결과값을 정의 합니다.(true = 결과값이 있다. 'Number' = 숫자 타입이다.)
* setInputsInline의 경우 입력 값을 한줄로 받겠다는 내용입니다.
* 블록정의의 경우 현재 구글의 Blockly를 사용하고 있습니다. 블록의 모양은 다르지만 사용법은 유사하므로
* https://developers.google.com/blockly/custom-blocks/defining-blocks#init_function
* Blockly의 Defining Blocks 섹션을 참고하시면 도움이 되실것 같습니다.
#### 블록 기능 정의
* 블록 기능 정의는 Entry.block 에 선언하도록 되어 있습니다.
* 예) ![엔트리블록예제](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B2%5D.png)
* Entry.block에 더하기 블록의 경우 위와 같이 작성하도록 되어 있습니다.
* 일단 Blockly에서 작성하였던 이름 그대로 calc_plus로 정의를 하고 사용합니다.
* Blockly 에서 입력 필드로  LEFTHAND와 RIGHTHAND 2가지를 정의해 놓았기 때문에 script.getNumberValue 로 접근해 해당 값을 가져올수 있습니다.
* 덧셈 블록이므로 두 값을 더해 return 하는것을 볼수 있습니다.
#### 블록 삽입
* 블록 삽입의 경우에는 EntryStatic.getAllBlocks와 EntryStatic.blockInfo 두곳에 정의를 하도록 되어 있습니다.
* getAllBLocks의 경우는 category와 사용할 블록의 이름을 적도록 되어 있습니다. 해당 이름은 Blockly.Blocks에 선언 했던 이름과 같은것을 사용합니다.
* 예) ![getAllBlocks](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B3%5D.png)
* 위와 같은 방법으로 작성되어 있으며 해당 내용은 entryjs에 static.js 라는 이름의 파일 안에서 관리되고 있습니다.
* calc_plus의 경우 category가 calc인 블록 모임에 포함되어 있으므로 categoryt: "calc"에 추가 하도록 합니다.
* 예) ![static value](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B4%5D.png)
* 카테고리는 엔트리 워크 스페이스 상에서 탭으로 관리 되고 있습니다.
* ![workspace](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B5%5D.png)
* calc category는 실제로는 계산 이라는 탭으로 보여지게 됩니다.
* blockInfo에는 블록에 대한 초기 값 세팅과 기타 속성들을 세팅하도록 되어 있습니다.
* 예) ![static value](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B6%5D.png)
* xml : 블록의 내부의 모양과 초기값등을 설정할수 있습니다.
	* 설정할 내용이 없을경우 "<block type='calc_plus'></block>"와 같이 작성하면 됩니다.
	* 스크린샷의 설정값의 경우는 LEFTHAND와 RIGHTHAND에 기본값이 10을 넣는 XML입니다.
	* 작성된 xml은 아래와 같습니다.  

{% highlight xml %}
<block type='calc_plus'>
    <value name='LEFTHAND'>
        <block type='number'>
            <field name='NUM'>0</field>
        </block> 
    </value>
    <value name='RIGHTHAND'>
        <block type='number'>
            <field name='NUM'>10</field>
        </block>
    </value>
</block>
{% endhighlight %}  

* class: 블록 간의 구분선을 만들기 위한 속성 입니다.
	* ![class function](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B7%5D.png)
	* 해당 속성 사용법은 소스상에서 위에서 아래로 순차적으로 탐색하면서 변화가 있을때마다 구분선을 작성합니다.
	* 즉, 이름이 똑같은 것을이 모여서 구분되는것이 아니라 값이 변경되는 순간에 구분선이 만들어 지도록 되어 있습니다.
* isNotFor: 조건에 따라 블록을 보이거나 보이지 않도록 하는 기능을 수행합니다.
	* 예) 변수가 생기면 변수를 감추거나 보이게 하는 블록이 보이게 된다.
	* ![static value](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B8%5D.png)
* usage: 강의 분석 용도.(일반적으로 비워 두시거나 class와 똑같이 이름 사용하시면 됩니다.)
* description: 블록에 대한 설명입니다.  

### 일반블록   

#### 블록 모양 정의
* 일반 블록의 경우는 일반적으로 위아래로 추가로 블록이 붙는 설정이 필요합니다.
* ![일반블록 샘플](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B9%5D.png) = ![반복하기 블록](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B10%5D.png)
* 위의 예제는 반복 블록 예제입니다.
* 기본적인 작성법은 데이터 블록과 같습니다.
* Entry경우에는 전부 InputsInline 블록이기 떄문에 setInputsInline 블록의 경우는 항상 true입니다.
* appendStatemenInput('DO')와 같이 작성하면 블록 안에 블록을 작성할수 있는 형태가 됩니다.
* setPreviousStatement 값이 true인 경우 어떤 블록 밑에 붙일수 있도록 블럭이 생성됩니다.
* ![상단 연결](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B11%5D.png) 
* setNextStatement 값이 true인 경우 어떤 블록을 밑에 붙일수 있도록 블럭이 생성됩니다.
* ![하단 연결](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B12%5D.png)
* 이외 기타 사용법은 위에 언급드린대로 Blockly의 Defining Block섹션을 확인하시면 될것 같습니다.  

#### 블록 기능 정의
* ![블록 정의](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B13%5D.png)
* 일반 블록은 기본적으로 data블록과 같은 방식으로 작성이 됩니다.
* 다만 리턴값이 data가 아닌 script.callReturn();이 일반적으로 작성됩니다.
* callReturn()의 경우는 다음 블록을 수행하도록 작동합니다.  

#### 블록 삽입
* 블록 삽입의 방식은 위의 데이터 블록과 똑같습니다.

### 오프라인 버전을 이용하여 간단한 블록 만들어 보기
* node.js 설치 및 npm의존성 툴 등의 설치를 제외한 Entry offline 버전에서 블록을 간단하게 추가하는 법을 소개합니다.
* 먼저 http://play-entry.com/#!/offlineEditor 사이트에 들어가 오프라인 버전을 다운 받아 설치를 진행합니다.
* 기본 폴더의 위치는 c:\Entry 입니다.(이하 c:\Entry 에 엔트리가 설치되었다는 가정하에 진행합니다.)
* C:\Entry\entry 폴더에 example.js 라는 자바스크립트 파일을 하나 생성합니다.
* C:\Entry\entry\entry_offline.html 파일을 문서 편집기에서 열어 example.js를 추가합니다.(entry.js 이후에 작성해야 합니다.)
* ![자바스크립트 등록](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B14%5D.png) 
* example.js에 적당한 블록을 추가합니다.(에제 파일 첨부해 보내드립니다.)
* ![예제](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B15%5D.png)
* 위의 예제는 이동방향으로 value만큼 이동시키는 블럭과 LEFTHAND에서 RIGHTHAND값을 빼는 블록 2가지 예를 만들었습니다.
* 해당 블록을 static.js에 추가합니다.
* getAllBlocks
	* ![이동](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B16%5D.png) ![계산](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B17%5D.png)
	* simple_example은 moving category에 data_example은 calc category에 넣었습니다.
* blockInfo
	* ![블록정보](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B18%5D.png)
	* 필요한 가장 기본적인 블록 속성을 입력하였습니다.
* 나온 결과
	* ![예제 실패블록1](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B19%5D.png) ![예제 실패블록2](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B20%5D.png)
	* blockInfo xml에 서 기본값을 정의 하지 않아 실제 값이 들어 가는 부분이 비어 있게 됬습니다.
* 기본 값 세팅
	* data_example 의 경우 xml을 아래와 같이 작성합니다.
{% highlight xml %}
<block type='data_example'>
    <value name='LEFTHAND'>
        <block type='text'>
            <field name='NAME'>99</field>
        </block>
    </value>
    <value name='RIGHTHAND'>
        <block type='text'>
            <field name='NAME'>1</field>
        </block>
    </value>
</block>
{% endhighlight %}  
* simple_example의 경우 아래와 같이 xml을 작성합니다.
{% highlight xml %}
<block type='simple_example'>
    <value name='VALUE'>
        <block type='number'>
            <field name='NUM'>100</field>
        </block>
    </value>
</block>
{% endhighlight %}

* 결과
	* ![예제 성공블록1](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B21%5D.png) ![예제 성공블록2](https://raw.githubusercontent.com/entrylabs/entry-hw/gh-pages/wiki-image/module/block_create/Image%20%5B22%5D.png)
 	위와 같이 나오게 되었습니다.
