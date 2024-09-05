- GET请求和POST请求有什么区别？

  - get请求发送数据的时候，数据会挂在URI的后面，并且在URI后面添加一个“?”，"?"后面是数据。这样会导致发送的数据回显在浏览器的地址栏上。（get请求在“请求行”上发送数据）
    - http://localhost:8080/servlet05/getServlet?username=zhangsan&userpwd=1111
  - post请求发送数据的时候，在请求体当中发送。不会回显到浏览器的地址栏上。也就是说post发送的数据，在浏览器地址栏上看不到。（post在“请求体”当中发送数据）
  - get请求只能发送普通的字符串。并且发送的字符串长度有限制，不同的浏览器限制不同。这个没有明确的规范。
  - get请求无法发送大数据量。
  - post请求可以发送任何类型的数据，包括普通字符串，流媒体等信息：视频、声音、图片。
  - post请求可以发送大数据量，理论上没有长度限制。
  - get请求在W3C中是这样说的：get请求比较适合从服务器端获取数据。
  - post请求在W3C中是这样说的：post请求比较适合向服务器端传送数据。
  - get请求是安全的。get请求是绝对安全的。为什么？因为get请求只是为了从服务器上获取数据。不会对服务器造成威胁。（get本身是安全的，你不要用错了。用错了之后又冤枉人家get不安全，你这样不好（太坏了），那是你自己的问题，不是get请求的问题。）
  - post请求是危险的。为什么？因为post请求是向服务器提交数据，如果这些数据通过后门的方式进入到服务器当中，服务器是很危险的。另外post是为了提交数据，所以一般情况下拦截请求的时候，大部分会选择拦截（监听）post请求。
  - get请求支持缓存。
    - https://n.sinaimg.cn/finance/590/w240h350/20211101/b40c-b425eb67cabc342ff5b9dc018b4b00cc.jpg
    - 任何一个get请求最终的“响应结果”都会被浏览器缓存起来。在浏览器缓存当中：
      - 一个get请求的路径a  对应  一个资源。
      - 一个get请求的路径b  对应  一个资源。
      - 一个get请求的路径c  对应  一个资源。
      - ......
    - 实际上，你只要发送get请求，浏览器做的第一件事都是先从本地浏览器缓存中找，找不到的时候才会去服务器上获取。这种缓存机制目的是为了提高用户的体验。
    - 有没有这样一个需求：我们不希望get请求走缓存，怎么办？怎么避免走缓存？我希望每一次这个get请求都去服务器上找资源，我不想从本地浏览器的缓存中取。
      - 只要每一次get请求的请求路径不同即可。
      - https://n.sinaimg.cn/finance/590/w240h350/20211101/7cabc342ff5b9dc018b4b00cc.jpg?t=789789787897898
      - https://n.sinaimg.cn/finance/590/w240h350/20211101/7cabc342ff5b9dc018b4b00cc.jpg?t=789789787897899
      - https://n.sinaimg.cn/finance/590/w240h350/20211101/7cabc342ff5b9dc018b4b00cc.jpg?t=系统毫秒数
      - 怎么解决？可以在路径的后面添加一个每时每刻都在变化的“时间戳”，这样，每一次的请求路径都不一样，浏览器就不走缓存了。
  - post请求不支持缓存。（POST是用来修改服务器端的资源的。）
    - post请求之后，服务器“响应的结果”不会被浏览器缓存起来。因为这个缓存没有意义。

- GET请求和POST请求如何选择，什么时候使用GET请求，什么时候使用POST请求？

  - 怎么选择GET请求和POST请求呢？衡量标准是什么呢？你这个请求是想获取服务器端的数据，还是想向服务器发送数据。如果你是想从服务器上获取资源，建议使用GET请求，如果你这个请求是为了向服务器提交数据，建议使用POST请求。
  - 大部分的form表单提交，都是post方式，因为form表单中要填写大量的数据，这些数据是收集用户的信息，一般是需要传给服务器，服务器将这些数据保存/修改等。
  - 如果表单中有敏感信息，还是建议适用post请求，因为get请求会回显敏感信息到浏览器地址栏上。（例如：密码信息）
  - 做文件上传，一定是post请求。要传的数据不是普通文本。
  - 其他情况都可以使用get请求。

- 不管你是get请求还是post请求，发送的请求数据格式是完全相同的，只不过位置不同，格式都是统一的：

  - name=value&name=value&name=value&name=value
  - name是什么？
    - 以form表单为例：form表单中input标签的name。
  - value是什么？
    - 以form表单为例：form表单中input标签的value。



### session

