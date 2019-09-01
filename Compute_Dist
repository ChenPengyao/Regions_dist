# *_*coding:utf-8 *_*
import requests
from bs4 import BeautifulSoup
from math import radians, cos, sin, asin, sqrt
import sys
"""
类说明：爬取中国统计局2016年全国各省份城市划分准则及其统计编号
parameters：无
Returns：无
Modify：2018-6-14
注：频繁点击网页的链接可能会导致网页的崩溃从而没有办法实现输出，
而在尝试后，发现在county level设置一个输出可以降低访问网页链接的速度，但同时也增加了程序运行时间
"""

class Inf_Province(object):
    def __init__(self):
        self.serve='http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/2016/'
        self.target='http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/2016/index.html'
        self.pnames=[]
        self.cinames=[]
        self.conames=[]
        self.tnames=[]
        self.vnames=[]
        self.cinames1 = []
        self.conames1 = []
        self.tnames1 = []
        self.purls=[]
        self.ciurls=[]
        self.courls=[]
        self.turls=[]
        self.vurls=[]
        self.pnums=0
        self.cinums=0
        self.conums=0
        self.tnums=0
        self.vnums=0
    """
    函数说明：爬取省份信息以及它的超链接
    Parameters：无
    Returns：无
    Modify：2018-6-14
    """
    def get_province(self):
        req = requests.get(self.target)
        html = req.text.encode("iso-8859-1").decode("gb18030")
        tr_bf = BeautifulSoup(html,"lxml")
        tr = tr_bf.find_all('tr', class_='provincetr')
        a_bf = BeautifulSoup(str(tr),"lxml")
        a = a_bf.find_all('a')
        self.pnums=len(a)
        for each in a:
            self.pnames.append(each.text)
            self.purls.append(self.serve+each.get('href'))

    """
    函数说明：爬取城市信息包括名字和统计代码，以及它的超链接
    Parameters：target-下载链接(string)
    Returns：无
    Modify：2018-6-14
    """
    def get_city(self,target):
        req = requests.get(url=target)
        html = req.text.encode("iso-8859-1").decode("gb18030")
        tr_bf = BeautifulSoup(html,"lxml")
        tr = tr_bf.find_all('tr', class_='citytr')
        a_bf = BeautifulSoup(str(tr),"lxml")
        a = a_bf.find_all('a')
        self.cinums = int(len(a)/2)
        for each in a:
            self.cinames.append(each.text)
            self.ciurls.append(each.get('href'))
        self.ciurls = sorted(set(self.ciurls))
        for i in range(0,len(a),2):
            self.cinames1.append(CN.cinames[i+1])

    """
    函数说明：爬取县区信息包括名字和统计代码，以及它的超链接
    Parameters：target-下载链接(string)
    Returns：无
    Modify：2018-6-14
    """
    def get_county(self,target):
        req = requests.get(url=target)
        html = req.text.encode("iso-8859-1").decode("gb18030")
        tr_bf = BeautifulSoup(html,"lxml")
        tr = tr_bf.find_all('tr', class_='countytr')
        a_bf = BeautifulSoup(str(tr),"lxml")
        a = a_bf.find_all('a')
        self.conums = int(len(a)/2)
        for each in a:
            self.conames.append(each.text)
            self.courls.append(each.get('href'))
        self.courls = sorted(set(self.courls))
        for i in range(0,len(a),2):
            self.conames1.append(CN.conames[i+1])

    """
        函数说明：爬取办事处信息包括名字和统计代码，以及它的超链接
        Parameters：target-下载链接(string)
        Returns：无
        Modify：2018-6-14
        """
    def get_town(self, target):
        req = requests.get(url=target)
        html = req.text.encode("iso-8859-1").decode("gb18030")
        tr_bf = BeautifulSoup(html,"lxml")
        tr = tr_bf.find_all('tr', class_='towntr')
        a_bf = BeautifulSoup(str(tr),"lxml")
        a = a_bf.find_all('a')
        self.tnums = int(len(a)/2)
        for each in a:
            self.tnames.append(each.text)
            self.turls.append(each.get('href'))
        self.turls = sorted(set(self.turls))
        for i in range(0,len(a),2):
            self.tnames1.append(CN.tnames[i+1])

    """
        函数说明：爬取居民委员会信息包括名字和统计代码
        Parameters：target-下载链接(string)
        Returns：无
        Modify：2018-6-14
        """
    def get_village(self, target):
        req = requests.get(url=target)
        html = req.text.encode("iso-8859-1").decode("gb18030")
        tr_bf = BeautifulSoup(html,"lxml")
        tr = tr_bf.find_all('tr', class_='villagetr')
        td_bf = BeautifulSoup(str(tr),"lxml")
        td = td_bf.find_all('td')
        self.vnums = int(len(td))
        for each in td:
            self.vnames.append(each.text)

    """
        函数说明：爬取居民委员会信息包括名字和统计代码
        Parameters：target-下载链接(string)
        Returns：无
        Modify：2018-6-14
        """

    def geocode(self,address):
        parameters = {'address': address,'key': '57933b53d1aabad39474996ce691689a'}
        base = 'http://restapi.amap.com/v3/geocode/geo'
        response = requests.get(base, parameters)
        answer = response.json()
        # print(address + "的经纬度：", answer['geocodes'][0]['location'])
        ad1 = answer['geocodes'][0]['location']
        return ad1

    """
        函数说明：爬取居民委员会信息包括名字和统计代码
        Parameters：target-下载链接(string)
        Returns：无
        Modify：2018-6-14
        """

    def haversine(self,lon1, lat1, lon2, lat2):  # 经度1，纬度1，经度2，纬度2 （十进制度数）
        # 将十进制度数转化为弧度
        lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])
        # haversine公式
        dlon = lon2 - lon1
        dlat = lat2 - lat1
        a = sin(dlat / 2) ** 2 + cos(lat1) * cos(lat2) * sin(dlon / 2) ** 2
        c = 2 * asin(sqrt(a))
        r = 6371.393  # 地球平均半径，单位为公里
        return round(c * r*1000, 6)

