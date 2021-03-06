---
title: Murano Getting Started Manually
template: default
---

# Murano - Manual Device example
Getting started instructions for manual API calls to Murano for Device activation and device interaction.  

Devices need two pieces of information to activate with the platform.  These are the Product ID when you create your product and a unique identifier serial number.  The Product ID can be found on the `Info` tab on the Product page.

![product id](assets/get_product_id.png)



# Hardware Setup
This example does not require any hardware.  It assumes developer will use command line CURL commands or implement in language of choice using HTTP client library commands.


# Software Setup

## Using CURL

1. Add a New Device in your Murano product, any Unique Identifier Serial Number is ok, but this example will use `000001`.

  ![product list](assets/product_list.png)

  ![add device](assets/add_device.png)

2. Run the following curl command, first inserting your product id anywhere that says `<productid>`.

  ```
  curl -k https://<productid>.m2.exosite.com/provision/activate \
     -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" \
     -d "vendor=<productid>&model=<productid>&sn=000001"

  ```

3. The response should return a 40 Character string, called the CIK.  This CIK is the private device API key which is typically securely stored in the devices non-volatile memory.  The device is now activated.

4. To make write or read requests, you must use this CIK from the Activation call.  Here are examples of writing and reading data defined in the Example Consumer Application product definition.  Replace <CIK_HERE> with your 40 character CIK.


  ```
  curl -k http://m2.exosite.com/onep:v1/stack/alias?state \
      -H "X-Exosite-CIK: <CIK_HERE>" \
      -H "Accept: application/x-www-form-urlencoded; charset=utf-8" \
      -d "temperature=72&humidity=23&uptime=1"

  ```


## Using Straight HTTP Requests

1. Add a Device in your Murano product, any Unique Identifier Serial Number is ok, but this example will use `000001`.

  ![product list](assets/product_list.png)

  ![add device](assets/add_device.png)

2. Run the following HTTP request, first inserting your product id anywhere that says `<productid>`.

  ```
  POST /provision/activate HTTP/1.1
  Host: <productid>.m2.exosite.com
  Content-Type: application/x-www-form-urlencoded; charset=utf=8
  Content-Length: <length>

  vendor=<productid>&model=<productid>&sn=000001
  ```

3. The response should return a 40 Character string, called the CIK.  This CIK is the private device API key which is typically securely stored in the devices non-volatile memory.  The device is now activated.

4. To make write or read requests, you can use this CIK.  Here are examples of writing and reading using the data ports defined in the Example Consumer Application product definition.

  ```
  POST /onep:v1/stack/alias?state HTTP/1.1
  Host: m2.exosite.com
  X-Exosite-CIK: <CIK>
  Accept: application/x-www-form-urlencoded; charset=utf-8
  Content-Type: application/x-www-form-urlencoded; charset=utf-8
  Content-Length: <length>

  temperature=72&humidity=23&uptime=1

  ```
