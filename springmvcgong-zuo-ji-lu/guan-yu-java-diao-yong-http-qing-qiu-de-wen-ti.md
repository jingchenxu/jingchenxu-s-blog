# 关于java调用http请求的问题

> 最近项目中涉及到了与第三方公司对接的问题，对方提供的是resetful API,主要总结载这次接口调试中出现的问题。

* java中的post请求

post请求代码如下：

```java
    public JSONObject sendPostRequest(String inteface, String param) throws java.io.IOException {

        //Build parameter string
        String data = param;
        try {

            URL url = new URL(inteface);
            URLConnection conn = url.openConnection();
            // 设置通用的请求属性
            conn.setRequestProperty("accept", "*/*");
            conn.setRequestProperty("connection", "Keep-Alive");
            conn.setDoOutput(true);
            conn.setDoInput(true);
            OutputStreamWriter writer = new OutputStreamWriter(conn.getOutputStream(),"UTF-8");

            //write parameters
            writer.write(data);
            writer.flush();

            // Get the response
            StringBuffer answer = new StringBuffer();
            BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream(),"UTF-8"));
            String line;
            while ((line = reader.readLine()) != null) {
                answer.append(line);
            }
            writer.close();
            reader.close();

            //Output the response
            System.out.println(answer);
            JSONObject _result = JSONObject.fromObject(answer.toString());
            return _result;

        } catch (MalformedURLException ex) {
            ex.printStackTrace();
        }
        return null;
    }
```

经过测试，get请求同样也可以使用此方法实现，（最近看到一个文章说，get、post请求并无本质的不同，个人感觉还是存在区别的）。

* java.net.unknownhostexception异常

本次调试过程中出现了java.net.unknownhostexception异常，从字面意思理解就是域名解析异常，对于此问题，我在此次的调试过程中总结的原因有三：

1. DNS解析异常，这个需要配置DNS解析服务器；
2. 网络原因，尤其是在移动端；
3. 请求域名存在问题。

本次调试过程中导致异常的原因其实很简单，请求的url中存在中文字符串，但是为什么没发现呢，因为在eclipse编译器中中英文的“-”显示的一模一样，只有在txt中你才会发现2者的不同。

* 总结

此次异常并不是代码的BUG等其他问题，就是url没写对，我通过ping的方式请求域名是有回复的，但是使用java进行http请求的时候，这个地方的url我一直就没有修改过，还是错误的url，对于此类BUG，最好是将代码重写一遍，最后少用复制和粘贴。

