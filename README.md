# trade_snippets

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


   
   
   
