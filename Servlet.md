
# 定义

Servlet这个接口起到的是一个「规范」的作用


# 

  public interface Servlet {

      public void init(ServletConfig config) throws ServletException;

      public ServletConfig getServletConfig();

      public void service(ServletRequest req, ServletResponse res)
              throws ServletException, IOException;

      public String getServletInfo();

      public void destroy();
  }


