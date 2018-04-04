SI- SMS API PHP client
============================

Prerequisites
-------------

- You have installed a [PHP interpreter](http://php.net/manual/en/install.php) (minimal required version is 5.6).

Installation
-----------

For using SI API PHP client in your project, you have to add the following file to your project file:

	src/main/php/api/Sms.php

or recommended to use [composer](http://getcomposer.org) to install the library.

	$ composer require si/si-api-php

Running examples
----------------

The first thing that needs to be done is to include `Sms.php` and to initialize the messaging client. Before you start any of the examples, you have to populate specific data (API Key, SenderID, Base Url) by instantiate Sms rest client class object.	

	require_once '<PATH-TO-FOLDER>/Sms.php';
	$smsObj = new Sms(<Api_key>, <sender_id>, <base_url>);

### Basic messaging example

After including `Sms.php` and to initialize the messaging client you need to follow the function below:

The first step is to prepare the message:

    $msg = <your_message_comes_here>;

Then next step is to prepare your contact list:
	
	$receivers = <receiver_number1,receiver_number2>;

Receiver Mobile number to which SMS needs to be sent. It can be with or without 91. Also provide multiple numbers in comma separated format.

Now you are ready to send the message:

	$response = $smsObj->sendSms($receivers, $msg);

### Basic messaging example with optional parameters

We can give optional parameter for different kinds of functionality, has explained below:

1. Schedule Sms :- We have to provide Date and time for scheduling an SMS

		$response = $smsObj->sendSms($receivers , $msg , [    
	    	'time' => '2017-05-19 11:17:55 AM',
	    ]);

2. Unicode Messgae :- To specify that the message to be sent is in unicode format. Also can be used for automatic detection of unicode SMS.
		
		$response = $smsObj->sendSms($receivers , $msg , [    
	    	'unicode' => '1',
	    ]);

3. Flash Message :- To specify that the message is to be sent in the flash format

		$response = $smsObj->sendSms($receivers , $msg , [    
	    	'flash' => '1',
	    ]);

4. Receive Delivery Report Url :- The encoded URL to receive delivery reports. Spiffing custom in the DLR url is mandatory.
	
		$drl_url = 'http://exapmle.com?sent={sent}&delivered={delivered}&msgid={msgid}&sid={sid}&status={status}&reference={reference}&custom1={custom1}&custom2={custom2}';

		$response = $smsObj->sendSms($receivers , $msg , [    
	    	'dlr_url' => $dlr_url,
	    ]);

### Basic messaging example with advance parameters(optional)

1. Format :- Output format should be as specified by this variable ex.-XML/JSON/JSONP. Default response will be in JSON
	
		$response = $smsObj->sendSms($receivers , $msg , [    
	    	'format'  => 'json',
	    ]);

2. Custom1 & Custom2 :- Custom reference fields.

		$response = $smsObj->sendSms($receivers , $msg , [    
	    	'custom1'  => 'xxxxxxx',
	    	'custom2'  => 'xxxxxxx'
	    ]);

3. Port :- Port number to which SMS has to be sent. Valid integer port number above 2000

		$response = $smsObj->sendSms($receivers , $msg , [    
	    	'port'  => '8223',	    	
	    ]);

###  Messaging with JSON data

SMS can be sent using the JSON Data by posting values to the preceding URL by the POST method with urlencoded json data.

Sample json data

	$jsonData = {"sms": 
		[
		  { 
		    "to": "9xxxxxxxx", 
		    "custom": 9xxxxxxxx, 
		    "message": "Message from json api node 1"  
		  }, 
		  {
		    "to": "91xxxxxxxx",   
		    "custom": 34,
		    "message": "Message from json api node 2"  
		  }
		]
	}

	$response = $smsObj->sendSmsUsingJsonApi($jsonData,['formate'=>'json']);

JSON Optional Parameters

1. Sending to multiple numbers with same message 

		$jsonData = {
		  "message": "test json",
		  "sms":[
		  {
		    "to": "95XXXXXXXX",
		    "msgid": "1",
		    "message": "test sms",
		    "custom1": "11",
		    "custom2": "22",
		    "sender": "AAAAAA"
		  },
		  {
		    "to": "99XXXXXXXX",
		     "msgid": "2",
		    "custom1": "1",
		    "custom2": "2" 
		  }],
		  "unicode": 1,
		  "flash": 1,
		  "dlrurl": "http://www.example.com/dlr.php" 
		}

2. Sending to multiple numbers with different message

		$jsonData = {
		  "message": "test json",
		  "sms":[
		  {
		    "to": "95XXXXXXXX",
		    "msgid": "1",
		    "message": "test sms",
		    "custom1": "11",
		    "custom2": "22",
		    "sender": "AAAAAA"
		  },
		  {
		    "to": "99XXXXXXXX",
		     "msgid": "2",
		    "message": "json test sms",
		    "custom1": "1",
		    "custom2": "2"
		  }],
		  "unicode": 1,
		  "flash": 1,
		  "dlrurl": "http://www.example.com/dlr.php"
		}

### Messaging with XML data

SMS can be sent using the XML values by posting values to the preceding functions.

	$xml = <?xml version="1.0" encoding="UTF-8"?>
	<xmlapi>
	  <sender>AAAAAA</sender>
	  <message>xml test</message>
	  <unicode>1</unicode>
	  <flash>1</flash>
	  <campaign>xml test</campaign>
	  <dlrurl>
	     <![CDATA[http://domain.com/receive?sent={sent}&delivered={delivered}&msgid={msgid}&sid={sid}&status={status}&reference={reference}
	     &custom1={custom1}&custom2={custom2}&credits={credits}]]>
	  </dlrurl>
	  <sms>
	    <to>95xxxxxxxx</to>
	    <custom>22</custom>
	    <custom1>99</custom1>
	  </sms>
	  <sms>
	    <to>99xxxxxxxx</to>
	    <custom>229</custom>
	    <custom1>995</custom1>
	  </sms>
	</xmlapi>

	$response = $smsObj->sendSmsUsingXmlApi($xml,['formate'=>'json']);

1. Sending to multiple numbers with same message 

		$xml = <?xml version="1.0" encoding="UTF-8"?>  
		<api>  
		  <campaign>campaign</campaign>  
		  <dlrurl>
		    <![CDATA[http://domain.com/receive?msgid={msgid}&sid={sid}&status={status}&custom1={custom1}]]>
		  </dlrurl> 
		  <time>2014-12-26 04:00pm</time>  
		  <unicode>0</unicode>  
		  <flash>0</flash>  
		  <sender>senderid</sender>  
		  <message><![CDATA[smstext]]></message>  
		  <sms>  
		      <to>9190********</to>  
		  </sms>  
		  <sms>  
		      <to>9191********</to>  
		  </sms>  
		</api>

2. Sending to multiple numbers with different message 

		$xml = <api>
		<campaign>campaign</campaign>
		<time>2014-12-26 04:00pm</time>
		<unicode>0</unicode>
		<flash>0</flash>
		<sms>
			<to>9190********</to>
			<sender>senderid</sender>
			<message>smstext</message>
			<custom>2</custom>
			<dlrurl>
			<![CDATA[http://domain.com/receive?msgid={msgid}&sid={sid}&status={status}&custom1={custom1}]]>
			</dlrurl> 
		</sms>
		<sms>
			<to>9191********</to>
			<sender>senderid</sender>
			<message><![CDATA[smstext]]></message>
			<custom>2</custom>
			<dlrurl>
			<![CDATA[http://domain.com/receive?msgid={msgid}&sid={sid}&status={status}&custom1={custom1}]]>
			</dlrurl> 
		</sms>
		</api>

### Getting status of message

To check status of any sent SMS campaign, you must have message id only (not group ID) of the respective message(s). You can only check status for messages which have been sent on the same day. If using POST method for pulling messages status, then statuses for maximum 500 messages can be pulled at a time. Here is a function for checking the status of an SMS in the following format:

	$statusResponse = $smsObj->smsStatusPull("fe5a70a3-1d65-40de-93b3-e50ebdc69272:1",['formate'=>'json']);

Optional parameter

1. Format :- Output format should be as specified by this variable, XML/JSON/JSONP. Default response will be in JSON.

2. Numberinfo :- Flag to query service provider and location data, i.e 0 or 1.

3. Page :- Page number to fetch status details of a pariticular page, where page is 1 or more.

### Sending SMS to an Optin Group

To send an SMS to an optin group,one must create a optin group and keyword associated with it in your account. Also, need to add numbers for those optin keywords to a group i.e. optin group.

Mandatory Parameters

1. BASE_URL :- URL of your SMS Service
2. method :- Predefined method, i.e optin
3. sender :- Sender ID assigned to your account
4. id/name :- id-optin group ID/name-Optin keyword, comma separated id or optin group keyword
5. message :- Message to be sent. Message text which is URL encoded(1000 char for normal, 500 for unicode)

Optional Parameters

1. time :- Date and time for scheduling an SMS 	EX Format: YYYY-MM-DD HH:MM:SS OR YYYY-MM-DD HH:MM AM/PM
2. unicode:- To specify that the message to be sent is in unicode format. Also can be used for automatic detection of unicode SMS. i.e 1/0/auto
3. flash :- To specify that the message is to be sent in the flash format, i.e 1 or 0
4. format :- Output format should be as specified by this variable,XML/PHP/JSON/JSONP. Default response will be in JSON
5. callback :- Callback function for JSONP response format, i.e String

	$response = $smsObj->smsToOptinGroup($msg, $optinId, ['time' => '2017-06-11 11:17:55 AM',
	            'unicode' => '1',
	            'flash' => '1',
	            'formate'=>'json']);

### Send Message to a group

In your account, there must be an existing group and numbers under that group to send any message to a group. API for sending a simple message to a group is in the following format.

	$response = $smsObj->sendMessageToGroup($msg, $name , $group_id);

Optional Parameters

1. format :- Output format should be as specified by this variable,XML/PHP/JSON/JSONP. Default response will be in JSON 	
2. id :- Comma separated group ID

Mandatory Parameters

1. BASE_URL :- URL of your SMS Service
2. method :- Predefined method, i.e groups
3. sender :- Sender ID assigned to your account,Sender ID (6 alphabets)
4. name :- Group name
5. message :- Message to be sent,message text which is URL encoded(1000 char for normal, 500 for unicode)

### Add Contact to a Group

First, you need to create a group in your account only then you can add contact to an existing valid group. Function for adding contact to a group will be parameterised in the following format.

	$response = $smsObj->addContactsToGroup( $name , $receiverNumber ,['fullname'=>'abc','formate'=>'json']);

Mandatory Parameters

1. BASE_URL :- -URL of your SMS Service
2. method :- Predefined method is `groups.register`
3. number :- Mobile number of the contact (With or without 91) 	99XXXXXXXX or 9199XXXXXXXX
4. name :- Group name (case insensitive)

Optional Parameters

1. fullname :- name of the contact to be added i.e name of the contact
2. email :- Email of the contact to be added i.e email of the contact
3. format :- Output format should be as specified by this variable, XML/PHP/JSON/JSONP. Default response will be in JSON
4. action :- Flag to specify the action can be `add/delete`.  Add action is Default

### Edit Schedule

Application must have a scheduled SMS campaign to further modify it and also must save a Group ID of an SMS campaign to be rescheduled. In order to edit a scheduled slot, there should a minimum gap of 5mins before its execution. Parameter for an Edit schedule sms slot will be in the following format:

	$response = $smsObj->editSchedule( $new_time , $group_id ,['formate'=>'json']);

Mandatory Parameters

1. URL :- URL of your SMS Service
2. method :- Predefined method `sms.schedule`
3. groupid :- group id whose schedule needs to be edited
4. task :- Specifies that the scheduled time should be modified to the one mentioned within time parameter i.e `modify`
5. time :- Time to which the slot needs to be re-scheuled. EX Format: YYYY-MM-DD HH:MM:SS OR YYYY-MM-DD HH:MM AM/PM

Optional Parameters

1. format :- Output format should be as specified by this variable,XML/PHP/JSON/JSONP. Default response will be in JSON.

Expected API Error Codes

	A431 	Invalid Group ID
	A434 	Campaign within next 5 minutes cannot be modified/cancelled.

### Delete Schedule

Application must have a scheduled SMS campaign to further modify it and also must save a Group ID of an SMS campaign to be deleted. In order to delete a scheduled slot, there should a minimum gap of 5mins before its execution. Parameter for deleting an SMS campaign that is already scheduled is in the following format:

	$response = $smsObj->deleteScheduledSms($group_id ,['formate'=>'json']);

Mandatory Parameters

1. BASE_URL :- URL of your SMS Service.
2. method :- Predefined method `sms.schedule`.
3. groupid :- Group id that has to be deleted.

### Credit Availability Check

This function can also be used to check the credits in your our account which can be in the following format:

	$response = $smsObj->creditAvailabilityCheck(['formate'=>'json']);

Mandatory Parameters

1. URL :- URL of your SMS Service
2. method :- Predefined method `account.credits`

Optional Parameters

1. format :- Output format should be as specified by this variable,XML/PHP/JSON/JSONP. Default response will be in JSON.

### SI Lookup

To look information about any number, one must put country code along with number. Function for performing a lookup for specific numbers is in the following format: 

	$response = $smsobj->SILookup($mobile_number, ['format' => 'json']);

Mandatory Parameters

1. BASE_URL :- URL of your SMS Service
2. method :- Predefined method `lookup`
3. to :- Mobile number along with country prefix to which the SMS is to be sent. Also one provide multiple numbers in comma separated format. 	Mobile number(GET- 10numbers,POST- 100numbers)

### Creating a Txtly

Txtly is basically a shortened URL which can be used in text messages so that SMS would not exceed the characters. Function to create a Txtly link is as follows:
	
	$response = $smsObj->createtxtly("https://facebook.com/xyz/lmn/abc",['format' => 'json']);

Mandatory Parameters

1. URL 	URL of your SMS Service 	URL
2. method 	Predefined method 	txtly.create
3. url 	URL that requires to be shortened and tracked 	Long URL (url_encoded)

Optional Parameters

1. Format :- Output format should be as specified by this variable,XML/PHP/JSON/JSONP. Default response will be in JSON
2. Token :- http://msg.mn/heel Here heel is the token. It is unique for each txtly.This can customized word representing your brand/compnay. If not provided a random unique token is generated
3. Title :- A significant title to your txtly. If not provided, your txtly will not contain any title
4. Advanced :- Advanced analytics gives an option to track who (Recipient mobile numbers) visited the page. 1- will enable advanced analytics/0(default) - will disable advanced analytics
5. Track :- Location Track gives the city and state details of URL visitor. 1- will enable location tracking/0(default) - will disable location tracking
6. Attach :-media file that requires to be compressed to a short link. 	Provide the media file in a CURL request
7. callback :-callback URL along with parameters to extract customer device/IP details. http://TestDomain.com/TestFile.php?os={platform}&mobile={mobile} For raw response data format. If you want to get response in JSON format, then use (json) tag before callback URL. For eg. (json)http://TestDomain.com/TestFile.php?os={platform}&mobile={mobile}

Callback Parameters to be used in URL

1. client_ip :- IP from which the URL was accessed like `156.151.23.65`
2. host :- Domain name of the URL Ex.- Example.com
3. query_string :- Query string of the URL ex. `http://example.com/over/there?name=ferret`
4. user_agent :- User agent used by the URL i.e Mozilla/5.0 (iPad; U; CPU OS 3_2_1 like Mac OS X; en-us) AppleWebKit/531.21.10
5. browser :- Browser in which the URL was accessed i.e `msie`
6. browser_version :- Version of the browser in which the URL was accessed ex. `11.0`
7. browser_lang :- Language of the operating system in which the URL was accessed ex. `English`
8. browser_engine :- Browser engine in which the URL was accessed i.e `Trident`
9. resolution :- Resolution with which the URL was accessed `1024*768`
10. platform :- Operating system of the device in which the URL was accessed `Android OS`
11. platform_version :- Version of the operating system of the device in which the URL was accessed `5.0`
12. device_type :- Type of device in which the URL was accessed `phone`
13. device_brand :- Brand of the device in which the URL was accessed `Motorola`
14. device_version :- Version number of the device in which the URL was accessed `XT1068`
15. device_model :- Model name of the device in which the URL was accessed 	`HTC Dream`
16. touch_enabled :- If or not the device from where the URL was accessed is touch enabled or not ex. yes
17. latitude :- Latitude coordinates from where the URL was accessed `12.9667° N`
18. longitude :- Longitude coordinates from where the URL was accessed `77.5667° E`
19. country :- Country from where the URL was accessed 	`India`
20. region :- State from where the URL was accessed `Karnataka`
21. city :- City from where the URL was accessed `Bangalore`
22. created :- Requested time in unixtimestamp 	`1426243175`
23. mobile :- Short URL recepient mobile number `99XXXXXXXX`
24. os_code :- code of the OS from where the URL was accessed i.e `(AN)` for Android'

Expected Error Codes

1. E500 	Please provide url to redirect
2. E5011 	Txtly Already Exists. Please try another one.
3. E502 	Please attach a file
4. E503 	Invalid file format. Upload valid file.
5. E504 	File size exceed. Maxmium 50000 Kb.
6. E505 	File upload failed. Please try again.

### Deleting a Txtly

One must have a txtly id to delete it from database. Here is a function to delete a Txtly link:

	$response = $smsObj->deletetxtly("205XXX",['format' => 'json']);

Mandatory Parameters

1. URL :- URL of your SMS Service
2. method :- Predefined method 	`txtly`
3. task :- Task to be performed on txtly `delete`
4. id :- id of the txtly `123XXXX`
5. app :- Application reference `1`

Optional Parameters

1. format :- Output format should be as specified by this variable,XML/PHP/JSON/JSONP. Default response will be in JSON.

### Txtly Reports Extraction

Function to Pull logs for all txtly shortened links in your account:

	$response = $smsObj->txtlyReportExtract(['format' => 'json']);

Mandatory Parameters

1. URL :- URL of your SMS Service
2. method :- Predefined method 	`txtly`
3. app :- Txtly Application Logs `1`

Optional Parameter

1. format :- Output format should be as specified by this variable,XML/PHP/JSON/JSONP. Default response will be in JSON

### Pull logs for Individual Txtly

Here is an function to pull logs for individual txtly from our account:

	$response = $smsObj->pullLogForIndividualtxtl("223XXX",['format' => 'json']);

Mandatory Parameters

1. URL :- URL of your SMS Service
2. method :- Predefined method `txtly.logs`
3. app :- Txtly Application Logs `1`
4. id :- Id of the txtly for which you want logs `123XXXXX`


License
-------

This library is licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)
