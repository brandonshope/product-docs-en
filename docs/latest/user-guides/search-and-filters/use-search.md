[al-sqli]:                ../../attacks-vulns-list.md#sql-injection
[al-xss]:                 ../../attacks-vulns-list.md#crosssite-scripting-xss
[al-rce]:                 ../../attacks-vulns-list.md#remote-code-execution-rce
[al-brute-force]:         ../../attacks-vulns-list.md#bruteforce-attack
[al-path-traversal]:      ../../attacks-vulns-list.md#path-traversal
[al-crlf]:                ../../attacks-vulns-list.md#crlf-injection
[al-open-redirect]:       ../../attacks-vulns-list.md#open-redirect
[al-nosqli]:              ../../attacks-vulns-list.md#nosql-injection
[al-logic-bomb]:          ../../attacks-vulns-list.md#data-bomb
[al-xxe]:                 ../../attacks-vulns-list.md#attack-on-xml-external-entity-xxe
[al-virtual-patch]:       ../../attacks-vulns-list.md#virtual-patch
[al-forced-browsing]:     ../../attacks-vulns-list.md#forced-browsing
[al-ldapi]:               ../../attacks-vulns-list.md#ldap-injection
[al-port-scanner]:        ../../attacks-vulns-list.md#resource-scanning
[al-infoleak]:            ../../attacks-vulns-list.md#information-exposure
[al-overlimit]:           ../../attacks-vulns-list.md#overlimiting-of-computational-resources
[email-injection]:        ../../attacks-vulns-list.md#email-injection
[ssi-injection]:          ../../attacks-vulns-list.md#ssi-injection
[invalid_xml]:            ../../attacks-vulns-list.md#unsafe-xml-header
[ssti-injection]:         ../../attacks-vulns-list.md#serverside-template-injection-ssti

# Using search and filters

Wallarm provides convenient methods for searching detected attacks, incidents and vulnerabilities. In the **Events** section of Wallarm Console, there are the following search methods available:

* **Filters** to select filtering criteria
* **Search field** to input search queries with attributes and modifiers similar to human language

The values set in the filters are automatically duplicated in the search field, and vice versa.

Any search query or a filter combination can be saved by clicking **Save a query**.

## Filters

Available filters are presented in Wallarm Console in multiple forms:

* Filters panel that is expanded and collapsed using the **Filter** button
* Quick filters for excluding or showing only events with the specific parameter values

![!Filters in the UI](../../images/user-guides/search-and-filters/filters.png)

When values of different filters are selected, the results will meet all those conditions. When different values for the same filter are specified, the results will meet any of those conditions.

## Search field

The search field accepts queries with attributes and modifiers similar to human language which makes submitting queries intuitive. For example:

* `attacks xss`: to search for all [XSS-attacks][al-xss]
* `attacks today`: to search for all attacks that happened today
* `vulns sqli`: to search for [SQL-injection][al-sqli] vulnerabilities
* `vulns 11/01/2020-11/10/2020`: to search for vulnerabilities within a certain period of time
* `xss 12/14/2020`: to search for all vulnerabilities, suspicions, attacks, and incidents of [cross‑site scripting][al-xss] on 14 December 2020
* `p:xss 12/14/2020`: to search for all vulnerabilities, suspicions, attacks, and incidents of all types within the xss HTTP request parameter (i.e. `http://localhost/?xss=attack-here`) as of 14 December 2020
* `attacks 9-12/2020`: to search for all attacks from September to December 2020
* `rce /catalog/import.php`: to search for all [RCE][al-rce] attacks, incidents, and vulnerabilities on `/catalog/import.php` path since yesterday

When values of different parameters are specified, the results will meet all those conditions. When different values for the same parameter are specified, the results will meet any of those conditions.

!!! info "Setting the attribute value to NOT"
    To negate the attribute value, please use `!` before the attribute or modifier name. For example: `attacks !ip:111.111.111.111` to show all attacks originated from any IP address excluding `111.111.111.111`.

Below you will find the list of attributes and modifiers available for use in search queries.

### Search by object type

