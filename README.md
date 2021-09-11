
### MarketGoogleAppsScript

- Start by install [cheerio for Google Apps Script](https://github.com/tani/cheeriogs)
- Use these function in *script editor* 




```
function doGet(url, tag) {
  const contentText = UrlFetchApp.fetch(url).getContentText();
  const $ = Cheerio.load(contentText);
  return($(tag).text());
}

function stock_info(ticker, market, attribute, date){
  emptycell = 'Z1000'
  /////////// NOT WORKING from Google spreadsheet
  try {
    // Google spreadsheet
    if (market){
      var googlefinance_value = SpreadsheetApp.getActiveSheet().getRange(emptycell).setValue('=GOOGLEFINANCE("' + market + ':' +  ticker + '", "price")').getValue();
    }else{
      var googlefinance_value = SpreadsheetApp.getActiveSheet().getRange(emptycell).setValue('=GOOGLEFINANCE("' + ticker + '" , "price")').getValue();
    }
  } catch (e) {
  googlefinance_value = "#N/A"
  }
    if ( isError(googlefinance_value)) {
      Logger.log("Error fetching from GOOGLEFINANCE");
    } else {
      return googlefinance_value ;
    }



  try {
    investing_value = doGet("https://www.investing.com/equities/" + ticker, ".instrument-price_last__KQzyA"  );
  } catch (e) {
    investing_value = "#N/A"
  }
    if ( isError(investing_value)) {
      Logger.log("Error fetching from INVESTING.com");
    } else {
      return investing_value ;
    }
}

function test() {
  Logger.log(stock_info('GOOG')); // returns price of the stock
  Logger.log(stock_info('GOOG', "NASDAQ")); // returns price of the stock
  Logger.log(stock_info('dasdasdas')); // returns null
  Logger.log(stock_info('volvo-b')); // returns price of the stock from investing.com
};


function isError(value) {
  const possibleErrors = ["#N/A", "#REF"];
  if (value == ''){
    return true ;
  }
  return possibleErrors.includes(value);
}
```

