# 04장 변수
## 4.1 변수란 무엇인가? 왜 필요한가? 
컴퓨터는 CPU를 사용해 연산하고, 메모리를 사용해 데이터를 기억한다. 

컴퓨터의 메모리는 메모리 셀의 집합이며 메모리 셀은 8bit (1byte)이고, 컴퓨터는 1바이트 단위로 데이터를 저장하거나 읽어들인다.

<p align='center'>
<img src='https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a26aee94-71e8-4082-a453-d9a55121a597/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220310%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220310T020211Z&X-Amz-Expires=86400&X-Amz-Signature=cd0a56396d4d17c713cf0ab32e124ebd26450d2e99c3555acc58fc8fd97f08fa&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject' width='200px'></p>

그렇다면 자바스크립트에서 10 + 20 을 수행하면 어떻게 될까? 

컴퓨터는 10과 20이라는 값을 임의의 메모리 주소에 할당하고 CPU는 이 값을 읽어 + 연산자를 통해 30이라는 결과 값을 다시 임의의 메모리 주소에 저장한다.