# nmap_vulners

[![Current Release](https://img.shields.io/github/release/vulnersCom/nmap-vulners.svg "Current Release")](https://github.com/vulnersCom/nmap-vulners/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/vulnersCom/nmap-vulners/total.svg "Downloads")](https://github.com/vulnersCom/nmap-vulners/releases) [![PayPal](https://img.shields.io/badge/donate-PayPal-green.svg)](https://paypal.me/videns)

---

## Note About Changes

The `vulners.nse` script worked but `mincvss` script argument was not working as expected and so the script would list all the CVEs it found regardless of their CVSS scores. I found a fix to this behaviour [on this issue here](https://github.com/vulnersCom/nmap-vulners/issues/43) suggested by [@mavzerburak0](https://github.com/mavzerburak0).

Another change I made was to set the default CVSS score to 7.0 instead of 0.0 (when no `mincvss` script argument is specified) for my own convenience as I mostly only need CVEs with CVSS scores of 7.0 and above listed in my nmap results. Providing `mincvss` script argument with the score of your preference (example: `--script-args mincvss=4.0`) will override this default as intended.

---

## Description

NSE script uses info about known services to provide data on vulnerabilities. Note that it is already included into the standard nmap NSE library.

![Result example](example.png)

## Dependencies:
    nmap libraries:
        http
        json
        string
    http-vulners-regex

Keep in mind that the script depends on having information about software versions, so it only works with -sV flag.

**NB:** Now it can actually be run without -sV flag if http-vulners-regex script is run as well.

## Installation
    locate where your nmap scripts are located on your system
        for *nix system it might be  ~/.nmap/scripts/ or $NMAPDIR
        for Mac it might be /usr/local/Cellar/nmap/<version>/share/nmap/scripts/
        for Windows it might be C:\Program Files (x86)\Nmap\scripts
    copy the provided script (vulners.nse) into that directory
    run *nmap --script-updatedb* to update the nmap script DB 

## Usage
    Use it as straightforward as you can:
        nmap -sV --script vulners [--script-args mincvss=<arg_val>] <target>
        
It is KISS after all.

# http-vulners-regex

## Description

NSE script scans HTTP responses and identifies CPEs for the mentioned software. It can therefore boost the efficiency of the main vulners script.

![Result example](simple_regex_example.png)
Or with the paths:
![Result example](paths_regex_example.png)

## Dependencies
    nmap libraries:
        http
        json
        string
        stdnse
        shortport
        table

## Installation
    locate where nmap is located on your system
        for *nix system it might be  ~/.nmap/ or $NMAPDIR
        for Mac it might be /usr/local/Cellar/nmap/<version>/share/nmap/
        for Windows it might be C:\Program Files (x86)\Nmap\
    copy the provided script (http-vulners-regex.nse) into <nmap_dir>/scripts/
    copy the provided json with the regexes to <nmap_dir>/nselib/data/
    copy the provided txt file with the default paths to <nmap_dir>/nselib/data/
        note that you can specify your own file via command line
    run *nmap --script-updatedb* to update the nmap script DB 

## Usage
    As a usual NSE script:
        nmap --script http-vulners-regex.nse [--script-args paths={"/"}] <target> 
