---
keyword : Java, Spring
class : Programming
---


## Controller와 HTTP Response Message

![이미지](C:\Users\User\iCloudDrive\iCloud~md~obsidian\g4dalcom\img\Response.png)

-   (1) 정적 웹페이지
    1.  static 폴더
        
        <aside> 🌐 [http://localhost:8080](http://localhost:8080)**/hello.html**
        
        </aside>
        
        resources/static**/hello.html**
        
    2.  Redirect
        
        <aside> 🌐 [http://localhost:8080](http://localhost:8080)**/hello/response/html/redirect**
        
        </aside>
        
        ```java
        @Controller
        @RequestMapping("**/hello/response**")
        public class HelloResponseController {
        		@GetMapping("**/html/redirect**")
            public String htmlFile() {
                return "**redirect:/hello.html**";
            }
        }
        ```
        
               
    3.  Template engine 에 View 전달
        
        <aside> 🌐 [http://localhost:8080](http://localhost:8080)**/hello/response/html/templates**
        
        </aside>
        
        ```java
        @GetMapping("/html/templates")
        public String htmlTemplates() {
            return **"hello"**;
        }
        ```
        
        타임리프 default 설정
        
        -   prefix: **classpath:/templates/**
        -   suffix: **.html**
        
        resources**/templates/**hello**.html**
        
                
    4.  @ResponseBody
        
        <aside> 🌐 [http://localhost:8080](http://localhost:8080)**/hello/response/html/templates**
        
        </aside>
        
        ```java
        @GetMapping("/body/html")
        @ResponseBody
        public String helloStringHTML() {
            return "<!DOCTYPE html>" +
                   "<html>" +
                       "<head><title>By @ResponseBody</title></head>" +
                       "<body> Hello, 정적 웹 페이지!!</body>" +
                   "</html>";
        }
        ```
        
        -   @ResponseBody
            -   View 를 사용하지 않고, HTTP Body 에 들어갈 String 을 직접 입력


-   (2) 동적 웹페이지
    
    <aside> 🌐 [http://localhost:8080](http://localhost:8080)**/hello/response/html/dynamic**
    
    </aside>
    
    ```java
    private static long visitCount = 0;
    
    @GetMapping("/html/dynamic")
    public String helloHtmlFile(Model model) {
        visitCount++;
        model.addAttribute("visits", visitCount);
        return "hello-visit";
    }
    ```
    
    -   View, Model 정보 → 타임리프에게 전달
    -   타임리프 처리방식
        -   View 정보
            
            -   "hello-visit" → resources**/templates/**hello-visit**.html**
            
            ```html
            <div>
              (방문자 수: <span th:text="${**visits**}"></span>)
            </div>
            ```
            
        -   Model 정보
            
            -   **visits**: 방문 횟수 (visitCount)
            -   예) 방문 횟수: **1,000,000** 번
            
            ```html
            <div>
              (방문자 수: <span>**1000000**</span>)
            </div>
            ```

-   (3) JSON 데이터
    1.  반환값: String
        
        <aside> 🌐 [http://localhost:8080](http://localhost:8080)**/hello/response/json/string**
        
        </aside>
        
        ```java
        @GetMapping("/json/string")
        @ResponseBody
        public String helloStringJson() {
            return "{\\"name\\":\\"BTS\\",\\"age\\":28}";
        }
        ```
        
    2.  반환값: String 외 자바 클래스
        
        <aside> 🌐 [http://localhost:8080](http://localhost:8080)**/hello/response/json/class**
        
        </aside>
        
        ```java
        @GetMapping("/json/class")
        @ResponseBody
        public Star helloJson() {
            return new Star("BTS", 28);
        }
        ```
        
        -   "자바 객체 → JSON 으로 변환" 은 스프링이 해 줌



## Controller와 HTTP Request Message

![이미지](C:\Users\User\iCloudDrive\iCloud~md~obsidian\g4dalcom\img\Request.png)

RequestParam과 ModelAttribute 의 기능은 동일하나

**Model은 객체를 만들어서 가져올 수 있음** 이 때, Model 객체(Star) 에 Setter를 반드시 만들어두어야 한다

**ModelAttribute를 사용할 때는 객체에 Setter를 확인하자**

**RequestBody는 Content type를 JSON으로 받을 때!**