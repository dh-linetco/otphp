# Preventing TOTP Token Reuse

The method `verifyOtpWithWindow` has a `after` parameter. By default, its value is `null`. A timestamp can be given to only accept token which are generated after the period of the timestamp.
This can be used to prevent token reuse by keeping track of the last time a uses's OTP was verified successfully. To get the exact timestamp of the code which was used the method `verifyOtpWithWindow` returns the timestamp 
of the verified code if its valid and null otherwise.

```php
$user->lastSuccessfulOtpLogin = null;

$lastOtpAt = $totp->verifyOtpWithWindow('123456', null, null, $user->lastSuccessfulOtpLogin); # will return the timestamp 

$user->lastSuccessfulOtpLogin = $lastOtpAt;
$user->save();

# Attempting to try the same code again inside the 30s period
$lastOtpAt = $totp->verifyOtpWithWindow('123456', null, null, $user->lastSuccessfulOtpLogin); # will return null
```
