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
