#jQuery Slider &amp; Rolling


##소개


jQuery 용 슬라이더 그리고 롤링 플러그인 입니다.<br>
이미지들(혹은 컨텐츠 및 대상 엘리먼트)를 원하는 방향으로 슬라이드 혹은 롤링 (이하 슬라이드로 표시함) 되는 플러그입니다.<br>
이동되는 방향은 클릭 이벤트를 이용하여 왼쪽, 오른쪽, 위, 아래 로 이동되며 자동 이동도 지원합니다.<br>
순차적으로 아이템이 노출되는 방식이 아닌 원하는 아이템이 노출되게 블릿을 클릭하면 해당 아이템이 노출되는 방식도 지원합니다.<br>

* 데모 : http://syakuis.github.io/demo/jquery-slider-rolling/demo.html
* 블로그 : http://syaku.tistory.com/244


### 초기 설정

jQuery 1.5 이상에서 작동합니다.

```html
<script type="text/javascript" language="javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.5/jquery.min.js"></script>
<script type="text/javascript" language="javascript" src="./jquery-syaku.rolling.js"></script>
```


### 옵션 설명

필수 옵션 : data, width, height

```javascript
{
  data : [ ], // 롤링될 아이템 데이터
  name : '#srolling_area', // 롤링 대상 id element
  item_count : 1, // 한번 이동될 아이템 수
  cache_count : 5, // 최초 한번 임시로 생성될 아이템 수 (고치지마세요)
  width : 100, // 노출될 아이템 크기 (필수 : 실제 아이템 보다 약간 크게 설정하세요)
  height : 100, // 노출될 아이템 크기 (필수 : 실제 아이템 보다 약간 크게 설정하세요)
  auto : false, // 자동 이동 설정
  delay : 1000, // 자동 이동 후 대기 시간
  delay_frame : 500, // 이동 속도 (움직이는 효과를 없애고 싶으면 0)
  move : 'left', // 이동 방향 left , right , up , down 
  prev : '#srolling_prev', // 이전 아이템 이동 버튼 element
  next : '#srolling_next', // 다음 아이템 이동 버튼 element
  is_bullet : false, // 블릿 사용여부
  bullet : '#srolling_bullet', // 블릿 버튼 element
  bullet_item : '#srolling_bullet_item', // 블릿 버튼 item element
  bullet_style_func : null // 선택된 블릿 효과주기
}
```

###기본 사용 예제

아이템을 자동으로 왼쪽으로 슬라이드 됩니다. 

####HTML 소스

style 속성을 아래와 같이 작성하셔야 합니다.
width 를 늘일 경우 보여지는 아이템 영역이 늘어나게 됩니다. 아이템 크기에 영향을 미치지는 않습니다.

```html
<div id="srolling" style="position: relative;overflow:hidden;width:330px;height:25px;"></div>
```


####jQuery 소스

data 변수는 배열 형식입니다. 배열에 추가되는 변수를 아이템으로 지칭하겠습니다.

**유동적으로 배열의 아이템을 추가할 경우 array_push 함수를 이용하세요.**

이템 변수의 값을 입력할 때, ' , " 문자가 들어가지않게 주의해주세요.

**'," 문자는 역슬러쉬로 치환하시면 됩니다.**


```javascript

    var data = [
      "<div style='border:1px solid #000;width:100px;height:20px;'>1</div>",
      "<div style='border:1px solid #000;width:100px;height:20px;'>2</div>",
      "<div style='border:1px solid #000;width:100px;height:20px;'>3</div>",
      "<div style='border:1px solid #000;width:100px;height:20px;'>4</div>",
      "<div style='border:1px solid #000;width:100px;height:20px;'>5</div>",
      "<div style='border:1px solid #000;width:100px;height:20px;'>6</div>",
      "<div style='border:1px solid #000;width:100px;height:20px;'>7</div>"
    ];
    
    jQuery("#srolling").srolling({
      data : data,
      width : 110,
      height : 25
    });    

```


