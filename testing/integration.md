# Integration testing

## Motivation

Verifying correct interoperability of a collection of units is called integration testing.

## Goals

* Identify appropriate integration tests
* Learn about the role of integration testing and how it relates to unit and UI testing
* Gain experience writing integration tests

## Steps

Given:

```
import javax.servlet.http.*;
import org.eclipse.jetty.server.Server;
import org.eclipse.jetty.servlet.*;

public class Main extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
      // ...
  }

  public static void main(String[] args) {
    Server server = new Server(8080);
    ServletContextHandler context = new ServletContextHandler(ServletContextHandler.SESSIONS);
    context.setContextPath("/");
    server.setHandler(context);
    context.addServlet(new ServletHolder(new Main()),"/*");
    server.start();
    server.join();
  }
}
```

Modify doGet to:
* returns a string, eg "foo"

Write an integration test asserting the output
