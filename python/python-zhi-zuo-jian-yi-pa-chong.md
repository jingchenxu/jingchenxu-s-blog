# python 制作简易爬虫

> 以前一直想爬一些使用了cloudfare的网站，后来发现，难度还是太大了，使用知乎上的那些常规的手段完全不行，基本上在知乎爬虫教程里出现的网站都加大了反爬虫的力度，主要是一些招聘网站，现在爬取数据的难度还是比较大的。

## 准备工作

* [ ] 抓取数据网站：[https://www.foxebook.net](https://www.foxebook.net)
* [ ] 使用http请求工具：requests
* [ ] 数据保存方式：excel
* [ ] 开发工具：jupyter
* [ ] 需要安装的pip包：requests、bs4、xlwt、xlrd、xlutils、time、datetime

## 实现的代码

```python
import requests
from bs4 import BeautifulSoup
import xlwt
import xlrd
from xlutils import copy
import time, datetime

BASE_URL = 'https://www.foxebook.net/page/{}/'
DETAIL_URL = 'https://www.foxebook.net{}'

EXCEL_BOOK = 'foxebook.xlsx'
ROW_INDEX = 0
NEW_EXCEL = None

# 下载图书封面
def download_bookimage(bookimage):
    print('download book face')

# 获取书名
def get_bookname(book_detail):
    return book_detail.find('h1').string

# 获取图书封面
def get_bookimage(book_detail):
    return book_detail.find('img').get('src')

# 获取书籍发布时间
def get_publishdate(book_detail):
    print(book_detail.select('.list-unstyled')[0].children)
    return book_detail.find('.list-unstyled')


def get_book_detail(bookname):
    detail_url = DETAIL_URL.format(bookname)
    book_detail = requests.get(detail_url)
    if book_detail.status_code == 200:
        global NEW_EXCEL
        booksheet = NEW_EXCEL.get_sheet(0)
        global ROW_INDEX
        print('详情页请求成功:{}'.format(bookname))
        book_detail = BeautifulSoup(book_detail.text, 'html.parser')
        bookname = get_bookname(book_detail)
        print('bookname:'+bookname)
        bookimage = get_bookimage(book_detail)
        print('bookimage:'+bookimage)
        bookdetails = book_detail.select('.list-unstyled')[0].find_all('li')
        title = bookdetails[0].text.split(':')[1]
        print('title:'+title)
        author = bookdetails[1].text.split(':')[1]
        print('author:'+author)
        length = bookdetails[2].text.split(':')[1]
        print('length:'+length)
        edition = bookdetails[3].text.split(':')[1]
        print('edition:'+edition)
        language = bookdetails[4].text.split(':')[1]
        print('language:'+language)
        publisher = bookdetails[5].text.split(':')[1]
        print('publisher:'+publisher)
        publication_date = bookdetails[6].text.split(':')[1]
        print('publication_date:'+publication_date)
        ISBN_10 = bookdetails[7].text.split(':')[1]
        print('ISBN_10:'+ISBN_10)
        print(len(bookdetails))
        if len(bookdetails) == 9:
            ISBN_13 = bookdetails[8].text.split(':')[1]
            print('ISBN_13:'+ISBN_13)
            booksheet.write(ROW_INDEX, 8, ISBN_13)
        booksheet.write(ROW_INDEX, 0, bookname)
        booksheet.write(ROW_INDEX, 1, author)
        booksheet.write(ROW_INDEX, 2, length)
        booksheet.write(ROW_INDEX, 3, edition)
        booksheet.write(ROW_INDEX, 4, language)
        booksheet.write(ROW_INDEX, 5, publisher)
        booksheet.write(ROW_INDEX, 6, publication_date)
        booksheet.write(ROW_INDEX, 7, ISBN_10)
        
        ROW_INDEX += 1
    else:
        print('书本：'+bookname+'页面请求失败'+'状态码为：'+str(book_detail.status_code))     

def get_bookname_from_list(book_list):
    for book in book_list:
        bookname = book.select('a')[0].get('href')
        get_book_detail(bookname)
        
# open excel
def open_excel():
    wb = xlrd.open_workbook(filename=EXCEL_BOOK)
    global NEW_EXCEL
    NEW_EXCEL = copy.copy(wb)
    # write column into excel
    booksheet = NEW_EXCEL.get_sheet(0)
    global ROW_INDEX
    booksheet.write(ROW_INDEX, 0, 'bookname')
    booksheet.write(ROW_INDEX, 1, 'author')
    booksheet.write(ROW_INDEX, 2, 'length')
    booksheet.write(ROW_INDEX, 3, 'edition')
    booksheet.write(ROW_INDEX, 4, 'language')
    booksheet.write(ROW_INDEX, 5, 'publisher')
    booksheet.write(ROW_INDEX, 6, 'publication_date')
    booksheet.write(ROW_INDEX, 7, 'ISBN_10')
    booksheet.write(ROW_INDEX, 8, 'ISBN_13')
    ROW_INDEX += 1
    
# close excel
def close_excel():
    timestamp = time.strftime('%Y%m%d%H%M%S')
    NEW_EXCEL.save('foxeboot{}.xls'.format(timestamp))
    

def get_page_by_number(page_number):
    url = BASE_URL.format(page_number)
    current_page = requests.get(url)
    if current_page.status_code == 200:
        soup = BeautifulSoup(current_page.text, 'html.parser')
        book_list = soup.select('.thumbnail')
        get_bookname_from_list(book_list)
    else:
        print('error on get list')
        close_excel()
    
def link_start():
    for i in range(2):
        get_page_by_number(i)
    
if __name__ == '__main__':
    open_excel()
    link_start() 
    close_excel()
    
```

## 注意事项

全局变量的使用

