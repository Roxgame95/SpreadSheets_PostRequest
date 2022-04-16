
#  Post Request to SpreadSheets



**Table of Contents**

[TOCM]

Tutorial Part 1 
=============


###Prepare SpreadSheet 
----
First of all follow this link to create you spreadsheet page  **[Press me ](http://https://docs.google.com/spreadsheets/u/0/ "Press me ")**
###Click thge Blank Button
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/Image1.jpg)

> When you fellow the link it should take you to page like the image

###Make sure that you have same
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image2.jpg)

> Make sure that you have Date and id column also Rename the sheet to DataClient , you can add as many as column you need like studentName , StudentAdress or whatever

###Press Extentions then Apps Script
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image3.jpg)

> After the prevouis steps on top of the page select extentions then apps script it should open new window with you

###Select Code.gs and remove every line
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image4.jpg)

> We will replace the code with our script

----

####Paste our script in the editor　

```javascript
const sheetName = 'DataClient'
const scriptProp = PropertiesService.getScriptProperties()

function initialSetup() {
  const activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
  scriptProp.setProperty('key', activeSpreadsheet.getId())
}
function doGet() {

}
function doPost(e) {
  const lock = LockService.getScriptLock()
  lock.tryLock(10000)
  var cipher = new Cipher("kMFvwRo7jt", 'aes');
  var encryptedjson = cipher.decrypt(e.postData.contents);
  var data = JSON.parse(encryptedjson);
  try {
    const doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
    const sheet = doc.getSheetByName(sheetName)
    for (let index = 0; index < data.length; index++) {
      const product = data[index];
      var range = sheet.getRange(1, 2, sheet.getLastRow(), 1).getValues();
      if (isNotExist(range, product['id'])) {
      const headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
      const nextRow = sheet.getLastRow() + 1
      const newRow = headers.map(function (header) {
        return header === 'Date' ? new Date() : product[header]
      })
      sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])
      }
    }
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success' }))
      .setMimeType(ContentService.MimeType.JSON)
  }
  catch (e) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error' }))
      .setMimeType(ContentService.MimeType.JSON)
  }
  finally {
    lock.releaseLock()
  }
}
function isNotExist(range, fText) {
  var _isNotExist = true;
  for (var i = 1; i < range.length; i++) {
    if (range[i] == fText.toString()) {
      _isNotExist = false;
    }
  }
  return _isNotExist;
}
```
###You will have something like that
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image5.jpg)

> Select everything step by step from 1 to 4 after pressing Run the app will ask you to give it a permission for editing your spreadsheet just allow it

###Set Trigger function
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image6.jpg)

> After preparing the script we have to tell our application what function should fire we send data from client side follow from 1 to 3 and we will be ready for next step

###Deployement
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image7.jpg)

> After everything done we have to deploy the app so we can use it

![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image8.jpg)

> Make sure the set type of the app as web app also make sure it exute as you and access for everyone

![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/image9.jpg)

> Make sure to copy the web app url we will need it in part2


Tutorial Part 2 
=============
###Sending Data to our spreadsheet using javascript
```javascript
const url = 'https://script.google.com/..../exec' // put your web app url instead of mine
fetch(url,{
method: "POST",
body: JSON.stringify({id:'1',StudentName:'test'}), //  every key should be exist as a column in your sheet or the whole proccess will fail !
}
```
>Finally after sending that post request you should see you spreadsheets pupilated with the data you sent

###Results
![](https://github.com/Roxgame95/SpreadSheets_PostRequest/raw/main/results.jpg)

> After everything done we have to deploy the app so we can use it

**feel free to contact me if you need any help !**
