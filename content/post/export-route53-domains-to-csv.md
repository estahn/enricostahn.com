---
title: "Export AWS Route53 Domains as CSV for Excel people"
date: 2021-01-04T23:54:04+11:00
tags: [python, aws, route53, csv, excel]
toc: true
---

If you have a fairly large list of domains in your AWS account and want to create an easily consumable format for external departments (finance, legal, etc) then this script might be of help.

The script requires a valid AWS session with permissions to read domain details.

:arrow_right: [Available as Gist on GitHub](https://gist.github.com/estahn/33ee9f0ecede6416a168489a7a24ee24)

## Installation

Copy the following script into a file `domains.py` (*skip if you want to use `wget`*).

```python
# Export AWS Route53 Domains to a CSV for Excel people
#
# Usage:
#
#   python3 <(wget -q -O - https://gist.github.com/estahn/33ee9f0ecede6416a168489a7a24ee24/raw/5eef9122e573ff23bcd40732856565c37c708efd/domains.py)
#
from itertools import chain, starmap

import pandas as pd
from pandas.io.json import json_normalize #package for flattening json in pandas df

import boto3

listofdomains = []
client = boto3.client('route53domains', region_name='us-east-1')

p = client.get_paginator('list_domains')
for page in p.paginate():
    for domain in page['Domains']:
        domain_detail = client.get_domain_detail(DomainName=domain['DomainName'])

        # Remove response data, so it doesn't make it into the spreadsheet
        del domain_detail['ResponseMetadata']

        # Re-map "ExtraParams" to flatten the JSON for the spreadsheet
        for c in ['RegistrantContact', 'TechContact', 'AdminContact']:
            for p in domain_detail[c]['ExtraParams']:
                domain_detail[c][p['Name']] = p['Value']
            del domain_detail[c]['ExtraParams']

        listofdomains.append(domain_detail)

df = pd.json_normalize(listofdomains)
df.to_csv (r'domains.csv', index = False, header=True)
```

## Usage

Either run the script created during the installation step:

```shell
$ python3 domains.py
```

or run:

```shell
$ python3 <(wget -q -O - https://gist.github.com/estahn/33ee9f0ecede6416a168489a7a24ee24/raw/5eef9122e573ff23bcd40732856565c37c708efd/domains.py)
```

This will create a file `domains.csv` containing the required information.
