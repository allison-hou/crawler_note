# 爬蟲筆記
## 1. 安裝 python 
1. 在寫爬蟲之前的第一步驟先在電腦上安裝 Python 

1. 上網搜尋 "python download"點進去下載你要的版本就完成啦 ~

## 2. 套入 selenium 套件
接下來我們要套入selenium，過程不難，但要學會搜尋的方法!
1. 搜尋"selenium python download windows"
1. 點進教學(要注意有些教學過舊可能無法使用)
1. 在 cmd 打"pip install selenium"指令

    附上我用的網站(https://medium.com/@NorthBei/%E5%9C%A8windows%E4%B8%8A%E5%AE%89%E8%A3%9Dpython-selenium-%E7%B0%A1%E6%98%93%E6%95%99%E5%AD%B8-eade1cd2d12d)

## 3. 安裝webdriver
1. google 搜尋 selenium webdriver 
1. 選擇Chrome的
 ![](photos/photo6235354446146153109.jpg)

1. 再選擇自己使用的版本(以chrome為例)
 ![](photos/photo6235354446146153110.jpg)


    附上我用的網站(https://sites.google.com/a/chromium.org/chromedriver/downloads)

    接下來就直接開始來寫爬蟲吧!

以下我以Taylor Swift 的維基百科做例子
## Taylor Swift wikipedia crawler

    import selenium
    from selenium import webdriver
    from selenium.webdriver.common.by import By
    from selenium.webdriver.common.keys import Keys
    from selenium.webdriver.support.ui import WebDriverWait
    from selenium.webdriver.support.expected_conditions import presence_of_element_located
    from selenium.webdriver.chrome.options import Options
    import sys
    import time 

    options = Options()             
    options.binary_location = "C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe"
    webdriver_path = 'C:\\Users\\hou\\Desktop\\allison_code\\python\\chromedriver.exe'
    driver = webdriver.Chrome(executable_path=webdriver_path, options=options)

    something = driver.get('https://en.m.wikipedia.org/wiki/Taylor_Swift')

    #名字
    find_name = driver.find_element(By.CSS_SELECTOR, '.fn')   尋找: ".fn" 這個class
    name = find_name.text   摘錄文字內容的部分
    # print(name)   測試用 Python最快檢查的方法

    #照片
    find_picture = driver.find_element(By.CSS_SELECTOR, 'a.image img')  尋找: tag<a>裡面的 image這個class 的img
    picture = find_picture.get_attribute('src') 選取連結的部分
    # print(picture)
    #實際上 <a class = 'image'>
            #<img class ="img">
                #</a>

    #生日
    infobox = {} 設一個dictionary，再放入所有元素
    trs = driver.find_elements(By.CSS_SELECTOR, "table.infobox tr")  先抓所有tr
    for tr in trs :
        # print(tr)
        try :
            th = tr.find_element(By.CSS_SELECTOR, "th")
        except selenium.common.exceptions.NoSuchElementException:
            print("找不到th一次!")
        #為防止若有tr裡沒有th時出錯
            
        if th.text == "Born" :
            td =  tr.find_element(By.CSS_SELECTOR, "td")  尋找: "<td>"這個tag 
            infobox["Born"] = td.text
            print(infobox["Born"])

    #Origin
        if th.text == "Origin" :
            td = tr.find_element(By.CSS_SELECTOR, "td")
            infobox["Origin"] = td.text
            # print(infobox["Origin"])

    #Genres
        if th.text == "Genres" :
            a_s = tr.find_elements(By.CSS_SELECTOR, "td li a")  尋找: <td> 裡面的 <li> 裡面的 <a>
            genres = []  設一個list 
            for a in a_s :
                genres.append(a.text)
            infobox['Genres'] = genres
            # print(infobox['Genres'])

    print(infobox)
    driver.close()

## 備註
* 當你要找一個element時:

driver.find_element(By.CSS_SELECTOR, " ")

* 當一次要找多個elements時:

driver.find_elements(By.CSS_SELECTOR, " ")