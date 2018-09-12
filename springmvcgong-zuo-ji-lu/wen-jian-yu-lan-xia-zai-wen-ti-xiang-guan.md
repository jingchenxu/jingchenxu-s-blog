# 文件预览下载问题相关

> 关于文件的下载及预览目前我了解的有2种处理方式，一种方式是通过虚拟路径的方式进行处理，这种方式需要涉及的代码少，但是会需要一定的服务器配置，将文件以静态资源的方式进行管理，访问权限上，springMVC应该有对静态资源权限管理的方案，本文暂不做讨论，还有一种是通过java代码读取文件，通过接口返回文件的方式来进行管理，这样对文件权限及流都有更好的管控。

### 下载

````java
	@SuppressWarnings("rawtypes")
	@RequestMapping(value = "/downattach")
	public ResponseEntity DownFileAttach(@ModelAttribute("file") FileAttach file, HttpServletResponse response) {
		ReturnValue rtv = new ReturnValue();

		try {
			FileAttach item = PubDao.GetFileAttach(file);

			String path = FileUtils.GetFileBasePath();

			File df = new File(path + item.getAttachurl());
			HttpHeaders headers = new HttpHeaders();
			
			if (file.getDowntype().equals("attach")) {
				headers.setContentDispositionFormData("attachment", new String(item.getAttachname().getBytes("UTF-8"), "iso-8859-1"));
				headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
			}
			else {
				switch (FileUtils.GetFileType(item.getAttachurl())) {
					case WORD:
						headers.add("Content-Disposition", "inline;filename=" + new String(item.getAttachname().getBytes("UTF-8"), "iso-8859-1"));
						headers.add("Content-Type", "application/vnd.ms-word");
						break;
						
					case EXCEL:
						headers.add("Content-Disposition", "inline;filename=" + new String(item.getAttachname().getBytes("UTF-8"), "iso-8859-1"));
						headers.add("Content-Type", "application/vnd.ms-excel");
						break;
						
					case PPT:
						headers.add("Content-Disposition", "inline;filename=" + new String(item.getAttachname().getBytes("UTF-8"), "iso-8859-1"));
						headers.add("Content-Type", "application/vnd.ms-powerpoint");
						break;
	
					case PDF:
						headers.add("Content-Disposition", "inline;filename=" + new String(item.getAttachname().getBytes("UTF-8"), "iso-8859-1"));
						headers.add("Content-Type", "application/pdf");
						break;
						
					case IMAGE:
						headers.add("Content-Disposition", "inline;filename=" + new String(item.getAttachname().getBytes("UTF-8"), "iso-8859-1"));
						headers.add("Content-Type", "image/jpeg");
						break;
						
					default:
						headers.setContentDispositionFormData("attachment", new String(item.getAttachname().getBytes("UTF-8"), "iso-8859-1"));
						headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
						break;
				}
			}
			
			return new ResponseEntity<byte[]>(org.apache.commons.io.FileUtils.readFileToByteArray(df), headers,	HttpStatus.OK);
		} catch (Exception e) {
			rtv.setMsg("未能获取到资源");
		}

		return this.OutReturnBean(rtv);
	}
````

### 预览





