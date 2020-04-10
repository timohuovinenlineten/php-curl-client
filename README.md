Example usage:

```php
<?php
require 'vendor/autoload.php';
header('Content-Type: text/plain;charset=utf-8');

use TH\Constants\ContentType;
use TH\Constants\HttpRequestHeader;
use TH\CurlClient\CurlMulti;
use TH\CurlClient\CurlClient;
use TH\CurlClient\CurlResponse;
use TH\CurlClient\Request\Authorization;
use TH\CurlClient\Request\Headers;
use TH\CurlClient\Request\Json;
use TH\CurlClient\Request\QueryParams;

// Sync request
echo 'Sync request'.PHP_EOL;
echo '================================='.PHP_EOL;
$response = CurlClient::post('https://example.com')
    ->with(new Json(['key'=>'value']))
    ->with(new QueryParams(['test' => 1]))
    ->with(new Authorization('Bearer: abc'))
    ->with(new Headers(['X-Test: test']))
    ->send();

$status = $response->getStatusCode();
$body = $response->getBody()->__toString();

echo 'StatusCode: '.$status.PHP_EOL;
foreach($response->getHeaders() as $name => $headers) {
    foreach($headers as $value){
        echo $name.': '.$value.PHP_EOL;
    }
}
echo PHP_EOL.$body.PHP_EOL;


// Async request
echo PHP_EOL;
echo PHP_EOL;
echo 'Async request'.PHP_EOL;
echo '================================='.PHP_EOL;
$request = CurlClient::get('https://google.com')
    ->with(new Json(['key'=>'value']))
    ->with(new QueryParams(['test' => 1]))
    ->with(new Authorization('Bearer: abc'));

$multi = new CurlMulti;
$multi = $multi->add($request, function(CurlResponse $response){
    $status = $response->getStatusCode();
    $body = $response->getBody()->__toString();
    
    echo 'StatusCode: '.$status.PHP_EOL;
    foreach($response->getHeaders() as $name => $headers) {
        foreach($headers as $value){
            echo $name.': '.$value.PHP_EOL;
        }
    }
    echo PHP_EOL.$body.PHP_EOL;
});
$multi->send();


// Use built-in helper functions
$response = CurlClient::post('https://example.com')
    ->withJson(['key'=>'value'])
    ->withQueryParams(['test' => 1]);
```