- Http是一种无状态的协议。也就是说Tomcat不知道浏览器的关闭状态，session是通过session超时机制进行销毁。（例子：登录超时，请重新登录）。

- 1. **session怎么获取？**

  ```html
  HttpSession session = request.getSession();
  从服务器中获取当前的session对象。
  如果没有获取到任何session对象，则新建。
  
  Httpsession session = request.getSession(false);
  从服务器中获取当前session对象，
  如果获取不到session，则不会新建，返回一个null
  ```

注：当关闭浏览器后，浏览器中保存的sessionid会被清空；再次启动时，浏览器缓存中没有保存的sessionid，自然找不到服务器中对应的session对象，session对象找不到等同于会话结束。

- 2. **session对象什么时候被销毁？**

     —种销毁:是超时销毁。

     —种销毁:是手动销毁。

- 3. **session的实现原理：**

  - JSESSIONID=xxxxxx  这个是以Cookie的形式保存在浏览器的内存中的。浏览器只要关闭。这个cookie就没有了。
  - session列表是一个Map，map的key是sessionid，map的value是session对象。
  - 用户第一次请求，服务器生成session对象，同时生成id，将id发送给浏览器。
  - 用户第二次请求，自动将浏览器内存中的id发送给服务器，服务器根据id查找session对象。
  - 关闭浏览器，内存消失，cookie消失，sessionid消失，会话等同于结束。

- 4. **Cookie禁用了，session还能找到吗？**

  - cookie禁用是什么意思？服务器正常发送cookie给浏览器，但是浏览器不要了。拒收了。并不是服务器不发了。
  - 找不到了。每一次请求都会获取到新的session对象。
  - cookie禁用了，session机制还能实现吗？
    - 可以。需要使用*URL重写机制。*
    - http://localhost:8080/servlet12/test/session;jsessionid=19D1C99560DCBF84839FA43D58F56E16
    - URL重写机制会提高开发者的成本。开发人员在编写任何请求路径的时候，后面都要添加一个sessionid，给开发带来了很大的难度，很大的成本。所以大部分的网站都是这样设计的：你要是禁用cookie，你就别用了。

- 销毁session对象：

  - ```java
    session.invalidate();
    ```



## Cookie

- session的实现原理中，每一个session对象都会关联一个sessionid，例如：

  - JSESSIONID=41C481F0224664BDB28E95081D23D5B8
  - 以上的这个键值对数据其实就是cookie对象。
  - 对于session关联的cookie来说，这个cookie是被保存在浏览器的“运行内存”当中。
  - 只要浏览器不关闭，用户再次发送请求的时候，会自动将运行内存中的cookie发送给服务器。
  - 例如，这个Cookie: JSESSIONID=41C481F0224664BDB28E95081D23D5B8就会再次发送给服务器。
  - 服务器就是根据41C481F0224664BDB28E95081D23D5B8这个值来找到对应的session对象的。

- cookie怎么生成？cookie保存在什么地方？cookie有啥用？浏览器什么时候会发送cookie，发送哪些cookie给服务器？？？？？？？

- cookie最终是保存在浏览器客户端上的。

  - 可以保存在运行内存中。（浏览器只要关闭cookie就消失了。）
  - 也可以保存在硬盘文件中。（永久保存。）

- cookie有啥用呢？

  - cookie和session机制其实都是为了**保存会话的状态。**
  - cookie是将会话的状态保存在*浏览器客户端*上。（cookie数据存储在浏览器客户端上的。）
  - session是将会话的状态保存在*服务器端*上。（session对象是存储在服务器上。）
  - 为什么要有cookie和session机制呢？因为HTTP协议是无状态 无连接协议。

- cookie的经典案例

  - 京东商城，在未登录的情况下，向购物车中放几件商品。然后关闭商城，再次打开浏览器，访问京东商城的时候，购物车中的商品还在，这是怎么做的？我没有登录，为什么购物车中还有商品呢？
    - 将购物车中的商品编号放到cookie当中，cookie保存在硬盘文件当中。这样即使关闭浏览器。硬盘上的cookie还在。下一次再打开京东商城的时候，查看购物车的时候，会自动读取本地硬盘中存储的cookie，拿到商品编号，动态展示购物车中的商品。
      - 京东存储购物车中商品的cookie可能是这样的：productIds=xxxxx,yyyy,zzz,kkkk
      - 注意：cookie如果清除掉，购物车中的商品就消失了。
  - 126邮箱中有一个功能：十天内免登录
    - 这个功能也是需要cookie来实现的。
    - 怎么实现的呢？
      - 用户输入正确的用户名和密码，并且同时选择十天内免登录。登录成功后。浏览器客户端会保存一个cookie，这个cookie中保存了用户名和密码等信息，这个cookie是保存在硬盘文件当中的，十天有效。在十天内用户再次访问126的时候，浏览器自动提交126的关联的cookie给服务器，服务器接收到cookie之后，获取用户名和密码，验证，通过之后，自动登录成功。
      - 怎么让cookie失效？
        - 十天过后自动失效。
        - 或者改密码。
        - 或者在客户端浏览器上清除cookie。

