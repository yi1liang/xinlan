# xinlan
shuju
日线='http://stock2.finance.sina.com.cn/futures/api/json.php/IndexService.getInnerFuturesDailyKLine?symbol='#日线
时线='http://stock2.finance.sina.com.cn/futures/api/json.php/IndexService.getInnerFuturesMiniKLine60m?symbol='#60分钟
物品='wr0'#RB0 螺纹钢 AG0 白银 AU0 黄金 CU0 沪铜 AL0 沪铝 ZN0 沪锌 PB0 沪铅 RU0 橡胶 FU0 燃油 WR0 线材 A0 大豆 M0 豆粕 Y0 豆油 J0 焦炭
            # C0 玉米 L0 乙烯 P0 棕油 V0 PVC RS0 菜籽 RM0 菜粕 FG0 玻璃 CF0 棉花 WS0 强麦 ER0 籼稻 ME0 甲醇 RO0 菜油 TA0 甲酸 

url1=日线

from urllib import request
import json
import pandas as pd


def get_data(id):
    url=url1
    url = url+id
    req = request.Request(url)
    rsp =request.urlopen(req)
    res = rsp.read()
    res_json = json.loads(res)

   
    bar_list = []
    
    #res_json.reverse()      #数据日期排序方向
    for line in res_json:
        bar = {}
        bar['datetime'] = line[0]
        bar['open'] = float(line[1])
        bar['high'] = float(line[2])
        bar['low'] = float(line[3])
        bar['close'] = float(line[4])
        bar['volume'] = int(line[5])
        bar_list.append(bar)
         
    df = pd.DataFrame(data=bar_list)  
    print(df)   
    df.to_csv('D:/data.csv',index=None)

if __name__ =='__main__' :
    get_data(物品)   
