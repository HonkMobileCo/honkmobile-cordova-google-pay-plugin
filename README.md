## Installation

```
cordova plugin add
```

For Android, register and fill all required forms at https://pay.google.com/business/console. Add following to
config.xml:

```
<config-file parent="/manifest/application" target="AndroidManifest.xml">
    <meta-data
            android:name="com.google.android.gms.wallet.api.enabled"
            android:value="true" />
</config-file>
```

## Usage

`canMakePayments()` checks whether device is capable to make payments via Apple Pay or Google Pay.

```
// use as plain Promise
async function checkForApplePayOrGooglePay(){
    let isAvailable = await cordova.plugins.HonkGooglePay.canMakePayments()
}

// OR
let available;

cordova.plugins.HonkGooglePay.canMakePayments((r) => {
  available = r
})
```

`makePaymentRequest()` initiates pay session.

```
let request = {
    merchantId: 'merchant.com.example', // obtain it from https://developer.apple.com/account/resources/identifiers/list/merchant
    purpose: `Payment for your order #1`,
    amount: 100,
    countryCode: "US",
    currencyCode: "USD"
}

cordova.plugins.HonkGooglePay.makePaymentRequest(request, r => {
        // in success callback, raw response as encoded JSON is returned. Pass it to your payment processor as is.
      let responseString = r

      },
      r => {
        // in error callback, error message is returned.
        // it will be "Payment cancelled" if used pressed Cancel button.
      }
   )
```

All parameters in request object are required.

## For Android

You will have to provide few extra parameters:

```
request.gateway = 'stripe'; // or any another processor you are using: https://developers.google.com/pay/api#participating-processors
request.merchantId = 'XXXXXXX'; // merchant id provided by your processor
request.gpMerchantName = 'Your Company Name'; // will be displayed in transaction info
request.gpMerchantId = 'XXXXXXXXXXXX'; // obtain it at https://pay.google.com/business/console
```

Also, on Android checking payment availability calling `canMakePayments()` always returns false even if user has a valid card attached to GooglePay.
