# The Kite Connect API Java client
The official Java client for communicating with [Kite Connect API](https://kite.trade).

Kite Connect is a set of REST-like APIs that expose many capabilities required to build a complete investment and trading platform. Execute orders in real time, manage user portfolio, stream live market data (WebSockets), and more, with the simple HTTP API collection.

[Rainmatter](http://rainmatter.com) (c) 2016. Licensed under the MIT License.

##Documentation
- [Kite Connect HTTP API documentation](https://kite.trade/docs/connect/v1)

## API usage
```java
Initialize Kiteconnect using apiKey
Kiteconnect kiteSdk = new Kiteconnect("xxxx");

Set userId
kiteSdk.setUserId("xxxx");

First you should get request_token, public_token using kitconnect login and then use request_token, public_token, api_secret to make any kiteconnect api call.
Get login url. Use this url in webview to login user, after authenticating user you will get requestToken. Use the same to get accessToken.  
String url = kiteSdk.getLoginUrl();

Get accessToken as follows,
UserModel userModel =  kiteSdk.requestAccessToken("xxxxx", "xxxxx");

Set request token and public token which are obtained from login process.
kiteSdk.setAccessToken(userModel.accessToken);
kiteSdk.setPublicToken(userModel.publicToken);

Set session expiry callback.
kiteSdk.registerHook(new SessionExpiryHook() {
    @Override
    public void sessionExpired() {
    System.out.println("session expired");                    }
});

#Get margins

Get margins returns margin model, you can pass equity or commodity as arguments to get margins of respective segments.
Margins margins = kiteSdk.getMargins("equity");
System.out.println(margins.available.cash);
System.out.println(margins.utilised.debits);

#Place order

Place order method requires a map argument which contains,
tradingsymbol, exchange, transaction_type, order_type, quantity, product, price, trigger_price, disclosed_quantity, validity
squareoff_value, stoploss_value, trailing_stoploss
and variety  which can be value can be regular, bo, co, amo.
place order which will return order model which will have only orderId in the order model.

Following is an example param for SL order,
if a call fails then KiteException will have error message in it
Success of this call implies only order has been placed successfully, not order execution.

Map<String, Object> param = new HashMap<String, Object>(){
   {
        put("quantity", "1");
        put("order_type", "SL");
        put("tradingsymbol", "HINDALCO");
        put("product", "CNC");
        put("exchange", "NSE");
        put("transaction_type", "BUY");
        put("validity", "DAY");
        put("price", "158.0");
        put("trigger_price", "157.5");
    }};
Order order = kiteconnect.placeOrder(param, "regular");
System.out.println(order.orderId);