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
            
            

