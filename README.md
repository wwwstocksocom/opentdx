直接解析 通达信行情服务器协议

remote:https://github.com/freestockso/opentdx


local: F:\stock\opentdx>

blog:https://www.stockso.com/blogpost-6a07569fa173f22e4e9d2b02-opentdx-httpsgithubcomfreestocksoopentdx

参数及返回值

https://github.com/freestockso/opentdx/blob/main/opentdx/tdxClient.py

for example

~~~

  def stock_list(self, market: MARKET, start = 0, count = 0) -> list[dict]:
        '''
        获取股票列表
        Args:
            market: MARKET - 市场类型 (SZ: 深圳, SH: 上海, BJ: 北交所)
            start: int     - 起始位置，默认为0
            count: int     - 获取数量，默认为0（获取全部）
        Return: 
            List[Dict]: 股票信息列表，每个元素包含：
                - code: str      - 股票代码
                - name: str      - 股票名称
                - pre_close: int - 昨日收盘价
        '''
        return self.q_client().get_list(market, start, count)

~~~

## test 202605

PS F:\stock\opentdx>  tdxdata_test opentdx.py


~~~
 # 股票列表（带排序过滤）
    print(pd.DataFrame(client.stock_quotes_list(CATEGORY.A, sort_type=SORT_TYPE.TOTAL_AMOUNT)))
~~~

output

~~~
market    code    close     open     high      low  pre_close  ... short_turnover  min2_amount  opening_rush  vol_rise_speed    depth  active   turnover0
MARKET.SZ  300308  1049.87  1068.99  1099.87  1034.00    1078.00  ...          0.06%  294744064.0         0.00%           0.86%  -16.02%    4756   24925.611
MARKET.SH  688008   247.98   265.00   272.00   242.63     265.08  ...          0.18%  178872320.0         0.00%           0.80%  -55.36%    4755   99900.522

MARKET.SZ  300394   399.99   401.00   414.08   381.91     392.00  ...          0.36%  346271744.0        -0.46%           1.11%  -69.18%    4765   86946.683
MARKET.SZ  300274   152.95   136.00   155.99   131.02     135.03  ...          0.21%  237209600.0         0.00%           0.85%  -79.84%    4778  109094.284
MARKET.SH  601138    67.62    69.50    71.76    67.37      68.60  ...          0.17%  155734016.0         0.00%           0.77%   43.83%    4744   17262.97..
 ...     ...      ...      ...      ...      ...        ...  ...            ...          ...           ...             ...      ...     ...        ...75
MARKET.SZ  300124    77.32    74.49    79.88    74.49      74.42  ...          0.05%   26427904.0         0.00%           0.62%   -7.02%    4747   29836.0776
MARKET.SZ  002015    21.71    23.11    23.60    21.20      23.50  ...          0.55%   36787200.0         2.53%           0.89%  -66.83%    4743  154027.7277
MARKET.SH  688072   529.94   493.00   565.00   472.06     493.00  ...          0.07%   21304832.0         0.00%           0.59%  -54.67%    4386   37391.4478
MARKET.SZ  000063    36.71    37.64    38.29    36.58      37.56  ...          0.09%   51426816.0         0.21%           0.91%   73.82%    4742   36489.8579
MARKET.SZ  300757   589.98   605.00   620.00   580.79     589.00  ...          0.19%   57696256.0        -0.15%           1.01%  -56.29%    4390   60693.20

~~~



~~~
from datetime import date

import pandas as pd
from opentdx.tdxClient import TdxClient
from opentdx.const import MARKET, CATEGORY, EX_MARKET, PERIOD, SORT_TYPE

if __name__ == "__main__":
  with TdxClient() as client:
 
    # 历史成交
    print(pd.DataFrame(client.stock_transaction(MARKET.SZ, '301487', date(2026, 4, 28))))    
~~~

output

~~~
          time  price   vol   action  unknown
0     09:25:00  23.00   163  NEUTRAL        0
1     09:30:00  23.11   155      BUY        0
2     09:30:00  23.11    62      BUY        0
3     09:30:00  23.04    56     SELL        0
4     09:30:00  23.02    64     SELL        0
...        ...    ...   ...      ...      ...
3406  14:56:00  22.81     6      BUY        0
3407  14:56:00  22.78     4     SELL        0
3408  14:56:00  22.80     3      BUY        0
3409  14:57:00  22.78     1     SELL        0
3410  15:00:00  22.78  1424  NEUTRAL        0

