## ARTS - Share 补2019.2.27

## Spring容器启动过程

![](https://images2018.cnblogs.com/blog/592104/201808/592104-20180824155830858-1586865600.png)

根据这个图了解spring的启动过程:

Web.xml

-Start-contextInitialzed(ServletContextEvent event)

​	ContextLoaderListener

​	-1. initWebApplicationContext(event.getServletContext)

​		ContextLoader

​			-1.1 createWebApplicationContext(servletContext)

​			-1.1.1 determineContextClass(sc) 获取web应用上下文类的class

​			-1.1.1.1 BeanUtils.instantiateClass(contextClass)工具实例化bean

​			-1.1.1.2 T

​			1.1.2 Class<?>contextClass

​			1.2 WebApplicationContext context

​			1.3 configureAndRefreshWebApplicationContext(cwac, servletContext)

​			1.3.1 customizeContext(sc, wac) 查找所有配置的ApplicationContext初始化容器

​			1.3.2 void

​			1.3.3 -wac.refresh()

​			1.3.4 void

​			1.4 void 

#### web.xml

Tomcat启动会首先找web.xml文件，spring容器的入口自然就是这里注册的ContextLoaderListener

```xml
<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```

#### ContextLoaderListener

进入这个类

```java
public class ContextLoaderListener extends ContextLoader implements ServletContextListener{
  /**
   * 创建一个新的 ContextLoaderListener ，将会创建一个基于 contextClass 和 	 ContextConfigLocation context-params 的web应用上下文。看父类的ContextLoader来看默认配置值。
	 *
	 *  在web.xml里声明ContextLoaderListener时候一个无参构造是必须的。
	 * 创建的应用上下文将会注册到ServletContext里， 在属性名为WebApplicationContext 的ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE。
	 *
	 */
	public ContextLoaderListener() {
	}

	/**
	 * 使用给定的应用context来创建一个新的ContextLoaderListener.这个构造方法在基于实例的Servet3.0+环境中使用javax.servlet.ServletContext#addListener注册listener  也是可以的。
	 * 
	 * 如果以下情况context可能没被ConfigurableApplicationContext 重新刷新:
	 * a. 该context实现了ConfigurableWebApplicationContext接口
	 * b. 没有被推荐方法重新刷新
	 * 之后会发生以下情况：
   * 1.如果给定的context 没有被分配一个ConfigurableApplicationContext#setId的 id，将会分配一个
   * 2.ServletContext和ServletConfig 对象将会委托给应用上下文
   * 3.customizeContext将会被调用
   * 4.任意的ApplicationContextInitializer通过contextInitializerClasses定义的init-param参数将会被应用 
	 * 5.ConfigurableApplicationContext#refresh 将会被调用
	 *
	 * 如果context上下文已经被重新刷新或者没有实现ConfigurableWebApplicationContext，以上都不会发生。
	 * 用户根据自己需要明确定义。
	 * 看 org.springframework.web.WebApplicationInitializer} 使用例子.
	 * 无论何种情况，给定的context都会被注册到ServletContext 的属性WebApplicationContext#ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE之下，spring应用上下文将在调用contextDestroyed 方法后关闭。
	 */
	public ContextLoaderListener(WebApplicationContext context) {
		super(context);
	}


	/**
	 * 初始化根web应用上下文
	 */
	@Override
	public void contextInitialized(ServletContextEvent event) {
		initWebApplicationContext(event.getServletContext());
	}


	/**
	 * 关闭根web应用上下文
	 */
	@Override
	public void contextDestroyed(ServletContextEvent event) {
		closeWebApplicationContext(event.getServletContext());
		ContextCleanupListener.cleanupAttributes(event.getServletContext());
	}

}

```

#### 1.initWebApplicationContext(event.getServletContext)

初始化WebApplicationContext。我们继续看看这里的初始化方法，它调用了父类ContextLoader的initWebApplicationContext方法，

```java
/**
	 * 初始化给定的servlet上下文 ，或者根据CONTEXT_CLASS_PARAM 上下文类和 CONFIG_LOCATION_PARAM上下文配置的context-params来创建一个新的。
	 */
	public WebApplicationContext initWebApplicationContext(ServletContext servletContext) {
		if (servletContext.getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE) != null) {
      // 如果已经存在这个属性，说明已经创建过根上下文了，抛异常
			throw new IllegalStateException(
					"Cannot initialize context because there is already a root application context present - " +
					"check whether you have multiple ContextLoader* definitions in your web.xml!");
		}

		Log logger = LogFactory.getLog(ContextLoader.class);
		servletContext.log("Initializing Spring root WebApplicationContext");
		if (logger.isInfoEnabled()) {
			logger.info("Root WebApplicationContext: initialization started");
		}
		long startTime = System.currentTimeMillis();

		try {
      // 储存context到本地实例变量，保证ServletContext关闭时候也存在
			if (this.context == null) {
				this.context = createWebApplicationContext(servletContext);
			}
      // 如果context实现了ConfigurableWebApplicationContext 
			if (this.context instanceof ConfigurableWebApplicationContext) {
				ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext) this.context;
				if (!cwac.isActive()) {
          // 如果context没被刷新，就去配置父context, context id 等
					// The context has not yet been refreshed -> provide services such as
					// setting the parent context, setting the application context id, etc
					if (cwac.getParent() == null) {
						// The context instance was injected without an explicit parent ->
						// determine parent for root web application context, if any.
						ApplicationContext parent = loadParentContext(servletContext);
						cwac.setParent(parent);
					}
          // 配置并刷新context
					configureAndRefreshWebApplicationContext(cwac, servletContext);
				}
			}
			servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, this.context);

			ClassLoader ccl = Thread.currentThread().getContextClassLoader();
			if (ccl == ContextLoader.class.getClassLoader()) {
				currentContext = this.context;
			}
			else if (ccl != null) {
				currentContextPerThread.put(ccl, this.context);
			}

			if (logger.isDebugEnabled()) {
				logger.debug("Published root WebApplicationContext as ServletContext attribute with name [" +
						WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE + "]");
			}
			if (logger.isInfoEnabled()) {
				long elapsedTime = System.currentTimeMillis() - startTime;
				logger.info("Root WebApplicationContext: initialization completed in " + elapsedTime + " ms");
			}

			return this.context;
		}
		catch (RuntimeException ex) {
			logger.error("Context initialization failed", ex);
			servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, ex);
			throw ex;
		}
		catch (Error err) {
			logger.error("Context initialization failed", err);
			servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, err);
			throw err;
		}
	}

```



#### 1.1 createWebApplicationContext(servletContext)

```java
/**
	 * 为loader 初始化根WebApplicationContext,不是默认的context class 就是一个明确自定义的context class 
	 * 这个实现期望自定义context实现ConfigurableWebApplicationContext接口
	 * 可以在自类被覆盖
	 * 另外，customizeContext调用刷新context， 允许子类自定义修改context.
	 *
	 * @param sc 当前context
	 * @return the root WebApplicationContext
	 * @see ConfigurableWebApplicationContext
	 */
	protected WebApplicationContext createWebApplicationContext(ServletContext sc) {
		Class<?> contextClass = determineContextClass(sc);
		if (!ConfigurableWebApplicationContext.class.isAssignableFrom(contextClass)) {
			throw new ApplicationContextException("Custom context class [" + contextClass.getName() +
					"] is not of type [" + ConfigurableWebApplicationContext.class.getName() + "]");
		}
		return (ConfigurableWebApplicationContext) BeanUtils.instantiateClass(contextClass);
	}
```

#### 1.1.1 determineContextClass(sc) 获取web应用上下文类的class

```java
/**
	 * 返回WebApplicationContext实现的class来使用，不是默认XmlWebApplicationContext就是明确自定义的context 类。
	 * @param servletContext 当前 servlet context
	 * @return the WebApplicationContext implementation class to use
	 * @see #CONTEXT_CLASS_PARAM
	 * @see org.springframework.web.context.support.XmlWebApplicationContext
	 */
	protected Class<?> determineContextClass(ServletContext servletContext) {
		String contextClassName = servletContext.getInitParameter(CONTEXT_CLASS_PARAM);
		if (contextClassName != null) {
			try {
				return ClassUtils.forName(contextClassName, ClassUtils.getDefaultClassLoader());
			}
			catch (ClassNotFoundException ex) {
				throw new ApplicationContextException(
						"Failed to load custom context class [" + contextClassName + "]", ex);
			}
		}
		else {
			contextClassName = defaultStrategies.getProperty(WebApplicationContext.class.getName());
			try {
				return ClassUtils.forName(contextClassName, ContextLoader.class.getClassLoader());
			}
			catch (ClassNotFoundException ex) {
				throw new ApplicationContextException(
						"Failed to load default context class [" + contextClassName + "]", ex);
			}
		}
	}
```

#### 1.1.1.1 BeanUtils.instantiateClass(contextClass)工具实例化bean

```java
// createWebApplicationContext方法里determineContextClass 返回加载的Class, 之后实例化这些class
return (ConfigurableWebApplicationContext) BeanUtils.instantiateClass(contextClass);



/**
	 * 使用无参构造初始化class
	 * 注意如果方法非public ，会尝试设置为可操作的
	 * <p>Note that this method tries to set the constructor accessible
	 * if given a non-accessible (that is, non-public) constructor.
	 * @param clazz class to instantiate
	 * @return the new instance
	 * @throws BeanInstantiationException if the bean cannot be instantiated
	 * @see Constructor#newInstance
	 */
	public static <T> T instantiateClass(Class<T> clazz) throws BeanInstantiationException {
		Assert.notNull(clazz, "Class must not be null");
		if (clazz.isInterface()) {
			throw new BeanInstantiationException(clazz, "Specified class is an interface");
		}
		try {
			return instantiateClass(clazz.getDeclaredConstructor());
		}
		catch (NoSuchMethodException ex) {
			throw new BeanInstantiationException(clazz, "No default constructor found", ex);
		}
	}



/**
	 *
	 * 使用给定的构造器实例化class
	 * <p>Note that this method tries to set the constructor accessible if given a
	 * non-accessible (that is, non-public) constructor.
	 * @param ctor the constructor to instantiate
	 * @param args the constructor arguments to apply
	 * @return the new instance
	 * @throws BeanInstantiationException if the bean cannot be instantiated
	 * @see Constructor#newInstance
	 */
	public static <T> T instantiateClass(Constructor<T> ctor, Object... args) throws BeanInstantiationException {
		Assert.notNull(ctor, "Constructor must not be null");
		try {
			ReflectionUtils.makeAccessible(ctor);
			return ctor.newInstance(args);
		}
		catch (InstantiationException ex) {
			throw new BeanInstantiationException(ctor, "Is it an abstract class?", ex);
		}
		catch (IllegalAccessException ex) {
			throw new BeanInstantiationException(ctor, "Is the constructor accessible?", ex);
		}
		catch (IllegalArgumentException ex) {
			throw new BeanInstantiationException(ctor, "Illegal arguments for constructor", ex);
		}
		catch (InvocationTargetException ex) {
			throw new BeanInstantiationException(ctor, "Constructor threw exception", ex.getTargetException());
		}
	}

// 实例化完成，强转为ConfigurableWebApplicationContext 类型返回
```

#### 1.3 configureAndRefreshWebApplicationContext(cwac, servletContext)

之后就进入了ConfigurableWebApplicationContext逻辑了

```java
// 如果当前context是实现了可配置的WebApplicationContet, 就去刷新一下
if (this.context instanceof ConfigurableWebApplicationContext) {
				ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext) this.context;
  			// 没被刷新(设置父context, 设置context id等)，
				if (!cwac.isActive()) {
					// The context has not yet been refreshed -> provide services such as
					// setting the parent context, setting the application context id, etc
					if (cwac.getParent() == null) {
						// The context instance was injected without an explicit parent ->
						// determine parent for root web application context, if any.
						ApplicationContext parent = loadParentContext(servletContext);
						cwac.setParent(parent);
					}
          // 之后刷新context
					configureAndRefreshWebApplicationContext(cwac, servletContext);
				}
			}
```



```java
protected void configureAndRefreshWebApplicationContext(ConfigurableWebApplicationContext wac, ServletContext sc) {
		if (ObjectUtils.identityToString(wac).equals(wac.getId())) {
			// The application context id is still set to its original default value
			// -> assign a more useful id based on available information
      // context id 设置为原始默认值
			String idParam = sc.getInitParameter(CONTEXT_ID_PARAM);
			if (idParam != null) {
				wac.setId(idParam);
			}
			else {
				// Generate default id...
        // 生成默认ID
				wac.setId(ConfigurableWebApplicationContext.APPLICATION_CONTEXT_ID_PREFIX +
						ObjectUtils.getDisplayString(sc.getContextPath()));
			}
		}

		wac.setServletContext(sc);
		String configLocationParam = sc.getInitParameter(CONFIG_LOCATION_PARAM);
		if (configLocationParam != null) {
			wac.setConfigLocation(configLocationParam);
		}

		// The wac environment's #initPropertySources will be called in any case when the context
		// is refreshed; do it eagerly here to ensure servlet property sources are in place for
		// use in any post-processing or initialization that occurs below prior to #refresh
		ConfigurableEnvironment env = wac.getEnvironment();
		if (env instanceof ConfigurableWebEnvironment) {
			((ConfigurableWebEnvironment) env).initPropertySources(sc, null);
		}

  // 处置自定义的Context
		customizeContext(sc, wac);
		wac.refresh();
	}
```



#### 1.3.1 customizeContext(sc, wac) 查找所有配置的ApplicationContext初始化容器

```java
/**
   * context refresh 之前、config location应用之后，设置自定义参数
	 *
	 * <p>The default implementation {@linkplain #determineContextInitializerClasses(ServletContext)
	 * determines} what (if any) context initializer classes have been specified through
	 * {@linkplain #CONTEXT_INITIALIZER_CLASSES_PARAM context init parameters} and
	 * {@linkplain ApplicationContextInitializer#initialize invokes each} with the
	 * given web application context.
	 * <p>Any {@code ApplicationContextInitializers} implementing
	 * {@link org.springframework.core.Ordered Ordered} or marked with @{@link
	 * org.springframework.core.annotation.Order Order} will be sorted appropriately.
	 * @param sc the current servlet context
	 * @param wac the newly created application context
	 * @see #CONTEXT_INITIALIZER_CLASSES_PARAM
	 * @see ApplicationContextInitializer#initialize(ConfigurableApplicationContext)
	 */
	protected void customizeContext(ServletContext sc, ConfigurableWebApplicationContext wac) {
    // 要初始化的类
		List<Class<ApplicationContextInitializer<ConfigurableApplicationContext>>> initializerClasses =
				determineContextInitializerClasses(sc);

		for (Class<ApplicationContextInitializer<ConfigurableApplicationContext>> initializerClass : initializerClasses) {
			Class<?> initializerContextClass =
					GenericTypeResolver.resolveTypeArgument(initializerClass, ApplicationContextInitializer.class);
			if (initializerContextClass != null && !initializerContextClass.isInstance(wac)) {
				throw new ApplicationContextException(String.format(
						"Could not apply context initializer [%s] since its generic parameter [%s] " +
						"is not assignable from the type of application context used by this " +
						"context loader: [%s]", initializerClass.getName(), initializerContextClass.getName(),
						wac.getClass().getName()));
			}
			this.contextInitializers.add(BeanUtils.instantiateClass(initializerClass));
		}

		AnnotationAwareOrderComparator.sort(this.contextInitializers);
		for (ApplicationContextInitializer<ConfigurableApplicationContext> initializer : this.contextInitializers) {
			initializer.initialize(wac);
		}
	}
```



```java
/**
   * 返回使用CONTEXT_INITIALIZER_CLASSES_PARAM指定的实现class
	 * Return the {@link ApplicationContextInitializer} implementation classes to use
	 * if any have been specified by {@link #CONTEXT_INITIALIZER_CLASSES_PARAM}.
	 * @param servletContext current servlet context
	 * @see #CONTEXT_INITIALIZER_CLASSES_PARAM
	 */
	protected List<Class<ApplicationContextInitializer<ConfigurableApplicationContext>>>
			determineContextInitializerClasses(ServletContext servletContext) {

		// 创建集合储存所有的全局参数定义，和自定的
		List<Class<ApplicationContextInitializer<ConfigurableApplicationContext>>> classes =
				new ArrayList<Class<ApplicationContextInitializer<ConfigurableApplicationContext>>>();


		String globalClassNames = servletContext.getInitParameter(GLOBAL_INITIALIZER_CLASSES_PARAM);
		if (globalClassNames != null) {
			for (String className : StringUtils.tokenizeToStringArray(globalClassNames, INIT_PARAM_DELIMITERS)) {
				classes.add(loadInitializerClass(className));
			}
		}

		String localClassNames = servletContext.getInitParameter(CONTEXT_INITIALIZER_CLASSES_PARAM);
		if (localClassNames != null) {
			for (String className : StringUtils.tokenizeToStringArray(localClassNames, INIT_PARAM_DELIMITERS)) {
				classes.add(loadInitializerClass(className));
			}
		}

		return classes;
	}
```

这之后，刷新context :  wac.refresh();

刷新完context， 所有调用结束，返回。



