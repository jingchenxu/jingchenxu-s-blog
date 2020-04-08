# 前端用户信息加密

### 前端base64加密函数

前端将用户账号及密码使用base64加密的方式提交到后端，后端同样使用base64加密方式进行解密，前端base64加密函数如下：

具体代码：Base64.js

```javascript
/**
 *BASE64 Encode and Decode By UTF-8 unicode
 *可以和java的BASE64编码和解码互相转化
 */
(function () {
  var BASE64_MAPPING = [
    'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H',
    'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P',
    'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X',
    'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f',
    'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n',
    'o', 'p', 'q', 'r', 's', 't', 'u', 'v',
    'w', 'x', 'y', 'z', '0', '1', '2', '3',
    '4', '5', '6', '7', '8', '9', '+', '/'
  ];

  /**
   *ascii convert to binary
   */
  var _toBinary = function (ascii) {
    var binary = new Array();
    while (ascii > 0) {
      var b = ascii % 2;
      ascii = Math.floor(ascii / 2);
      binary.push(b);
    }
    /*
      var len = binary.length;
      if(6-len > 0){
          for(var i = 6-len ; i > 0 ; --i){
              binary.push(0);
          }
      } */
    binary.reverse();
    return binary;
  };

  /**
   *binary convert to decimal
   */
  var _toDecimal = function (binary) {
    var dec = 0;
    var p = 0;
    for (var i = binary.length - 1; i >= 0; --i) {
      var b = binary[i];
      if (b == 1) {
        dec += Math.pow(2, p);
      }
      ++p;
    }
    return dec;
  };

  /**
   *unicode convert to utf-8
   */
  var _toUTF8Binary = function (c, binaryArray) {
    var mustLen = (8 - (c + 1)) + ((c - 1) * 6);
    var fatLen = binaryArray.length;
    var diff = mustLen - fatLen;
    while (--diff >= 0) {
      binaryArray.unshift(0);
    }
    var binary = [];
    var _c = c;
    while (--_c >= 0) {
      binary.push(1);
    }
    binary.push(0);
    // eslint-disable-next-line one-var
    var i = 0, len = 8 - (c + 1);
    for (; i < len; ++i) {
      binary.push(binaryArray[i]);
    }

    for (var j = 0; j < c - 1; ++j) {
      binary.push(1);
      binary.push(0);
      var sum = 6;
      while (--sum >= 0) {
        binary.push(binaryArray[i++]);
      }
    }
    return binary;
  };

  var __BASE64 = {
    /**
           *BASE64 Encode
           */
    encoder: function (str) {
      // eslint-disable-next-line camelcase
      var base64_Index = [];
      var binaryArray = [];
      for (var i = 0, len = str.length; i < len; ++i) {
        var unicode = str.charCodeAt(i);
        var _tmpBinary = _toBinary(unicode);
        if (unicode < 0x80) {
          var _tmpdiff = 8 - _tmpBinary.length;
          while (--_tmpdiff >= 0) {
            _tmpBinary.unshift(0);
          }
          binaryArray = binaryArray.concat(_tmpBinary);
        } else if (unicode >= 0x80 && unicode <= 0x7FF) {
          binaryArray = binaryArray.concat(_toUTF8Binary(2, _tmpBinary));
        } else if (unicode >= 0x800 && unicode <= 0xFFFF) { // UTF-8 3byte
          binaryArray = binaryArray.concat(_toUTF8Binary(3, _tmpBinary));
        } else if (unicode >= 0x10000 && unicode <= 0x1FFFFF) { // UTF-8 4byte
          binaryArray = binaryArray.concat(_toUTF8Binary(4, _tmpBinary));
        } else if (unicode >= 0x200000 && unicode <= 0x3FFFFFF) { // UTF-8 5byte
          binaryArray = binaryArray.concat(_toUTF8Binary(5, _tmpBinary));
        } else if (unicode >= 4000000 && unicode <= 0x7FFFFFFF) { // UTF-8 6byte
          binaryArray = binaryArray.concat(_toUTF8Binary(6, _tmpBinary));
        }
      }

      var extra_Zero_Count = 0;
      for (var i = 0, len = binaryArray.length; i < len; i += 6) {
        var diff = (i + 6) - len;
        if (diff == 2) {
          extra_Zero_Count = 2;
        } else if (diff == 4) {
          extra_Zero_Count = 4;
        }
        // if(extra_Zero_Count > 0){
        //    len += extra_Zero_Count+1;
        // }
        var _tmpExtra_Zero_Count = extra_Zero_Count;
        while (--_tmpExtra_Zero_Count >= 0) {
          binaryArray.push(0);
        }
        base64_Index.push(_toDecimal(binaryArray.slice(i, i + 6)));
      }

      var base64 = '';
      for (var i = 0, len = base64_Index.length; i < len; ++i) {
        base64 += BASE64_MAPPING[base64_Index[i]];
      }

      for (var i = 0, len = extra_Zero_Count / 2; i < len; ++i) {
        base64 += '=';
      }
      return base64;
    },
    /**
           *BASE64  Decode for UTF-8
           */
    decoder: function (_base64Str) {
      var _len = _base64Str.length;
      var extra_Zero_Count = 0;
      /**
               *计算在进行BASE64编码的时候，补了几个0
               */
      if (_base64Str.charAt(_len - 1) == '=') {
        // alert(_base64Str.charAt(_len-1));
        // alert(_base64Str.charAt(_len-2));
        if (_base64Str.charAt(_len - 2) == '=') { // 两个等号说明补了4个0
          extra_Zero_Count = 4;
          _base64Str = _base64Str.substring(0, _len - 2);
        } else { // 一个等号说明补了2个0
          extra_Zero_Count = 2;
          _base64Str = _base64Str.substring(0, _len - 1);
        }
      }

      var binaryArray = [];
      for (var i = 0, len = _base64Str.length; i < len; ++i) {
        var c = _base64Str.charAt(i);
        for (var j = 0, size = BASE64_MAPPING.length; j < size; ++j) {
          if (c == BASE64_MAPPING[j]) {
            var _tmp = _toBinary(j);
            /* 不足6位的补0 */
            var _tmpLen = _tmp.length;
            if (6 - _tmpLen > 0) {
              for (var k = 6 - _tmpLen; k > 0; --k) {
                _tmp.unshift(0);
              }
            }
            binaryArray = binaryArray.concat(_tmp);
            break;
          }
        }
      }

      if (extra_Zero_Count > 0) {
        binaryArray = binaryArray.slice(0, binaryArray.length - extra_Zero_Count);
      }

      var unicode = [];
      var unicodeBinary = [];
      for (var i = 0, len = binaryArray.length; i < len;) {
        if (binaryArray[i] == 0) {
          unicode = unicode.concat(_toDecimal(binaryArray.slice(i, i + 8)));
          i += 8;
        } else {
          var sum = 0;
          while (i < len) {
            if (binaryArray[i] == 1) {
              ++sum;
            } else {
              break;
            }
            ++i;
          }
          unicodeBinary = unicodeBinary.concat(binaryArray.slice(i + 1, i + 8 - sum));
          i += 8 - sum;
          while (sum > 1) {
            unicodeBinary = unicodeBinary.concat(binaryArray.slice(i + 2, i + 8));
            i += 8;
            --sum;
          }
          unicode = unicode.concat(_toDecimal(unicodeBinary));
          unicodeBinary = [];
        }
      }
      return unicode;
    }
  };

  window.BASE64 = __BASE64;
})();

BASE64.js

```

