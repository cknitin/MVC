# MVC

  ## 1.       MVC lifecycle
  
  ### MVC has two life cycles −
  
  1. The application life cycle
  
     Application life cycle is the time when the application process actually begins running on IIS until it get stop. It mark by the
     methods Application_Start() to Application_End() of Global.ashx.
  
  2. The request life cycle
      
     Its is sequence of events that executed every time when HTTP request received and handled by our application. 
  
     Request --> Routing --> Controller Initialization --> Action Execution --> Result Execution --> View Engine --> 
     
     Result Execution --> Response
  
  ## 2.       Routing
  
  A process of directing an HTTP request to a controller in MVC application with the help of System.Web.Routing namespace. its a prat of   asp.net runtime it is used by MVC framework.
  
  The Global.asax file define the route for your application in Application_Start() method.
  
  In Global.ashx file
  
    protected void Application_Start()
    {
         RouteConfig.RegisterRoutes(RouteTable.Routes);
    }
  
 In App_Start/RouteConfig.cs file
 
     public class RouteConfig 
     {
          public static void RegisterRoutes(RouteCollection routes){
             routes.IgnoreRoute("{resource}.axd/{*pathInfo}");
             routes.MapRoute(
                name: "Default",
                url: "{controller}/{action}/{id}",
                defaults: new{ controller = "Home", action = "Index", id = UrlParameter.Optional});
          }
      } 

 ## Attribute Routing in ASP.NET MVC 5   
 
 To enable routing, add routes.MapMvcAttributeRoutes() in App_Start/RouteConfig.cs file.
 
    public class RouteConfig
    {
        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute(“{resource}.axd/{*pathInfo}”);

            routes.MapMvcAttributeRoutes();  // <--- Enabling Attribute Routing
        }
    }
  
    public class HomeController : Controller
    {
          public ViewResult Index()
          {
              return View();
          }

          [Route("Contact")]     <----- Attribute Routing
          public ViewResult ContactUs()
          {
              return View();
          }
    }
  
  Using RoutePrefix user will need to add some prefix text brfore action in URL. 
  
  Example: http://localhost:4200/infromation/contact
  
    [RoutePrefix("Infromation")]  <----- Route Prefix  
    public class HomeController : Controller
    {
        [Route("Contact")]
        public ViewResult ContactUs()
        {
            return View();
        }
    }
    
  for more infromation : https://blogs.msdn.microsoft.com/webdev/2013/10/17/attribute-routing-in-asp-net-mvc-5/
  
  ## 3.       Views
  ## 4.       Razor Views
  ## 5.       Filters
  
  In ASP.NET MVC, http request is routed to the controller and action. sometimes we want to execute some logic before or after an action   method executes. To do that wecan use ASP.NET MVC provides filters.

  Filter is a custom class where you can write custom logic whcih can execute before or after an action method executes.
  It can be applied to an action method or controller 
  1. declarative 
  2. programmatic 
  
  In Declarative way applying a filter attribute to an action method or controller
  and In programmatic way by implementing a corresponding interface.
  
  Filters are used to perform the following common functionalities
  
  1. Custom Authentication
  2. Custom Authorization(User based or Role based)
  3. Error handling or logging
  4. Data Caching
  5. Activity Logging
  6. Data Compression
  
  Types of Filters
  
  1. Authencation filters
  2. Authorization filters
  3. Action filters
  4. Result filters
  5. Exception filters
  
  Order of Filter Execution
  1. Authentication filters will get execute first then,
  2. Authorization filters
  3. Action filters
  4. Result filters
  
  Configuring Filters
  
  In Global.ashx
  
    protected void Application_Start()
    {
         FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
    }
  
  In App_Start/FilterConfig.cs

    public class FilterConfig
    {
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
        }
    }
 
 ## Authencation filters
 
    public interface IAuthenticationFilter
    {
        void OnAuthentication(AuthenticationContext filterContext);

        void OnAuthenticationChallenge(AuthenticationChallengeContext filterContext);
    }
 
    public class CustomAuthenticationAttribute : ActionFilterAttribute, IAuthenticationFilter
    {
        public void OnAuthentication(AuthenticationContext filterContext)
        {
            throw new NotImplementedException();
        }

        public void OnAuthenticationChallenge(AuthenticationChallengeContext filterContext)
        {
            throw new NotImplementedException();
        }
    }
    
    
 ## Authorization Filters
 
    public interface IAuthorizationFilter
    {
        void OnAuthorization(AuthorizationContext filterContext);
    }
 
    public class CustomAuthorizeAttribute : FilterAttribute, IAuthorizationFilter
    {
        public void OnAuthorization(AuthorizationContext filterContext)
        {
            throw new NotImplementedException();
        }
    }
    
  ## Action Filters
  
    public interface IActionFilter
    {
        void OnActionExecuting(ActionExecutingContext filterContext);
        void OnActionExecuted(ActionExecutedContext filterContext);
    }
    
    public class CustomActionFilter : IActionFilter
    {
        public void OnActionExecuted(ActionExecutedContext filterContext)
        {
            throw new NotImplementedException();
        }

        public void OnActionExecuting(ActionExecutingContext filterContext)
        {
            throw new NotImplementedException();
        }
    }
  
  ## Result Filters
  
    public interface IResultFilter
    {
        void OnResultExecuted(ResultExecutedContext filterContext);
        void OnResultExecuting(ResultExecutingContext filterContext);
    }
    
    public class CustomResultFilter : IResultFilter
    {
        public void OnResultExecuted(ResultExecutedContext filterContext)
        {
            throw new NotImplementedException();
        }

        public void OnResultExecuting(ResultExecutingContext filterContext)
        {
            throw new NotImplementedException();
        }
    }
  
  ## Exception Filters
 
    public interface IExceptionFilter
    {
        void OnException(ExceptionContext filterContext);
    }
  
  
  
  
  ## 6.       Authentication
  ## 7.       Security implementation
  ## 8.       Exception handling
             
  ### 1. try/catch
             
                  public ActionResult Index()
                  {
                      try
                      {
                          int a = 1;
                          int b = 0;
                          int c = 0;
                          c = a / b; //it would cause exception. 

                          return View();
                      }
                      catch
                      {
                          Trace.Write("Error");

                          return View("Error");
                      }
                  }
                  
  ### 2. In web.config
                  
                  <system.web>
                    <customErrors mode="On" defaultRedirect="~/ErrorHandler/Index">
                        <error statusCode="404" redirect="~/ErrorHandler/NotFound"/>
                    </customErrors>
                  </system.web>
                               
  ### 3. using [HandleError] attribute
             
                  [HandleError(ExceptionType = typeof(DivideByZeroException), View = "~/Views/CommonExceptionView.cshtml")]
                  public ActionResult Contact()
                  {
                      int a = 1;
                      int b = 0;
                      int c = 0;
                      c = a / b; //it would cause exception.    
                      return View();
                  }
             
  ### 4. Overriding OnException() method of controller base class
             
                  public class HomeController : Controller
                  {
                      public ActionResult Index()
                      {
                          return View();
                      }

                      protected override void OnException(ExceptionContext filterContext)
                      {
                          filterContext.ExceptionHandled = true;

                          //Log the error!!
                          Trace.Write(filterContext.Exception);

                          //Redirect or return a view, but not both.
                          filterContext.Result = RedirectToAction("Index", "ErrorHandler");
                          // OR 
                          filterContext.Result = new ViewResult
                          {
                              ViewName = "~/Views/ErrorHandler/Index.cshtml"
                          };
                      }
                  }
                  
  ### 5. HandleErrorAttribute by create action filter class
                  
                  [CustomErrorHandling]
                  public ActionResult About()
                  {
                      int a = 1;
                      int b = 0;
                      int c = 0;
                      c = a / b; //it would cause exception.    

                      ViewBag.Message = "Your application description page.";

                      return View();
                  }
                  
                  public class CustomErrorHandling : HandleErrorAttribute
                  {
                      public override void OnException(ExceptionContext filterContext)
                      {
                          if (filterContext.ExceptionHandled || filterContext.HttpContext.IsCustomErrorEnabled)
                          {
                              return;
                          }
                          Exception e = filterContext.Exception;
                          filterContext.ExceptionHandled = true;
                          filterContext.Result = new ViewResult()
                          {
                              ViewName = "~/Views/CommonExceptionView.cshtml"
                          };
                      }

                  }

             
  ### 6. Application_Error() using Global.asax page.
                  
                  protected void Application_Error()
                  {
                      var ex = Server.GetLastError();
                      //log the error!
                      Trace.Write(ex);
                  }
  
  ## 9.       Caching
  ## 10.      Validations
  ## 11.      Areas
  ## 12.      Cookies
  ## 13.      Value Provider / Custom Value Provider
  ## 14.      handler
  
   Create a class library project and impliment "IHttpHandler"    
            public class RssHandler : IHttpHandler
            {
                public bool IsReusable
                {
                    get
                    {
                        return false;
                    }
                }

                public void ProcessRequest(HttpContext context)
                {
                    context.Response.ContentType = "text/html";

                    using (XmlWriter writer = XmlWriter.Create(context.Response.OutputStream))
                    {
                        writer.WriteStartDocument();
                        writer.WriteElementString("rss", "This is test feed.");
                        writer.WriteEndDocument();
                        writer.Flush();
                    }
                }
            } 
            
 Add Class library project reference in MVC project. Also add the below line in web.config            
                
                <system.webServer>
                  <handlers>
                    <add name="RssHandler" verb="*" path="*.rss" type="CustomHttpHandler.RssHandler, CustomHttpHandler"/>
                  </handlers>
                </system.webServer>
  
  ## 15.       module
              
    Create a class library project and impliment "IHttpModule"            
              
              public class LogginHttpModule : IHttpModule
              {
                  public void Dispose()
                  {

                  }

                  public void Init(HttpApplication context)
                  {
                      context.LogRequest += Context_LogRequest;
                  }

                  private void Context_LogRequest(object sender, EventArgs e)
                  {
                      HttpApplication application = (HttpApplication)sender;
                      HttpContext context = application.Context;

                      string requestPath = context.Request.Path;

                      Trace.WriteLine(String.Format("Request Path: {0}",requestPath));

                  }
              } 
              
      Add Class library project reference in MVC project.
      Also add the below line in web.config        
              
              <system.webServer>
                <modules>
                  <add name="LogginHttpModule" type="CustomHttpModule.LogginHttpModule, CustomHttpModule"/>
                </modules>
              </system.webServer>
              
  ## 16. PreApplicationStartMethod          
  
  This method will call before the Application_Start() method.
  
        [assembly: PreApplicationStartMethod(typeof(PreApplicationStartup), "Initialize")]
        namespace PreApplicationStartMethodDemo
        {
            public class PreApplicationStartup
            {

                public static void Initialize()
                {

                }
            }
        }
        
  or in side the "AssemblyInfo.cs"
  
    [assembly: PreApplicationStartMethod(typeof(PreApplicationStartup), "Initialize")]
  
  Now add the project reference in MVC project

 ## 17. ActionFilterAttribute
 
 Add a file with the name "CustomFilter" in the MVC project
 
    public class CustomFilter : ActionFilterAttribute
    {
        public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            string controllerName =filterContext.ActionDescriptor.ControllerDescriptor.ControllerName;
            string actionName = filterContext.ActionDescriptor.ActionName;
            Trace.Write(string.Format("OnActionExecuting : {0}, {1}", controllerName, actionName));
        }

        public override void OnActionExecuted(ActionExecutedContext filterContext)
        {
            string controllerName = filterContext.ActionDescriptor.ControllerDescriptor.ControllerName;
            string actionName = filterContext.ActionDescriptor.ActionName;
            Trace.Write(string.Format("OnActionExecuted : {0}, {1}", controllerName, actionName));
        }
    }
    
    [CustomFilter]
    public class HomeController : Controller
    {
    
    }
