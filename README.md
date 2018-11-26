# S3 signed URL generator for Google Apps Script

#### TL;DR 

`getS3SignedGetUrl()` function to create so called [presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-query-string-auth.html) to private buckets in AWS S3. Copy [UrlGenerator.gs](https://raw.githubusercontent.com/kmotrebski/gas-s3-url-generator/master/UrlGenerator.gs) file into your Script Editor, fill in your credentials and use it this way:

 ```
 =getS3SignedGetUrl("bucket_name", "path/to/file.csv")
 ```
or
 ```
 =IMPORTDATA(getS3SignedGetUrl("bucket_name", "path/to/file.csv"))
 ```

### Goal

Goal of this helper is to download, in a secure way, data from **private** S3 buckets on AWS directly into your Google Sheets spreadsheet.

To do this you need to generate so called "signed" URLs but the logic responsible for it is quite complex. You can check [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-query-string-auth.html) if you don't belive! 

This helper function does all of it and you can use it in a simple way directly in your cell formulas: `=getS3SignedGetUrl("bucket_name", "path/to/file.csv")`!

##### Use case scenario

It may look like this: `production DB` => `script running daily/hourly` => `data uploaded into S3` => `data pulled into Google Sheets`. Data uploaded into S3 can be in a form of CSV file. You can use `IMPORTDATA()` function to populate columns. You can also set up triggers in your spreadsheet settings that will download data even every minute. This will make your reports/charts always up-to-date and completely automatic. See sections below for usage.

### Prerequisites

- Private S3 bucket with some files (aka objects) uploaded that you now want to import into your spreadsheet, e.g. some `*.csv` file
- Security credentials (access keys) with read access to the bucket, [read more here](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)
- Google Sheet document that you want to download data into

### Installation

It's about single copy&paste:
 
 - open Google Spreadsheet document or create new one, go to Script Editor by `Tools` > `Script Editor`
 - In Script Editor add a `UrlGenerator.gs` file this way: `File` > `New` -> `Script file`. Copy&paste contents of `UrlGenerator.gs` file from this repository, [it is here](https://raw.githubusercontent.com/kmotrebski/gas-s3-url-generator/master/UrlGenerator.gs)
 - fill in your AWS access keys, your bucket's region and URLs expiry time (7 days max). These are top 4 lines of code, [see here](https://github.com/kmotrebski/gas-s3-url-generator/blob/master/UrlGenerator.gs#L1-L4). By default data are filled with credentials from exemplary case from AWS documentation.
 - save the file, `File` -> `Save`
 
 That is all! You can now use it in your spreadsheet. See section below.
 
 ### Usage
 
 Go back to your spreadsheet, go to any cell, e.g. `A1` and type the following:
 
 ```
 =getS3SignedGetUrl("sales-bucket", "vodka/201806.csv")
 ```
 You can now use this URL to download data to your sheet, e.g. by passing it into simple `IMPORTDATA()` function ([see its documentation](https://support.google.com/docs/answer/3093335)):
 ```
 =IMPORTDATA(A1)
 ```
 Where `A1` is the cell from previous step. You can get same effect with single cell too:
 ```
  =IMPORTDATA(getS3SignedGetUrl("sales-bucket", "vodka/201806.csv"))
 ```
`IMPORTDATA()` function will use the URL generated by `getS3SignedGetUrl` and download data from S3.

#### Automatic triggers

You can also set up automatic triggers to update data regularly, even every single minute! This will make your data constantly updated.

Add new file to your project, in a similar way you did it before. Copy&paste `Trigger.gs` file [from this repository](https://github.com/kmotrebski/gas-s3-url-generator/blob/master/UrlGenerator.gs) into your project and follow instructions in the file comments.

### Reach out to me

Contact me please and describe your use cases! I would be very interested into knowing how you use it and what for. I would like to describe it on my [blog](https://kmotrebski.github.io/).

### Bugs

Please reports bugs here [https://github.com/kmotrebski/gas-s3-url-generator/issues](https://github.com/kmotrebski/gas-s3-url-generator/issues)

Code was tested on the exemplary case from AWS S3 documentation on [https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-query-string-auth.html](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-query-string-auth.html)

### Other tools

Check [ImportJSON](https://github.com/bradjasper/ImportJSON) for importing JSON data into Google Sheets.
