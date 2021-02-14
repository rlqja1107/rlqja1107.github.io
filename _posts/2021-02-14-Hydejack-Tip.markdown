---
layout: post
title: "Hydejack Mathjax 적용"
subtitle: "Hydejack Mathjax 적용"
categories: tip
comments: true  
---


# Hydejack에 Mathjax 적용하기  



<h2 style="color: royalblue; font-weight: bold">Mathjax 적용시 문제점</h2>   

1. Mathjax를 적용시켰는데 새로고침을 해야 실행되는 경우, 바로 적용이 안되는 경우    
2. 수식이 여러개 나오는 경우  

<h2 style="color: royalblue; font-weight: bold">해결</h2>  

분명히 인터넷 보고 Mathjax.html 파일을 만들고 적용시켰는데, 왜 적용이 안되는지 몇 시간 날린 후에야 이유를 알게 되었고 해결하게 되었다.  
다른 Jekyll 테마 같은 경우에는 새로운 url이 입력되면서 새로운 창이 뜬다. 새로운 창이 뜨면서 **Mathjax.html** 파일에서의 script 파일이 자동으로 적용되게 된다. 하지만, Hydejack의 경우에는 새로운 창이 뜨는 것이 아니라 animation형태로 창이 transform이 된다. 그 증거로, 옆에 있는 side-bar는 그대로 있고 Main 화면만 animation효과로 부드럽게 넘어가는 것을 볼 수 있다.  
처음에는 animation효과 적용파일에 Mathjax.html을 적용시키려고 했지만, 그 위치를 찾을 수가 없었다. 그래서 html의 코드를 잠시 빌렸다.  
### **window.onload**  
window.onload 함수는 html에서 제공하는 함수로 스크립트가 새로 열릴 때, 자동으로 실행하게 하는 함수다.  
만약 hydejack에서 새로운 post 파일을 열 때, window.onload를 통해 **Mathjax.html**를 자동으로 실행하게 해주면 된다.  
#### 사용법  
``` python  
windonw.onload = function(){
    """실행할 함수"""
}
```   

<h2 style="color: royalblue; font-weight: bold">해결과정</h2>  
  
1. _includes 디렉토리에 **mathjax_support.html**를 추가  
위에서는 Mathjax.html이라고 했지만 사실 파일명은 상관이 없다. 아래의 코드를 _includes 디렉토리에 추가시켜준다.  
##### mathjax_support.html  
``` math     
<script type="text/javascript">
    window.MathJax = {
        tex: {
            packages: ['base', 'ams']
        },
        loader: {
            load: ['ui/menu', '[tex]/ams']
        },
        startup: {
            ready() {
                MathJax.startup.defaultReady();
                const Macro = MathJax._.input.tex.Symbol.Macro;
                const MapHandler = MathJax._.input.tex.MapHandler.MapHandler;
                const Array = MathJax._.input.tex.ams.AmsMethods.default.Array;
                const env = new Macro('psmallmatrix', Array, [null, '(', ')', 'c', '.333em', '.2em', 'S', 1]);
                MapHandler.getMap('AMSmath-environment').add('psmallmatrix', env);
            }
        }
    };
</script>
<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>
```  

2. _layout 디렉토리에서 post.html에 mathjax_support.html 추가  


##### post.html  
   
``` python  
 
" include post.html post=page link_title=false "  

window.onload = function(){  
    " include mathjax_support.html "   
}   
" include about.html "   
  
" include related.html post=page "     

```   

    

**(위의 ""를 {% %}형태로 전환)**  


post.html에서 window.onload 함수가 사용된다. github blog에서 파일을 열려고 하면 post.html 파일이 실행되는데, window.onload를 통해서 자동으로 실행시켜준다.   


---