###버튼을 이용한 예제

이벤트 효과를 줄 id 엘리면트를 설정합니다.
p_click : 이전
n_click : 다음

```html
  <ul>
  <li id='p_click' class="btn">이전</li>
  <!-- width 를 늘일 경우 노출되는 아이템이 늘어납니다. 다음 이전 버튼은 필수는 아닙니다. -->
  <li><div id="srolling" style="position: relative;overflow:hidden;width:330px;height:25px;"></div></li>
  <li id='n_click' class="btn">다음</li>
  </ul>
```

아래의 설정은 자동으로 아이템이 흐르고 흐리는 속도는 200 이동방향은 왼쪽 입니다.
prve 는 이전 이벤트를 적용할 엘리먼트를 next 다음 이벤트를 적용할 엘리먼트를 id 속성으로 입력하면 됩니다.

```javascript
    jQuery("#srolling").srolling({
      data : data,
      auto : true,
      width : 110,
      height : 25, 
      delay_frame : 200,
      move : 'left',
      prev : '#p_click',
      next : '#n_click'
    });
```

###블릿을 이용한 예제

이전 다음 클릭 이벤트도 추가하였으나, 빼셔도 됩니다.
블릿을 사용할 때는 srolling_bullet, srolling_bullet_item id 엘리먼트가 필요합니다.
srolling_bullet_item 은 아이템 수만큼 반복되어 노출되는 대상입니다.
{idx} 는 해당 블릿의 index 값을 치환합니다. 
*onclick="_func({idx});" 과 같은 방법으로 사용할 수 있습니다.*

```html
<div>
  <h1>좌우 블릿 자동 슬라이드 예제</h1>
  <ul class="movie">
  <li id='p_click3'>이전</li>
  <li><div id="srolling3" style="position: relative;overflow:hidden;width:620px;height:280px;"></div></li>
  <li id='n_click3'>다음</li>
  </ul>
  <!-- 블릿을 사용할 경우 추가하세요. 꼭 id 값을 이용해야 하며, srolling_bullet_item 아이템 수만큼 노출되는 블릿영역입니다. 블릿 idx 값을 얻고 싶다면 {idx}  를 삽입하시면 idx 값으로 치환됩니다.-->
  <div>
  <ul id='srolling_bullet' class='bullet'>
  <li id='srolling_bullet_item'><img src='./images/bullet_off.png' title="{idx} 입니다."></li>
  </ul>
</div>
```

bullet_style_func : 아이템이 이동될때마다 호출되는 함수입니다.
선택된 블릿 효과는 아래와 같이 직접 코딩해야 합니다.

```javascript
    var data2 = [
      "<img src='./images/a.png' width='620' height='280'>",
      "<img src='./images/b.png' width='620' height='280'>",
      "<img src='./images/c.png' width='620' height='280'>",
      "<img src='./images/d.png' width='620' height='280'>"
    ];
    
    jQuery("#srolling3").srolling({
      data : data2,
      auto : true,
      item_count : 1, // 블릿은 아이템 1개만 사용할 수 있습니다.
      cache_count : 4 * 2, // 블릿을 사용할 경우 아이템수 2배로 입력하세요
      width : 620,
      height : 280,
      delay_frame : 500,
      prev : '#p_click3',
      next : '#n_click3',
      is_bullet : true,
      bullet_style_func : function(bullet,get_bullet) {
        // bullet = $('#srolling_bullet_item') 리턴받음
        // get_bullet = $('선택된 class 블릿') 리턴받음

        // 모든 버튼 블릿 초기화
        jQuery('li',bullet).each(function() {
          var img = jQuery('img',this);
          var src = img.attr('src');
          src = src.replace('on','off');
          img.attr('src',src);
        });

        // 선택된 블릿 효과 주기
        var img = jQuery('img',get_bullet);
        var src = img.attr('src');
        src = src.replace('off','on');
        img.attr('src',src);

      }
    });
```