[3411 rows x 5 columns]

~~~

获取个股F10

~~~
from datetime import date

import pandas as pd
from opentdx.tdxClient import TdxClient
from opentdx.const import MARKET, CATEGORY, EX_MARKET, PERIOD, SORT_TYPE

if __name__ == "__main__":
  with TdxClient() as client:
 
    # 获取个股F10
    print(pd.DataFrame(client.stock_f10(MARKET.SZ, '301487'))) 
~~~

output

~~~
    name                                            content
0   最新提示  最新提示☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
1   公司概况  公司概况☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
2   财务分析  财务分析☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
3   股本结构  股本结构☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
4   股东研究  股东研究☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
5   机构持股  机构持股☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
6   分红融资  分红融资☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
7   高管治理  高管治理☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
8   资金动向  资金动向☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
9   资本运作  资本运作☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
10  热点题材  热点题材☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
11  公司公告  公司公告☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
12  公司报道  公司报道☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
13  经营分析  经营分析☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
14  行业分析  行业分析☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
15  研报评级  研报评级☆ ◇301487 盟固利 更新日期：2026-05-15◇ 通达信沪深京F10\r...
16  除权分红  [{'market': MARKET.SZ, 'code': b'301487', 'dat...
17    财报  {'market': MARKET.SZ, 'code': '301487', 'liuto...

~~~




### 股票列表

~~~

  def stock_list(self, market: MARKET, start = 0, count = 0) -> list[dict]:
        '''
        获取股票列表
        Args:
            market: MARKET - 市场类型 (SZ: 深圳, SH: 上海, BJ: 北交所)
            start: int     - 起始位置，默认为0
            count: int     - 获取数量，默认为0（获取全部）
        Return: 
            List[Dict]: 股票信息列表，每个元素包含：
                - code: str      - 股票代码
                - name: str      - 股票名称
                - pre_close: int - 昨日收盘价
        '''
        return self.q_client().get_list(market, start, count)

~~~

~~~
print(pd.DataFrame(client.stock_list(MARKET.SZ,count=0)))

         code  vol          name  decimal_point     pre_close                             unknown1
17124  300010  100          ST豆神              2      2.940000       [4565.4619140625, 12656, 9616]
17125  300011  100          鼎汉技术              2      6.960000    [361.28167724609375, 12644, 9600]
17126  300012  100          华测检测              2     15.520000      [1021.45947265625, 12636, 9609]
17127  300013  100          新宁物流              2      4.610000      [1632.06982421875, 12683, 9611]
17128  300014  100          亿纬锂能              2     65.300003      [3597.39404296875, 12670, 9624]
17129  300015  100          爱尔眼科              2     10.780000     [3845.337646484375, 12644, 9576]
17130  300016  100          北陆药业              2      9.080000     [789.2800903320312, 12679, 9624]
17131  300017  100          网宿科技              2     18.700001       [11134.431640625, 12671, 9617]
17132  300018  100          中元股份              2     15.000000    [1285.7086181640625, 12638, 9607]

~~~



~~~
print(pd.DataFrame(client.stock_list(MARKET.SZ)))

         code  vol       name  decimal_point  pre_close                    unknown1
0      395001  100       主板Ａ股              2     1493.0         [34391.90625, 0, 0]
1      395002  100       主板Ｂ股              2       38.0  [1874.2574462890625, 0, 0]
2      395004  100        创业板              2     1398.0     [2375.7333984375, 0, 0]
3      395005  100       主板DR              2        0.0                 [0.0, 0, 0]
4      395006  100      创业板DR              2        0.0                 [0.0, 0, 0]
...       ...  ...        ...            ...        ...                         ...
23263  159063  100    粮食ETF南方              3        0.0                 [0.0, 0, 0]
23264  159319  100    粮食产业ETF              3        0.0                 [0.0, 0, 0]
23265  159585  100    软件ETF富国              3        0.0                 [0.0, 0, 0]
23266  159756  100  BOCI电池ETF              3        0.0                 [0.0, 0, 0]
23267  159799  100   港股通消费ETF              3        0.0                 [0.0, 0, 0]

[23268 rows x 6 columns]

print(pd.DataFrame(client.index_momentum(MARKET.SZ, '399001')))

        0
0    -133
1    -832
2     -29
3   -1166
4   -1635
..    ...
235    83
236   809
237   254
238   254
239  -538

[240 rows x 1 columns]

print(pd.DataFrame(client.index_momentum(MARKET.SH, '999999')))

        0
0     -69
1     556
2    -184
3    -897
4   -1420
..    ...
235   152
236   966
237   518
238   518
239  -614

[240 rows x 1 columns]

print("获取指数概况")
print(pd.DataFrame(client.index_info([(MARKET.SZ, '399001'), (MARKET.SH, '999999')])))

      market    code  active  pre_close    diff     close      open      high       low        vol        amount  up_count  down_count
0  MARKET.SZ  399001    4748   15745.74 -184.37  15561.37  15753.19  15855.55  15447.40  849695631  1.825236e+12       971        1894
1  MARKET.SH  999999    4747    4177.92  -42.53   4135.39   4174.17   4191.81   4114.09  733162357  1.519240e+12       753        1543
               datetime     open    close     high      low          vol        amount  up_count  down_count  turnover
0   2023-01-19 15:00:00  3221.52  3240.28  3240.28  3210.38  226465024.0  2.968156e+11      1566         547     47.08
1   2023-01-20 15:00:00  3247.20  3264.81  3267.06  3247.20  273219168.0  3.290282e+11      1604         562     56.80
2   2023-01-30 15:00:00  3308.87  3269.32  3310.49  3266.76  353325824.0  4.531770e+11      1490         655     73.45
3   2023-01-31 15:00:00  3266.14  3255.67  3277.25  3252.01  300162944.0  3.752607e+11      1173         962     62.40
4   2023-02-01 15:00:00  3262.20  3284.92  3284.92  3245.41  339509408.0  4.133952e+11      1905         259     70.58
..                  ...      ...      ...      ...      ...          ...           ...       ...         ...       ...
795 2026-05-11 15:00:00  4201.35  4225.02  4229.58  4186.08  762736256.0  1.584367e+12      1363         916    158.57
796 2026-05-12 15:00:00  4229.28  4214.49  4230.18  4199.34  718984000.0  1.465273e+12       625        1686    149.47
797 2026-05-13 15:00:00  4192.31  4242.57  4245.07  4192.31  705018752.0  1.447464e+12      1299         975    146.57
798 2026-05-14 15:00:00  4256.16  4177.92  4258.86  4177.92  764235008.0  1.498117e+12       470        1844    158.88
799 2026-05-15 15:00:00  4174.18  4135.39  4191.81  4114.09  733162368.0  1.519240e+12       753        1543    152.42

[800 rows x 10 columns]


获取K线数据
print(pd.DataFrame(client.stock_kline(MARKET.SH, '999999', PERIOD.DAILY)))
print(pd.DataFrame(client.stock_kline(MARKET.SH, '999999', PERIOD.MINS, times=10)))

               datetime     open    close     high      low          vol        amount  up_count  down_count  turnover
0   2026-03-25 13:50:00  3928.23  3926.66  3932.55  3926.66  251823312.0  2.518234e+10      2092         229     52.35
1   2026-03-25 14:00:00  3927.12  3924.98  3928.24  3919.68  228756320.0  2.287564e+10      1970         335     47.56
2   2026-03-25 14:10:00  3925.35  3921.44  3927.44  3920.89  207785216.0  2.077853e+10      2006         305     43.20
3   2026-03-25 14:20:00  3921.10  3919.65  3922.80  3916.08  252423008.0  2.524231e+10      1909         396     52.48
4   2026-03-25 14:30:00  3919.22  3920.14  3922.05  3918.63  202894416.0  2.028945e+10      1966         347     42.18
..                  ...      ...      ...      ...      ...          ...           ...       ...         ...       ...
795 2026-05-15 14:20:00  4126.13  4129.88  4136.04  4124.86  340451136.0  3.404512e+10       659        1650     70.78
796 2026-05-15 14:30:00  4129.48  4121.36  4129.91  4120.85  317720384.0  3.177205e+10       633        1680     66.05
797 2026-05-15 14:40:00  4120.97  4123.79  4124.94  4114.09  483141824.0  4.831420e+10       543        1776    100.44
798 2026-05-15 14:50:00  4123.95  4134.62  4141.34  4123.18  465770944.0  4.657711e+10       721        1585     96.83
799 2026-05-15 15:00:00  4134.63  4135.39  4135.85  4132.47  563534016.0  5.635341e+10       744        1556    117.16

[800 rows x 10 columns]

获取分时图
print(pd.DataFrame(client.stock_tick_chart(MARKET.SH, '999999')))

       price        avg      vol
0    4177.73  4185.1268  3965325
1    4179.18  4186.9500  3097651
2    4172.90  4184.4749  2482193
3    4171.81  4183.0755  2082249
4    4168.14  4179.8363  1917860
..       ...        ...      ...
235  4134.25  4152.6932   755572
236  4135.62  4154.7591   813989
237  4135.84  4154.9906    46933
238  4135.84  4154.9906        0
239  4135.39  4153.8060  1163234

[240 rows x 3 columns]

print(pd.DataFrame(client.stock_tick_chart(MARKET.SZ, '000001')))

     price      avg    vol
0    11.03  11.0327  44595
1    11.03  11.0298  17581
2    11.02  11.0286   9983
3    11.02  11.0268   9605
4    11.02  11.0262   6524
..     ...      ...    ...
235  10.99  11.0255   6268
236  10.99  11.0254   4034
237  10.99  11.0254      7
238  10.99  11.0254      0
239  10.99  11.0250  10564

[240 rows x 3 columns]

print(pd.DataFrame(client.stock_tick_chart(MARKET.SZ, '000001', date(2026, 3, 16))))

     price      avg    vol
0    10.92  10.9233  23457
1    10.94  10.9299  15292
2    10.94  10.9323  12413
3    10.94  10.9333   7064
4    10.93  10.9330   4184
..     ...      ...    ...
235  10.92  10.9300   4986
236  10.92  10.9298  10757
237  10.90  10.9295   8307
238  10.90  10.9295      0
239  10.92  10.9294   7581

[240 rows x 3 columns]


获取股票详细报价
print(pd.DataFrame(client.stock_quotes_detail(MARKET.SZ, '000001')))

      market    code  close   open   high    low  ...                                           handicap           unknown  rise_speed  active1  active2  turnover
0  MARKET.SZ  000001  10.99  11.05  11.11  10.96  ...  {'bid': [{'price': 10.99, 'vol': 691}, {'price...  0001001100010110       0.09%     4512     4512   5022.99


      market    code  close   open  high    low  pre_close   server_time  neg_price    vol  cur_vol       amount  s_vol  b_vol  s_amount  open_amount                                                                                                                                                                                                                                                                                                              handicap           unknown rise_speed  active1  active2  turnover
0  MARKET.SZ  300298  16.83  16.81  17.2  16.74      16.84  15:33:07.908        0.0  59168      683  100566864.0  28467  30702         0       497600  {'bid': [{'price': 16.82, 'vol': 130}, {'price': 16.81, 'vol': 64}, {'price': 16.8, 'vol': 68}, {'price': 16.79, 'vol': 2}, {'price': 16.78, 'vol': 1176}], 'ask': [{'price': 16.83, 'vol': 35}, {'price': 16.84, 'vol': 24}, {'price': 16.85, 'vol': 71}, {'price': 16.86, 'vol': 10}, {'price': 16.87, 'vol': 1}]}  0001010110010110      0.00%     2760     2760  13121.82

[1 rows x 22 columns]

print("获取股票排行榜")
print(pd.DataFrame(client.stock_top_board()))

                                             increase  ...                                           turnover
0   {'market': MARKET.BJ, 'code': '920178', 'price...  ...  {'market': MARKET.SZ, 'code': '300308', 'price...
1   {'market': MARKET.SZ, 'code': '001393', 'price...  ...  {'market': MARKET.SH, 'code': '688008', 'price...
2   {'market': MARKET.SH, 'code': '688549', 'price...  ...  {'market': MARKET.SZ, 'code': '300394', 'price...
3   {'market': MARKET.SZ, 'code': '301696', 'price...  ...  {'market': MARKET.SZ, 'code': '300274', 'price...
4   {'market': MARKET.SH, 'code': '688697', 'price...  ...  {'market': MARKET.SH, 'code': '601138', 'price...
5   {'market': MARKET.SZ, 'code': '300263', 'price...  ...  {'market': MARKET.SH, 'code': '603986', 'price...
6   {'market': MARKET.SZ, 'code': '300878', 'price...  ...  {'market': MARKET.SZ, 'code': '300502', 'price...
7   {'market': MARKET.SZ, 'code': '300721', 'price...  ...  {'market': MARKET.SZ, 'code': '002384', 'price...
8   {'market': MARKET.SZ, 'code': '301081', 'price...  ...  {'market': MARKET.SH, 'code': '688256', 'price...
9   {'market': MARKET.SZ, 'code': '300276', 'price...  ...  {'market': MARKET.SZ, 'code': '300476', 'price...
10  {'market': MARKET.SH, 'code': '688820', 'price...  ...  {'market': MARKET.SH, 'code': '688041', 'price...
11  {'market': MARKET.SH, 'code': '688143', 'price...  ...  {'market': MARKET.SZ, 'code': '002281', 'price...
12  {'market': MARKET.BJ, 'code': '920100', 'price...  ...  {'market': MARKET.SH, 'code': '688981', 'price...
13  {'market': MARKET.SH, 'code': '688507', 'price...  ...  {'market': MARKET.SH, 'code': '688012', 'price...
14  {'market': MARKET.SH, 'code': '688535', 'price...  ...  {'market': MARKET.SZ, 'code': '001309', 'price...
15  {'market': MARKET.SH, 'code': '688017', 'price...  ...  {'market': MARKET.SH, 'code': '600522', 'price...
16  {'market': MARKET.SH, 'code': '688700', 'price...  ...  {'market': MARKET.SZ, 'code': '002050', 'price...
17  {'market': MARKET.SH, 'code': '688409', 'price...  ...  {'market': MARKET.SZ, 'code': '002475', 'price...
18  {'market': MARKET.SZ, 'code': '301538', 'price...  ...  {'market': MARKET.SH, 'code': '601899', 'price...
19  {'market': MARKET.SZ, 'code': '300508', 'price...  ...  {'market': MARKET.SH, 'code': '600584', 'price...

[20 rows x 9 columns]

print("获取各类股票行情列表")
print(pd.DataFrame(client.stock_quotes_list(CATEGORY.A, count = 0, sort_type=SORT_TYPE.TOTAL_AMOUNT)))

         market    code    close     open     high      low  pre_close  ... short_turnover  min2_amount  opening_rush  vol_rise_speed    depth  active   turnover
0     MARKET.SZ  300308  1049.87  1068.99  1099.87  1034.00    1078.00  ...          0.06%  294744064.0         0.00%           0.86%  -16.02%    4756   24925.61
1     MARKET.SH  688008   247.98   265.00   272.00   242.63     265.08  ...          0.18%  178872320.0         0.00%           0.80%  -55.36%    4755   99900.52
2     MARKET.SZ  300394   399.99   401.00   414.08   381.91     392.00  ...          0.36%  346271744.0        -0.46%           1.11%  -69.18%    4765   86946.68
3     MARKET.SZ  300274   152.95   136.00   155.99   131.02     135.03  ...          0.21%  237209600.0         0.00%           0.85%  -79.84%    4778  109094.28
4     MARKET.SH  601138    67.62    69.50    71.76    67.37      68.60  ...          0.17%  155734016.0         0.00%           0.77%   43.83%    4744   17262.97
...         ...     ...      ...      ...      ...      ...        ...  ...            ...          ...           ...             ...      ...     ...        ...
5518  MARKET.SZ  002808     0.00     0.00     0.00     0.00       2.83  ...          0.00%          0.0         0.00%           0.00%    0.00%       0        NaN
5519  MARKET.SZ  002731     0.00     0.00     0.00     0.00       4.35  ...          0.00%          0.0         0.00%           0.00%    0.00%       0        NaN
5520  MARKET.SZ  002629     0.00     0.00     0.00     0.00       7.66  ...          0.00%          0.0         0.00%           0.00%    0.00%       0        NaN
5521  MARKET.SZ  000638     0.00     0.00     0.00     0.00       0.89  ...          0.00%          0.0         0.00%           0.00%    0.00%       0        NaN
5522  MARKET.SZ  000430     0.00     0.00     0.00     0.00       8.26  ...          0.00%          0.0         0.00%           0.00%    0.00%       0        NaN

[5523 rows x 26 columns]

print("获取股票报价")
print(pd.DataFrame(client.stock_quotes(MARKET.SZ, '000001')))

      market    code  close   open   high    low  pre_close  ... short_turnover  min2_amount  opening_rush  vol_rise_speed   depth  active  turnover
0  MARKET.SZ  000001  10.99  11.05  11.11  10.96      11.05  ...          0.03%   11609088.0         0.00%           1.03%  41.71%    4512   5022.99

[1 rows x 26 columns]

~~~

### 获取异动数据

~~~
    def stock_unusual(self, market: MARKET, start: int = 0, count: int = 0) -> list[dict]:
        '''
        获取异动数据
        Args:
            market: MARKET - 市场类型 (SZ: 深圳, SH: 上海, BJ: 北交所)
            start: int      - 起始位置，默认为0
            count: int      - 获取数量，默认为0（获取全部）
        Return: 
            List[Dict]: 股票信息列表，每个元素包含：
                - index: int     - 序号
                - market: MARKET - 市场类型 (SZ: 深圳, SH: 上海, BJ: 北交所)
                - code: str      - 股票代码
                - time: time     - 时间
                - desc: str      - 异动类型
                - value: str     - 异动值
        '''
        return self.q_client().get_unusual(market, start, count)
~~~

~~~

print("获取异动数据")

print(pd.DataFrame(client.stock_unusual(MARKET.SZ)))

       index     market    code      time  desc            value  unusual_type
0          0  MARKET.SZ  000159  09:15:00  逼近涨停     7.55/3512.00            20
1          1  MARKET.SZ  000803  09:15:00  封跌停板       7.61/11.00            20
2          2  MARKET.SZ  000818  09:15:00  封涨停板      19.31/62.00            20
3          3  MARKET.SZ  000819  09:15:00  封跌停板       15.37/6.00            20
4          4  MARKET.SZ  001317  09:15:00  封跌停板       52.51/3.00            20
...      ...        ...     ...       ...   ...              ...           ...
11558  11558  MARKET.SZ  300136  15:00:03  大单托盘  99.60/201600.00            16
11559  11559  MARKET.SZ  300580  15:00:03  尾盘拉升    0.59%/1106.00            21
11560  11560  MARKET.SZ  300760  15:00:03  大单托盘  160.32/31300.00            16
11561  11561  MARKET.SZ  301196  15:00:03  大单压盘  155.00/35000.00            17
11562  11562  MARKET.SZ  301387  15:00:03  尾盘拉升     0.65%/861.00            21

[11563 rows x 7 columns]

~~~

### 获取竞价数据

~~~
def stock_auction(self, market: MARKET, code: str) -> list[dict]:
        '''
        获取竞价数据
        Args:
            market: MARKET - 市场类型 (SZ: 深圳, SH: 上海, BJ: 北交所)
            code: str      - 指数代码
        Return: 
            List[Dict]: 股票竞价列表，每个元素包含：
                - time: time        - 时间
                - price: float      - 撮合价
                - matched: int      - 匹配量
                - unmatched: int    - 未匹配量
        '''
        return self.q_client().get_auction(market, code)
~~~

~~~

print("获取竞价数据")
print(pd.DataFrame(client.stock_auction(MARKET.SZ, '300308')))

        time       price  matched   unmatched
0   09:15:00  1078.00000       49          33
1   09:15:09  1078.00000       99          55
2   09:15:18  1078.00000      103          51
3   09:15:27  1078.00000      105          50
4   09:15:36  1078.00000      105          51
..       ...         ...      ...         ...
82  14:59:15  1048.98999     1336           5
83  14:59:24  1048.98999     1405          11
84  14:59:33  1049.00000     1501  4294966797
85  14:59:42  1049.00000     1559  4294966791
86  14:59:51  1049.00000     1608  4294966763

[87 rows x 4 columns]

print("获取历史委托数据")
print(pd.DataFrame(client.stock_history_orders(MARKET.SZ, '000001', date(2026, 3, 16))))

     price  unknown    vol
0    10.92       33  23457
1    10.94       66  15292
2    10.94       24  12413
3    10.94        9   7064
4    10.93       -2   4184
..     ...      ...    ...
235  10.92       -1   4986
236  10.92       -2  10757
237  10.90       -3   8307
238  10.90        0      0
239  10.92       -1   7581

[240 rows x 3 columns]

print("获取历史成交数据")
print(pd.DataFrame(client.stock_transaction(MARKET.SZ, '000001')))

          time  price    vol  trans   action  unknown
0     09:25:00  11.05   5018    384  NEUTRAL        0
1     09:30:00  11.05    697     77     SELL        0
2     09:30:00  11.05   5884    364      BUY        0
3     09:30:00  11.05   3279    247     SELL        0
4     09:30:00  11.04   1503    123     SELL        0
...        ...    ...    ...    ...      ...      ...
4507  14:56:00  10.99     31      4      BUY        0
4508  14:56:00  10.99    114     19      BUY        0
4509  14:56:00  10.99    120     17      BUY        0
4510  14:57:00  10.99      7      3      BUY        0
4511  15:00:00  10.99  10564    345  NEUTRAL        0

[4512 rows x 6 columns]

print(pd.DataFrame(client.stock_transaction(MARKET.SZ, '000001', date(2026, 3, 16))))

          time  price   vol   action  unknown
0     09:25:00  10.93  9020  NEUTRAL        0
1     09:30:00  10.92  1129     SELL        0
2     09:30:00  10.91  1434     SELL        0
3     09:30:00  10.92  7198      BUY        0
4     09:30:00  10.92   561      BUY        0
...        ...    ...   ...      ...      ...
4177  14:56:00  10.92    15     SELL        0
4178  14:56:00  10.92   411     SELL        0
4179  14:56:00  10.92    24      BUY        0
4180  14:57:00  10.90  8307     SELL        0
4181  15:00:00  10.92  7581  NEUTRAL        0

[4182 rows x 5 columns]

print("获取股票分时缩略")
print(pd.DataFrame(client.stock_chart_sampling(MARKET.SZ, '000001')))

        0
0   11.05
1   11.02
2   11.05
3   11.03
4   11.04
..    ...
56  10.99
57  10.98
58  10.98
59  10.99
60  10.99
[61 rows x 1 columns]

 print("获取F10数据")
# print(pd.DataFrame(client.stock_f10(MARKET.SZ, '000001')))

    name                                            content
0   最新提示  最新提示☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
1   公司概况  公司概况☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
2   财务分析  财务分析☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
3   股本结构  股本结构☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
4   股东研究  股东研究☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
5   机构持股  机构持股☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
6   分红融资  分红融资☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
7   高管治理  高管治理☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
8   资金动向  资金动向☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
9   资本运作  资本运作☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
10  热点题材  热点题材☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
11  公司公告  公司公告☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
12  公司报道  公司报道☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
13  经营分析  经营分析☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
14  行业分析  行业分析☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
15  研报评级  研报评级☆ ◇000001 平安银行 更新日期：2026-05-17◇ 通达信沪深京F10\...
16  除权分红  [{'market': MARKET.SZ, 'code': b'000001', 'dat...
17    财报  {'market': MARKET.SZ, 'code': '000001', 'liuto...
PS D:\Thirdprogram\newtdxtqv772\PYPlugins\user> 

 公司概况->主营业务、经营范围 

~~~

~~~


    def stock_f10(self, market: MARKET, code: str) -> list[dict]:
  '''      
        获取F10数据
        Args:
            market: MARKET - 市场类型 (SZ: 深圳, SH: 上海, BJ: 北交所)
            code: str      - 指数代码
        Return: 
            List[Dict]:  股票公司信息
                - name: str
                - content: str | dict
  '''       
        return self.q_client().get_company_info(market, code)


 see:https://github.com/freestockso/opentdx/blob/main/opentdx/tdxClient.py
~~~



# opentdx — Python TDX 量化行情数据接口

项目创意来自[`pytdx`](https://github.com/rainx/pytdx)

感谢[@rainx](https://github.com/rainx)迈出的第一步

### ✨ 声明

> 本项目为个人**学习项目，并非已完成的开箱即用的产品**，仅用于学习交流
>
> 对于数据有迫切需求的朋友，通达信新推出了[官方量化平台](https://help.tdx.com.cn/quant/)，建议食用。

> 由于项目连接的是通达信客户端明文公开的服务器，是财富趋势科技公司既有的行情软件兼容行情服务器，只是简单整理便于大家学习，**严禁**用于任何**商业用途**，更**严禁滥用接口**，对此造成的任何问题本人概不负责。

又因本项目在持续推进中，接口**难免会有大幅改动，带来的不便请予宽宥**。

> ### 应biner建议，本项目精简为基础数据接口库，mcp相关将移动到 [tdx_mcp](https://github.com/LisonEvf/tdx_mcp)
> ### 又因pytdx2库名rainx已经用了，因此本库改名为opentdx，再次致敬rainx
> ### 又又，协议基本完成解析了，后期就着力于 [tdx_mcp](https://github.com/LisonEvf/tdx_mcp)了和少量组合技接口


## 主要功能

| 功能 | 说明 | 新增 |
|------|------| -|
| 股票行情 | A股、创业板、科创板、北交所 | ✅支持北交所 | 
| 扩展行情 | 期货、港股、美股、期权等 | ✅支持AH股关联查询 |
| K线数据 | 多周期（1分/5分/日线/周线等）|  ✅支持复权、即时换手率 |
| 分时图 | 实时/历史分时数据 | |
| 排行榜 | 涨跌幅、振幅、换手率等 |  |
| 板块数据 | 行业/地区/概念板块列表及成分股 | 🌟 板块K线数据 | 
| 异动监控 | 主力监控精灵数据 | |
| F10资料 | 公司基本信息、财报 | |

## 安装

```bash
pip install opentdx

```

## 指南

```bash
opentdx doc 
```

## 快速上手

```python
from datetime import date

import pandas as pd
from opentdx.tdxClient import TdxClient
from opentdx.const import MARKET, CATEGORY, EX_MARKET, PERIOD, SORT_TYPE

if __name__ == "__main__":
  with TdxClient() as client:
    # 指数信息
    print(pd.DataFrame(client.index_info([(MARKET.SH, '999999'), (MARKET.SZ, '399001')])))
    # 股票列表（带排序过滤）
    print(pd.DataFrame(client.stock_quotes_list(CATEGORY.A, sortType=SORT_TYPE.TOTAL_AMOUNT)))
    # 股票报价
    print(pd.DataFrame(client.stock_quotes(MARKET.SZ, '000001')))
    # 获取行情全景
    for name, board in client.stock_top_board().items():
        print(f"榜单：{name}")
        print(pd.DataFrame(board))
    # 获取k线
    print(pd.DataFrame(client.stock_kline(MARKET.SZ, '000001', PERIOD.DAILY)))
    # 获取指数k线
    print(pd.DataFrame(client.stock_kline(MARKET.SH, '999999', PERIOD.MINS, times=10)))
    # 获取历史分时
    print(pd.DataFrame(client.stock_tick_chart(MARKET.SZ, '000001', date(2026, 3, 16))))
    # 获取个股F10
    print(pd.DataFrame(client.stock_f10(MARKET.SZ, '000001')))
    # 历史成交
    print(pd.DataFrame(client.stock_transaction(MARKET.SZ, '000001', date(2024, 1, 15))))
    
    # 期货K线
    print(pd.DataFrame(client.goods_kline(EX_MARKET.SH_FUTURES, 'AUL8', PERIOD.DAILY)))
    # 获取扩展市场行情列表
    print(pd.DataFrame(client.goods_quotes_list(EX_MARKET.SH_FUTURES, count=5)))
    # 获取美股K线
    print(pd.DataFrame(client.goods_kline(EX_MARKET.US_STOCK, 'TSLA', PERIOD.DAILY)))
    # 美股行情
    print(pd.DataFrame(client.goods_quotes(EX_MARKET.US_STOCK, 'TSLA')))
```

### 🌟 本项目亮点

- ✅ **整体重构**：更加简洁易读
- ✅ **协议简化**：明确了一些协议的细节，更加清晰易懂
- ✅ **自动选服**：自动检查服务器连接速度，并选择最快的服务器
- ✅ **主力监控**：新增异动消息的获取
- ✅ **板块列表**：像 `通达信`一样根据板块获取股票列表，支持 `深市`、`沪市`、`创业板`、`科创板`、`北交所`
- ✅ **扩展行情**：支持 `期货`、`期权`、`债券`、`基金`、`港股`、`美股`等行情的获取
- ✅ **交互式文档**：```python doc.py```一键开启项目探索

#量化交易 #TDX接口 #Python金融

---

[![Star History Chart](https://api.star-history.com/svg?repos=LisonEvf/opentdx&type=Date)](https://star-history.com/#LisonEvf/opentdx&Date)
