## 화면 

![img.png](/assets/img/post/2023-11-19/img.png)

## JSP
```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ page import="com.example.demo.orders.dto.OrdersDTO" %>
<%@ page import="com.example.demo.utils.PageHelper" %>
<%@ page import="java.util.List" %>

<html>
<head>
    <title>테스트</title>
    <meta charset="UTF-8">
    <style>
        .data-table { width: 100%; border-collapse: collapse; }
        .data-table td, th { border: 1px solid #000; padding: 5px 10px; }
        .data-table td { text-align: center; }
        .pagination { text-align: center; margin-top: 30px; }
        .pagination a { margin: 0 5px; }
    </style>
</head>

<body>
    <table class="data-table">
        <thead>
            <th>ORDER ID</th>
            <th>CUSTOMER ID</th>
            <th>STATUS</th>
            <th>SALSEMAN ID</th>
            <th>ORDER DATE</th>
        </thead>
        <tbody>
            <%
                List<OrdersDTO> list = (List<OrdersDTO>) request.getAttribute("list");
                for (OrdersDTO item : list) {
            %>
            <tr>
                <td><%=item.getOrderId() %></td>
                <td><%=item.getCustomerId() %></td>
                <td><%=item.getStatus() %></td>
                <td><%=item.getSalesmanId() %></td>
                <td><%=item.getOrderDate() %></td>
            </tr>
            <%
                }
            %>
        </tbody>
    </table>
    
    <div class="pagination"></div>
    
    
    <%
    PageHelper pageHelper = (PageHelper)request.getAttribute("pageHelper");
    %>
    
    <script>
        var pageNum = <%=pageHelper.getPageNum() %>;
        var isPrevBlock = <%=pageHelper.isPrevBlock() %>;
        var isNextBlock = <%=pageHelper.isNextBlock() %>;
        var startPage = <%=pageHelper.getStartPage() %>;
        var endPage = <%=pageHelper.getEndPage() %>;
        var totalPageCount = <%=pageHelper.getTotalPageCount() %>;
        var pageCount = <%=pageHelper.getPageCount() %>;
        
        var pagination = document.querySelector(".pagination");
        var URLSearch = new URLSearchParams(location.search);


        function appendAtag(pageNum, html) {

            URLSearch.set("pageNum", pageNum);
            var aTag = document.createElement("a");
            aTag.setAttribute("href", "?" + URLSearch.toString());
            aTag.innerHTML = html;
                
            pagination.appendChild(aTag);
        }
        
        if (pageNum > 1) {
            appendAtag(1, "&lt;&lt; 처음");
        }
        
        if (isPrevBlock) {
            appendAtag(startPage - pageCount, "&lt; 이전");
        }
    
        for (var i = startPage; i <= endPage; i++) {
            appendAtag(i, i);
        }
    
        if (isNextBlock) {
            appendAtag(startPage + pageCount, "다음 &gt;");
        }

        if (pageNum < totalPageCount) {
            appendAtag(totalPageCount, "마지막 &gt;&gt;");
        }
        
    </script>

</body>
</html>
```

## Thymeleaf
```html
<html lang="ko" xmlns="http://www.thymeleaf.org">
<head>
    <title>테스트</title>
    <meta charset="UTF-8">
    <style>
        .data-table { width: 100%; border-collapse: collapse; }
        .data-table td, th { border: 1px solid #000; padding: 5px 10px; }
        .data-table td { text-align: center; }
        .pagination { text-align: center; margin-top: 30px; }
        .pagination a { margin: 0 5px; }
    </style>
</head>

<body>
    <table class="data-table">
        <thead>
            <th>ORDER ID</th>
            <th>CUSTOMER ID</th>
            <th>STATUS</th>
            <th>SALSEMAN ID</th>
            <th>ORDER DATE</th>
        </thead>
        <tbody>
            <tr th:each="item : ${list}">
                <td th:text="${item.orderId}"></td>
                <td th:text="${item.customerId}"></td>
                <td th:text="${item.status}"></td>
                <td th:text="${item.salesmanId}"></td>
                <td th:text="${item.orderDate}"></td>
            </tr>
        </tbody>
    </table>
    
    <div class="pagination"></div>
    
    <script>
        var pageNum = [[${pageHelper.getPageNum}]];
        var isPrevBlock = [[${pageHelper.isPrevBlock}]];
        var isNextBlock = [[${pageHelper.isNextBlock}]];
        var startPage = [[${pageHelper.startPage}]];
        var endPage = [[${pageHelper.endPage}]];
        var totalPageCount = [[${pageHelper.totalPageCount}]];
        var pageCount = [[${pageHelper.pageCount}]];
        
        var pagination = document.querySelector(".pagination");
        var URLSearch = new URLSearchParams(location.search);


        function appendAtag(pageNum, html) {

            URLSearch.set("pageNum", pageNum);
            var aTag = document.createElement("a");
            aTag.setAttribute("href", "?" + URLSearch.toString());
            aTag.innerHTML = html;
                
            pagination.appendChild(aTag);
        }
        
        if (pageNum > 1) {
            appendAtag(1, "&lt;&lt; 처음");
        }
        
        if (isPrevBlock) {
            appendAtag(startPage - pageCount, "&lt; 이전");
        }
    
        for (var i = startPage; i <= endPage; i++) {
            appendAtag(i, i);
        }
    
        if (isNextBlock) {
            appendAtag(startPage + pageCount, "다음 &gt;");
        }

        if (pageNum < totalPageCount) {
            appendAtag(totalPageCount, "마지막 &gt;&gt;");
        }
        
    </script>
</body>
</html>
```
