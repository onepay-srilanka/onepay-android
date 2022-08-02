# onepay-android

### Gradle Setup

```gradle
repositories {
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation 'com.github.onepay-srilanka:onepay-android:1.0.0'
}
```

### Registering a callback for the OnepayIPGActivity Result
```
private val getIpgContent = registerForActivityResult(
        ActivityResultContracts.StartActivityForResult()
    ) { result ->

        when (result.resultCode) {
            Activity.RESULT_OK -> {

                when (result.data?.extras?.get(OnepayIPG.IPG_RESULT) as IPGStatus) {

                    IPGStatus.SUCCESS -> {

                        val resultData =
                            result.data?.extras?.get(OnepayIPG.IPG_RESULT_DATA) as OnepayIPGSuccess
                        Toast.makeText(this, resultData.description, Toast.LENGTH_LONG).show()
                    }
                    IPGStatus.FAILED -> {

                        val resultData =
                            result.data?.extras?.get(OnepayIPG.IPG_RESULT_DATA) as OnepayIPGError
                        Toast.makeText(this, resultData.description, Toast.LENGTH_LONG).show()

                    }
                }
            }
        }
    }
```

### Build an intent for the OnepayIPGActivity
```
val intent = Intent(this, OnepayIPGActivity::class.java)
            val onepayIPGInit = OnepayIPGInit.Builder()
                .setUser(
                    Customer(
                        firstName = "<<firstName>>",
                        lastName = "<<lastName>>",
                        phone = "<<phone with county code>>",
                        email = "<<email>>"
                    )
                )
                .setConfigurations(
                    Configurations(
                        token = "<<your token>>",
                        appID = "<<your app id>>",
                        hashKey = "<<your hash key>>"
                    )
                )
                .setProduct(
                    Product(
                        reference = "<<reference>>",
                        amount = <<amount>>,
                        currency = OnepayIPGInit.CurrencyTypes.LKR,
                        transactionOrder = arrayListOf(
                            Product.TransactionItem(
                                "abc",
                                "001",
                                2,
                                21.35f
                            )
                        )
                    )
                )
                .build()
            intent.putExtra(OnepayIPG.IPG_DATA, onepayIPGInit)
```

### Launch the intent
```
getIpgContent.launch(intent)
```
