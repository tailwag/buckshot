# Buckshot Web Troubleshooting Suite

This tool was created out of neccesity after working as a linux support tech in the hosting industry for quite a while. The goal is to be able to identify common problems at a glance, and create a snapshot of a site at a specific time. When a report is run, all information is saved to a log file (~/.buckshot) so the report can be veiwed at a later date as if you had just run it. Along with this is the ability to compare two reports, to identify the affect a particular change may have had on the site. 

## Getting Started

Clone this repo and cd into the directory. From there you can call the script with ``./buckshot``. To display a help menu, use the ``-h`` or ``--help`` flags.

### Prerequisites

Python 3 running on Linux. 

## Authors

* **Devin Shoemaker** - *Initial work* - [tailwag](https://github.com/tailwag)

## License

This project is licensed under the GPL v2 - see the [LICENSE.txt](LICENSE.txt) file for details

## Acknowledgments

* [Thomas Waldmann](https://github.com/ThomasWaldmann) for the argparse library
* [Kenneth Reitz](https://github.com/kennethreitz) for the requests library 
* [Bob Halley](https://github.com/rthalley) for the dns library


## Examples

A few examples of some of the functionality:


Running a full test::

     [2009][user@host ~]$ buckshot github.com
     bd20c3 - 2019-04-10 20:11:26 - github.com
       ==) Connection Statistics:
         Path       :  HTTP/1.1 301 Moved Permanently
                    :  Location: https://github.com
                    :  HTTP/1.1 200 OK
         Address    :  192.30.253.112
         Connect    :  0.081592
         Redirect   :  0.078428
         Ttfb       :  0.288245
         Total      :  0.364072
         Full       :  1.1s
     
     
       ==) DNS Information:
             A      :  192.30.253.113
                    :  192.30.253.112
             NS     :  ns3.p16.dynect.net.
                    :  ns4.p16.dynect.net.
                    :  ns-421.awsdns-52.com.
                    :  ns-520.awsdns-01.net.
                    :  ns-1283.awsdns-32.org.
                    :  ns-1707.awsdns-21.co.uk.
                    :  ns1.p16.dynect.net.
                    :  ns2.p16.dynect.net.
             MX     :  10 alt4.aspmx.l.google.com.
                    :  1 aspmx.l.google.com.
                    :  5 alt1.aspmx.l.google.com.
                    :  5 alt2.aspmx.l.google.com.
                    :  10 alt3.aspmx.l.google.com.
             SOA    :  ns-1707.awsdns-21.co.uk. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400
             TXT    :  "MS=6BF03E6AF5CB689E315FB6199603BABF2C88D805"
                    :  "docusign=087098e3-3d46-47b7-9b4e-8a23028154cd"
                    :  "v=spf1 ip4:192.30.252.0/22 ip4:208.74.204.0/22 ip4:46.19.168.0/23 include:_spf.google.com include:esp.github.com include:_spf.createsend.com include:servers.mcsv.net ~all"
                    :  "MS=ms44452932"
     
     
       ==) Page Requisites:
             200    :  http://github.com/
             200    :  https://github.githubassets.com/robots.txt
             200    :  https://customer-stories-feed.github.com/customer_stories/ariya/hero.jpg
             200    :  https://customer-stories-feed.github.com/customer_stories/freakboy3742/hero.jpg
             200    :  https://customer-stories-feed.github.com/customer_stories/mailchimp/hero.jpg
             200    :  https://customer-stories-feed.github.com/customer_stories/kris-nova/hero.jpg
             200    :  https://customer-stories-feed.github.com/customer_stories/yyx990803/hero.jpg
             200    :  https://customer-stories-feed.github.com/customer_stories/mapbox/hero.jpg
             200    :  https://customer-stories-feed.github.com/customer_stories/jessfraz/hero.jpg
             404    :  https://customer-stories-feed.github.com/robots.txt
     
     
       ==) SSL Information:
         Issuer     :   C = US, O = DigiCert Inc, OU =
                    :  www.digicert.com, CN = DigiCert SHA2 Extended Validation Server CA
         Startdate  :  2018-05-08 00:00:00
         Expiration :  2020-06-03 12:00:00
         Subject    :   businessCategory = Private Organization, jurisdictionC = US, jurisdictionST =
                    :  Delaware, serialNumber = 5157550, C = US, ST = California,
                    :  L = San Francisco, O = "GitHub, Inc.", CN = github.com
         SANs       :  github.com
                    :  www.github.com
         Serial     :  0a:06:30:42:7f:5b:bc:ed:69:57:39:65:93:b6:45:1f