- cookie机制和session机制其实都不属于java中的机制，实际上cookie机制和session机制都是HTTP协议的一部分。php开发中也有cookie和session机制，只要是你是做web开发，不管是什么编程语言，cookie和session机制都是需要的。

- HTTP协议中规定：任何一个cookie都是由name和value组成的。name和value都是字符串类型的。

- 在java的servlet中，对cookie提供了哪些支持呢？

  - 提供了一个Cookie类来专门表示cookie数据。jakarta.servlet.http.Cookie;
  - java程序怎么把cookie数据发送给浏览器呢？response.addCookie(cookie);

- 在HTTP协议中是这样规定的：当浏览器发送请求的时候，会自动携带该path下的cookie数据给服务器。（URL。）

- 关于cookie的有效时间

  - 怎么用java设置cookie的有效时间
    - cookie.setMaxAge(60 * 60); 设置cookie在一小时之后失效。
  - 没有设置有效时间：默认保存在浏览器的运行内存中，浏览器关闭则cookie消失。
  - 只要设置cookie的有效时间 > 0，这个cookie一定会存储到硬盘文件当中。
  - 设置cookie的有效时间 = 0 呢？
    - cookie被删除，同名cookie被删除。
  - 设置cookie的有效时间 < 0 呢？
    - 保存在运行内存中。和不设置一样。

- 关于cookie的path，cookie关联的路径：

  - 假设现在发送的请求路径是“http://localhost:8080/servlet13/cookie/generate”生成的cookie，如果cookie没有设置path，默认的path是什么？
    - 默认的path是：http://localhost:8080/servlet13/cookie 以及它的子路径。
    - 也就是说，以后只要浏览器的请求路径是http://localhost:8080/servlet13/cookie 这个路径以及这个路径下的子路径，cookie都会被发送到服务器。
  - 手动设置cookie的path
    - cookie.setPath(“/servlet13”); 表示只要是这个servlet13项目的请求路径，都会提交这个cookie给服务器。

- 浏览器发送cookie给服务器了，服务器中的java程序怎么接收？

  - ```java
    Cookie[] cookies = request.getCookies(); // 这个方法可能返回null
    if(cookies != null){
        for(Cookie cookie : cookies){
            // 获取cookie的name
            String name = cookie.getName();
            // 获取cookie的value
            String value = cookie.getValue();
        }
    }
    
    ```

- 使用cookie实现一下十天内免登录功能。

  - 先实现登录功能
    - 登录成功
      - 跳转到部门列表页面
    - 登录失败
      - 跳转到登录失败页面
  - 修改前端页面
    - 在登录页面给一个复选框，复选框后面给一句话：十天内免登录。
    - 用户选择了复选框：表示要支持十天内免登录。
    - 用户没有选择复选框：表示用户不想使用十天内免登录功能。
  - 修改Servlet中的login方法
    - 如果用户登录成功了，并且用户登录时选择了十天内免登录功能，这个时候应该在Servlet的login方法中创建cookie，用来存储用户名和密码，并且设置路径，设置有效期，将cookie响应给浏览器。（浏览器将其自动保存在硬盘文件当中10天）
  - 用户再次访问该网站的时候，访问这个网站的首页的时候，有两个走向:
    - 要么跳转到部门列表页面
    - 要么跳转到登录页面
    - 以上分别有两个走向，这显然是需要编写java程序进行控制的。



**一：什么是Cookie和Session？**

Cookie是服务器发送到客户浏览器，并保存在本地的数据，它会在浏览器下次访问服务器时，被一块带过去；通常会用来告知服务器该请求是否来自同一个浏览器，记录用户是否登录；

Session是一个会话级别的数据存储，数据保存在服务器端，在整个会话过程中，保存在session中的数据都会存在；当服务器与浏览器的会话结束或者浏览器关闭时，数据就会失效；

**二：Cookie和Session的区别？**

存储位置不同：cookie保存在客户端浏览器，session保存在服务器端；

存取方式不同：cookie只能保存ASCII（是基于拉丁字母的一套电脑编码系统），session能保存任意数据类型；

有效期不同：cookie中的数据可以长时间存在，session中的数据在会话结束或者浏览器关闭时失效；

