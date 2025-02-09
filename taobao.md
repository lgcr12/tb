import random
from itertools import product

from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from selenium.webdriver.support.expected_conditions import visibility_of_element_located
from selenium.webdriver.support.wait import WebDriverWait
"""
commend: all
ie: utf8
initiative_id: tbindexz_20170306
page: 1
q: 鼠标
search_type: item
sourceId: tb.index
spm: a21bo.jianhua/a.201856.d13
ssid: s5-e
tab: all
"""
def get_search():
    driver.find_element(By.XPATH,'//input[@id="q"]').send_keys(product)
    driver.find_element(By.XPATH,'//button[@class="btn-search tb-bg"]').click()
    time.sleep(2)
    page_turn()
    # # 初始化前一个页面高度
    # prev_height = driver.execute_script('return document.body.scrollHeight')
    # time.sleep(2)
    # while True:
    #     try:
    #         ele=driver.find_element(By.XPATH,"//div[@class='helper J_Helper']")
    #         driver.execute_script("arguments[0].scrollIntoView();", ele)
    #         time.sleep(random.randint(1,3))
    #         # 获取当前页面高度
    #         curr_height = driver.execute_script('return document.body.scrollHeight')
    #         # 如果页面高度没有变化，说明已经到达页面底部，退出循环
    #         if curr_height == prev_height:
    #             break
    #         # 更新前一个页面高度
    #         prev_height = curr_height
    #     except:
    #         break
def page_turn():
    for x in range(2,page+1):
        driver.get(f'https://s.taobao.com/search?commend=all&ie=utf8&initiative_id=tbindexz_20170306&page={x}&q={product}&search_type=item&sourceId=tb.index&spm=a21bo.jianhua%2Fa.201856.d13&ssid=s5-e&tab=all')
        time.sleep(5)  # 跳转页面后暂停十秒
        list = driver.find_elements(By.XPATH, '//div[@class="Card--mainPicAndDesc--wvcDXaK"]')
        for item in list:
            title = item.find_element(By.XPATH, './/div[@class="Title--title--jCOPvpf "]/span').text
            price_int = item.find_element(By.XPATH,".//div[@class='Price--priceWrapper--Q0Dn7pN ']/div/span[@class='Price--priceInt--ZlsSi_M']").text
            price_double = item.find_element(By.XPATH,".//div[@class='Price--priceWrapper--Q0Dn7pN ']/div/span[@class='Price--priceFloat--h2RR0RK']").text
            number = item.find_element(By.XPATH,".//div[@class='Price--priceWrapper--Q0Dn7pN ']/span[@class='Price--realSales--FhTZc7U']").text
            print("产品名:", title, "价格:", price_int, "", price_double, "有", number)
        print(len(list))

if __name__ == '__main__':
    #url='https://s.taobao.com/search?commend=all&ie=utf8&initiative_id=tbindexz_20170306&page=1&q={product}&search_type=item&sourceId=tb.index&spm=a21bo.jianhua%2Fa.201856.d13&ssid=s5-e&tab=all'.format(product=product)
    headers={
        "User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36",
        "Referer": "https://www.google.com/"
    }
    print("请输入想要查找的商品名称:")
    product=input()
    print("请输入需要爬取的页数:")
    page=int(input())
    driver = webdriver.Chrome()
    driver.get("https://www.taobao.com/")
    get_search()
