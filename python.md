# trade_snippets

https://www.allthesnippets.com/search/quick_links.html
`https://www.interviewqs.com/ddi-code-snippets/home`
https://ostwalprasad.github.io/machine-learning/Must-have-code-snippets-for-every-Data-Science-Notebook.html

### filter zerodha instruments
    import pandas as pd
    import os

    def create_url(row):
        final_url = "https://kite.zerodha.com/chart/ext/tvc/NFO-OPT/" + row['tradingsymbol'] + "/" +  str(row['instrument_token'])
        return final_url

    df = pd.read_csv('https://api.kite.trade/instruments')
    df = df[df['segment'].str.contains("NFO-OPT") == True]
    df = df[df['instrument_type'].str.contains("CE") == True]
    df.drop(df[df['name'] == 'NIFTY'].index, inplace=True)
    df.drop(df[df['name'] == 'BANKNIFTY'].index, inplace=True)
    df['url'] = df.apply(lambda row: create_url(row), axis=1)
    df.to_csv(r'C:\Users\91956\Desktop\alloptstk.csv', index=False)


 





### symbol_link mapping tradingview
    pre = "https://in.tradingview.com/chart/?symbol=NSE:"
    post = "1!"
    link_list = list()
    dict_list = dict()
    for i in lis:
        if isinstance(i, str):
            mid = i
            mid2 = mid.replace("-", "_")
            mid2 = mid2.replace("&", "_")
            link = pre + mid2 + post
            link_list.append(link)
            dict_list[i] = link
            
            

### filter bhavcopy
    filename = r'C:\Users\91956\Desktop\fo01JUN2021bhav.csv'
    df1 = pd.read_csv(filename)
    df1.drop(df1[df1['INSTRUMENT'] != 'FUTSTK'].index, inplace=True)
    df1.drop(df1[df1['EXPIRY_DT'] != '24-Jun-2021'].index, inplace=True)
    df1.to_csv(r'C:\Users\91956\Desktop\BHAVA.csv')
    
    

### filter angels instrument dump
    df2 = pd.read_json(
        'http://margincalculator.angelbroking.com/OpenAPI_File/files/OpenAPIScripMaster.json')
    df2 = df2[df2['symbol'].str.endswith('24JUN21FUT')]
    df2.to_csv(r'C:\Users\91956\Desktop\ANGELS.csv')
    
   
### merge 
    df3 = pd.merge(df1, df2, left_on='SYMBOL', right_on='name')
    df3.to_csv(r'C:\Users\91956\Desktop\MERGED.csv')



### create pivot columns
    df = pd.read_csv(filename)
    df = df3[['symbol', 'HIGH', 'LOW', 'CLOSE', 'token']].copy()
    df['ltp'] = df['token']
    df['P'] = df.apply(lambda row: (
        row['HIGH'] + row['LOW'] + row['CLOSE']) / 3, axis=1)
    df['R2'] = df.apply(lambda row: (
        row['P'] + (0.618 * (row['HIGH'] - row['LOW']))), axis=1)
    df['R3'] = df.apply(lambda row: (
        row['P'] + (1 * (row['HIGH'] - row['LOW']))), axis=1)
    df.to_csv(r'C:\Users\91956\Desktop\PIVOTS.csv', index=False)


 
 
 
 
### save tick data to df
    import pandas as pd
    from smartapi import SmartConnect
    from smartapi import WebSocket
    from plyer import notification 
 
    filename = r'C:\Users\91956\Desktop\PIVOTS.csv'
    df = pd.read_csv(filename)
    df = df.assign(ltp=0)
    dic_map = pd.Series(df.symbol.values, index=df.token).to_dict()
    # print(dic_map)
    lis = df['token'].tolist()
    result = ""
    for i in lis:
        result = result + "nse_fo|" + str(i) + '&'
    result = result[:-1]
 
    obj = SmartConnect(api_key="x")
    data = obj.generateSession("x", "x@x#")
    refreshToken = data['data']['refreshToken']
    feedToken = obj.getfeedToken()
    FEED_TOKEN = feedToken
    CLIENT_CODE = "x"
    token = result
    task = "mw"
    ss = WebSocket(FEED_TOKEN, CLIENT_CODE)
 

    def on_tick(ws, tick):
        # print(tick)
        print()
        if len(tick) > 0:
            if 'ltp' in tick[0]:
                ltp = (tick[0]['ltp'])
                token = (tick[0]['tk'])
                token = int(token)
                global df
                df.loc[df.token == token, 'ltp'] = ltp
                nonpopulated = (df.loc[df.ltp == 0, 'ltp'].count())
                print(nonpopulated)
                stock_name = (dic_map[token])
                print(f'stock name : {stock_name: <20} ltp : {ltp}')        
                df['check_r3'] = df.apply(lambda row: int(float(row.ltp)) > int(float(row.R3)), axis=1)
                df['check_r2'] = df.apply(lambda row: int(float(row.ltp)) > int(float(row.R2)) and (int(float(row.ltp)) < int(float(row.R3))), axis=1)
                df.to_csv(r'c:\Users\91956\Desktop\checker.csv')



    def on_connect(ws, response):
        ws.websocket_connection()  # Websocket connection
        ws.send_request(token, task)


    def on_close(ws, code, reason):
        ws.stop()


    # Assign the callbacks.
    ss.on_ticks = on_tick
    ss.on_connect = on_connect
    ss.on_close = on_close
    ss.connect()
   
   
