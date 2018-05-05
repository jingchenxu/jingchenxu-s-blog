# 文件上传先关问题

### 数据类型能否封装

前端代码

```
<input type="file" @change="inputChange" name="fileUpload" />
```

```js
        inputChange (e) {
            console.dir(e.target.files);
            let data = new FormData();
            data.append('multipartFile', e.target.files[0]);
            data.append('fileName', 'ceshi');
            this.$axios
                .post('/pestiot.web/smo/fileupload.do', data)
                .then(function (data) {
                    console.dir(data);
                });
        },
```

后端代码

```
public class BasUploader {
    private MultipartFile multipartFile;
    private String fileName;

    public MultipartFile getMultipartFile() {
        return multipartFile;
    }

    public void setMultipartFile(MultipartFile multipartFile) {
        this.multipartFile = multipartFile;
    }

    public String getFileName() {
        return fileName;
    }

    public void setFileName(String fileName) {
        this.fileName = fileName;
    }
}
```

```
@Controller
@RequestMapping("smo")
public class TestUploadController {

    @RequestMapping(value = "/fileupload", method = RequestMethod.POST)
    @ResponseBody
    public ReturnValue FileUpload (BasUploader basUploader) {
        ReturnValue rtv = new ReturnValue();
        return rtv;
    }
}
```