if __name__=="__main__":
    CN=Inf_Province()
    CN.get_province()
    print('爬虫开始！')
    """for i in range(CN.pnums):
        print(CN.pnames[i])
        print(CN.purls[i])"""
    with open(u'广东居委会距离.txt','w') as f:
        for i in range(18,19): # 31个省份的循环
            f.write('\n\n------------------省份分割线------------------\n')
            f.write(CN.pnames[i]+'\n') # 广东
            print(CN.pnames[i])
            p_target = CN.purls[i]
            #print(p_target)
            CN.get_city(p_target)  # 广东的市的链接
            for j in range(0,16):  # 东莞和中山需要单独拿出来
                f.write('    '+CN.cinames1[j]+'人民政府'+'\n')
                print(CN.cinames1[j])
                ci_target=p_target.replace('.html','/')[0:-3]+CN.ciurls[j]
                CN.get_county(ci_target) # 直辖区的区的链接
                #print(ci_target)
                for k in range(CN.conums):
                    f.write('        '+CN.cinames1[j]+CN.conames1[k]+'人民政府'+'\n') # 第一个：东城区
                    print(CN.conames1[k])
                    co_target=ci_target.replace('.html','/',1)[0:-5]+CN.courls[k]
                    CN.get_town(co_target)

                    #print('  '+co_target)# 东城区的所有街道办事处链接
                    for l in range(CN.tnums):
                        f.write('            '+CN.conames1[k]+CN.tnames1[l]+'办事处'+'\n')# 输出所有街道办事处
                        #print(CN.tnames1[l])
                        t_target=co_target.replace('.html','/',1)[0:-7]+CN.turls[l]
                        CN.get_village(t_target)
                        a=CN.geocode(address=CN.conames1[k]+CN.tnames1[l]+'办事处')
                        longitude1=float(a[0:10])
                        latitude1=float(a[11:20])
                        tname1=[]
                        for m in range(0,CN.vnums,3):
                            tname1.append(CN.conames1[k]+CN.tnames1[l]+CN.vnames[m+2])
                        for n in range(len(tname1)):
                            b=CN.geocode(tname1[n])
                            longitude2=float(b[0:10])
                            latitude2=float(b[11:20])
                            c=CN.haversine(longitude1, latitude1, longitude2, latitude2)
                            f.write("                "+tname1[n] + " " + b +" "+"到上一级办事处的距离:"+" "+str(c) +"m"+'\n')
                            del longitude2
                            del latitude2
                        tname1.clear()
                        del longitude1
                        del latitude1
                        CN.vnames.clear()
                    CN.tnames1.clear()
                    CN.tnames.clear()
                    CN.turls.clear()
                CN.conames1.clear()
                CN.conames.clear()
                CN.courls.clear()
            CN.cinames1.clear()
            CN.cinames.clear()
            CN.ciurls.clear()
    print('爬虫结束！')
del(CN)

