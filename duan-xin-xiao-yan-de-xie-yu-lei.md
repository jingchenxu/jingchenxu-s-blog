## 短信校验的血与泪

data: 2017-12-20

- 祸患隐现

在一个月前的下午,当时风和日丽，在去食堂吃饭的路上和领导讨论如何进行系统预警，因为一次订单积压，领导未能及时发现，我想了想，客户提供了短信接口，我们除了用来做短信校验，完全也可以用来做预警短信嘛！领导觉得不错就在系统中添加了预警短信，但是我想到我们的短信校验接口好像是完全裸露在外的，没有session校验，注册的时候也没办法做session校验，用前端的跨域调试就能调用我们的接口，此外还可以使用postman这样的工具，更可怕的是，如果用人用python写个脚本，估计能嗨翻天。

- 厄运如约而至

世上该来的事情终究会来的，果然突然有一天就是今天，发现系统正在疯狂的发短信，我有种不详的预感，这应该不是系统故障，应该是被黑了，我们当即决定先把短信调用的屏蔽，但是我们在日志中还是能够看到，短信发送请求的接口还在被调用。

到这时已经可以确定的是黑客是通过对外暴露的http接口进行调用的，那么如何才能避免这样的接口落入敌人的魔爪呢？在其他的接口的调用需要判断调用这个接口的是否是用户，这部分是通过session判断的，对于账户修改中的短信接口可以通过session来判断，但是用户的注册接口是需要在用户未登录的时候尽心的，这里的接口是没办法做校验的，只能通过限制只能是正常的请求才能发送，所谓的正常的请求就是该请求是一位好心肠的客户发出的，如何确认该请求是好心肠的客户发出的呢？这就需要客户自己来证明，我们提供一个证明的方式，这个证明的方式就是市面上各种形形式式的图形验证码，其中以12306最甚。

以上表达了一个思想，任何暴露在客户端的行为都是危险的，所有的关键信息都应该在服务端生成及校验，客户端只是为了用户输入指令，不要期望任何客户端告诉你信息的安全性，如果是下单，你需要知道客户购买的商品及数量，就这些，其中商品还需要进行后端的校验，判断商品是否存在，是否是正确的商品，其他的传递的后台的数据都是不可信的，需要从数据库中重新查询。

这就导致了一个问题，每次校验都需要访问数据库，这是个蛋疼的问题，这回不断的降低用户的体验，这个问题可以通过使用redis来解决，对于订单的校验，校验的数据从redis中获取，这里暂不赘述。

对于一个赤裸暴露的接口，我们可以通过跨域调试的方法、postman之类工具、脚本这样三种方法来请求这个接口，这三个方法都不是好心肠的用户做的。

跨域调试，我们知道正常情况下，如果我们想要用ajax调用http接口，请求该接口的代码文件和接口是需要在同一域名下的，这就是所谓的同源策略，使用这一策略，我们简单的防范了80%的用户，跨域调试时一种在前后端分离的情况下进行调试的方法，多数的前端开发者利用这一点就可以调用裸露的接口，我们会常常在一些开源的项目中使用这样的方法完成如V2EX等客户端的搭建工作。

postman是前后端常用来进行接口调试的工具，这种方法可能知晓的人更加的多，图形化的操作界面可以很方便的完成接口的调用。

脚本请求，使用python这样的脚本语言，完成一个http请求只需要几行代码，添加简单的for循环，接口将会被调用的近乎崩溃。

还有一些更难防范的，诸如抓包之类的额方法更难防范。

- 添加图形验证码校验

添加生成图形校验码的类：

````java
package com.nestle.utils;

import javax.imageio.ImageIO;  
import java.awt.*;  
import java.awt.image.BufferedImage;  
import java.io.FileOutputStream;  
import java.io.IOException;  
import java.io.OutputStream;  
import java.util.Date;  
import java.util.Random;  

// 验证码类
public class ValidateCode {
	
	// 图片宽度
    private int width = 160;  
    // 图片的高度。  
    private int height = 40;  
    // 验证码字符个数  
    private int codeCount = 5;  
    // 验证码干扰线数  
    private int lineCount = 150;  
    // 验证码  
    private String code = null;  
    // 验证码图片Buffer  
    private BufferedImage buffImg = null;  
  
    // 验证码范围,去掉0(数字)和O(拼音)容易混淆的(小写的1和L也可以去掉,大写不用了)  
    private char[] codeSequence = {'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J',  
            'K', 'L', 'M', 'N', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W',  
            'X', 'Y', 'Z', '1', '2', '3', '4', '5', '6', '7', '8', '9'};  
  
