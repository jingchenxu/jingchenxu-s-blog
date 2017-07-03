## 关于java调用http请求的问题

> 最近项目中涉及到了与第三方公司对接的问题，对方提供的是resetful API,主要总结载这次接口调试中出现的问题。

- java中的post请求

post请求代码如下：

````java
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
````