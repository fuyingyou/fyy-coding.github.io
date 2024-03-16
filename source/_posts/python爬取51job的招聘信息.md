---
title: python爬取51job的招聘信息
author: 
tags: 
       - python

category: 
       - 其它

date: 2020-04-29 15:55:07
---
**前言**
最近的脚本课程有了新的作业。爬取51job上的一些招聘信息，包括城市 薪资最大最小值等。

这里示例的是 济南、北京、上海、广州、深圳的招聘信息
**求助**
薪资有些招聘信息并没有填写，也就是说薪资所在标签的值为空值，这些空值无法在集合中占位，进而导致薪资和招聘信息不匹配，如果有大佬会，能不能指点一下啊 ，555 -.-||

招聘信息包括
职位 公司 工作地点 薪水 发布时间 最低薪资 最高薪资 
```js 
from lxml import etree
import requests
import xlwt
import string
#workbook = xlwt.Workbook(encoding='utf-8')#创建 workbook 即新建 excel 文件/工作簿，
myxls = xlwt.Workbook()
#worksheet = workbook.add_sheet('my_worksheet') #创建工作表，如果想创建多个工作表，直接在后面再 add_sheet
sheet1 = myxls.add_sheet(u'top250', cell_overwrite_ok=True)

#请求头
HEADERS = {
    
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36',
}

base_urls = 'https://search.51job.com/list/120200%252C010000%252C020000%252C030200%252C040000,000000,0000,00,9,99,python,2,{}.html'
for x in range(1,51):
    url = base_urls.format(x)
    print('第%s页爬取完成' % x)
    response = requests.get(url,HEADERS)
    text = response.content.decode('gbk')
    tree = etree.HTML(text)

    PositionAndCompany = tree.xpath("//div[@class='el']//span/a/@title")
    positions = PositionAndCompany[::2]
    companys = PositionAndCompany[1::2]
    workplaces = tree.xpath('//div[@class="el"]//span[@class="t3"]/text()')
    salarys = tree.xpath('//div[@class="el"]/span[@class="t4"]/text()')
    times = tree.xpath('//div[@class="el"]/span[@class="t5"]/text()')
    sheet1.write(0,1,"职位")
    sheet1.write(0, 5, "公司")
    sheet1.write(0, 8, "工作地方")
    sheet1.write(0, 10, "薪水")
    sheet1.write(0, 12, "时间")
    sheet1.write(0,14,"最低薪水")
    sheet1.write(0, 16, "最高薪水")
    for i in range(1,len(salarys)):
        pos = positions[i].replace(" ", "").replace("\t", "").replace("\n", "")
        sheet1.write((x-1)*50 + i, 1, pos)
        com = companys[i]
        sheet1.write((x-1)*50 +i, 5, com)
        workplace = workplaces[i]
        sheet1.write((x-1)*50 +i, 8, workplace)
        salary = salarys[i]
        sheet1.write((x-1)*50 +i, 10, salary)
        time = times[i]
        sheet1.write((x-1)*50 +i,12,time)
        peace = salary.split('-')
        #将数值与单位分离
        if(len(peace)> 1):
            peace[0] = float(peace[0])
            #unit 是薪资的单位
            unit = peace[1].replace("0","").replace("1","").replace("2","").replace("3","").replace("4","").replace("5","").replace("6","").replace("7","").replace("8","").replace("8","").replace("9","").replace(".","")
            data1 = peace[1].replace("元","").replace("千", "").replace("万", "").replace("/", "").replace("小时","").replace("天","").replace("月", "").replace("年","").replace("以上","")
            num = float(data1)
            #这里是将薪资的单位统一为 万/月
            if (unit == "元/小时"):
                salary_1 = str(round((data2 * 24 * 30 / 10000),2))
                salary_2 = str(round((num * 24 * 30 / 10000),2))
            elif (unit == "元/天"):
                salary_1 = str(round((peace[0] * 30 / 10000),2))
                salary_2 = str(round((num *30 / 10000),2))
            elif(unit == "千/月"):
                salary_1 = str(round((peace[0]/10),2))
                salary_2 = str(round((num/10),2))
            elif (unit == "万/年"):
                salary_1 = str(round((peace[0] / 12),2))
                salary_2 = str(round((num / 12),2))
            else:
                salary_1 = str(peace[0])
                salary_2 = str(num)
            sheet1.write((x - 1) * 50 + i, 14, salary_1 + "万/月")
            sheet1.write((x - 1) * 50 + i, 16, salary_2 + "万/月")
        else:
            #unit 是薪资的单位
            unit = peace[0].replace("0", "").replace("1", "").replace("2", "").replace("3", "").replace("4","").replace("5", "").replace("6", "").replace("7", "").replace("8", "").replace("8", "").replace("9", "").replace(".", "")
            data2 = peace[0].replace("元","").replace("千", "").replace("万", "").replace("/", "").replace("小时","").replace("天","").replace("月", "").replace("年","").replace("以上","")
            num = float(data2)
            if (unit == "元/小时"):
                salary_3 = str(round((num * 24 * 30 / 10000),2))

            elif(unit == "元/天"):
                salary_3 = str(round(num * 30 / 10000))

            elif (unit == "千/月"):
                salary_3 = str(round(num/10))

            elif(unit == "万/年" or unit == "万以上/年"):
                salary_3 = str(round(num / 12))

            elif (unit == "万/月"):
                salary_3 = str(num)
			#如果提供的单位不是这几个中的一个，输出标记可自行查看！哈哈哈哈
            else:
                print("我也没辙了")
            sheet1.write((x - 1) * 50 + i, 14, salary_3 + "万/月")
            sheet1.write((x - 1) * 50 + i, 16, salary_3 + "万/月")

        #print(len(pos),len(com),len(workplace),len(salary),len(time))
    myxls.save('爬取结果.xls')
```