    /** 
     * 默认构造函数,设置默认参数 
     */  
    public ValidateCode() {  
        this.createCode();  
    }  
  
    /** 
     * @param width  图片宽 
     * @param height 图片高 
     */  
    public ValidateCode(int width, int height) {  
        this.width = width;  
        this.height = height;  
        this.createCode();  
    }  
  
    /** 
     * @param width     图片宽 
     * @param height    图片高 
     * @param codeCount 字符个数 
     * @param lineCount 干扰线条数 
     */  
    public ValidateCode(int width, int height, int codeCount, int lineCount) {  
        this.width = width;  
        this.height = height;  
        this.codeCount = codeCount;  
        this.lineCount = lineCount;  
        this.createCode();  
    }  
  
    public void createCode() {  
        int x = 0, fontHeight = 0, codeY = 0;  
        int red = 0, green = 0, blue = 0;  
  
        x = width / (codeCount + 2);//每个字符的宽度(左右各空出一个字符)  
        fontHeight = height - 2;//字体的高度  
        codeY = height - 4;  
  
        // 图像buffer  
        buffImg = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);  
        Graphics2D g = buffImg.createGraphics();  
        // 生成随机数  
        Random random = new Random();  
        // 将图像填充为白色  
        g.setColor(Color.WHITE);  
        g.fillRect(0, 0, width, height);  
        // 创建字体,可以修改为其它的  
        Font font = new Font("Fixedsys", Font.PLAIN, fontHeight);  
//        Font font = new Font("Times New Roman", Font.ROMAN_BASELINE, fontHeight);  
        g.setFont(font);  
  
        for (int i = 0; i < lineCount; i++) {  
            // 设置随机开始和结束坐标  
            int xs = random.nextInt(width);//x坐标开始  
            int ys = random.nextInt(height);//y坐标开始  
            int xe = xs + random.nextInt(width / 8);//x坐标结束  
            int ye = ys + random.nextInt(height / 8);//y坐标结束  
  
            // 产生随机的颜色值，让输出的每个干扰线的颜色值都将不同。  
            red = random.nextInt(255);  
            green = random.nextInt(255);  
            blue = random.nextInt(255);  
            g.setColor(new Color(red, green, blue));  
            g.drawLine(xs, ys, xe, ye);  
        }  
  
        // randomCode记录随机产生的验证码  
        StringBuffer randomCode = new StringBuffer();  
        // 随机产生codeCount个字符的验证码。  
        for (int i = 0; i < codeCount; i++) {  
            String strRand = String.valueOf(codeSequence[random.nextInt(codeSequence.length)]);  
            // 产生随机的颜色值，让输出的每个字符的颜色值都将不同。  
            red = random.nextInt(255);  
            green = random.nextInt(255);  
            blue = random.nextInt(255);  
            g.setColor(new Color(red, green, blue));  
            g.drawString(strRand, (i + 1) * x, codeY);  
            // 将产生的四个随机数组合在一起。  
            randomCode.append(strRand);  
        }  
        // 将四位数字的验证码保存到Session中。  
        code = randomCode.toString();  
    }  
  
    public void write(String path) throws IOException {  
        OutputStream sos = new FileOutputStream(path);  
        this.write(sos);  
    }  
  
    public void write(OutputStream sos) throws IOException {  
        ImageIO.write(buffImg, "png", sos);  
        sos.close();  
    }  
  
    public BufferedImage getBuffImg() {  
        return buffImg;  
    }  
  
    public String getCode() {  
        return code;  
    }  
  
    /** 
     * 测试函数,默认生成到d盘 
     * @param args 
     */  
    public static void main(String[] args) {  
        ValidateCode vCode = new ValidateCode(160,40,5,150);  
        try {  
            String path="D:/"+new Date().getTime()+".png";  
            System.out.println(vCode.getCode()+" >"+path);  
            vCode.write(path);  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
    }  
}

````

需要注意的是在服务端生成图片需要在tomcat中进行一些配置，主要需要的配置，否则会出现一些你不想看到的报错，uti是修改catalina.sh文件中的所有````-Djava.io.tmpdir="$CATALINA_TMPDIR" \
````后添加````-Djava.awt.headless=true \
````记住是所有

````bash
-Djava.io.tmpdir="$CATALINA_TMPDIR" \
-Djava.awt.headless=true \
````