前端在提交参数时需要通过此方法进行加密

## 后端用户信息解密

具体代码如下：

```java
package com.smopark.seed.utils;

import java.io.Serializable;
import java.io.UnsupportedEncodingException;

public class Base64Utils implements Serializable {
    private static final long serialVersionUID = 3762133767673900132L;

    private static char[] base64EncodeChars = new char[] { 'A', 'B', 'C', 'D',
            'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q',
            'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd',
            'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q',
            'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '0', '1', '2', '3',
            '4', '5', '6', '7', '8', '9', '+', '/' };

    private static byte[] base64DecodeChars = new byte[] { -1, -1, -1, -1, -1,
            -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
            -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
            -1, -1, -1, -1, 62, -1, -1, -1, 63, 52, 53, 54, 55, 56, 57, 58, 59,
            60, 61, -1, -1, -1, -1, -1, -1, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
            10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, -1,
            -1, -1, -1, -1, -1, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37,
            38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, -1, -1, -1,
            -1, -1 };

    // 编码
    public final static String encode(byte[] data) {
        StringBuffer sb = new StringBuffer();
        int len = data.length;
        int i = 0;
        int b1, b2, b3;
        while (i < len) {
            b1 = data[i++] & 0xff;
            if (i == len) {
                sb.append(base64EncodeChars[b1 >>> 2]);
                sb.append(base64EncodeChars[(b1 & 0x3) << 4]);
                sb.append("==");
                break;
            }
            b2 = data[i++] & 0xff;
            if (i == len) {
                sb.append(base64EncodeChars[b1 >>> 2]);
                sb.append(base64EncodeChars[((b1 & 0x03) << 4)
                        | ((b2 & 0xf0) >>> 4)]);
                sb.append(base64EncodeChars[(b2 & 0x0f) << 2]);
                sb.append("=");
                break;
            }
            b3 = data[i++] & 0xff;
            sb.append(base64EncodeChars[b1 >>> 2]);
            sb.append(base64EncodeChars[((b1 & 0x03) << 4)
                    | ((b2 & 0xf0) >>> 4)]);
            sb.append(base64EncodeChars[((b2 & 0x0f) << 2)
                    | ((b3 & 0xc0) >>> 6)]);
            sb.append(base64EncodeChars[b3 & 0x3f]);
        }
        return sb.toString();
    }

    // 解码
    public final static byte[] decode(String str)
            throws UnsupportedEncodingException {
        StringBuffer sb = new StringBuffer();
        byte[] data = str.getBytes("US-ASCII");
        int len = data.length;
        int i = 0;
        int b1, b2, b3, b4;
        while (i < len) {
            /* b1 */
            do {
                b1 = base64DecodeChars[data[i++]];
            } while (i < len && b1 == -1);
            if (b1 == -1)
                break;
            /* b2 */
            do {
                b2 = base64DecodeChars[data[i++]];
            } while (i < len && b2 == -1);
            if (b2 == -1)
                break;
            sb.append((char) ((b1 << 2) | ((b2 & 0x30) >>> 4)));
            /* b3 */
            do {
                b3 = data[i++];
                if (b3 == 61)
                    return sb.toString().getBytes("ISO-8859-1");
                b3 = base64DecodeChars[b3];
            } while (i < len && b3 == -1);
            if (b3 == -1)
                break;
            sb.append((char) (((b2 & 0x0f) << 4) | ((b3 & 0x3c) >>> 2)));
            /* b4 */
            do {
                b4 = data[i++];
                if (b4 == 61)
                    return sb.toString().getBytes("ISO-8859-1");
                b4 = base64DecodeChars[b4];
            } while (i < len && b4 == -1);
            if (b4 == -1)
                break;
            sb.append((char) (((b3 & 0x03) << 6) | b4));
        }
        return sb.toString().getBytes("ISO-8859-1");
    }

    /**
     * 获得指定字符串的Base64编码值字符串
     * <br>
     * @param srcString
     * @return
     */
    public final static String encodeToBase64(String srcString) {
        return encode(srcString.getBytes());
    }

    /**
     * 获得Base64编码字符串的解码值字符串
     * @param base64String
     * @return
     * @throws UnsupportedEncodingException
     */
    public final static String decodeFromBase64(String base64String){
        String s = null;
        try {
            s = new String(decode(base64String));
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        return s;
    }
}
```



