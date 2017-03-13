# Android使用Nanohttpd搭建http服务器 #
*NanoHttpd是个很强大的开源库，仅仅用一个Java类，就实现了一个轻量级的 Web Server，可以非常方便地集成到Android应用中去，让你的App支持 HTTP GET, POST, PUT, HEAD 和 DELETE 请求。*

*在Android上建立Http服务器最好把页面放到assert文件夹中，这样读取静态页面会更方便一些*

- 导入Nanohttpd库
	
with Gradle
    
    compile 'org.nanohttpd:nanohttpd:2.3.1'

- 创建自己的路由类
- 

     public class App extends NanoHTTPD {

        public App(int port) throws IOException {
            super(port);
            start(NanoHTTPD.SOCKET_READ_TIMEOUT, false);
            System.out.println("\nRunning! Point your browsers to http://localhost:8080/ \n");
        }

        public static void main(String[] args) {
            try {
                new App();
            } catch (IOException ioe) {
                System.err.println("Couldn't start server:\n" + ioe);
            }
        }

        @Override
        public Response serve(IHTTPSession session) {
			//通过该方法来对http请求进行处理
            String msg = "<html><body><h1>Hello server</h1>\n";
            Map<String, String> parms = session.getParms();
            if (parms.get("username") == null) {
                msg += "<form action='?' method='get'>\n  <p>Your name: <input type='text' name='username'></p>\n" + "</form>\n";
            } else {
                msg += "<p>Hello, " + parms.get("username") + "!</p>";
            }
            return newFixedLengthResponse(msg + "</body></html>\n");
        }
    }

- 返回Html页面
-
    public Response responseHtmlStream(IHTTPSession session)
    {
        try {
            InputStream fis = assetManager.open("web/public/dist" + session.getUri());
			//注意：第二个参数为页面格式
            return newFixedLengthResponse(Response.Status.OK, "text/html", fis, fis.available());
        }
        catch (FileNotFoundException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "Not Found " + session.getUri());
            return ErrorResponse.fileNotFoundResponse(session.getUri());
        } catch (IOException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "IoError " + session.getUri());
            return newFixedLengthResponse("IoError" + session.getUri());
        }
    }

- 返回Css页面
-
    public Response responseCssStream(IHTTPSession session)
    {
        try {
            InputStream fis = assetManager.open("web/public/dist" + session.getUri());
			//注意：第二个参数为页面格式
            return newFixedLengthResponse(Response.Status.OK, "text/css", fis, fis.available());
        }
        catch (FileNotFoundException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "Not Found " + session.getUri());
            return ErrorResponse.fileNotFoundResponse(session.getUri());
        } catch (IOException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "IoError " + session.getUri());
            return newFixedLengthResponse("IoError " + session.getUri());
        }
    }

- 返回Js页面
-
    public Response responseJavascriptStream(IHTTPSession session)
    {
        try
        {
            InputStream fis = assetManager.open("web/public/dist" + session.getUri());
			//注意：第二个参数为页面格式
            return newFixedLengthResponse(Response.Status.OK, "text/javascript", fis, fis.available());
        }
        catch (FileNotFoundException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "Not Found " + session.getUri());
            return ErrorResponse.fileNotFoundResponse(session.getUri());
        } catch (IOException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "IoError " + session.getUri());
            return newFixedLengthResponse("IoError " + session.getUri());
        }
    }

- 下载文件
-

    public Response responseDownFile(IHTTPSession session)
	{
		try
        {
            InputStream fis = assetManager.open("web/public/dist" + session.getUri());
            NanoHTTPD.Response response = NanoHTTPD.newChunkedResponse(NanoHTTPD.Response.Status.OK, "application/octet-stream", inputStream);
			response.addHeader("Content-Disposition", "attachment; filename="+ URLEncoder.encode(file.getName()));
			return response;
        }
        catch (FileNotFoundException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "Not Found " + session.getUri());
            return ErrorResponse.fileNotFoundResponse(session.getUri());
        } catch (IOException e)
        {
            e.printStackTrace();
            Log.d("SimpleServer error", "IoError " + session.getUri());
            return newFixedLengthResponse("IoError " + session.getUri());
        }
	}
