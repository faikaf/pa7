7. Develop a data driven JSP application for user authentication.
db.jsp
<%@ page import="java.sql.*" %>
<%!
 public Connection getConnection() {
 Connection conn = null;
 try {
 Class.forName("com.mysql.cj.jdbc.Driver");
 conn =
DriverManager.getConnection("jdbc:mysql://localhost:3306/servlets"
, "root", "Gaurav@123");
 } catch (Exception e) {
 e.printStackTrace();
 }
 return conn;
 }
%>
Login.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
 <title>Login Page</title>
</head>
<body>
 <h2>User Login</h2>
 <form action="authenticate.jsp" method="post">
 Username: <input type="text" name="username" required><br>
 Password: <input type="password" name="password"
required><br>
 <input type="submit" value="Login">
 </form>
 <p style="color:red;">${errorMessage}</p>
</body>
</html>
authenticate.jsp
<%@ page import="java.sql.*" %>
<%@ include file="db.jsp" %>
<%
 String username = request.getParameter("username");
 String password = request.getParameter("password");

 if (username != null && password != null) {
 Connection conn = getConnection();
 PreparedStatement ps = conn.prepareStatement("SELECT *
FROM users WHERE username=? AND password=?");
 ps.setString(1, username);
 ps.setString(2, password);
 ResultSet rs = ps.executeQuery();
 if (rs.next()) {
 session.setAttribute("username", username);
 response.sendRedirect("welcome.jsp");
 } else {
 request.setAttribute("errorMessage", "Invalid username or
password.");
 request.getRequestDispatcher("login.jsp").forward(request,
response);
 }
 conn.close();
 }
%>
Welcome.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<%@ page import="jakarta.servlet.http.*, jakarta.servlet.*" %>
<!DOCTYPE html>
<html>
<head>
 <title>Welcome</title>
</head>
<body>
 <h2>Welcome, <%= session.getAttribute("username") %>!</h2>
</body>
</html>
