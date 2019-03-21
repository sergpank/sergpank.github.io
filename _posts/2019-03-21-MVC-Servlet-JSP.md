---
layout: post
title: MVC Example - Servlet + JSP
date: 2019-03-21
---  

## Here is the project structure:
![project structure][project] 

## And this is deployment structure:
![deployment structure][deployment]

## And here is all the code:
web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="4.0"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
    <servlet>
        <servlet-name>Beer Servlet</servlet-name>
        <servlet-class>com.example.web.BeerServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>Beer Servlet</servlet-name>
        <url-pattern>/SelectBeer.do</url-pattern>
    </servlet-mapping>

</web-app>
```

result.jsp
```html
<%@ page import="java.util.*" %>

<html>
    <body>
        <h1>Beer Recommendations JSP</h1>
        <p>
            <%
                List styles = (List)request.getAttribute("styles");
                Iterator it = styles.iterator();
                while(it.hasNext())
                {
                    out.print("<li>try: " + it.next());
                }
            %>
        </p>
    </body>
</html>
```

BeerServlet.java
```java
package com.example.web;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;
import java.util.List;

import com.example.model.BeerExpert;

public class BeerServlet extends HttpServlet
{
    public void doPost(HttpServletRequest request,
                       HttpServletResponse response) throws IOException, ServletException
    {
        String color = request.getParameter("color");
        List brands = new BeerExpert().getBrands(color);

        request.setAttribute("styles", brands);

        RequestDispatcher view = request.getRequestDispatcher("result.jsp");
        view.forward(request, response);
    }
}
```

BeerExpert.java
```java
package com.example.model;

import java.util.*;

public class BeerExpert
{
    public List getBrands(String color)
    {
        List brands = new ArrayList();

        switch(color)
        {
            case "amber":
            {
                brands.add("Jack Amber");
                brands.add("Red Moose");
                break;
            }
            default:
            {
                brands.add("Jail Pale Ale");
                brands.add("Gout Stout");
                break;
            }
        }
        return brands;
    }
}
```

form.html
```html
<html>
    <body>
        <h1>Beer selection page</h1>
        <form method="POST" action="SelectBeer.do">
            <p>Select beer characteristics</p>
            <p>Color:
                <select name="color" size="1">
                    <option value="light">light</option>
                    <option value="amber">amber</option>
                    <option value="brown">brown</option>
                    <option value="dark">dark</option>
                </select>
            </p>
            <input type="submit">
        </form>
    </body>
</html>
```

## Now we just need to compile our files and copy all the necessary files to TOMCAT/webapps/beer
```bash
javac -classpath ./lib/servlet-api.jar:classes:. -d classes src/com/example/model/BeerExpert.java
javac -d classes src/com/example/model/BeerExpert.java
```

[project]: /img/project_structure.png
[deployment]: /img/deployment_structure.png