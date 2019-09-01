#-*- coding:utf-8 -*-
'''
利用高德地图api实现地址和经纬度的转换
'''

import requests
def geocode(address):
    parameters = {'address': address, 'key': '57933b53d1aabad39474996ce691689a'}
    base = 'http://restapi.amap.com/v3/geocode/geo'
    response = requests.get(base, parameters)
    answer = response.json()
    #print(address + "的经纬度：", answer['geocodes'][0]['location'])
    a=answer['geocodes'][0]['location']
    return a

address='广州市荔湾区岭南街道扬仁西社区居民委员会'
b=geocode(address)
print(b[0:10],b[11:20])




if __name__=='__main__':
    with open(u'广东经纬度.txt', 'a') as ft:
        f = open(r"C:\Users\John\.PyCharmCE2018.1\config\scratches\广东2.txt")
        lines = f.readlines()
        ads=[]
        print('Started')
        for line in lines:
            ads.append(line.replace('\n',' '))
        t=int(len(ads))
        ft.write('---------------分割线-------------------')
        for i in range(t):
            b=geocode(ads[i])

            ft.write(ads[i]+" "+b+'\n')
            print('Processed:'+str(round(i/t*100,2))+'%\n')
            del b
        print('Finished!!')

'''
利用经纬度来对距离进行估计
'''

from math import radians, cos, sin, asin, sqrt
def haversine(lon1, lat1, lon2, lat2):  # 经度1，纬度1，经度2，纬度2 （十进制度数）
    # 将十进制度数转化为弧度
    lon1, lat1, lon2, lat2 = map(radians, [lon1, lat1, lon2, lat2])
    # haversine公式
    dlon = lon2 - lon1
    dlat = lat2 - lat1
    a = sin(dlat / 2) ** 2 + cos(lat1) * cos(lat2) * sin(dlon / 2) ** 2
    c = 2 * asin(sqrt(a))
    r = 6378.1  # 地球平均半径，单位为公里
    return round(c * r,6)
print(haversine(113.264385,23.12911,113.247348,23.107249))
