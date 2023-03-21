---
parser: v2
auto_validation: false
time: 30
tags: [ tutorial>intermediate, software-product>sap-business-application-studio, software-product>sap-hana-cloud]
primary_tag: software-product-function>sap-cloud-application-programming-model
---

# Display Time-Dependent Data with CAP in Value Help
<!-- description --> You have time-dependent entries in your database and want to limit the value help selection to values, that are valid at the moment. The other way around, that means you want to exclude outdated data from being displayed.

## Prerequisites
 - SAP Business Application Studio
 - dev space

## You will learn
  - Create time-dependent data
  - Filter time-dependent data for value help

## Intro
Add additional information: Background information, longer prerequisites

---

### Clone samples


```Shell/Bash

git clone https://github.com/SAP-samples/cloud-cap-samples.git
```




### Extend data model


Open `db/data-model.cds` and add the following:

```CDS
[..]
extend sap.common.Currencies with {
  validTo: Date;
}
```





### Edit service definition


Open the `srv/oders-service.cds` file and replace the content completely with the following:

```CDS
using {
  sap.capire.orders as my, sap
} from '../db/schema';

service OrdersService {
  entity Orders              as projection on my.Orders {
    *,
    currency : redirected to Currencies
  };

  entity Currencies       as projection on sap.common.Currencies;

  entity CurrenciesValueHelp as projection on sap.common.Currencies where validTo is null or validTo >= $now;

}

So basically we have the entity Currencies which serves all classical requests like "foreign key" associations.

The new entity `CurrenciesValueHelp` is build for the value help only and it is a projection which gives you only those records which are valid right now.

```



### Enhance UI annotations


Open the `app/orders/fiori-service.cds` file and add the following:

```CDS
// new value help for Currency
annotate OrdersService.Orders with {
    currency @(
        Common: {
            Text: currency.name , // TextArrangement: #TextOnly,
            ValueList: {
                Label: 'Currency Value Help',
                CollectionPath: 'CurrenciesValueHelp',
                Parameters: [
                    { $Type: 'Common.ValueListParameterInOut',
                        LocalDataProperty: currency_code,
                        ValueListProperty: 'code'
                    },
                    { $Type: 'Common.ValueListParameterDisplayOnly',
                        ValueListProperty: 'name'
                    }
                ]
            }
        },
    );
}

```

Here the most important part is to use the newly constructed value help in the

`CollectionPath: 'CurrenciesValueHelp'`



### Add sample data


Where? In the common module? Or "overwrite" common with regards to sample data? How?

```CSV
code;symbol;name;descr;validTo
EUR;€;Euro;Euro;9999-12-31
USD;$;US Dollar;US Dollar;9999-12-31
DEM;DM;D Mark; Deutsche Mark;2001-12-31

```



### Send test requests


Use the rest client? Or the preview?


### Test UI value help


Nowhere to select the currency. What is missing?


### XXXXX





### XXXXX





### XXXXX





### XXXXX





### XXXXX





### XXXXX





### XXXXX





### XXXXX







### XXXXX





### XXXXX





### XXXXX





### XXXXX






### XXXXX





### XXXXX





### XXXXX





### XXXXX





### XXXXX





### XXXXX






---
