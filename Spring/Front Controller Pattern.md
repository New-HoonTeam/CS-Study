# 프론트 컨트롤러 패턴 적용 전

![Untitled1](https://user-images.githubusercontent.com/86050295/208562970-30476166-a13e-44e9-9c65-2898cd885622.png)

- 공통 처리가 어렵다

```java
@WebServlet(name = "mvcMemberListServlet", urlPatterns = "/servlet-mvc/members")
public class MvcMemberListServlet extends HttpServlet {
		private MemberRepository memberRepository = MemberRepository.getInstance();
 
		@Override
		protected void service(HttpServletRequest request, HttpServletResponse response)
		 throws ServletException, IOException {
			 System.out.println("MvcMemberListServlet.service");
				 List<Member> members = memberRepository.findAll();
				 request.setAttribute("members", members);
				 String viewPath = "/WEB-INF/views/members.jsp";
				 RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
				 dispatcher.forward(request, response);
 }
}
```

# 프론트 컨트롤러 패턴 적용

![Untitled3](https://user-images.githubusercontent.com/86050295/208563174-4b96fb5a-ca98-4c97-9fca-807e9e4c6fa9.png)


# `FrontController` 패턴 특징

- 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음
→ 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨

- 공통 처리 로직 한 곳에서 관리 가능
→ 유지보수성 증대

![Untitled3](https://user-images.githubusercontent.com/86050295/208563174-4b96fb5a-ca98-4c97-9fca-807e9e4c6fa9.png)

# 스프링 웹 MVC와 프론트 컨트롤러

스프링 웹 MVC의 DispatcherServlet이 FrontController패턴으로 구현되어 있음

김영한님 강의 참고
