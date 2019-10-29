---
title: Eclipse里jsp文件Bootstrap的css实效解决办法
date: 2018-05-08 09:40:28
tags: IDE
---
在Eclipse中新建了一个Project，打算用Bootstrap美化一下页面，可是发现在本地引用Bootstrap失败，用火狐检查源码发现原因是Bootstrap.min.css一直无法引入。于是以为是引用路径的问题，就像https://blog.csdn.net/hyh0327/article/details/62893301
这样，可还是无法解决。之后看了http://blog.csdn.net/u013456370/article/details/59483687
<!--more-->
方法一：加上：${pageContext.request.contextPath }/

<link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath }/static/theme/css/yuqing/easyui.css">
<link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath }/static/theme/css/yuqing/icon.css">
<link rel="stylesheet" type="text/css" href="${pageContext.request.contextPath }/static/theme/css/yuqing/demo.css">
<script type="text/javascript" src="${pageContext.request.contextPath }/static/js/ywzbindex/yuqing/jquery.min.js"></script>
<script type="text/javascript" src="${pageContext.request.contextPath }/static/js/ywzbindex/yuqing/jquery.easyui.min.js"></script>.

方法二：
<%@ page language="Java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html>
<html>
<head>
<base href="<%=basePath%>">
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" type="text/css" href="<%=basePath%>/static/theme/css/yuqing/easyui.css">
<link rel="stylesheet" type="text/css" href="<%=basePath%>/static/theme/css/yuqing/icon.css">
<link rel="stylesheet" type="text/css" href="<%=basePath%>/static/theme/css/yuqing/demo.css">
<script type="text/javascript" src="<%=basePath%>/static/js/ywzbindex/yuqing/jquery.min.js"></script>
<script type="text/javascript" src="<%=basePath%>/static/js/ywzbindex/yuqing/jquery.easyui.min.js"></script>
</head>

问题还是无法解决。在差点放弃的时候，在Eclipse中对WebContent文件夹刷新了一下，出现了其下的Bootstrap-3.37-dist文件夹，问题才得到解决。
