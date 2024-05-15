<h1>Investigation of Malicious Port and Vulnerability Scanning Attempts</h1>


<h2>Scenario</h2>
Our website has shown signs of high resource usage, but it doesn't look like a distributed denial-of-service attack, because the request are coming from one single IP address. They seem to be performing a port scan and trying to access different resources on the website - it could be a vulnerability scanner. We need you to investigate and gather information about this activity, include where it's coming from, what tool are they using, and the resources they have accessed.
<br />


<h2>What we know!</h2>

- Investigation should be scoped to using Fortigate_utm logs only.
- Domain: imreallynotbatman.com

<h2>Environments Used </h2>

- Splunk

<h2>Investigation walk-through:</h2>

<p align="center">
<b/>First we're going to find the name of the vulnerabiltiy scanner being used. To do so we enter the following string and set sampling to 1:100
  so we're not waiting forever for Splunk to collect the data, then 
  we're going to scroll down to the 'Selected Fields' section and click on 'sourcetype. Since we're using logs form fortigate_utm
  only, we're going to click it and add to our search query: <br/>
<img src="https://i.imgur.com/ydto8J4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/OKzy7zq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next we're going to add the domain we need to the search query. To do so we're going to to scroll down to 'url_domain' section on
  the left panel, once clicked, under 'values' click on 'imnotreallybatman.com':  <br/>
<img src="https://i.imgur.com/DjR46Zj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Now we're going to add the string 'vulnerability' to the search to find data involving the vulnerability scanner. The complete search up to this point should be: index="botsv1" sourcetype=fortigate_utm url_domain="imreallynotbatman.com" vulnerability:<br/>
<img src="https://i.imgur.com/aEnnCwa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
From our search we have a lot of valuable information such as the sourceIP conducting the scanning, the destinationIP(company website),
  the country location and the type of vulnerabiltiy scanner being used to target the website **adding ( | sort_time asc) will sort events in time ascending order**:  <br/>
<img src="https://i.imgur.com/d3FO0fm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/cxI9E5J.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

<h1>Summary</h1>
From the information gathered, we can see that a malicious actor located in the United States with an IP address of 40.80.148.42 used an Acunetix web vulnerability scanner on the companies webserver.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
