# Buckshot Web Troubleshooting Suite

This tool was created out of neccesity after working as a linux support tech in the hosting industry for quite a while. The goal is to be able to identify common problems at a glance, and create a snapshot of a site at a specific time. When a report is run, all information is saved to a log file (~/.buckshot) so the report can be veiwed at a later date as if you had just run it. Along with this is the ability to compare two reports, to identify the affect a particular change may have had on the site. 

## Getting Started

Clone this repo and cd into the directory. From there you can call the script with ``./buckshot``.

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
