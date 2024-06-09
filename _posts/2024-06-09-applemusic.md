---
layout: post
title: 애플뮤직 API로 현재 재생중인 음악 표시하기
tags:
  - applemusic
  - api
  - JavaScript
description: >
  애플뮤직 API 이용해보기
hero:
overlay: skyblue
published: true
---
시험기간이라 시작된 뻘짓중 하나.  
어느 시험기간과 같이 공부는 미뤄두고 할만한 뻘짓은 없나 탐색하던도중 친구로부터 애플뮤직 API를 알게되었다.  
방금 전 재생했던 음악 정보를 가져올수있다니, 이거 재미있어보인다.  



## API로 정보 가져오기
애플은 개발자 계정이 있으면 관련된 API를 제공한다.  
하지만 애플 개발자 계정을 등록하는데에는 99달러(약 13만원)을 지불해야한다. ~~심지어 1년마다 갱신~~  

~~등록할 생각은 없지만~~ 한번 가격 구경이라도 해볼려고 들어갔는데 나오질 않는다.  
![getoff](/images/applemusic/getaway.png)
~~구경도 못하게 하네...~~  

아무튼 API를 통해 정보를 가져올려면 토큰이 필요한데, 이 토큰은 개발자 계정을 등록해야만 발급받을 수 있다.  



## 그럼 어떻게 정보를 가져오는가?
원래대로라면 연간 99달러라는~~무친~~가격을 지불하고 사용하는게 맞는데,  
친구가 ~~신비한방법~~으로 토큰을 ~~얻어서~~ 사용해 볼 수 있었다.  

덕분에 얻게된 API 주소로 정보를 가져와보면
![API_GET](/images/applemusic/api_get.png)
###### 불러온 API를 보기 좋게 표시하면 이렇다.  
다음과 같이 다양한 정보가 나온다.  
여기서 내가 필요한 정보만 가져오면 된다.  



## 필요한 정보 가져오기
위에서 불러온 API에서 나온 정보를 용도에 맞게 사용할려면 정보를 가공해야한다.  
나는 웹사이트에 제목, 가수, 앨범커버, 미리듣기, 자세한 정보 보기 기능을 구현할려고 했다.  
따라서 이와 관련된 데이터를 추출해내야한다.  

불러온 값을 다시보면  
제목은 name, 가수는 artistName, 앨범커버는 artwork, 미리듣기는 preview, 애플뮤직으로 이어지는 링크는 url에 있는것을 확인할 수 있다.  
나는 이 정보를 JSON 형식으로 불러왔기 때문에 배열 방식으로 접근하면 원하는 정보만을 추출할 수 있다.  

먼저 원하는 정보가 제대로 추출되는지 확인해보자.  
fetch를 이용하여 API 정보를 불러온다.  
console.log() 를 이용하면 웹페이지의 개발자도구 콘솔에 로그를 남길 수 있다.  
이를 활용하여 코드를 작성해보면

###### fetch("https://simple-proxy.taein.workers.dev/?destination=https://yuntae.in/api/music/recent/noa")
######        .then(res => res.json())
######           console.log(data.data[0].attributes.artistName);
######           console.log(data.data[0].attributes.name);
######           console.log(data.data[0].attributes.artwork.url);
######           console.log(data.data[0].attributes.previews[0].url);
######           console.log(data.data[0].attributes.url);
~~블로그이슈로 코드박스 대신 글로 처리했다. 양해좀;~~  

웹사이트로 들어가서 개발자 도구의 콘솔을 확인해보면
![get_info](/images/applemusic/need_info.png)
원하는 정보들만 뜨는것을 확인할 수 있다.  



## 이제 웹사이트 구현을 해보자
필요한 정보들을 가져왔으면 이제 내가 원하는 모양으로 웹사이트를 구현하면 된다.  
먼저 html로 가져오기 위해 각각의 정보를 변수로 선언하였다.

###### var artistName = data.data[0].attributes.artistName;
###### var songName = data.data[0].attributes.name;
###### var albumCover = data.data[0].attributes.artwork.url;
###### var preview = data.data[0].attributes.previews[0].url;
###### var moreInfo = data.data[0].attributes.url;
~~노션으로 이사갈까~~  

그런다음 html에 id형식으로 넘겨받아서 출력해줬다.  
이때 앨범커버는 API에서 보낸 링크에 있는 {w}x{h}값을 지정헤줘야 정상적으로 이미지를 출력할 수 있다.  
~~안하면 404에러가 뜬다. 보기 싫죠?~~

JavaScript에서 구현한 내용은 거의 대부분 정보를 가져오고 가공하는 내용이였다.  



## 디자인
나의 디자인은 심플하면서 필요한 정보만 전달하는게 목표였다.  
결과물은 다음과 같다.
![web_design](/images/applemusic/final_design.png)
그냥... 뭐... 딱히 설명할게 없는 디자인인듯 하다.

모바일에서 접속하는것을 감안하여 `@media (max-width: 768px)`를 이용해 모바일 버전에 맞게 대응시켰다.
![mobile_design](/images/applemusic/mobile_design.png)

다크 모드를 중요시 여기는(?) 사람이라 JavaScript에 `const prefersDarkScheme = window.matchMedia("(prefers-color-scheme: dark)");`를 이용하여
기기의 설정값을 불러와 현재 설정된 화면 모드에 따라 다크 모드와 라이트 모드를 변환하는 기능도 추가하였다.  



## 다 만들었으면 웹사이트를 배포해보자.
열심히 만들어놓고 냅두면 아무 쓸모가 없다.  
일단 GitHub에 먼저 올린 후 vercel을 이용하여 배포하였다.  
GitHub에도 웹페이지를 배포할 수 있는 기능이 있지만, 나는 구매해놓은 도메인을 이용하기 위해 vercel을 이용하였다.

GitHub에 새로운 Repositories를 하나 생성하고 파일을 push 한다.  
그 후 vercel에 들어가면 GitHub에 올린 Repositories를 선택하여 추가하면 vercel의 주소를 가진 웹을 배포할 수 있다.  
https://witch.work/posts/cloudflare-make-subdomain 이 글을 참고하여 내가 구매한 도메인을 연결하였다.  
작업이 완료되면 연결한 도메인 주소인 https://music.noa.kim 으으로 들어가보면 결과를 확인할 수 있다.  



## 결론
~~그냥 시험기간에 공부하기싫어서 한 뻘짓2~~  
사실 요즘 학교에서 파이썬만 주로 하다보니까 기존에 익혀둔 웹프로그래밍을 다 잊어버리게 될뻔했는데, 이 기회에 다시 써봐서 덕분에 공부를 할 수 있었다. ~~시험공부나 하지~~
솔직히 이번 코딩은 chatGPT의 도움도 많이 받았는데, chatGPT가 정말 코드를 잘 작성해준다. ~~개발자 없어질듯~~  
그리고 이참에 JavaScript도 더 공부를 해봐야할꺼같다.

시놀로지 소프트웨어 파일을 이용한 헤놀로지 나스도 집에 남아있는 미니PC를 이용하여 구축해봤는데 ~~이거 또한 시험기간 뻘짓~~,  
시간이 되면 이것도 글로 한번 써봐야겠다.  


https://github.com/yesa0513/Now_Playing