Running a test on github.com, displaying connection stats, and comparing that test to a new test:
``[0359][user@host ~]$ buckshot -c github.com
704b52 - 2019-04-10 20:08:50 - github.com
  ==) Connection Statistics:
    Path       :  HTTP/1.1 301 Moved Permanently
               :  Location: https://github.com
               :  HTTP/1.1 200 OK
    Address    :  192.30.253.113
    Connect    :  0.083018
    Redirect   :  0.083881
    Ttfb       :  0.277915
    Total      :  0.352950
    Full       :  1.4s


[2008][user@host ~]$ buckshot -cD 704b52 https://github.com
704b52 - 2019-04-10 20:08:50 - github.com                --> 26dee6 - 2019-04-10 20:09:24 - https://github.com       

  ==) +- Connection Stats
      |
      |---- Address
      |     - 192.30.253.113
      |     + 192.30.253.112
      |
      |---- Redirect Path
      |     =========== 704b52 ===========      =========== 26dee6 ===========
      |     HTTP/1.1 301 Moved Permanently      HTTP/1.1 200 OK
      |     Location: https://github.com              
      |     HTTP/1.1 200 OK                           
      |
      |---- Load Time
      |               +=============+== 704b52 ==+== 26dee6 ==+
      |      Connect  |   -0.041412 |  0.083018  |  0.041606  |
      |     Redirect  |   -0.083881 |  0.083881  |  0.000000  |
      |         TTFB  |   -0.080498 |  0.277915  |  0.197417  |
      |        Total  |   -0.080612 |  0.352950  |  0.272337  |
      |               +=============+============+============+
``


Displaying previous tests:
``[2013][user@host ~]$ buckshot -l 4
    247 tests found, displaying last 4
)================================================================================================================
 704b52 - 2019-04-10 20:08:50 - github.com         - 192.30.253.113 - Load: 1.4s -   Valid SSL -  10 req    1 err
 26dee6 - 2019-04-10 20:09:24 - https://github.com - 192.30.253.112 - Load: 1.1s -   Valid SSL -  10 req    1 err
 bd20c3 - 2019-04-10 20:11:26 - github.com         - 192.30.253.112 - Load: 1.1s -   Valid SSL -  10 req    1 err
 11a179 - 2019-04-10 20:13:12 - www.github.com     - 192.30.253.113 - Load: 0.8s -   Valid SSL -   2 req    0 err
``



Comparing two previous tests
``[2013][user@host ~]$ buckshot -D 704b52 11a179        
704b52 - 2019-04-10 20:08:50 - github.com                --> 11a179 - 2019-04-10 20:13:12 - www.github.com           

  ==) +- Connection Stats
      |
      |---- Redirect Path
      |     ============ 704b52 ============      ============ 11a179 ============
      |     HTTP/1.1 301 Moved Permanently        HTTP/1.1 301 Moved Permanently
      |     Location: https://github.com          Location: https://www.github.com
      |     HTTP/1.1 200 OK                       HTTP/1.1 301 Moved Permanently
      |                                           Location: https://github.com
      |                                           HTTP/1.1 200 OK
      |
      |---- Load Time
      |               +=============+== 704b52 ==+== 11a179 ==+
      |      Connect  |   0.040659  |  0.083018  |  0.123677  |
      |     Redirect  |   0.128807  |  0.083881  |  0.212688  |
      |         TTFB  |   0.1398499 |  0.277915  |  0.417765  |
      |        Total  |   0.1396510 |  0.352950  |  0.492601  |
      |               +=============+============+============+


  ==) +- DNS Records
      |
      |---- CNAME
      |     + github.com.


  ==) +- Page Requisites
      |
      |---- 200
      |     - http://github.com/
      |     - https://github.githubassets.com/robots.txt
      |     - https://customer-stories-feed.github.com/customer_stories/ariya/hero.jpg
      |     - https://customer-stories-feed.github.com/customer_stories/freakboy3742/hero.jpg
      |     - https://customer-stories-feed.github.com/customer_stories/mailchimp/hero.jpg
      |     - https://customer-stories-feed.github.com/customer_stories/kris-nova/hero.jpg
      |     - https://customer-stories-feed.github.com/customer_stories/yyx990803/hero.jpg
      |     - https://customer-stories-feed.github.com/customer_stories/mapbox/hero.jpg
      |     - https://customer-stories-feed.github.com/customer_stories/jessfraz/hero.jpg
      |     + https://github.com/
      |     + https://github.com/robots.txt
      |
      |---- 404
      |     - https://customer-stories-feed.github.com/robots.txt

``