Specify in the search string:

* `attack`, `attacks`: to search only for the attacks that are *not* aimed at known vulnerabilities.
* `incident`, `incidents`: to search only for incidents (attacks exploiting a known vulnerability).
* `vuln`, `vulns`, `vulnerability`, `vulnerabilities`: to search only for vulnerabilities.

### Search by attack type or vulnerability type

Specify in the search string:

* `sqli`: to search for [SQL injection][al-sqli] attacks/vulnerabilities.
* `xss`: to search for [Cross Site Scripting][al-xss] attacks/vulnerabilities.
* `rce`: to search for [OS Commanding][al-rce] attacks/vulnerabilities.
* `brute`: to search for [brute-force][al-brute-force] attacks.
* `ptrav`: to search for [path traversal][al-path-traversal] attacks.
* `crlf`: to search for [CRLF injection][al-crlf] attacks/vulnerabilities.
* `redir`: to search for [open redirect][al-open-redirect] vulnerabilities.
* `nosqli`: to search for [NoSQL injection][al-nosqli] attacks/vulnerabilities.
* `data_bomb`: to search for [logic bomb][al-logic-bomb] attacks.
* `ssti`: to search for [Server‑Side Template Injections][ssti-injection].
* `invalid_xml`: to search for [usage of unsafe XML header][invalid-xml].
* `overlimit_res`: to search for attacks aimed at [overlimiting of computational resources][al-overlimit].
* `xxe`: to search for [XML External Entity][al-xxe] attacks.
* `vpatch`: to search for [virtual patches][al-virtual-patch].
* `dirbust`: to search for [forced browsing][al-forced-browsing] attacks.
* `ldapi`: to search for [LDAP injection][al-ldapi] attacks/vulnerabilities.
* `scanner`: to search for [port scanner][al-port-scanner] attacks/vulnerabilities.
* `infoleak`: to search for attacks/vulnerabilities of [information disclosure][al-infoleak].
* `mail_injection`: to search for [Email Injections][email-injection].
* `ssi`: to search for [SSI Injections][ssi-injection].
* * `experimental`: to search for experimental attacks detected based on [custom regular expression](../rules/regex-rule.md).

An attack or vulnerability name can be specified in both uppercase and lowercase letters: `SQLI`, `sqli`, and `SQLi` are equally correct.

### Search by known attacks (CVE and well‑known exploits)

* `known`: to search for requests that precisely attack since they exploit CVE vulnerabilities or other well‑known vulnerability types.

    To filter attacks by certain CVE or another well‑known vulnerability type, you can pass the appropriate tag in addition to the tag `known` or separate from it. For example: `known:CVE-2004-2402 CVE-2018-6008` or `CVE-2004-2402 CVE-2018-6008` to search for attacks exploiting the [CVE-2004-2402](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2004-2402) and [CVE-2018-6008](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6008) vulnerabilities.
* `!known`: potential false positives. These requests may contain little‑known exploits or the context turning the exploits into legitimate parameter values.

To filter attacks by CVE and well‑known exploits, quick filters by event types and **CVE and exploits** can be used.

### Search by the attack target or the vulnerability target

Specify in the search string:

* `client`: to search for clients' data attacks/vulnerabilities.
* `database`: to search for database attacks/vulnerabilities.
* `server`: to search for app server attacks/vulnerabilities.

### Search by risk level

Specify the risk level in the search string:

* `low`: low risk level.
* `medium`: medium risk level.
* `high`: high risk level.

### Search by vulnerability identifier

To search for a certain vulnerability, specify its identifier. It can be specified in two ways:

* either fully: `WLRM-ABCD-X0123`
* or in abbreviated form: `X0123`

### Search by vulnerability status

Specify vulnerability status in the search string. Vulnerability can have one of the three statuses:

* `open`: currently relevant vulnerability
* `closed`: fixed vulnerability

### Search by event time

Specify time period in the search string. If the period is not specified, the search is conducted within the events that occurred during the last 24 hours.

There are the following methods to specify the period:

* By date: `11/10/2020-11/14/2020`
* By date and time (seconds are disregarded): `11/10/2020 11:11`, `11:30-12:22`, `11/10/2020 11:12-01/14/2020 12:14`
* With relation to a certain moment of time: `>11/10/20`
* Using string aliases:
    * `yesterday` equal to yesterday's date
    * `today` equal to today's date
    * `last <unit>` equal to the period from the entire past unit start to current date and time

        `week`, `month`, `year` or the number of these units can be used as `<unit>`. For example: `last week`, `last 3 month` or `last 3 months`.
    
    * `this <unit>` equal to current unit

        `week`, `month`, `year` can be used as `<unit>`. For example: `this week` will return events detected on Monday, Tuesday and Wednesday this week if today is Wednesday.

Date and time format depends on the settings specified in your [profile](../settings/account.md#changing-your-date-time-format):

* MM/DD/YYYY if **MDY** is selected
* DD/MM/YYYY if **DMY** is selected
* `13:00` if **24‑hour** is ticked
* `1pm` if **24‑hour** is unticked

The month can be specified as both number and name: `01`, `1`, `January`, `Jan` for January. The year can be specified in both full form (`2020`) and shortened form (`20`). If the year is not specified in the date, then the current year is used.

### Search by IP address

To search by IP address, use the `ip:` prefix, after which you can specify
*   A specific IP address, for example `192.168.0.1`—in this case, all attacks and incidents will be found for which the source address of the attack corresponds to this IP address.
*   An expression describing a range of IP addresses.
*   A total number of IP addresses related to an attack or incident.

#### Search by IP address range

To set a required range of IP addresses, you can use
*   An explicit IP address range:
    *   `192.168.0.0-192.168.63.255`
    *   `10.0.0.0-10.255.255.255`
*   A part of an IP address:
    *   `192.168.`—equivalent to `192.168.0.0-192.168.255.255`. Redundant format with the `*` modifier is allowed—`192.168.*`
    *   `192.168.0.`—equivalent to `192.168.0.0-192.168.0.255`
*   An IP address or part of it with a range of values inside the last octet in the expression:
    *   `192.168.1.0-255`—equivalent to `192.168.1.0-192.168.1.255`
    *   `192.168.0-255`—equivalent to `192.168.0.0-192.168.255.255`
    
    !!! warning "Important"
        When using a range of values within an octet, a dot is not set at the end.

*   Subnet prefixes ([CIDR notation](https://tools.ietf.org/html/rfc4632)):
    *   `192.168.1.0/24`—equivalent to `192.168.1.0-192.168.1.255`
    *   `192.168.0.0/17`—equivalent to `192.168.0.1-192.168.127.255`

!!! note
    You can combine the above methods for defining IP address ranges. To do this, list all the necessary ranges with the ip: prefix separately.
    
    **Example**: `ip:192.168.0.0/24 ip:10.10. ip:10.0.10.0-128`

#### Search by number of IP addresses

It is possible to search by the total number of IP addresses that are related to an attack or an incident (only for attacks and incidents):
*   `ip:1000+ last month`—search for attacks and incidents over the past month for which the number of unique IP addresses is more than 1000 (equivalent to `attacks incidents ip:1000+ last month`).
*   `xss ip:100+`—search for all cross‑site scripting attacks and incidents. The search result will be empty if the number of attacking IP addresses (with the XSS attack type) is less than 100.
*   `xss p:id ip:100+`—search for all XSS attacks and incidents related to the id parameter (`?id=aaa`). This will return results only if the number of different IP addresses exceeds 100.

### Search by the data center the IP address belongs to

To search by the data center, to which the IP address originated the attacks belongs, use the `source:` prefix.

This attribute value can be:

* `tor` for the Tor network
* `proxy` for the public or web proxy server
* `vpn` for VPN
* `aws` for Amazon
* `azure` for Microsoft Azure
* `gce` for Google Cloud Platform

### Search by the country in which the IP address is registered

To search by the country, in which the IP address originated the attacks is registered, use the `country:` prefix.

The country name should be passed to the attribute in the format corresponding to the standard [ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1) in uppercase or lowercase letters. For example: `country:CN` or `country:cn` for attacks originated from China.

### Search by server response status

To search by server response status, specify `statuscode:` prefix.

Response status can be specified as:
* a number from 100 to 999.
* «N–M» range, where N and M are figures from 100 to 999.
* «N+» and «N-» ranges, where N is a number from 100 to 999.

### Search by server response size

To search by the server response size, use the `s:` or `size:` prefix.

You can search for any integer value. Figures above 999 can be specified without a prefix. The «N–M», «N+» and «N-» ranges can be specified, where figures above 999 can also be specified without a prefix.

### Search by HTTP request method

To search by HTTP request method, specify the `method:` prefix.

To search for `GET`, `POST`, `PUT`, `DELETE`, `OPTIONS`: if upper-case is used, then the search string can be specified without a prefix. For all other values, a prefix should be specified.

### Search by a number of hits within attack/incident

To search attacks and incidents by a number of hits, specify the `N:` prefix.

For example, you can search for attacks that have more than 100 hits: `attacks N:>100`. Or search for attacks with less than 10 hits with `attacks N:<10`.

### Search by domain

To search by domain, use the `d:` or `domain:` prefix.

Any string, that may be a domain of the second or a higher level can be specified without a prefix. Any string can be specified with a prefix. 

You may use masks within a domain. The symbol `*` replaces any number of characters; the symbol `?` replaces any single character.

### Search by path

To search by path, use the `u:` or `url:` prefix.

Strings that start with `/` are processed without a prefix. Any string can be specified with a prefix.

### Search by application

To search by the application to which the attack was sent or in which a vulnerability was found, use the `pool:` prefix.

The attribute value is the application name set on the **Applications** tab in the **Settings** section. For example: `pool:'Example application'`.

### Search by parameter or parser

To search by parameter or parser, use the `p:`, `param:`, or `parameter:` prefix, or the `=` suffix. If using the suffix, a string that does not start with `/` is considered to be a parameter (wherein the ending `=` character is not included in the value).

Possible attribute values:

* Name of the aimed parameter.

    For example, if you need to find attacks aimed at the `xss` parameter but not at XSS-attacks (for instance, SQL-injection attack having `xss` in the GET-parameter), then specify `attacks sqli p:xss` in the search string.
* Name of the [parser](../rules/request-processing.md) used by Wallarm node to read the parameter value. The name must be in uppercase.

    For example, `attacks p:*BASE64` to find attacks aimed at any parameter parsed by the base64 parser.
* Sequence of parameters and parsers.

    For example: `attacks p:"POST_JSON_DOC_HASH_from"` to find attacks sent in the `from` parameter in the JSON body of a request.

You may use masks within a value. The symbol `*` replaces any number of characters, the symbol `?` replaces any single character.

### Search for anomalies in attacks

To search for anomalies in attacks, use the `a:` or `anomaly:` prefix.

To refine an anomaly search, use the following parameters:

* `size`
* `statuscode`
* `time`
* `stamps`
* `impression`
* `vector`

Example:

`attacks sqli a:size` will search for all SQL-injection attacks, that have response size anomalies in their requests.

### Search by request identifier

To search for attacks and incidents by request identifier, specify the `request_id` prefix.
The `request_id` parameter has the following value form: `a79199bcea606040cc79f913325401fb`. In order to make it easier to read, this parameter has been replaced by the placeholder abbreviation `<requestId>` in the examples below.

Examples:
*   `attacks incidents request_id:<requestId>`: to search for an attack or an incident with the `request_id` equal to `<requestId>`.
*   `attacks incidents !request_id:<requestId>`: to search for attacks and incidents with the `request_id` not equal to `<requestId>`.
*   `attacks incidents request_id`: to search for attacks and incidents with any `request_id`.
*   `attacks incidents !request_id`: to search for attacks and incidents without any `request_id`.

### Search for sampled hits

To search for the [sampled hits](../events/analyze-attack.md#sampling-of-hits), add `sampled` to the search string.
