# python-safer
[![PyPI version](https://badge.fury.io/py/python-safer.svg)](https://badge.fury.io/py/python-safer)

python-safer is an web scraping API wrapper written in Python for the [Department of Transportation's Safety and Fitness Electronic Records System](http://www.safersys.org/).

Here is how you search for companies using python-safer
```python
from safer import CompanySnapshot

client = CompanySnapshot()

results = client.search('python')
print(len(results))
for company in results:
    print(company)

---
6
<SearchResult Python Corporation (698887) from  Lacombe, LA>
<SearchResult Python Pressure Pumping Llc (2346443) from  Ada, OK>
<SearchResult Python Services Llc (918670) from  Brighton, CO>
<SearchResult Python Transport Corp (2379682) from  Dania Beach, FL>
<SearchResult Python Transports Llc (2642177) from  Fort Worth, TX>
<SearchResult Python'S Of St Cloud Inc (604262) from  St Cloud, MN>
---

company = next(results).get_company_snapshot()
print(company.legal_name)

---
PYTHON CORPORATION
---

company.open_url()
```

### Todo

- Write some tests.
- Implement `CompanySnapshot.get_by_mc_mx_number()` function

### Issues

If there are any problems, just open issue.

### Installation

##### Prerequisites
[lxml](https://lxml.de) - the C bindings are needed as well. Just follow the installation instructions, should be fine.\
##### Install using pip

```python
pip install python-safer
```

### Usage

This was written with Python 3.5, but it will probably work for any Python 3 version

Be prepared to wait for results, the SAFER CompanySnapshot website is very slow, and half the time it's down.

**Import and create CompanySnapshot**
```python
from safer import CompanySnapshot

client = CompanySnapshot()
```

**Search by Name**

Searching by name will return a SearchResultSet object that can be iterated through,
each item in the SearchResultSet is a SearchResult object, to get the Company Snapshot of that object
you can call `get_company_snapshot()`
```python
for company in client.search('python'):
    company.get_company_snapshot()
```
Getting the company snapshot will return a Company Object.

**Search by USDOT Number**

Searching by USDOT will return a Company object or raise a `CompanySnapshotNotFoundException` exception for that USDOT.

```python
company = client.get_by_usdot_number(698887)
print(company.to_json())
---
'{"operation_classification": ["Private(Property)"], "physical_address": "29279 HWY 190 LACOMBE, LA  70445", "united_states_inspections": {"hazmat": {"out_of_service": "0", "inspections": "0", "out_of_service_percent": "0%", "national_average": "4.50%"}, "driver": {"out_of_service": "0", "inspections": "0", "out_of_service_percent": "0%", "national_average": "5.51%"}, "iep": {"out_of_service": 0, "inspections": 0, "out_of_service_percent": "0%", "national_average": "N/A"}, "vehicle": {"out_of_service": "0", "inspections": "0", "out_of_service_percent": "0%", "national_average": "20.72%"}}, "state_carrier_id": "", "mc_mx_ff_numbers": null, "out_of_service_date": null, "mcs_150_form_date": "05/13/2016", "safety_rating": null, "carrier_operation": ["Interstate"], "safety_review_date": null, "canada_crashes": {"injury": 0, "total": 0, "fatal": 0, "tow": 0}, "mcs_150_mileage_year": {"year": 2015, "mileage": 200000}, "mailing_address": "PO BOX 790 LACOMBE, LA  70445", "power_units": 8, "dba_name": "", "entity_type": "CARRIER", "safety_rating_date": null, "safety_type": null, "duns_number": null, "drivers": 7, "us_inspections": {"hazmat": {"out_of_service": 0, "inspections": 0, "out_of_service_percent": "0%", "national_average": "4.50%"}, "vehicle": {"out_of_service": 0, "inspections": 0, "out_of_service_percent": "0%", "national_average": "20.72%"}, "iep": {"out_of_service": 0, "inspections": 0, "out_of_service_percent": "0%", "national_average": "N/A"}, "driver": {"out_of_service": 0, "inspections": 0, "out_of_service_percent": "0%", "national_average": "5.51%"}}, "united_states_crashes": {"injury": 0, "total": 0, "fatal": 0, "tow": 0}, "phone": "(985) 882-6101", "usdot": "698887", "url": "http://www.safersys.org/query.asp?searchtype=ANY&query_type=queryCarrierSnapshot&query_param=USDOT&original_query_param=NAME&query_string=698887", "legal_name": "PYTHON CORPORATION", "latest_update": "09/12/2017", "cargo_carried": ["Building Materials"], "operating_status": "ACTIVE", "canada_inspections": {"vehicle": {"out_of_service": 0, "inspections": 0, "out_of_service_percent": "0%"}, "driver": {"out_of_service": 0, "inspections": 0, "out_of_service_percent": "0%"}}}'
```

**Viewing Company Snapshots in a web browser**

Using the `open_url()` function on a Company object, will open the Company Snapshot on the SAFER website.

```python
company = client.get_by_usdot_number(698887)
company.open_url()
```

**Company Object Properties**
```python
company.legal_name
---
'PYTHON CORPORATION'
---

company.drivers
---
7
---

company.power_units
---
8
---

company.phone_number
---
'(985) 882-6101'
---
```

There is more, just look at the source code. However all values that are shown on the Company Snapshot website are available in the Company class.