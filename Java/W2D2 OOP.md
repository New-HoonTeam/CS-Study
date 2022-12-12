# 객체지향 프로그래밍(OOP)

- 프로그램을 객체로 구성하는 것.
- 프로그램의 거대화로 인해 분할하기 위해 탄생
- 가장 중요한 것은

### **객체**

- 객체는 다른 객체와 서로 협력하여 역할을 수행하는 것이다.
- 역할을 맡은 객체는 책임이 생긴다.
- 객체간에 메세지로 통신을 한다.
- 객체는 책임을 요구하지만 수행하는 방법은 관여하지 않는다.
(메세지로 명령하지만 어떤 방식으로 수행하는지는 관여 X)

객체는 type으로 구분한다 → 타입은 class로 만든다.

## 객체 지향의 특성

1. **캡슐화**
    - 객체는 스스로 동작할 수 있는 환경을 갖고 있어야 한다.
    - 외부에 의존 또는 침략 받지 않는다.
        - 완성도가 있다. 기능을 수행하는 단위로써 완전함을 갖는다.
        - 정보가 은닉되어 있다. 객체의 정보가 밖에서 접근하거나 밖에서 객체 내의 정보에 접근하지 못하게 한다.
    
    ```
    public class Human {
    	private Heart heart;
    	private Blood blood;
    	protected Gene gene;
    
    	Blood donation() {
    		return null;
    	}
    }
    ```
    
2. **상속**
    
    → 추상과 구체를 위해 사용된다. / 공통 기능 전달과는 조금 다르다.
    
    상위(부모, super, [추상])
    
    하위(자식, this, [구체])
    
    `원자 > 물질 > 생물 > 동물 > 포유류 > 사람 > 남자 > 짱구`
    
3. **추상화**
    - 추상화된 객체 : 추상체
    - 구체적인 객체 : 구상체
    - 객체간의 관계에서 상위에 있는 것은 항상 하위보다 추상적이어야 한다.
    
    **의미적 추상체**
    
    ```
    class Login() {
    	void login() {}
    }
    
    class KakaoLogin extends Login {
    	void login() {}
    }
    ```
    
    **추상 기능을 가진 객체**
    
    ```
    abstract class Login() {
    	abstract void login();
    }
    
    class KakaoLogin extends Login{
    	void kakao() {}
    	@Override void login() {}
    }
    ```
    
    객체 전부가 추상적일 때(interface)
    
    ```
    interface Login(){
    	void login();
    }
    
    class KakaoLogin implements Login{
    	void kakao() {}
    	@Override void login() {}
    }
    ```
    
4. **다형성 ( 다른 유형의 객체가 동일한 메세지에 다르게 반응하게 위함)**
    
    → type을 여러가지고 표현할 수 있다.
    
    ```
    classs KakaoLogin implements Login {
    	void kakao() {}
    	@Override void login() {}
    }
    
    interface Portal {
    	void portal();
    }
    
    class NaverLogin implements Login, Portal{
    	void naver() {}
    	@Override void login() {}
    	@Override void portal() {}
    }
    
    KakaoLogin k = new KakaoLogin();
    Login k = new KakaoLogin(); //이게 다형성
    ```
    
    다형성을 가진 객체는 본인 타입의 메서드만 호출 가능.(캡슐화)
    
    ```
    Login login = new Login();
    Login login - new kakaoLogin();
    Login login = new NaverLogin();
    login.login();
    login.naver(); //불가능.
    
    Portal portal = new NaverLogin();
    portal.portal();
    portal.login(); //불가능
    ```
