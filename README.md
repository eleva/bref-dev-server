Development web server for serverless-native PHP web apps.

## Why?

This web server is meant for HTTP applications implemented without framework, using API Gateway as the router and PSR-15 controllers.

## Installation

```bash
composer require --dev bref/dev-server
```

## Usage

Run the webserver with:

```bash
vendor/bin/bref-dev-server
```

The application will be available at [http://localhost:8000/](http://localhost:8000/).

Routes will be parsed from `serverless.yml` in the current directory.

### Function Example
You can use this template as a sample for a function of your application.
Context will be mapped from `lambda-context` attribute of the request as stated [here](https://bref.sh/docs/use-cases/http/advanced-use-cases#lambda-event-and-context). 

```php
<?php

use Bref\Context\Context;
use Nyholm\Psr7\Response;

require 'vendor/autoload.php';

class Handler implements \Bref\Event\Handler
{
    public function handle($event, ?Context $context): Response
    {
        $attributes = $event->getAttributes();
        $queryParams = $event->getQueryParams();
        $parsedBody = $event->getParsedBody();

        $message = [
            "message" =>'Go Serverless v3.0! Your function executed successfully!',
            "context" => $context,
            "input" => [
                "attributes"=>$attributes,
                "queryParams"=>$queryParams,
                "parsedBody"=>$parsedBody
            ]
        ];

        $status = 200;

        return new Response($status, [], json_encode($message));
    }
}

return new Handler();

```


### Assets

By default, static assets are served from the current directory.

To customize that, use the `--assets` option. For example to serve static files from the `public/` directory:

```bash
vendor/bin/bref-dev-server --assets=public
```
