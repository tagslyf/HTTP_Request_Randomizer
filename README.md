# HTTP Request Randomizer in Python  [![Build Status](https://travis-ci.org/pgaref/HTTP_Request_Randomizer.svg?branch=master)](https://travis-ci.org/pgaref/HTTP_Request_Randomizer) [![PyPI version](https://badge.fury.io/py/http-request-randomizer.svg)](https://badge.fury.io/py/http-request-randomizer)

A convenient way to implement HTTP requests is using Pythons' **requests** library.
One of requests’ most popular features is simple proxying support.
HTTP as a protocol has very well-defined semantics for dealing with proxies, and this contributed to the widespread deployment of HTTP proxies

Proxying is very useful when conducting intensive web crawling/scrapping or when you just want to hide your identity (anomization).

In this project I am using public proxies to randomise http requests over a number of IP addresses and using a variety of known user agent headers these requests look to have been produced by different applications and operating systems.


## Proxies

Proxies provide a way to use server P (the middleman) to contact server A and then route the response back to you. In more nefarious circles, it's a prime way to make your presence unknown and pose as many clients to a website instead of just one client.
Often times websites will block IPs that make too many requests, and proxies is a way to get around this. But even for simulating an attack, you should know how it's done.


## User Agent

Surprisingly, the only thing that tells a server the application triggered the request (like browser type or from a script) is a header called a "user agent" which is included in the HTTP request.

## The source code

The project code in this repository is crawling **four** different public proxy websites:
* http://proxyfor.eu/geo.php
* http://free-proxy-list.net
* http://rebro.weebly.com/proxy-list.html
* http://www.samair.ru/proxy/time-01.htm 

After collecting the proxy data and filtering the slowest ones it is randomly selecting one of them to query the target url.
The request timeout is configured at 30 seconds and if the proxy fails to return a response it is deleted from the application proxy list.
I have to mention that for each request a different agent header is used. The different headers are stored in the **/data/user_agents.txt** file which contains around 900 different agents.

## How to use

The project is now distribured as a PyPI package!
To run an example simply include **http-request-randomizer==0.0.5** in your requirements.txt file.
Then run the code below:

````python
import time
from http.requests.proxy.requestProxy import RequestProxy

if __name__ == '__main__':
    print "Hello"
    start = time.time()
    req_proxy = RequestProxy()
    print "Initialization took: {0} sec".format((time.time() - start))
    print "Size : ", len(req_proxy.get_proxy_list())
    print " ALL = ", req_proxy.get_proxy_list()

    test_url = 'http://icanhazip.com'

    while True:
        start = time.time()
        request = req_proxy.generate_proxied_request(test_url)
        print "Proxied Request Took: {0} sec => Status: {1}".format((time.time() - start), request.__str__())
        if request is not None:
            print "\t Response: ip={0}".format(request.text)
        print "Proxy List Size: ", len(req_proxy.get_proxy_list())

        print"-> Going to sleep.."
        time.sleep(10)
````

## Contributing

Contributions are always welcome! Feel free to send a pull request.

## Faced an issue?

Open an issue [here](https://github.com/pgaref/HTTP_Request_Randomizer/issues), and be as detailed as possible :)

## License

This project is licensed under the terms of the MIT license.
