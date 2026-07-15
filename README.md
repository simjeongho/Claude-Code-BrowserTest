# 검증하고자 한 것

1. Skill 제작 -> Claude Design을 통해 만든 HTML을 Claude Code Desktop 앱에서 화면을 보면서 요구 사항 정의를 할 수 있는가?


# 결과

1. Claude Desktop 앱이 업데이트가 되어 Claude Code 내에서 브라우저를 띄울 수 있음(로컬 웹 서버)
2. Claude Cowork보다 Claude Code 내에서 요구 사항 정의를 하는 것이 더 편해보임


# 시연 이미지

1. 요구 사항 진행 스킬 실행

<img width="1006" height="954" alt="image" src="https://github.com/user-attachments/assets/f112ab63-e5fe-46c5-a999-f58e104d93df" />


2. 화면을 보면서 진행하고 싶다고 요청
3. file:: 경로는 Claude Browser에서 보안 상 이유로 막아서 로컬 웹 서버를 띄워서 그 주소로 열게 함 -> 스킬 명시 필요

<img width="899" height="862" alt="image" src="https://github.com/user-attachments/assets/dc0865fa-56e8-4d3e-864f-49a46c7e41b5" />

4. 로컬 웹 서버에 띄우자 미리보기 화면을 보면서 클로드 코드 데스크 탑 앱에서 요구 사항 정의 가능

<img width="1605" height="918" alt="image" src="https://github.com/user-attachments/assets/a748503e-f27d-4b67-9b68-a7fdbfad4172" />

5. 진행 시연

<img width="1910" height="963" alt="image" src="https://github.com/user-attachments/assets/d487fd84-0b46-4e86-8a93-3269eb1e48a6" />

6. 브라우저에서 띄우는 거라 html을 수정하면 바로바로 Claude 브라우저에 수정 사항 반영하는 것도 가능할 듯

<img width="1906" height="995" alt="image" src="https://github.com/user-attachments/assets/adc038e7-4994-472c-800f-046709a5ce19" />


# 버전 정보

1. `Claude Desktop` : `v1.20186.7` -> 7월 14일 업데이트
2. `Claude Desktop Code` 에서 브라우저 기능을 지원한 시점은 26.07.06 ~ 10
3. Browser 패널을 포함한 패널 레이아웃은 `Claude Desktop` `v1.2581.0` 이상 필요. 현재 버전 `v1.20186.7` 
4. `Claude Desktop Cowork`에서는 `Claude Desktop Code`에서의 브라우저 탭은 없는 것 확인
