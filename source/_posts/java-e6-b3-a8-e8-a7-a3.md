title: "JAVA注解"
date: 2014-03-06 17:33:05
tags:
id: 554
comment: false
categories:
  - 学习笔记
---

<pre class="brush:java">//ParseAnnotation.java
package com.lpcdma.annotation;

import java.lang.annotation.Annotation;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

public class ParseAnnotation {
	public static void parseTypeAnnotation() throws ClassNotFoundException{
		Class clazz = Class.forName("com.lpcdma.annotation.UserAnnotation"); 
		Annotation[] annotations = clazz.getAnnotations();
		for (Annotation annotation : annotations){
			TestA testA = (TestA)annotation;
			System.out.println("id= \""+testA.id()+"\"; "
					+ "name= \""+testA.name()+"\"; gid = "+testA.gid());
		}
	}
	public static void parseMethodAnnotation(){
		Method[] methods = UserAnnotation.class.getDeclaredMethods();
		for (Method method : methods){
			boolean hasAnnotation = method.isAnnotationPresent(TestA.class); 
			if (hasAnnotation){
				TestA annotation = method.getAnnotation(TestA.class);
				System.out.println("method = " + method.getName()  
                        + " ; id = " + annotation.id() + " ; description = "  
                        + annotation.name() + "; gid= "+annotation.gid());  
			}
		}
	}
	public static void parseConstructAnnotation(){
		Constructor[] constructors = UserAnnotation.class.getConstructors();  
        for (Constructor constructor : constructors) { 
        	/* 
             * 判断构造方法中是否有指定注解类型的注解 
             */  
            boolean hasAnnotation = constructor.isAnnotationPresent(TestA.class);  
            if (hasAnnotation) {  
                /* 
                 * 根据注解类型返回方法的指定类型注解 
                 */  
            	TestA annotation =(TestA) constructor.getAnnotation(TestA.class);  
                System.out.println("constructor = " + constructor.getName()  
                        + " ; id = " + annotation.id() + " ; description = "  
                        + annotation.name() + "; gid= "+annotation.gid());  
            }  
        }  
	}

	public static void main(String[] args) throws ClassNotFoundException {

		parseTypeAnnotation();
		parseMethodAnnotation();
		parseConstructAnnotation();

	}

}</pre>
<pre class="brush:java">//TestA.java
package com.lpcdma.annotation;

import java.lang.annotation.Target;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.ElementType;

@Target({ElementType.TYPE,
	ElementType.METHOD,
	ElementType.FIELD,
	ElementType.CONSTRUCTOR})
@Retention(RetentionPolicy.RUNTIME)

public @interface TestA {
	String name();
	int id() default 0;
	Class&lt;Long&gt; gid();
}</pre>
<pre class="brush:java">//UserAnnotation.java
package com.lpcdma.annotation;

import java.util.HashMap;
import java.util.Map;

@TestA(name="type",gid=Long.class)
public class UserAnnotation {

	@TestA(name="param",id=1,gid=Long.class)
	private Integer age;

	@TestA(name="construct",id=2,gid=Long.class)
	public UserAnnotation(){

	}

	@TestA(name="Public method",id=3,gid=Long.class)
	public void a(){
		Map&lt;String,String&gt; m = new HashMap&lt;String,String&gt; (0);
	}

	@TestA(name="Public method",id=4,gid=Long.class)
	public void b(){
		Map&lt;String,String&gt; m = new HashMap&lt;String,String&gt; (0);
	}

	@TestA(name="Private method",id=5,gid=Long.class)
	private void c(){
		Map&lt;String,String&gt; m = new HashMap&lt;String,String&gt; (0);
	}

	public void b(Integer a) {

	}

}</pre>
&nbsp;