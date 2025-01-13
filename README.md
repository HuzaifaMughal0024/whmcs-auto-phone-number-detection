# WHMCS GeoIP Country Detection Hook

Automatically set the default country during user registration on your WHMCS platform based on the client's IP address.

## Features
- Auto-detects the user's country using their IP address.
- Uses the reliable [ipinfo.io](https://ipinfo.io) API.
- Fallback to a default country (US) if detection fails.
- Seamless integration into WHMCS registration process.

## Installation
1. **Download the Hook File:**
   Save the provided `geoip_country.php` file in your WHMCS `includes/hooks/` directory.

2. **Modify API Token:**
   Replace `<YOUR_API_TOKEN>` with your personal token from [ipinfo.io](https://ipinfo.io/signup).

   ```php
   $response = file_get_contents("https://ipinfo.io/{$ip}/country?token=<YOUR_API_TOKEN>");
   ```

3. **Ensure PHP Configuration:**
   - Enable `allow_url_fopen` in your PHP configuration to fetch data from external sources.
   - Alternatively, use `cURL` if `file_get_contents()` is not allowed.

4. **Clear WHMCS Template Cache:**
   After adding the hook, clear the WHMCS cache:
   - Navigate to **Utilities > System > System Cleanup > Empty Template Cache**.

Here’s the complete code:

```php
<?php

add_hook('ClientAreaPageRegister', 1, function($vars) {
    $countryCode = 'US'; // Default fallback country

    // Use external service to get country based on IP
    if (isset($_SERVER['REMOTE_ADDR'])) {
        $ip = $_SERVER['REMOTE_ADDR'];
        $response = file_get_contents("https://ipinfo.io/{$ip}/country?token=<YOUR_API_TOKEN>"); //Replace with your api token
        if ($response) {
            $countryCode = trim($response);
        }
    }

    return ['defaultCountry' => $countryCode];
});
```

## License
This project is licensed under the MIT License. See the LICENSE file for more details.


## Support
For any questions or support, please contact _huzaifamughal. on dsicord or raise an issue on this repository.


