# Scala 개발을 위한 Eclipse 설정  
Scala 개발을 위해 사용할 수 있는 옵션은 여러가지가 있지만 보통은 이 세가지 선에서 하지 않을까싶다.  
- IntelliJ + Scala plugin(가장 인기 있는 옵션)
- Scala-IDE(Scala 재단에서 직접 케어하는 IDE)
- Visual Studio Code + Scala plugin(생태계 발전 중)

나의 경우는 첫 번째 옵션인 IntelliJ를 사용했지만, 여기에 글을 쓴 것처럼, eclipse로 갈아타게 되었다. 보통은 이해 못할 행동이지만, Scala 툴로서 IntelliJ역시 완벽하지 않으며, IntelliJ에서도 여전히 버그가 많다. 일단 내가 발견한 버그는 아래와 같다.  
- [val과 underscore를 사용했을 때 에러로 판단 못함](https://stackoverflow.com/questions/53458586/scala-error-code-is-not-judged-as-a-error-code-in-intellij-is-it-intellij-bug)

그래서 Scala-IDE로 갈아타려했지만, JDK11에서는 정상동작하지 않았다. 2018년 11월 기준으로, Scala-IDE는 Eclipse-oxygen을 기반으로 만들어졌는데, 이 버전이 JDK8 이상에서는 제대로 동작하지 않았던 이슈가 보고되었다. JDK9에서 동작하지 않아 버그가 수정되었지만, 더 높은 JDK에서는 이슈가 다시 발생되어서 JDK11과 Scala-IDE의 공존은 힘들 것 같아서, 직접 Eclipse 최신판 + Scala plugin을 설치하여 사용해보기로 했다.

## 선 요구사항
JDK가 설치되어야 하고, 환경변수에 JAVA_HOME을 설정한 상태여야 한다. 나의 경우는 [OpenJDK 11](https://jdk.java.net/11/)을 설치하여 사용중이다. Scala계의 Maven같은 [SBT](https://www.scala-sbt.org/)도 미리 설치하자.

## 과정
### 1. [다운로드](https://www.eclipse.org/downloads/)
Eclipse를 다운받고 설치하거나, 다른 Mirror에서 특정 용도의 eclipse의 zip파일을 직접 된다. 나의 경우는 [이 링크](https://www.eclipse.org/downloads/packages/)를 통해 Java EE용 eclipse를 다운받았다.

### 2. Scala Plugin 설치하기
이클립스의 상단 메뉴 Help > Eclipse MarketPlace로 가서 Scala를 검색하고 Scala IDE X.X.X를 설치한다.  
![Scala plugin](../assets/images/ide.png)

설치할 옵션은 여러가지가 있지만, Scala에서 인기 있는 Framework를 나도 사용할 가능성이 높으므로 추가로 다 설치하게 되어 모두 체크하고 Confirm > accept를 통해 설치하기 되었다.
![install](../assets/images/install.png)

이렇게 되면 JDK11에서 동작하는 Scala 설정은 완료된 것이다.  

### 3. 편리한 사용을 위한 설정들
설치는 끝났으나, 사용하려니 Scala 메뉴는 보이지도 않을뿐더러 불편하다. 아래의 과정을 통해 편의를 위한 설정을 하고 Scala를 즐겨보자

#### Scala 메뉴 추가하기
메뉴를 추가하지 않으면 기본적으로 Scala는 보이지 않는다. 그래서 Scala 파일 생성을 어디서 해야할 지 작성자도 헤맸다.

상단 메뉴의 Window > Perspective > Customize Perspective로 가자

첫번째 탭에서 File > New에서 scala 관련된 것들은 전부 체크해주자
![filenew](../assets/images/filenew.png)
상단탭에서 Shortcuts으로 가서 Scala Wizard를 체크하자
![shortcuts](../assets/images/shortcut.png)

작성자의 경우는 scala이외의 다른 언어는 eclipse에서 사용하지 않을거라서 JavaEE등은 체크해제하고 내 입맛대로 추가/제거했다.

#### Encoding
비영어권 국가의 사람으로서 제일 귀찮은 점이라면 바로 이 Encoding 설정이다.
상단메뉴의 Preferences에서 encoding을 검색하여 Default를 전부 UTF-8로 바꾸자.  
![encoding1](../assets/images/encoding1.png)
![encoding2](../assets/images/encoding2.png)
![encoding3](../assets/images/encoding3.png)
![encoding4](../assets/images/encoding4.png)

검색되지 않는 녀석도 바꿔준다. MS949는 아예 싹을 잘라버려야한다(OMG...)
![encoding5](../assets/images/encoding5.png)

글꼴도 한글 지원하는 폰트로 바꾸는게 안전하다. 폰트는 한글 지원 폰트 중에서 마음에 드는 것을 선택하자
![font](../assets/images/font.png)

이 정도 했으면 충분하다

#### Terminal 설정
개발하다보면 terminal쓸 일이 많기도 하고, 더군다나 eclipse 내부에서 sbt를 이용할 수 있는 여건이 안되기때문에 Terminal 설정을 추가한다. 기본으로 Terminal Plugin이 깔려있지만 아래와 같이 말을 더듬고 있기에 빡쳐서 지우고 다른 것을 깔았다.
![broken-terminal](../assets/images/terminal-break.png)


#### 단축키 설정과 유용한 단축키
단축키 설정은 상단 메뉴 Preferences에서 할 수 있다.
![keys](../assets/images/keys.png)

##### 추가한 단축키
Preference : `Ctrl` + `Shift` + `P`

##### 기존 단축키 중 알아두면 좋을 것

터미널 열기 : `Ctrl` + `Alt` + `Shift` + `T`

#### Git 통합


