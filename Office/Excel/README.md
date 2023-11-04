# CheatSheets Excel

## Sparkline
```bash
=SPARKLINE(GoogleFinance("CURRENCY:USDEUR"; "price"; HOY()-30; HOY()))
```

## Import XML
```bash
=SI.ERROR(ImportXml("http://services.packetizer.com/spotprices/?f=xml";"/SpotPrices/gold");"~")
```

## Range with slots
F1 = Price
G1 = Min Price of Day
H1 = Max Price of Day
15 = slots

* First Slot
     ```bash
     =SI.ERROR(SI(Y(F1 > (G1+(((H1-G1)/15)*0)); F1 <= ((((H1-G1)/15)*1)+G1) ); "X";"");"")
     ```

* Second Slot (change *0 -> *1 and *1 -> *2)
     ```bash
     =SI.ERROR(SI(Y(F1 > (G1+(((H1-G1)/15)*1)); F1 <= ((((H1-G1)/15)*2)+L13) ); "X";"");"")
     ```
* Third Slot ....etc

Results:
![img_DashboardGoogleFinanceRangeDayPrice](https://github.com/SpaikSaucus/cheatsheets/blob/main/Office/Excel/DashboardGoogleFinanceRangeDayPrice.png?raw=true)

## Generate update
```bash
="UPDATE ProjectCharter
SET ServiceManagerId ="&B3&",
        SaleOrder = "&D3&",
        ProjectManagerId = "&H3&",
        ProjectLeaderId = "&I3&",
        OpProjectStartDate = '"&YEAR(J3)&TEXT(J3;"mm")&DAY(J3)&"',
        OpProjectEndDate = '"&YEAR(K3)&TEXT(K3;"mm")&DAY(K3)&"'
WHERE Id = "&A3
```