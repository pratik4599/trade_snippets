// blocked scripts will be rendered on home page
// you can unblock them from there



    @app.route('/', methods=["GET", "POST"])
    def home():
        if request.method == "POST":
            stockname = request.form.get("stockname")
            stockname = str(stockname)
            stockname = stockname.replace("21MAYFUT","")
            data = [stockname]
            lis = []
            lis.append(data)
            #append data
            file = open('blocklist.csv', 'a+', newline='')
            with file:
                write = csv.writer(file)
                write.writerows(lis)

            lis_to_render = []
            with open('blocklist.csv', 'r') as csv_file: 
                csv_reader = csv.reader(csv_file)
                for line in csv_reader:  
                    lis_to_render.append(line[0])

            return redirect('http://127.0.0.1:5000/homepage')
          
          
          
          
          
          
###  function to block
    lis_to_render = []
    with open('blocklist.csv', 'r') as csv_file:
        csv_reader = csv.reader(csv_file)
        for line in csv_reader:
            lis_to_render.append(line[0])

    for i in lis_to_render:
        df.drop(df[df.Ticker == i].index, inplace=True)

    df = df.assign(upper_range=lambda x: (x['r2'] + (x['r2'] * 0.005)))
    df = df.assign(lower_range=lambda x: (x['r2'] - (x['r2'] * 0.001)))
    df = df.assign(check=lambda x: (
        (x['LTP'] >= x['lower_range']) & (x['LTP'] <= x['upper_range'])))
    bullish = df.loc[df['check'] == True]