数据安全性不同：因为cookie是保存在浏览器中的，数据安全性相对较差；session是存储在服务器端的，安全性相对较高；

存储大小不同：cookie一般保存的数据大小不会超过4K；而session理论上来说没有限制；

**三：为什么需要Cookie和Session，它们的联系是什么？**

因为浏览器是无状态的（http协议是无状态），浏览器并不知道是谁在跟服务器联系，这个时候就需要一个机制来告诉服务器，是谁在操作浏览器，操作人是否登录等，要实现这个机制，就需要cookie和session配合完成；

当用户第一次访问服务器时，服务器会根据用户提交的信息生成session，然后给浏览器返回一个sessionId，浏览器接收到这个sessionId后，保存在cookie中，同时cookie记录此sessionId属于哪个域名；

用户再次访问服务器时，请求会自动判断此域名下是否用cookie信息，如果存在cookie，则将cookie发送给服务器，服务器解析cookie拿到sessionId，然后根据sessionId查询session信息，如果能找到session信息，则可以执行后面的操作，如果没有则说明没有登录或者登录已经失效；

sessionId是联系cookie和session的桥梁，大部分系统也是通过这个来判断用户是否登录；

**四：如果浏览器禁止使用cookie，那如何保证判断用户是否登录的机制正常执行？**

第一种方案，每次请求中都携带一个 SessionID 的参数，也可以 Post 的方式提交，也可以在请求的地址后面拼接 `xxx?SessionID=123456...`。

第二种方案，Token 机制。Token 机制多用于 App 客户端和服务器交互的模式，也可以用于 Web 端做用户状态管理。

Token 的意思是“令牌”，是服务端生成的一串字符串，作为客户端进行请求的一个标识。Token 机制和 Cookie 和 Session 的使用机制比较类似。

当用户第一次登录后，服务器根据提交的用户信息生成一个 Token，响应时将 Token 返回给客户端，以后客户端只需带上这个 Token 前来请求数据即可，无需再次登录验证。





## Filter过滤器

- Filter是什么，有什么用，执行原理是什么？

  - Filter是过滤器。
  - Filter可以在Servlet这个目标程序执行之前添加代码。也可以在目标Servlet执行之后添加代码。之前之后都可以添加过滤规则。
  - 一般情况下，都是在过滤器当中编写公共代码。

- 一个过滤器怎么写呢？

  - 第一步：编写一个Java类实现一个接口：jarkata.servlet.Filter。并且实现这个接口当中所有的方法。

    - init方法：在Filter对象第一次被创建之后调用，并且只调用一次。
    - doFilter方法：只要用户发送一次请求，则执行一次。发送N次请求，则执行N次。在这个方法中编写过滤规则。
    - destroy方法：在Filter对象被释放/销毁之前调用，并且只调用一次。

  - 第二步：在web.xml文件中对Filter进行配置。这个配置和Servlet很像。

    - ```
      <filter>
          <filter-name>filter2</filter-name>
          <filter-class>com.bjpowernode.javaweb.servlet.Filter2</filter-class>
      </filter>
      <filter-mapping>
          <filter-name>filter2</filter-name>
          <url-pattern>*.do</url-pattern>
      </filter-mapping>
      ```

    - 或者使用注解：@WebFilter({"*.do"})

- 注意：

  - Servlet对象默认情况下，在服务器启动的时候是不会新建对象的。
  - Filter对象默认情况下，在服务器启动的时候会新建对象。
  - Servlet是单例的。Filter也是单例的。（单实例。）

- 目标Servlet是否执行，取决于两个条件：

  - 第一：在过滤器当中是否编写了：chain.doFilter(request, response); 代码。
  - 第二：用户发送的请求路径是否和Servlet的请求路径一致。

- **chain.doFilter(request, response); 这行代码的作用：**

  - 执行下一个过滤器，如果下面没有过滤器了，执行最终的Servlet。

- 注意：Filter的优先级，天生的就比Servlet优先级高。

  - /a.do 对应一个Filter，也对应一个Servlet。那么一定是先执行Filter，然后再执行Servlet。

- 关于Filter的配置路径：

  - /a.do、/b.do、/dept/save。这些配置方式都是精确匹配。
  - /* 匹配所有路径。
  - *.do 后缀匹配。不要以 / 开始
  - /dept/*  前缀匹配。

- 在web.xml文件中进行配置的时候，Filter的执行顺序是什么？

  - 依靠filter-mapping标签的配置位置，越靠上优先级越高。

- 过滤器的调用顺序，遵循栈数据结构。

- 使用@WebFilter的时候，Filter的执行顺序是怎样的呢？

  - 执行顺序是：比较Filter这个类名。
  - 比如：FilterA和FilterB，则先执行FilterA。
  - 比如：Filter1和Filter2，则先执行Filter1.

- Filter的生命周期？

  - 和Servlet对象生命周期一致。
  - 唯一的区别：Filter默认情况下，在服务器启动阶段就实例化。Servlet不会。

- Filter过滤器这里有一个设计模式：

  - 责任链设计模式。
  - 过滤器最大的优点：
    - 在程序编译阶段不会确定调用顺序。因为Filter的调用顺序是配置到web.xml文件中的，只要修改web.xml配置文件中filter-mapping的顺序就可以调整Filter的执行顺序。显然Filter的执行顺序是在程序运行阶段动态组合的。那么这种设计模式被称为责任链设计模式。
  - 责任链设计模式最大的核心思想：
    - 在程序运行阶段，动态的组合程序的调用顺序。

- 使用过滤器改造OA项目。





# Listener监听器

- 什么是监听器？

  - 监听器是Servlet规范中的一员。就像Filter一样。Filter也是Servlet规范中的一员。
  - 在Servlet中，所有的监听器接口都是以“Listener”结尾。

- 监听器有什么用？

  - 监听器实际上是Servlet规范留给我们javaweb程序员的特殊时机。
  - 特殊的时刻如果想执行这段代码，你需要想到使用对应的监听器。

- Servlet规范中提供了哪些监听器？

  - jakarta.servlet包下：
    - ServletContextListener
    - ServletContextAttributeListener
    - ServletRequestListener
    - ServletRequestAttributeListener
  - jakarta.servlet.http包下：
    - HttpSessionListener
    - HttpSessionAttributeListener
      - 该监听器需要使用@WebListener注解进行标注。
      - 该监听器监听的是什么？是session域中数据的变化。只要数据变化，则执行相应的方法。主要监测点在session域对象上。
    - HttpSessionBindingListener
      - 该监听器不需要使用@WebListener进行标注。
      - 假设User类实现了该监听器，那么User对象在被放入session的时候触发bind事件，User对象从session中删除的时候，触发unbind事件。
      - 假设Customer类没有实现该监听器，那么Customer对象放入session或者从session删除的时候，不会触发bind和unbind事件。
    - HttpSessionIdListener
      - session的id发生改变的时候，监听器中的唯一一个方法就会被调用。
    - HttpSessionActivationListener
      - 监听session对象的钝化和活化的。
      - 钝化：session对象从内存存储到硬盘文件。
      - 活化：从硬盘文件把session恢复到内存。

- 实现一个监听器的步骤：以ServletContextListener为例。

  - 第一步：编写一个类实现ServletContextListener接口。并且实现里面的方法。

    - ```
      void contextInitialized(ServletContextEvent event)
      void contextDestroyed(ServletContextEvent event)
      ```

  - 第二步：在web.xml文件中对ServletContextListener进行配置，如下：

    - ```
      <listener>
          <listener-class>com.bjpowernode.javaweb.listener.MyServletContextListener</listener-class>
      </listener>
      ```

    - 当然，第二步也可以不使用配置文件，也可以用注解，例如：@WebListener

- 注意：所有监听器中的方法都是不需要javaweb程序员调用的，由服务器来负责调用？什么时候被调用呢？

  - 当某个特殊的事件发生（特殊的事件发生其实就是某个时机到了。）之后，被web服务器自动调用。

- 思考一个业务场景：

  - 请编写一个功能，记录该网站实时的在线用户的个数。
  - 我们可以通过服务器端有没有分配session对象，因为一个session代表了一个用户。有一个session就代表有一个用户。如果你采用这种逻辑去实现的话，session有多少个，在线用户就有多少个。这种方式的话：HttpSessionListener够用了。session对象只要新建，则count++，然后将count存储到ServletContext域当中，在页面展示在线人数即可。
  - 业务发生改变了，只统计登录的用户的在线数量，这个该怎么办？
    - session.setAttribute("user", userObj); 
    - 用户登录的标志是什么？session中曾经存储过User类型的对象。那么这个时候可以让User类型的对象实现HttpSessionBindingListener监听器，只要User类型对象存储到session域中，则count++，然后将count++存储到ServletContext对象中。页面展示在线人数即可。

- 实现oa项目中当前登录在线的人数。

  - 什么代表着用户登录了？
    - session.setAttribute("user", userObj); User类型的对象只要往session中存储过，表示有新用户登录。
  - 什么代表着用户退出了？
    - session.removeAttribute("user"); User类型的对象从session域中移除了。
    - 或者有可能是session销毁了。（session超时）































































































